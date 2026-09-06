# AI Research Agent — LangGraph + OpenAI

An autonomous research agent that reasons about a question, decides on its own whether it needs to look something up or calculate something, uses the right tool, and comes back with an answer — built as a **LangGraph state machine** using the ReAct (Reason + Act) pattern.

## About this project

This project goes a step further than a simple chatbot: the agent itself decides its next action at every step. Given a question, it can:

- Look up factual/knowledge questions using **Wikipedia**
- Solve math using a **calculator** tool
- Answer date/time questions using a **datetime** tool
- Or just answer directly if no tool is needed

All of this is modeled as a graph with two nodes — `agent` (the LLM reasoning step) and `tools` (tool execution) — that loop back and forth until the agent is confident it has a final answer.

## Project Structure

```
ai-research-agent/
├── main.py            # entry point — run this to chat with the agent
├── agent.py           # builds the LangGraph state machine (the core logic)
├── tools.py           # tool definitions (wikipedia, calculator, datetime)
├── config.py          # loads API key safely from environment variables
├── requirements.txt   # dependencies
└── .env.example       # template for your API key (copy to .env)
```

## How the Agent Works

1. **User sends a message** → added to graph state
2. **`agent` node** sends the full conversation to GPT-4o-mini
3. The LLM decides: does this need a tool, or can I answer directly?
   - If a tool is needed → graph routes to the **`tools`** node, the tool runs, and the result is sent back to the `agent` node to form a final answer
   - If not → graph routes straight to **END** with the answer
4. Conversation history is preserved across turns using LangGraph's `MemorySaver` checkpointer

## Getting Started

1. Clone/download this project and install dependencies:
   ```
   pip install -r requirements.txt
   ```
2. Set up your API key (never hardcode it in the code):
   ```
   copy .env.example .env
   ```
   Then open `.env` and paste your real OpenAI API key.
3. Run the agent:
   ```
   python main.py
   ```

## Example Interactions

```
You: what is agentic ai
Agent: Agentic AI refers to intelligent agents in artificial intelligence that can
perceive their environment, take autonomous actions to achieve specific goals, and
potentially improve their performance through learning...

You: what is 245 * 12?
Agent: 2940

You: what time is it?
Agent: Sunday, 06 September 2026, 07:42 PM
```

## Why this project matters

This demonstrates core agentic AI engineering skills:
- Designing a **state machine** for reasoning + tool use, not just prompt-response
- Safe **secrets management** (no hardcoded API keys)
- Clean **separation of concerns** (config, tools, agent logic, entry point kept in separate files)
- Persistent **conversation memory** across multi-turn interactions
