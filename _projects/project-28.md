---
title: "Sidekick Personal Coworker"
excerpt: "An AI-powered assistant designed to help users complete tasks interactively using Gradio Chat <br/><img src='/images/Agent_Course_29.png'>"
collection: projects
date: 2025-07-10
category: LLM
description: "An agentic assistant using LangGraph and LangChain."
tags:
  - LangChain
  - LangGraph
  - Gradio
  - Playwright
  - lxml
  - nest_asyncio
  - beautifulsoup4
  - uuid
  - asyncio
---


Project Notes about LangChain

![image](/images/Agent_Course_26.png)
![image](/images/Agent_Course_27.png)
![image](/images/Agent_Course_28.png)
![image](/images/Agent_Course_29.png)
![image](/images/Agent_Course_30.png)
![image](/images/Agent_Course_31.png)
![image](/images/Agent_Course_32.png)
![image](/images/Agent_Course_33.png)
![image](/images/Agent_Course_34.png)
![image](/images/Agent_Course_35.png)

**[Sidekick Personal Coworker](./sidekick_personal_coworker)**

These files implement an agentic assistant using LangGraph and LangChain. The assistant can autonomously complete tasks, use external tools (like web browsing and Python code execution), and iteratively improve its work based on evaluator feedback and user-defined success criteria.

- **`sidekick.py`** defines the agent's workflow, state management, and evaluation logic. It builds a stateful graph with nodes for working, tool usage, and evaluation, allowing the assistant to loop until the task is complete or more user input is needed.
- **`sidekick_tools.py`** provides tool definitions (such as Playwright-based web browsing and other custom tools) that the agent can invoke during its workflow.

**Libraries:**
- `langgraph`: For building stateful agent graphs (`StateGraph`, `ToolNode`, etc.).
- `langchain_openai`: For LLM access (`ChatOpenAI`) and tool binding.
- `langchain_core.messages`: For structured message handling (`SystemMessage`, `HumanMessage`, `AIMessage`).
- `playwright`: web searching tool definitions for agent actions.
- `asyncio`, `uuid`: For async operations, unique IDs.

