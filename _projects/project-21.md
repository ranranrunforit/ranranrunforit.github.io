---
title: "Interview Preparations - Agent Cheatsheet 5"
excerpt: "When I have only 1 hour right before the interview...😅 This is about AI Agent Frameworks<br/><img src='/images/AI_agent_framework.jpg'>"
collection: projects
date: 2025-06-27
category: LLM
description: "Notes from online materials."
tags:
  - LangChain
  - CrewAI
  - AutoGen
  - MetaGPT
  - LangGraph
  - AgentOps
  - Superagent
  - Haystack Agents
---


AI agent tools have exploded and most people are still guessing what to use when. 

If you're building anything beyond a demo, the tool you choose matters early.

Here’s a clear breakdown, depending on what you’re building.

- **If you need to chain tools and memory across steps → LangChain**

Still the most flexible for logic-first workflows.
But the learning curve is real if you’re not used to designing agent state.

- **If you’re building teams of agents that pass tasks → CrewAI**

Roles, memory and handoff logic baked in.
It works when you need structure, not just clever prompts.

- **If your agent needs to ask, refine, repeat → AutoGen**

Great for tasks that evolve through feedback.
The agent learns more by asking than assuming.

- **If you want agents to behave like real dev teams → MetaGPT**

You define a spec, and the PM, Dev, QA agents execute.
Useful when you need product thinking, not just code output.

- **If your task needs retries, state or conditional flows → LangGraph**

This saved us time debugging long workflows.
Visual graphs help you spot logic loops fast.

- **If your agent is in production and failing silently → AgentOps**

Dashboards, logs, alerts.
 
- **If you’re prototyping fast with your own APIs → Superagent**

Open-source, vector DB, memory-ready.
Easy to fork and test something new in under an hour.
 
- **If you’re doing RAG with large docs or search flows → Haystack Agents**

Dev-centric, tuned for retrieval pipelines.
It’s modular, and it’s built to go deep into documents, not wide.

![image](/images/AI_agent_framework.jpg)


# Acknowledgements

This overview was created with the help of deep and manual research, drawing inspiration and information from the excellent resource:

[Nina Duran tweet](https://x.com/HeyNina101)
