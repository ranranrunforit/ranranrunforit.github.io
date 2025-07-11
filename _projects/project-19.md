---
title: "Interview Preparations - Agent Cheatsheet 3"
excerpt: "When I have only 1 hour right before the interview...😅 <br/><img src='/images/anthropic_build.png'>"
collection: projects
date: 2025-06-25
category: LLM
description: "Notes from online materials."
tags:
  - Workflow
  - Agents 
  - MCP
  - Anthropic
---


# Building effective agents

Here's the content about AI agents, workflows, and their characteristics, presented in Markdown format:

---

# Understanding AI Agents: Workflows vs. Dynamic Control 

At Anthropic, we categorize all these variations as **agentic systems** but draw a key architectural distinction between **workflows** and **agents**.

* **Workflows** are systems where Large Language Models (LLMs) and tools are orchestrated through predefined code paths.
* **Agents**, on the other hand, are systems where LLMs dynamically direct their own processes and tool usage, maintaining control over how they accomplish tasks.

Below, we'll explore both types of agentic systems in detail.

---

## When (and When Not) to Use Agents

When building applications with LLMs, it's recommended to find the **simplest solution possible** and only increase complexity when necessary. This might mean not building agentic systems at all. Agentic systems often trade **latency and cost for better task performance**, so consider when this tradeoff makes sense.

* When more complexity is warranted, **workflows** offer predictability and consistency for well-defined tasks.
* **Agents** are a better option when flexibility and model-driven decision-making are needed at scale.
* For many applications, **optimizing single LLM calls with retrieval and in-context examples** is usually sufficient.

---

## When and How to Use Frameworks

Many frameworks simplify the implementation of agentic systems:

* **LangGraph** from LangChain
* **Amazon Bedrock's AI Agent framework**
* **Rivet**, a drag-and-drop GUI LLM workflow builder
* **Vellum**, another GUI tool for building and testing complex workflows

These frameworks streamline low-level tasks like calling LLMs, defining tools, and chaining calls. However, they can create extra abstraction layers that obscure prompts and responses, making **debugging harder**. They might also tempt you to add unnecessary complexity.

**Recommendation:** Start by using LLM APIs directly, as many patterns can be implemented with just a few lines of code. If you use a framework, ensure you understand its underlying code; incorrect assumptions are a common source of errors. Refer to Anthropic's **cookbook** for sample implementations.

---

## Building Blocks, Workflows, and Agents: Common Patterns

This section explores common patterns for agentic systems seen in production, progressing from foundational building blocks to more complex autonomous agents.

### Building Block: The Augmented LLM

The basic building block is an LLM enhanced with **augmentations** like **retrieval, tools, and memory**. Current models can actively use these capabilities by generating search queries, selecting tools, and determining what information to retain.

![The_augmented_LLM](/images/The_augmented_LLM.png)
The augmented LLM

**Recommendation:** Focus on tailoring these capabilities to your specific use case and ensuring they provide an easy, well-documented interface for your LLM. Anthropic's **Model Context Protocol** offers one approach for integrating with third-party tools. For the rest of this discussion, assume each LLM call has access to these augmented capabilities.

### Workflow: Prompt Chaining

**Prompt chaining** decomposes a task into a sequence of steps, where each LLM call processes the output of the previous one. Programmatic checks (like a "gate" in a diagram) can be added to intermediate steps to ensure the process stays on track.

![The_prompt_chaining_workflow](/images/The_prompt_chaining_workflow.png)
The prompt chaining workflow

**When to use this workflow:** Ideal for situations where a task can be easily and cleanly decomposed into fixed subtasks. The main goal is to trade latency for higher accuracy by making each LLM call an easier task.

**Examples:**
* Generating marketing copy, then translating it into a different language.
* Writing an outline of a document, checking criteria, then writing the document based on the outline.

### Workflow: Routing

**Routing** classifies an input and directs it to a specialized follow-up task. This allows for separation of concerns and building more specialized prompts. Without routing, optimizing for one input type might hurt performance on others.

![The routing workflow](/images/The_routing_workflow.png)
The routing workflow

**When to use this workflow:** Works well for complex tasks with distinct categories that are better handled separately, and where classification (by an LLM or traditional model) can be accurate.

**Examples:**
* Directing different types of customer service queries (general, refund, tech support) to specific downstream processes, prompts, and tools.
* Routing easy/common questions to smaller models (e.g., Claude 3.5 Haiku) and complex ones to more capable models (e.g., Claude 3.5 Sonnet) to optimize cost and speed.

### Workflow: Parallelization

LLMs can sometimes work simultaneously on a task, with their outputs programmatically aggregated. This **parallelization** workflow has two main variations:

* **Sectioning:** Breaking a task into independent subtasks run in parallel.
* **Voting:** Running the same task multiple times to get diverse outputs.

![The parallelization workflow](/images/The_parallelization_workflow.png)
The parallelization workflow

**When to use this workflow:** Effective when subtasks can be parallelized for speed, or when multiple perspectives or attempts are needed for higher confidence results. For complex tasks with multiple considerations, LLMs often perform better when each consideration is handled by a separate LLM call.

**Examples:**
* **Sectioning:** Implementing guardrails where one model processes queries while another screens for inappropriate content, performing better than a single LLM handling both.
* Automating evaluations for LLM performance, where each LLM call evaluates a different aspect.
* **Voting:** Reviewing code for vulnerabilities with several prompts flagging problems.
* Evaluating content appropriateness, with multiple prompts assessing different aspects or requiring varying vote thresholds.

### Workflow: Orchestrator-Workers

In the **orchestrator-workers** workflow, a central LLM dynamically breaks down tasks, delegates them to worker LLMs, and synthesizes their results.

![The orchestrator-workers workflow](/images/The_orchestrator_workers_workflow.png)
The orchestrator-workers workflow

**When to use this workflow:** Well-suited for complex tasks where the subtasks needed cannot be predicted (e.g., in coding, the number and nature of file changes depend on the task). Unlike parallelization, its key difference is flexibility: subtasks are determined by the orchestrator based on the specific input.

**Examples:**
* Coding products that make complex changes to multiple files.
* Search tasks involving gathering and analyzing information from multiple sources.

### Workflow: Evaluator-Optimizer

In the **evaluator-optimizer** workflow, one LLM call generates a response while another provides evaluation and feedback in a loop.

![The evaluator-optimizer workflow](/images/The_evaluator_optimizer_workflow.png)
The evaluator-optimizer workflow

**When to use this workflow:** Particularly effective when there are clear evaluation criteria and iterative refinement provides measurable value. It's a good fit if LLM responses demonstrably improve with human-like feedback, and if the LLM can provide such feedback. This mirrors a human writer's iterative process.

**Examples:**
* Literary translation where an evaluator LLM can critique nuances missed by the translator LLM.
* Complex search tasks requiring multiple rounds of searching and analysis, where the evaluator decides if further searches are needed.

### Agents

**Agents** are emerging in production as LLMs mature in key capabilities: understanding complex inputs, reasoning and planning, reliable tool usage, and error recovery. Agents begin with a user command or interactive discussion. Once the task is clear, they plan and operate independently, potentially returning to the human for more information or judgment.

During execution, agents must gain "ground truth" from the environment at each step (e.g., tool call results, code execution) to assess progress. They can pause for human feedback at checkpoints or when encountering blockers. Tasks often terminate upon completion, but stopping conditions (like a maximum number of iterations) are common for control.

Agents can handle sophisticated tasks, yet their implementation is often straightforward: typically just LLMs using tools based on environmental feedback in a loop. Therefore, designing clear and thoughtful toolsets and documentation is crucial.

![Autonomous agent](/images/Autonomous_agent.png)
Autonomous agent

**When to use agents:** Use agents for open-ended problems where it's difficult or impossible to predict the required number of steps, and where you can't hardcode a fixed path. The LLM will potentially operate for many turns, so you must trust its decision-making. Agents' autonomy makes them ideal for scaling tasks in trusted environments.

The autonomous nature of agents means higher costs and the potential for compounding errors. Extensive testing in sandboxed environments, along with appropriate guardrails, is recommended.

**Examples:**
* A coding agent to resolve **SWE-bench tasks**, involving edits to many files based on a task description.
* Anthropic's **"computer use" reference implementation**, where Claude uses a computer to accomplish tasks.

![High-level flow of a coding agent](/images/High_level_flow_of_a_coding_agent.png)
High-level flow of a coding agent

---

## Combining and Customizing These Patterns

These building blocks aren't prescriptive; they're common patterns developers can shape and combine to fit different use cases. The key to success is **measuring performance and iterating on implementations**. Only add complexity when it demonstrably improves outcomes.

---

## Summary

Success in the LLM space isn't about building the most sophisticated system, but the **right system** for your needs. Start with simple prompts, optimize with comprehensive evaluation, and add multi-step agentic systems only when simpler solutions fall short.

When implementing agents, Anthropic tries to follow three core principles:

1.  Maintain **simplicity** in your agent's design.
2.  Prioritize **transparency** by explicitly showing the agent’s planning steps.
3.  Carefully craft your agent-computer interface (ACI) through thorough tool **documentation and testing**.

Frameworks can help you get started quickly, but don't hesitate to reduce abstraction layers and build with basic components as you move to production. By following these principles, you can create agents that are powerful, reliable, maintainable, and trusted by their users.

Here's the provided text in Markdown format:

---

## Appendix 1: Agents in Practice

Our work with customers has revealed two particularly promising applications for AI agents that demonstrate the practical value of the patterns discussed above. Both applications illustrate how agents add the most value for tasks that require both conversation and action, have clear success criteria, enable feedback loops, and integrate meaningful human oversight.

### A. Customer Support

Customer support combines familiar chatbot interfaces with enhanced capabilities through tool integration. This is a natural fit for more open-ended agents because:

* Support interactions naturally follow a conversation flow while requiring access to external information and actions;
* Tools can be integrated to pull customer data, order history, and knowledge base articles;
* Actions such as issuing refunds or updating tickets can be handled programmatically; and
* Success can be clearly measured through user-defined resolutions.

Several companies have demonstrated the viability of this approach through usage-based pricing models that charge only for successful resolutions, showing confidence in their agents' effectiveness.

### B. Coding Agents

The software development space has shown remarkable potential for LLM features, with capabilities evolving from code completion to autonomous problem-solving. Agents are particularly effective because:

* Code solutions are verifiable through automated tests;
* Agents can iterate on solutions using test results as feedback;
* The problem space is well-defined and structured; and
* Output quality can be measured objectively.

In our own implementation, agents can now solve real GitHub issues in the **SWE-bench Verified** benchmark based on the pull request description alone. However, whereas automated testing helps verify functionality, human review remains crucial for ensuring solutions align with broader system requirements.

---

## Appendix 2: Prompt Engineering Your Tools

No matter which agentic system you're building, **tools** will likely be an important part of your agent. **Tools** enable Claude to interact with external services and APIs by specifying their exact structure and definition in our API. When Claude responds, it will include a **tool use block** in the API response if it plans to invoke a tool. Tool definitions and specifications should be given just as much prompt engineering attention as your overall prompts. In this brief appendix, we describe how to prompt engineer your tools.

There are often several ways to specify the same action. For instance, you can specify a file edit by writing a diff, or by rewriting the entire file. For structured output, you can return code inside markdown or inside JSON. In software engineering, differences like these are cosmetic and can be converted losslessly from one to the other. However, some formats are much more difficult for an LLM to write than others. Writing a diff requires knowing how many lines are changing in the chunk header before the new code is written. Writing code inside JSON (compared to markdown) requires extra escaping of newlines and quotes.

Our suggestions for deciding on tool formats are the following:

* Give the model enough tokens to "think" before it writes itself into a corner.
* Keep the format close to what the model has seen naturally occurring in text on the internet.
* Make sure there's no formatting "overhead" such as having to keep an accurate count of thousands of lines of code, or string-escaping any code it writes.

One rule of thumb is to think about how much effort goes into human-computer interfaces (HCI), and plan to invest just as much effort in creating good agent-computer interfaces (ACI). Here are some thoughts on how to do so:

* Put yourself in the model's shoes. Is it obvious how to use this tool, based on the description and parameters, or would you need to think carefully about it? If so, then it’s probably also true for the model. A good tool definition often includes **example usage, edge cases, input format requirements, and clear boundaries** from other tools.
* How can you change parameter names or descriptions to make things more obvious? Think of this as writing a great docstring for a junior developer on your team. This is especially important when using many similar tools.
* Test how the model uses your tools: Run many example inputs in our workbench to see what mistakes the model makes, and iterate.
* Poka-yoke your tools. Change the arguments so that it is harder to make mistakes.

While building our agent for SWE-bench, we actually spent more time optimizing our tools than the overall prompt. For example, we found that the model would make mistakes with tools using relative filepaths after the agent had moved out of the root directory. To fix this, we changed the tool to always require absolute filepaths—and we found that the model used this method flawlessly.

---

# Acknowledgements

This overview was created with the help of deep and manual research, drawing inspiration and information from the excellent resource:

[Anthropic: Building effective agents](https://www.anthropic.com/engineering/building-effective-agents)
