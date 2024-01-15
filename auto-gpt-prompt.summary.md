Based on the information provided, here are some prompt examples constructed to align with the context and structure of AutoGPT:

### Prompt Example to Plan

**System Instruction:**
```
You are an expert project planner. Your responsibility is to create work plans for autonomous agents. You will be given a name, a role, and a set of goals for the agent to accomplish. Your job is to break down those goals into a set of tasks that the agent can accomplish to achieve those goals. Agents are resourceful, but require clear instructions.
```

**User Task:**
```
"""Develop a detailed marketing strategy for the upcoming product launch."""
```

**Response:**
```json
{
  "thoughts": {
    "text": "Considering the product's target market, a multi-channel approach will be necessary.",
    "reasoning": "This broadens reach and maximizes engagement.",
    "plan": "- Research target audience\n- Identify optimal marketing channels\n- Plan content strategy",
    "criticism": "May need to adjust based on budget constraints.",
    "speak": "I'm formulating a multi-faceted marketing strategy tailored to our target demographic."
  },
  "command": {
    "name": "create_marketing_plan",
    "args": {
      "audience_research": "true",
      "channel_identification": "true",
      "content_planning": "true"
    }
  }
}
```

### Prompt Example to Execute Next Abilities

**System Instruction:**
```
Your current task is to increase website traffic. You have taken 5 actions on this task already.
```

**Action History:**
```
### Step 5: Executed `analyze_traffic_sources()`
- **Reasoning:** "Identified the most promising traffic sources."
- **Status:** `success`
```

**Additional Info:**
```
Search engine optimization shows potential for growth.
```

**Response:**
```json
{
  "thoughts": {
    "text": "SEO could be the key to sustainable traffic increase.",
    "reasoning": "Based on the analysis, organic search has untapped potential.",
    "self_criticism": "Need to ensure content is high quality to benefit from SEO.",
    "speak": "I'll focus on optimizing our content for search engines to attract more visitors."
  },
  "command": {
    "name": "optimize_seo",
    "args": {
      "keyword_research": "true",
      "on_page_optimization": "true"
    }
  }
}
```

### Prompt Example for Feedback

**System Instruction:**
```
The user interrupted the action with the following feedback: "The marketing plan is too focused on online channels, we need to consider offline interactions too."
```

**Response:**
```json
{
  "command": {
    "name": "revise_marketing_plan",
    "args": {
      "include_offline_channels": "true"
    }
  }
}
```

### Prompt Example to Summary

**User Request:**
```
"Write a concise summary of the following text:"

LITERAL TEXT: """The company's revenue has grown by 15% this quarter, largely due to improved online sales and marketing efforts."""
```

**Response:**
```
CONCISE SUMMARY: The company's quarterly revenue increased by 15%, with online sales and marketing being significant contributors to this growth.
```

### Prompt Example for Search Tools

**System Instruction:**
```
## Search results

### "Best SEO Practices"
**URL:** http://seoexample.com/best-practices  
**Excerpt:** "Effective SEO strategies include regular content updates, keyword optimization, and backlinking."

### "Offline Marketing Channels"
**URL:** http://marketingexample.com/offline-channels  
**Excerpt:** "Incorporating offline marketing can involve community events, print media, and direct mail campaigns."
```

### Command & Action Abstraction Example

**Command Example:**
```python
@command(
  "web_search",
  {
    "query": "Best practices for online and offline marketing"
  }
)
@command(
  "ask_user",
  {
    "question": "Do you have any specific offline channels in mind?"
  }
)
@command(
  "read_webpage",
  {
    "url": "http://marketingexample.com/offline-channels",
    "question": "What are the most effective offline marketing strategies?"
  }
)
```

### Task Abstraction Example

**Task Definition:** 
```json
{
  "objective": "Increase brand awareness in the local community",
  "type": "marketing",
  "priority": 8,
  "ready_criteria": ["Identify local community events", "Budget approval received"],
  "acceptance_criteria": ["Brand presence at minimum 3 local events", "10% increase in local website traffic"]
}
```

These examples provide a structured approach to demonstrate planning, executing abilities,receiving feedback, summarizing, and utilizing search tools within the framework of AutoGPT's planning and execution strategies.
