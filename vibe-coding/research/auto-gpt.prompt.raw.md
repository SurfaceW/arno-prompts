# Learn from AutoGPT prompt

## Prompt Strategies


### OneShot Core Prompt Builder

🌟🌟 from: https://github.com/Significant-Gravitas/AutoGPT/blob/autogpt-v0.5.0/autogpts/autogpt/autogpt/agents/prompt_strategies/one_shot.py

```python
from __future__ import annotations

import json
import platform
import re
from logging import Logger
from typing import TYPE_CHECKING, Callable, Optional

import distro

if TYPE_CHECKING:
    from autogpt.agents.agent import Agent
    from autogpt.models.action_history import Episode

from autogpt.agents.utils.exceptions import InvalidAgentResponseError
from autogpt.config import AIDirectives, AIProfile
from autogpt.core.configuration.schema import SystemConfiguration, UserConfigurable
from autogpt.core.prompting import (
    ChatPrompt,
    LanguageModelClassification,
    PromptStrategy,
)
from autogpt.core.resource.model_providers.schema import (
    AssistantChatMessageDict,
    ChatMessage,
    CompletionModelFunction,
)
from autogpt.core.utils.json_schema import JSONSchema
from autogpt.json_utils.utilities import extract_dict_from_response
from autogpt.prompts.utils import format_numbered_list, indent


class OneShotAgentPromptConfiguration(SystemConfiguration):
    DEFAULT_BODY_TEMPLATE: str = (
        "## Constraints\n"
        "You operate within the following constraints:\n"
        "{constraints}\n"
        "\n"
        "## Resources\n"
        "You can leverage access to the following resources:\n"
        "{resources}\n"
        "\n"
        "## Commands\n"
        "These are the ONLY commands you can use."
        " Any action you perform must be possible through one of these commands:\n"
        "{commands}\n"
        "\n"
        "## Best practices\n"
        "{best_practices}"
    )

    DEFAULT_CHOOSE_ACTION_INSTRUCTION: str = (
        "Determine exactly one command to use next based on the given goals "
        "and the progress you have made so far, "
        "and respond using the JSON schema specified previously:"
    )

    DEFAULT_RESPONSE_SCHEMA = JSONSchema(
        type=JSONSchema.Type.OBJECT,
        properties={
            "thoughts": JSONSchema(
                type=JSONSchema.Type.OBJECT,
                required=True,
                properties={
                    "observations": JSONSchema(
                        description=(
                            "Relevant observations from your last action (if any)"
                        ),
                        type=JSONSchema.Type.STRING,
                        required=False,
                    ),
                    "text": JSONSchema(
                        description="Thoughts",
                        type=JSONSchema.Type.STRING,
                        required=True,
                    ),
                    "reasoning": JSONSchema(
                        type=JSONSchema.Type.STRING,
                        required=True,
                    ),
                    "self_criticism": JSONSchema(
                        description="Constructive self-criticism",
                        type=JSONSchema.Type.STRING,
                        required=True,
                    ),
                    "plan": JSONSchema(
                        description=(
                            "Short markdown-style bullet list that conveys the "
                            "long-term plan"
                        ),
                        type=JSONSchema.Type.STRING,
                        required=True,
                    ),
                    "speak": JSONSchema(
                        description="Summary of thoughts, to say to user",
                        type=JSONSchema.Type.STRING,
                        required=True,
                    ),
                },
            ),
            "command": JSONSchema(
                type=JSONSchema.Type.OBJECT,
                required=True,
                properties={
                    "name": JSONSchema(
                        type=JSONSchema.Type.STRING,
                        required=True,
                    ),
                    "args": JSONSchema(
                        type=JSONSchema.Type.OBJECT,
                        required=True,
                    ),
                },
            ),
        },
    )

    body_template: str = UserConfigurable(default=DEFAULT_BODY_TEMPLATE)
    response_schema: dict = UserConfigurable(
        default_factory=DEFAULT_RESPONSE_SCHEMA.to_dict
    )
    choose_action_instruction: str = UserConfigurable(
        default=DEFAULT_CHOOSE_ACTION_INSTRUCTION
    )
    use_functions_api: bool = UserConfigurable(default=False)

    #########
    # State #
    #########
    # progress_summaries: dict[tuple[int, int], str] = Field(
    #     default_factory=lambda: {(0, 0): ""}
    # )


class OneShotAgentPromptStrategy(PromptStrategy):
    default_configuration: OneShotAgentPromptConfiguration = (
        OneShotAgentPromptConfiguration()
    )

    def __init__(
        self,
        configuration: OneShotAgentPromptConfiguration,
        logger: Logger,
    ):
        self.config = configuration
        self.response_schema = JSONSchema.from_dict(configuration.response_schema)
        self.logger = logger

    @property
    def model_classification(self) -> LanguageModelClassification:
        return LanguageModelClassification.FAST_MODEL  # FIXME: dynamic switching

    def build_prompt(
        self,
        *,
        task: str,
        ai_profile: AIProfile,
        ai_directives: AIDirectives,
        commands: list[CompletionModelFunction],
        event_history: list[Episode],
        include_os_info: bool,
        max_prompt_tokens: int,
        count_tokens: Callable[[str], int],
        count_message_tokens: Callable[[ChatMessage | list[ChatMessage]], int],
        extra_messages: Optional[list[ChatMessage]] = None,
        **extras,
    ) -> ChatPrompt:
        """Constructs and returns a prompt with the following structure:
        1. System prompt
        2. Message history of the agent, truncated & prepended with running summary
            as needed
        3. `cycle_instruction`
        """
        if not extra_messages:
            extra_messages = []

        system_prompt = self.build_system_prompt(
            ai_profile=ai_profile,
            ai_directives=ai_directives,
            commands=commands,
            include_os_info=include_os_info,
        )
        system_prompt_tlength = count_message_tokens(ChatMessage.system(system_prompt))

        user_task = f'"""{task}"""'
        user_task_tlength = count_message_tokens(ChatMessage.user(user_task))

        response_format_instr = self.response_format_instruction(
            self.config.use_functions_api
        )
        extra_messages.append(ChatMessage.system(response_format_instr))

        final_instruction_msg = ChatMessage.user(self.config.choose_action_instruction)
        final_instruction_tlength = count_message_tokens(final_instruction_msg)

        if event_history:
            progress = self.compile_progress(
                event_history,
                count_tokens=count_tokens,
                max_tokens=(
                    max_prompt_tokens
                    - system_prompt_tlength
                    - user_task_tlength
                    - final_instruction_tlength
                    - count_message_tokens(extra_messages)
                ),
            )
            extra_messages.insert(
                0,
                ChatMessage.system(f"## Progress\n\n{progress}"),
            )

        prompt = ChatPrompt(
            messages=[
                ChatMessage.system(system_prompt),
                ChatMessage.user(user_task),
                *extra_messages,
                final_instruction_msg,
            ],
        )

        return prompt

    def build_system_prompt(
        self,
        ai_profile: AIProfile,
        ai_directives: AIDirectives,
        commands: list[CompletionModelFunction],
        include_os_info: bool,
    ) -> str:
        system_prompt_parts = (
            self._generate_intro_prompt(ai_profile)
            + (self._generate_os_info() if include_os_info else [])
            + [
                self.config.body_template.format(
                    constraints=format_numbered_list(
                        ai_directives.constraints
                        + self._generate_budget_constraint(ai_profile.api_budget)
                    ),
                    resources=format_numbered_list(ai_directives.resources),
                    commands=self._generate_commands_list(commands),
                    best_practices=format_numbered_list(ai_directives.best_practices),
                )
            ]
            + [
                "## Your Task\n"
                "The user will specify a task for you to execute, in triple quotes,"
                " in the next message. Your job is to complete the task while following"
                " your directives as given above, and terminate when your task is done."
            ]
        )

        # Join non-empty parts together into paragraph format
        return "\n\n".join(filter(None, system_prompt_parts)).strip("\n")

    def compile_progress(
        self,
        episode_history: list[Episode],
        max_tokens: Optional[int] = None,
        count_tokens: Optional[Callable[[str], int]] = None,
    ) -> str:
        if max_tokens and not count_tokens:
            raise ValueError("count_tokens is required if max_tokens is set")

        steps: list[str] = []
        tokens: int = 0
        # start: int = len(episode_history)

        for i, c in reversed(list(enumerate(episode_history))):
            step = f"### Step {i+1}: Executed `{c.action.format_call()}`\n"
            step += f'- **Reasoning:** "{c.action.reasoning}"\n'
            step += (
                f"- **Status:** `{c.result.status if c.result else 'did_not_finish'}`\n"
            )
            if c.result:
                if c.result.status == "success":
                    result = str(c.result)
                    result = "\n" + indent(result) if "\n" in result else result
                    step += f"- **Output:** {result}"
                elif c.result.status == "error":
                    step += f"- **Reason:** {c.result.reason}\n"
                    if c.result.error:
                        step += f"- **Error:** {c.result.error}\n"
                elif c.result.status == "interrupted_by_human":
                    step += f"- **Feedback:** {c.result.feedback}\n"

            if max_tokens and count_tokens:
                step_tokens = count_tokens(step)
                if tokens + step_tokens > max_tokens:
                    break
                tokens += step_tokens

            steps.insert(0, step)
        #     start = i

        # # TODO: summarize remaining
        # part = slice(0, start)

        return "\n\n".join(steps)

    def response_format_instruction(self, use_functions_api: bool) -> str:
        response_schema = self.response_schema.copy(deep=True)
        if (
            use_functions_api
            and response_schema.properties
            and "command" in response_schema.properties
        ):
            del response_schema.properties["command"]

        # Unindent for performance
        response_format = re.sub(
            r"\n\s+",
            "\n",
            response_schema.to_typescript_object_interface("Response"),
        )

        instruction = (
            "Respond with pure JSON containing your thoughts, " "and invoke a tool."
            if use_functions_api
            else "Respond with pure JSON."
        )

        return (
            f"{instruction} "
            "The JSON object should be compatible with the TypeScript type `Response` "
            f"from the following:\n{response_format}"
        )

    def _generate_intro_prompt(self, ai_profile: AIProfile) -> list[str]:
        """Generates the introduction part of the prompt.

        Returns:
            list[str]: A list of strings forming the introduction part of the prompt.
        """
        return [
            f"You are {ai_profile.ai_name}, {ai_profile.ai_role.rstrip('.')}.",
            "Your decisions must always be made independently without seeking "
            "user assistance. Play to your strengths as an LLM and pursue "
            "simple strategies with no legal complications.",
        ]

    def _generate_os_info(self) -> list[str]:
        """Generates the OS information part of the prompt.

        Params:
            config (Config): The configuration object.

        Returns:
            str: The OS information part of the prompt.
        """
        os_name = platform.system()
        os_info = (
            platform.platform(terse=True)
            if os_name != "Linux"
            else distro.name(pretty=True)
        )
        return [f"The OS you are running on is: {os_info}"]

    def _generate_budget_constraint(self, api_budget: float) -> list[str]:
        """Generates the budget information part of the prompt.

        Returns:
            list[str]: The budget information part of the prompt, or an empty list.
        """
        if api_budget > 0.0:
            return [
                f"It takes money to let you run. "
                f"Your API budget is ${api_budget:.3f}"
            ]
        return []

    def _generate_commands_list(self, commands: list[CompletionModelFunction]) -> str:
        """Lists the commands available to the agent.

        Params:
            agent: The agent for which the commands are being listed.

        Returns:
            str: A string containing a numbered list of commands.
        """
        try:
            return format_numbered_list([cmd.fmt_line() for cmd in commands])
        except AttributeError:
            self.logger.warning(f"Formatting commands failed. {commands}")
            raise

    def parse_response_content(
        self,
        response: AssistantChatMessageDict,
    ) -> Agent.ThoughtProcessOutput:
        if "content" not in response:
            raise InvalidAgentResponseError("Assistant response has no text content")

        assistant_reply_dict = extract_dict_from_response(response["content"])

        _, errors = self.response_schema.validate_object(
            object=assistant_reply_dict,
            logger=self.logger,
        )
        if errors:
            raise InvalidAgentResponseError(
                "Validation of response failed:\n  "
                + ";\n  ".join([str(e) for e in errors])
            )

        # Get command name and arguments
        command_name, arguments = extract_command(
            assistant_reply_dict, response, self.config.use_functions_api
        )
        return command_name, arguments, assistant_reply_dict


#############
# Utilities #
#############


def extract_command(
    assistant_reply_json: dict,
    assistant_reply: AssistantChatMessageDict,
    use_openai_functions_api: bool,
) -> tuple[str, dict[str, str]]:
    """Parse the response and return the command name and arguments

    Args:
        assistant_reply_json (dict): The response object from the AI
        assistant_reply (ChatModelResponse): The model response from the AI
        config (Config): The config object

    Returns:
        tuple: The command name and arguments

    Raises:
        json.decoder.JSONDecodeError: If the response is not valid JSON

        Exception: If any other error occurs
    """
    if use_openai_functions_api:
        if not assistant_reply.get("tool_calls"):
            raise InvalidAgentResponseError("No 'tool_calls' in assistant reply")
        assistant_reply_json["command"] = {
            "name": assistant_reply["tool_calls"][0]["function"]["name"],
            "args": json.loads(
                assistant_reply["tool_calls"][0]["function"]["arguments"]
            ),
        }
    try:
        if not isinstance(assistant_reply_json, dict):
            raise InvalidAgentResponseError(
                f"The previous message sent was not a dictionary {assistant_reply_json}"
            )

        if "command" not in assistant_reply_json:
            raise InvalidAgentResponseError("Missing 'command' object in JSON")

        command = assistant_reply_json["command"]
        if not isinstance(command, dict):
            raise InvalidAgentResponseError("'command' object is not a dictionary")

        if "name" not in command:
            raise InvalidAgentResponseError("Missing 'name' field in 'command' object")

        command_name = command["name"]

        # Use an empty dictionary if 'args' field is not present in 'command' object
        arguments = command.get("args", {})

        return command_name, arguments

    except json.decoder.JSONDecodeError:
        raise InvalidAgentResponseError("Invalid JSON")

    except Exception as e:
        raise InvalidAgentResponseError(str(e))
```


## Agent

```python
constraints: [
  'Exclusively use the commands listed below.',
  'You can only act proactively, and are unable to start background jobs or set up webhooks for yourself. Take this into account when planning your actions.',
  'You are unable to interact with physical objects. If this is absolutely necessary to fulfill a task or objective or to complete a step, you must ask the user to do it for you. If the user refuses this, and there is no other way to achieve your goals, you must terminate to avoid wasting time and energy.'
]
resources: [
  'Internet access for searches and information gathering.',
  'The ability to read and write files.',
  'You are a Large Language Model, trained on millions of pages of text, including a lot of factual knowledge. Make use of this factual knowledge to avoid unnecessary gathering of information.'
]
best_practices: [
  'Continuously review and analyze your actions to ensure you are performing to the best of your abilities.',
  'Constructively self-criticize your big-picture behavior constantly.',
  'Reflect on past decisions and strategies to refine your approach.',
  'Every command has a cost, so be smart and efficient. Aim to complete tasks in the least number of steps.',
  'Only make use of your information gathering abilities to find information that you don''t yet have knowledge of.'
]
```

---

from: https://github.com/Significant-Gravitas/AutoGPT/blob/autogpt-v0.5.0/autogpts/autogpt/autogpt/prompts/prompt.py

DEFAULT_TRIGGERING_PROMPT = (
    "Determine exactly one command to use next based on the given goals "
    "and the progress you have made so far, "
    "and respond using the JSON schema specified previously:"
)

--- 

### Text Summary & Memory

from: https://github.com/Significant-Gravitas/AutoGPT/blob/autogpt-v0.5.0/autogpts/autogpt/autogpt/processing/text.py

'Include any information that can be used to answer the question: "%s". '
"Do not directly answer the question itself."

```python
ChatMessage.user(
    "Write a concise summary of the following text."
    f"{f' {instruction}' if instruction is not None else ''}:"
    "\n\n\n"
    f'LITERAL TEXT: """{text}"""'
    "\n\n\n"
    "CONCISE SUMMARY: The text is best summarized as"
)
```

```python

def from_ai_action(self, ai_message: ChatMessage, result_message: ChatMessage):
    # The result_message contains either user feedback
    # or the result of the command specified in ai_message

    if ai_message.role != "assistant":
        raise ValueError(f"Invalid role on 'ai_message': {ai_message.role}")

    result = (
        result_message.content
        if result_message.content.startswith("Command")
        else "None"
    )
    user_input = (
        result_message.content
        if result_message.content.startswith("Human feedback")
        else "None"
    )
    memory_content = (
        f"Assistant Reply: {ai_message.content}"
        "\n\n"
        f"Result: {result}"
        "\n\n"
        f"Human Feedback: {user_input}"
    )

    return self.from_text(
        text=memory_content,
        source_type="agent_history",
        how_to_summarize=(
            "if possible, also make clear the link between the command in the"
            " assistant's response and the command result. "
            "Do not mention the human feedback if there is none.",
        ),
    )
```

### Feedback

'The user interrupted the action with the following feedback: "%s"'
% self.feedback

### Action History Step logs

from: https://github.com/Significant-Gravitas/AutoGPT/blob/autogpt-v0.5.0/autogpts/autogpt/autogpt/models/action_history.py

```python
def fmt_paragraph(self) -> str:
    steps: list[str] = []

    for i, c in enumerate(self.episodes, 1):
        step = f"### Step {i}: Executed `{c.action.format_call()}`\n"
        step += f'- **Reasoning:** "{c.action.reasoning}"\n'
        step += (
            f"- **Status:** `{c.result.status if c.result else 'did_not_finish'}`\n"
        )
        if c.result:
            if c.result.status == "success":
                result = str(c.result)
                result = "\n" + indent(result) if "\n" in result else result
                step += f"- **Output:** {result}"
            elif c.result.status == "error":
                step += f"- **Reason:** {c.result.reason}\n"
                if c.result.error:
                    step += f"- **Error:** {c.result.error}\n"
            elif c.result.status == "interrupted_by_human":
                step += f"- **Feedback:** {c.result.feedback}\n"

        steps.append(step)

    return "\n\n".join(steps)
```

### Search Tools

from: https://github.com/Significant-Gravitas/AutoGPT/blob/autogpt-v0.5.0/autogpts/autogpt/autogpt/commands/web_search.py

```python
results = (
    "## Search results\n"
    # "Read these results carefully."
    # " Extract the information you need for your task from the list of results"
    # " if possible. Otherwise, choose a webpage from the list to read entirely."
    # "\n\n"
) + "\n\n".join(
    f"### \"{r['title']}\"\n"
    f"**URL:** {r['url']}  \n"
    "**Excerpt:** " + (f'"{exerpt}"' if (exerpt := r.get("exerpt")) else "N/A")
    for r in search_results
)
```

#### Command & Action Abstraction

from: https://github.com/Significant-Gravitas/AutoGPT/blob/autogpt-v0.5.0/autogpts/autogpt/autogpt/commands/image_gen.py

```python
@command(
    "web_search",
    "Searches the web",
    {
        "query": JSONSchema(
            type=JSONSchema.Type.STRING,
            description="The search query",
            required=True,
        )
    },
    aliases=["search"],
)
@command(
    "ask_user",
    (
        "If you need more details or information regarding the given goals,"
        " you can ask the user for input"
    ),
    {
        "question": JSONSchema(
            type=JSONSchema.Type.STRING,
            description="The question or prompt to the user",
            required=True,
        )
    },
    enabled=lambda config: not config.noninteractive_mode,
)
@command(
  "read_webpage",
  (
      "Read a webpage, and extract specific information from it"
      " if a question is specified."
      " If you are looking to extract specific information from the webpage,"
      " you should specify a question."
  ),
  {
      "url": JSONSchema(
          type=JSONSchema.Type.STRING,
          description="The URL to visit",
          required=True,
      ),
      "question": JSONSchema(
          type=JSONSchema.Type.STRING,
          description=(
              "A question that you want to answer using the content of the webpage."
          ),
          required=False,
      ),
  },
)
```

### Task Abstraction

```python
import enum
from typing import Optional

from pydantic import BaseModel, Field

from autogpt.core.ability.schema import AbilityResult


class TaskType(str, enum.Enum):
    RESEARCH = "research"
    WRITE = "write"
    EDIT = "edit"
    CODE = "code"
    DESIGN = "design"
    TEST = "test"
    PLAN = "plan"


class TaskStatus(str, enum.Enum):
    BACKLOG = "backlog"
    READY = "ready"
    IN_PROGRESS = "in_progress"
    DONE = "done"


class TaskContext(BaseModel):
    cycle_count: int = 0
    status: TaskStatus = TaskStatus.BACKLOG
    parent: Optional["Task"] = None
    prior_actions: list[AbilityResult] = Field(default_factory=list)
    memories: list = Field(default_factory=list)
    user_input: list[str] = Field(default_factory=list)
    supplementary_info: list[str] = Field(default_factory=list)
    enough_info: bool = False


class Task(BaseModel):
    objective: str
    type: str  # TaskType  FIXME: gpt does not obey the enum parameter in its schema
    priority: int
    ready_criteria: list[str]
    acceptance_criteria: list[str]
    context: TaskContext = Field(default_factory=TaskContext)


# Need to resolve the circular dependency between Task and TaskContext
# once both models are defined.
TaskContext.update_forward_refs()
```

### Planner Prompt Template

from: https://github.com/Significant-Gravitas/AutoGPT/blob/autogpt-v0.5.0/autogpts/autogpt/autogpt/core/planning/prompt_strategies/initial_plan.py

Initial plan:

```python
class InitialPlan(PromptStrategy):
    DEFAULT_SYSTEM_PROMPT_TEMPLATE = (
        "You are an expert project planner. "
        "Your responsibility is to create work plans for autonomous agents. "
        "You will be given a name, a role, set of goals for the agent to accomplish. "
        "Your job is to break down those goals into a set of tasks that the agent can"
        " accomplish to achieve those goals. "
        "Agents are resourceful, but require clear instructions."
        " Each task you create should have clearly defined `ready_criteria` that the"
        " agent can check to see if the task is ready to be started."
        " Each task should also have clearly defined `acceptance_criteria` that the"
        " agent can check to evaluate if the task is complete. "
        "You should create as many tasks as you think is necessary to accomplish"
        " the goals.\n\n"
        "System Info:\n{system_info}"
    )

    DEFAULT_SYSTEM_INFO = [
        "The OS you are running on is: {os_info}",
        "It takes money to let you run. Your API budget is ${api_budget:.3f}",
        "The current time and date is {current_time}",
    ]

    DEFAULT_USER_PROMPT_TEMPLATE = (
        "You are {agent_name}, {agent_role}\n" "Your goals are:\n" "{agent_goals}"
    )

    DEFAULT_CREATE_PLAN_FUNCTION = CompletionModelFunction(
        name="create_initial_agent_plan",
        description=(
            "Creates a set of tasks that forms the initial plan of an autonomous agent."
        ),
        parameters={
            "task_list": JSONSchema(
                type=JSONSchema.Type.ARRAY,
                items=JSONSchema(
                    type=JSONSchema.Type.OBJECT,
                    properties={
                        "objective": JSONSchema(
                            type=JSONSchema.Type.STRING,
                            description=(
                                "An imperative verb phrase that succinctly describes "
                                "the task."
                            ),
                        ),
                        "type": JSONSchema(
                            type=JSONSchema.Type.STRING,
                            description="A categorization for the task.",
                            enum=[t.value for t in TaskType],
                        ),
                        "acceptance_criteria": JSONSchema(
                            type=JSONSchema.Type.ARRAY,
                            items=JSONSchema(
                                type=JSONSchema.Type.STRING,
                                description=(
                                    "A list of measurable and testable criteria that "
                                    "must be met for the task to be considered "
                                    "complete."
                                ),
                            ),
                        ),
                        "priority": JSONSchema(
                            type=JSONSchema.Type.INTEGER,
                            description=(
                                "A number between 1 and 10 indicating the priority of "
                                "the task relative to other generated tasks."
                            ),
                            minimum=1,
                            maximum=10,
                        ),
                        "ready_criteria": JSONSchema(
                            type=JSONSchema.Type.ARRAY,
                            items=JSONSchema(
                                type=JSONSchema.Type.STRING,
                                description=(
                                    "A list of measurable and testable criteria that "
                                    "must be met before the task can be started."
                                ),
                            ),
                        ),
                    },
                ),
            ),
        },
    )

    default_configuration: InitialPlanConfiguration = InitialPlanConfiguration(
        model_classification=LanguageModelClassification.SMART_MODEL,
        system_prompt_template=DEFAULT_SYSTEM_PROMPT_TEMPLATE,
        system_info=DEFAULT_SYSTEM_INFO,
        user_prompt_template=DEFAULT_USER_PROMPT_TEMPLATE,
        create_plan_function=DEFAULT_CREATE_PLAN_FUNCTION.schema,
    )
```

Name and goals:

from: https://github.com/Significant-Gravitas/AutoGPT/blob/autogpt-v0.5.0/autogpts/autogpt/autogpt/core/planning/prompt_strategies/name_and_goals.py

```python
    DEFAULT_SYSTEM_PROMPT = (
        "Your job is to respond to a user-defined task, given in triple quotes, by "
        "invoking the `create_agent` function to generate an autonomous agent to "
        "complete the task. "
        "You should supply a role-based name for the agent, "
        "an informative description for what the agent does, and "
        "1 to 5 goals that are optimally aligned with the successful completion of "
        "its assigned task.\n"
        "\n"
        "Example Input:\n"
        '"""Help me with marketing my business"""\n\n'
        "Example Function Call:\n"
        "create_agent(name='CMOGPT', "
        "description='A professional digital marketer AI that assists Solopreneurs in "
        "growing their businesses by providing world-class expertise in solving "
        "marketing problems for SaaS, content products, agencies, and more.', "
        "goals=['Engage in effective problem-solving, prioritization, planning, and "
        "supporting execution to address your marketing needs as your virtual Chief "
        "Marketing Officer.', 'Provide specific, actionable, and concise advice to "
        "help you make informed decisions without the use of platitudes or overly "
        "wordy explanations.', 'Identify and prioritize quick wins and cost-effective "
        "campaigns that maximize results with minimal time and budget investment.', "
        "'Proactively take the lead in guiding you and offering suggestions when faced "
        "with unclear information or uncertainty to ensure your marketing strategy "
        "remains on track.'])"
    )
```

from: https://github.com/Significant-Gravitas/AutoGPT/blob/autogpt-v0.5.0/autogpts/autogpt/autogpt/core/planning/prompt_strategies/next_ability.py

next ability picker

```python
DEFAULT_SYSTEM_PROMPT_TEMPLATE = "System Info:\n{system_info}"

    DEFAULT_SYSTEM_INFO = [
        "The OS you are running on is: {os_info}",
        "It takes money to let you run. Your API budget is ${api_budget:.3f}",
        "The current time and date is {current_time}",
    ]

    DEFAULT_USER_PROMPT_TEMPLATE = (
        "Your current task is is {task_objective}.\n"
        "You have taken {cycle_count} actions on this task already. "
        "Here is the actions you have taken and their results:\n"
        "{action_history}\n\n"
        "Here is additional information that may be useful to you:\n"
        "{additional_info}\n\n"
        "Additionally, you should consider the following:\n"
        "{user_input}\n\n"
        "Your task of {task_objective} is complete when the following acceptance"
        " criteria have been met:\n"
        "{acceptance_criteria}\n\n"
        "Please choose one of the provided functions to accomplish this task. "
        "Some tasks may require multiple functions to accomplish. If that is the case,"
        " choose the function that you think is most appropriate for the current"
        " situation given your progress so far."
    )

    DEFAULT_ADDITIONAL_ABILITY_ARGUMENTS = {
        "motivation": JSONSchema(
            type=JSONSchema.Type.STRING,
            description=(
                "Your justification for choosing choosing this function instead of a "
                "different one."
            ),
        ),
        "self_criticism": JSONSchema(
            type=JSONSchema.Type.STRING,
            description=(
                "Thoughtful self-criticism that explains why this function may not be "
                "the best choice."
            ),
        ),
        "reasoning": JSONSchema(
            type=JSONSchema.Type.STRING,
            description=(
                "Your reasoning for choosing this function taking into account the "
                "`motivation` and weighing the `self_criticism`."
            ),
        ),
    }
```

Classical planning template:

from: https://github.com/Significant-Gravitas/AutoGPT/blob/autogpt-v0.5.0/autogpts/autogpt/autogpt/core/planning/templates.py

```python
# Rules of thumb:
# - Templates don't add new lines at the end of the string.  This is the
#   responsibility of the or a consuming template.

####################
# Planner defaults #
####################


USER_OBJECTIVE = (
    "Write a wikipedia style article about the project: "
    "https://github.com/significant-gravitas/AutoGPT"
)


# Plan Prompt
# -----------


PLAN_PROMPT_CONSTRAINTS = (
    "~4000 word limit for short term memory. Your short term memory is short, so "
    "immediately save important information to files.",
    "If you are unsure how you previously did something or want to recall past "
    "events, thinking about similar events will help you remember.",
    "No user assistance",
    "Exclusively use the commands listed below e.g. command_name",
)

PLAN_PROMPT_RESOURCES = (
    "Internet access for searches and information gathering.",
    "Long-term memory management.",
    "File output.",
)

PLAN_PROMPT_PERFORMANCE_EVALUATIONS = (
    "Continuously review and analyze your actions to ensure you are performing to"
    " the best of your abilities.",
    "Constructively self-criticize your big-picture behavior constantly.",
    "Reflect on past decisions and strategies to refine your approach.",
    "Every command has a cost, so be smart and efficient. Aim to complete tasks in"
    " the least number of steps.",
    "Write all code to a file",
)


PLAN_PROMPT_RESPONSE_DICT = {
    "thoughts": {
        "text": "thought",
        "reasoning": "reasoning",
        "plan": "- short bulleted\n- list that conveys\n- long-term plan",
        "criticism": "constructive self-criticism",
        "speak": "thoughts summary to say to user",
    },
    "command": {"name": "command name", "args": {"arg name": "value"}},
}

PLAN_PROMPT_RESPONSE_FORMAT = (
    "You should only respond in JSON format as described below\n"
    "Response Format:\n"
    "{response_json_structure}\n"
    "Ensure the response can be parsed by Python json.loads"
)

PLAN_TRIGGERING_PROMPT = (
    "Determine which next command to use, and respond using the format specified above:"
)

PLAN_PROMPT_MAIN = (
    "{header}\n\n"
    "GOALS:\n\n{goals}\n\n"
    "Info:\n{info}\n\n"
    "Constraints:\n{constraints}\n\n"
    "Commands:\n{commands}\n\n"
    "Resources:\n{resources}\n\n"
    "Performance Evaluations:\n{performance_evaluations}\n\n"
    "You should only respond in JSON format as described below\n"
    "Response Format:\n{response_json_structure}\n"
    "Ensure the response can be parsed by Python json.loads"
)


###########################
# Parameterized templates #
###########################
```
