# Homework 10: Business Agent

**Points:** 20 | **Due:** Sunday, April 19, 2026 @ 11pm Pacific

**Author:** Richard Young, Ph.D. | UNLV Lee Business School

**Compute:** CPU (free tier) — Uses OpenRouter API (no local GPU needed)

---

## Learning Objectives

1. **Understand** what AI agents are and how they work
2. **Build** an agent that uses **native tool calling** via an LLM API
3. **Design** tools as **JSON schemas** — the industry standard
4. **Implement** the ReAct (Reasoning + Acting) pattern
5. **Evaluate** agent behavior and handle failures gracefully

---

## Why This Matters for Business

> **Autonomous Operations:** Klarna's AI agent handles 2.3 million customer service conversations monthly—doing the work of 700 full-time agents. It doesn't just answer questions; it takes actions like processing refunds and updating accounts.

> **Sales Automation:** HubSpot's AI agents qualify leads, schedule meetings, and update CRM records automatically. Sales reps focus on closing deals while agents handle the repetitive coordination work.

> **Financial Analysis:** Morgan Stanley's AI agents pull data from multiple sources, generate reports, and flag anomalies—tasks that took analysts hours now happen in seconds.

> **The Agent Revolution:** Sam Altman predicts AI agents will be "the biggest platform shift in computing history." Businesses that master agents now will have a significant competitive advantage.

---

## Grading

| Component | Points | Effort | What We're Looking For |
|-----------|--------|--------|------------------------|
| Agent Setup | 3 | * | OpenRouter + Qwen connection working |
| Tool Design | 5 | ** | JSON schemas + Python functions |
| ReAct Loop | 5 | ** | Tool calling → Observation → Reasoning cycle |
| Business Task | 5 | ** | Multi-step business scenarios |
| Error Handling | 2 | * | Graceful failure on invalid inputs |
| **Total** | **20** | |

**Effort Key:** * Straightforward | ** Requires thinking | *** Challenge

---

## The Big Picture

An **AI Agent** = LLM + Tools + Loop

```
User Request
     ↓
┌─────────────────────────────────────┐
│  AGENT LOOP                         │
│                                     │
│  1. THINK: What should I do next?   │
│  2. ACT: Choose and use a tool      │
│  3. OBSERVE: Process tool result    │
│  4. REPEAT until task complete      │
│                                     │
└─────────────────────────────────────┘
     ↓
Final Answer
```

Unlike simple LLM calls, agents can:
- Take multiple steps to solve problems
- Use external tools (search, calculate, query databases)
- Adapt their strategy based on results
- Handle complex, multi-step workflows

![AI Agent Loop](ai_agent_loop.png)

---

## Prerequisites

You need a free OpenRouter API key:
1. Go to [openrouter.ai](https://openrouter.ai) and create an account
2. Navigate to **Keys** and create a new API key
3. In Google Colab, click the **key icon** in the left sidebar
4. Add a secret named `OPENROUTER_API_KEY` with your key

## Instructions

1. Open `MIS769_HW10_Business_Agent.ipynb` in Google Colab
2. Add your OpenRouter API key to Colab secrets
3. Understand the ReAct pattern and JSON tool calling format
4. Run the agent on business scenarios
5. Observe how the LLM autonomously selects and calls tools
6. Document the agent's reasoning and actions

---

## What Your Output Should Look Like

**Agent Tools (JSON Schema Format):**
```
✅ 5 tool schemas defined

Example schema (search_products):
{
  "type": "function",
  "function": {
    "name": "search_products",
    "description": "Search for products by name, category, or keyword.",
    "parameters": {
      "type": "object",
      "properties": {
        "query": {"type": "string", "description": "Search term"}
      },
      "required": ["query"]
    }
  }
}
```

**Agent Execution (Tool Calling):**
```
📋 TASK: "Find the cheapest laptop in stock and tell me about it."
============================================================

🔨 ACTION: search_products({"query": "laptop"})
👁️ OBSERVATION: [{"product_id":"LP001","name":"TechPro Laptop 15\"","price":899.99,...}, ...]

🔨 ACTION: filter_by_price({"min_price": 0, "max_price": 500})
👁️ OBSERVATION: [{"product_id":"LP004","name":"BudgetBook 15\"","price":399.99,...}]

🔨 ACTION: check_stock({"product_id": "LP004"})
👁️ OBSERVATION: {"product_id":"LP004","name":"BudgetBook 15\"","stock":42,"in_stock":true}

✅ FINAL ANSWER:
The cheapest laptop in stock is the BudgetBook 15" (LP004) at $399.99.
There are 42 units available. It features 4GB RAM, 128GB SSD, and
has a 3.8/5 rating — a solid budget option.
```

**Business Scenario:**
```
📋 TASK: "I need to set up a home office. I want a laptop, a monitor,
and a laptop stand. Find me options and tell me the total cost."
============================================================

🔨 ACTION: search_products({"query": "laptop"})
👁️ OBSERVATION: [4 laptops found with prices $399-$1299]

🔨 ACTION: search_products({"query": "monitor"})
👁️ OBSERVATION: [3 monitors found with prices $299-$549]

🔨 ACTION: get_product_details({"product_id": "AC004"})
👁️ OBSERVATION: {"name":"Laptop Stand Aluminum","price":34.99,...}

✅ FINAL ANSWER:
Here's a recommended mid-range home office setup:
- UltraBook Pro 14" (LP002): $649.99
- 4K Monitor 27" (MN002): $349.99
- Laptop Stand Aluminum (AC004): $34.99
Total: $1,034.97
```

---

## Common Mistakes (and How to Avoid Them)

| Mistake | Symptom | Fix |
|---------|---------|-----|
| No loop termination | Agent runs forever | Add max_steps limit |
| Tools with unclear names | Agent picks wrong tool | Use descriptive names + docstrings |
| No error handling | Agent crashes on tool failure | Catch exceptions, retry or report |
| Tools returning too much data | Token limit exceeded | Summarize tool outputs |
| Vague task description | Agent gets confused | Be specific about success criteria |
| No observation parsing | Agent ignores tool results | Explicitly include observations in prompt |

**If the agent gets stuck in a loop:**
- Add "If you've tried 3 times, report failure and stop"
- Limit max iterations: `for i in range(10):`

**If the agent picks wrong tools:**
- Improve tool descriptions
- Add few-shot examples of correct tool usage
- Make tool names more descriptive

---

## Questions to Answer

- **Q1:** Describe your agent's tools. Why did you choose them?
- **Q2:** Walk through one complete agent trace. What worked well?
- **Q3:** When did the agent fail? How would you improve it?
- **Q4:** How could this agent save time in a real business?

---

## Going Deeper (Optional Challenges)

### Challenge A: Multi-Agent System
Build two agents that can collaborate. Example: A "Researcher" agent finds information, and an "Analyst" agent interprets it. How do they communicate?

### Challenge B: Memory-Enhanced Agent
Add memory so your agent remembers previous conversations. Can it handle follow-up questions like "What about the second option you mentioned?"

### Challenge C: Self-Improving Agent
Let the agent learn from failures. If a tool call fails, have it record the mistake and avoid it in future runs.

---

## Quick Reference

```python
# Install dependency (only openai — it works with OpenRouter too)
!pip install openai

# 1. SETUP CLIENT (OpenRouter = OpenAI-compatible API)
from openai import OpenAI
client = OpenAI(
    base_url="https://openrouter.ai/api/v1",
    api_key="your-api-key-here",
)
MODEL = "qwen/qwen3-8b:free"  # Free model with tool calling

# 2. DEFINE TOOLS AS JSON SCHEMAS
tools = [
    {
        "type": "function",
        "function": {
            "name": "search_products",
            "description": "Search for products by keyword.",
            "parameters": {
                "type": "object",
                "properties": {
                    "query": {"type": "string", "description": "Search term"}
                },
                "required": ["query"]
            }
        }
    }
]

# 3. IMPLEMENT TOOL FUNCTIONS
def search_products(query):
    return json.dumps([{"id": "LP001", "name": "Laptop", "price": 899}])

TOOL_FUNCTIONS = {"search_products": search_products}

# 4. BUILD AGENT LOOP
def run_agent(task, max_steps=10):
    messages = [
        {"role": "system", "content": "You are a helpful assistant. Use tools."},
        {"role": "user", "content": task},
    ]
    for step in range(max_steps):
        response = client.chat.completions.create(
            model=MODEL, messages=messages, tools=tools, tool_choice="auto",
        )
        msg = response.choices[0].message

        if msg.tool_calls:
            messages.append(msg)  # Must append assistant msg first!
            for tc in msg.tool_calls:
                args = json.loads(tc.function.arguments)
                result = TOOL_FUNCTIONS[tc.function.name](**args)
                messages.append({
                    "role": "tool",
                    "tool_call_id": tc.id,
                    "content": result,
                })
        else:
            return msg.content  # Final answer
    return "Max steps reached"

# 5. RUN
run_agent("Find the cheapest product in our catalog")
```

**Agent Architecture Patterns:**
| Pattern | Description | Use Case |
|---------|-------------|----------|
| ReAct | Reason → Act → Observe loop | General tasks |
| Plan-and-Execute | Plan all steps, then execute | Complex multi-step |
| Tree of Thoughts | Explore multiple paths | Ambiguous problems |
| Reflexion | Self-critique and improve | Iterative refinement |

**Common Agent Tools:**
| Tool Type | Examples | Business Use |
|-----------|----------|--------------|
| Search | Web search, DB query | Information gathering |
| Calculate | Math, analytics | Financial analysis |
| API | REST calls, webhooks | System integration |
| File | Read, write, parse | Document processing |
| Communication | Email, Slack | Notifications |

---

## Submission

Upload to Canvas:
- Your completed `.ipynb` notebook with all cells executed

---

\vspace{1cm}

*— Richard Young, Ph.D.*
