---
title: "Interview Preparations - Agent Cheatsheet 2"
excerpt: "When I have only 1 hour right before the interview...😅 <br/><img src='/images/LLM.png'>"
collection: projects
date: 2025-06-24
category: LLM
description: "Notes from online materials."
tags: 
  - Multi-Agent
---


# Single vs. Multi-Agent System: Choosing the Right Tool 

 The choice between a single-agent and a multi-agent system isn't about ideology; it's about **picking the right tool for the right job**.

Before diving deeper, let's define an **AI agent**: A system that uses a Large Language Model (LLM) as its reasoning engine to decide the control flow of an application.

---

## What Are Single Agents?

A **single-agent system** operates as a "single process." Imagine it as one highly focused worker tackling a task from start to finish. It maintains a continuous thread of thought (memory) and action (tools) to ensure that every step is informed by everything that came before it.

![single_agent](/images/cognition.png)

### Key Characteristics:
* **Sequential**: Actions are performed one after another. The agent completes step A before moving to step B.
* **Unified Context**: Maintains a single, continuous history of the entire conversation. Every new step has access to all previous steps, thoughts, and tool outputs.
* **Stateful**: Decisions made early on directly and fully inform later actions without the need for message passing.

### Pros:
* **Context Continuity**: No information is lost between steps.
* **Simplicity**: Easier to debug, test, and maintain.
* **Transparency**: Clear execution path and decision trail.

### Cons:
* **Sequential Bottlenecks**: Slow for tasks where parts could be handled in parallel.
* **Context Window Limitations**: Eventually exceed the (useful) context window, leading to errors and forgotten details.
* **Inefficiency**: May waste tokens on repetitive context and are limited to single model capabilities/instructions.

---

## What Are Multi-Agent Systems?

A **multi-agent system** is structured like a team. It typically involves a "lead agent" that breaks down a goal into smaller subtasks, which it then delegates to multiple "worker" agents that can operate in parallel.

![multi_agent](/images/multi-agent.png)

### Key Characteristics:
* **Parallel Execution**: Subtasks can be handled simultaneously by multiple agents.
* **Delegation**: A "lead" agent typically decomposes the main goal, delegates subtasks, and synthesizes the results from the worker agents.
* **Distributed Context**: Each agent operates with its own context, which is often a subset of the total information.

### Pros:
* **Parallelization**: Can explore multiple paths simultaneously, reducing latency.
* **Specialization**: Each agent can be optimized and instructed for specific tasks.
* **Breadth**: Can solve complex, multi-faceted problems.

### Cons:
* **Context**: Sharing the right context between agents is difficult.
* **Coordination**: Agents may duplicate work or make conflicting decisions.
* **Cost**: Can be more token-intensive (Anthropic reported 15x more tokens compared to a standard chat interaction).

**Note:** It's worth mentioning other multi-agent patterns, e.g., **swarms** are not using a "lead" agent. Instead, they collaborate in a more peer-to-peer fashion to solve a problem, which has both single-agent and multi-agent characteristics (e.g., Unified Context, but individual instructions) but comes with its own challenges.

---

## Comparison: Single Agents vs. Multi-Agent Systems

| Aspect                 | Single Agent System                         | Multi-Agent System                                 |
| :--------------------- | :------------------------------------------ | :------------------------------------------------- |
| **Context Management** | Continuous, no loss                         | Complex sharing required                           |
| **Execution Speed** | Sequential                                  | Parallel                                           |
| **Token Usage** | 4x chat tokens                              | 15x chat tokens                                    |
| **Reliability** | High, predictable                           | Lower, emergent behaviors                          |
| **Debugging** | Straightforward                             | Complex, non-deterministic                         |
| **Best For** | Sequential, state-dependent tasks ("write" tasks). | Parallelizable, exploratory tasks ("read" tasks).  |
| **Coordination** | None needed                                 | Critical success factor                            |
| **Example Use Case** | Refactoring a codebase, writing a detailed document. | Researching a broad market trend, identifying all board members of the S&P 500. |
| **Core Strength** | Context Continuity & Reliability            | Parallelism & Scalability                          |
| **Primary Challenge** | Context Window Limits & Sequential Speed    | Context Fragmentation & Coordination Complexity    |

---

## The Universal Truths of Building Agents

Despite their differences, common principles apply for building serious Agentic systems:

* **Context Engineering is everything**: Architecting systems that dynamically maintain the right information at the right time for reliable decision-making is the key to success and reliability. This isn't just prompt engineering.
* **"Read" vs. "Write" Agents**: The important distinction isn't single vs. multi-agent. It's whether your task primarily involves **reading** (research, analysis, information gathering) or **writing** (code generation, content creation, file editing).
    * **Read tasks** are easier to parallelize and suit multi-agent systems better.
    * **Write tasks** create coordination problems when parallelized, favoring single agents.
    * **Mixed tasks** should separate read and write phases architecturally.
* **Reliable Agents Need Different Tooling**: Building reliable agents requires more than a good model. It requires a robust infrastructure for **durable execution** to survive failures, **observability** to debug behavior, and **evaluation** to measure what actually matters.
* **Economic Viability and Model Improvements**: Models themselves are improving at an incredible rate. Don't over-engineer a solution for today that a much simpler one can solve for you tomorrow.

---



# Acknowledgements

This overview was created with the help of deep and manual research, drawing inspiration and information from the excellent resource:

[Philschmid blog](https://www.philschmid.de/single-vs-multi-agents)
