# Basic Prompt Framework

source from [here](https://learningprompt.wiki/docs/chatGPT/tutorial-extras/chatGPT-prompt-framework)

After researching many ChatGPT prompt frameworks, I find Elavid Saravia's summary to be very clear. He states prompts contain:

Instruction（required）： The specific task you want the model to perform.
Context（optional）： Background information providing context to guide better responses.
Input Data (optional): Data for the model to process.
Output Indicator (optional): Specifies desired output type or format.
If you construct a prompt following this framework, the model's results will be consistent.

Of course, you don't necessarily have to include all four elements in your prompt. You can arrange and combine them according to your specific requirements. For example, let's consider a few scenarios:

Inference: Instruction + Context + Input Data
Information Extraction: Instruction + Context + Input Data + Output Indicator


## Prompt Template

### Task

* [ ] Task: `{{task}}`

### Context

```context info```

### Input Data

```input data```

### Output Indicator

```output indicator``` 