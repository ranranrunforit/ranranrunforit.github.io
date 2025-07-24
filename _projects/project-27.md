---
title: "Deep Research"
excerpt: "An application that automates the process of conducting in-depth web research, generating detailed reports, and emailing the final output<br/><img src='/images/Agent_Course_6.png'>"
collection: projects
date: 2025-07-05
category: LLM
description: "A Research Agent workflow using the OpenAI Agents SDK"
tags:
  - OpenAI Agent SDK
  - SendGrid
  - Asyncio
  - Workflow
  - Agent
  - WebSearchTool
  - Runner
  - trace
  - function_tool
---

Project Notes 

![image](/images/Agent_Course_6.png)
![image](/images/Agent_Course_9.png)

**[Deep Research (Code Preview)](https://github.com/ranranrunforit/Agents/tree/main/deep_research)**

This project demonstrates a Research Agent workflow using the OpenAI Agents SDK. The agent automates the process of planning, executing, and synthesizing web research, then delivers the results via email.

- Uses a `PlannerAgent` (based on the `Agent` class) to generate relevant search queries for a given research topic.  
- Employs the `WebSearchTool` to perform web searches for each planned query.
- A `WriterAgent` creates a detailed markdown report from the search results.
- Integrates SendGrid via the `send_email` function (decorated with `@function_tool`) to send the final report as a formatted HTML email.

**Libraries:**
- `OpenAI Agents SDK`: `Agent`, `WebSearchTool`, `Runner`, `trace`, `function_tool`
- `SendGrid`: For sending emails (`sendgrid.SendGridAPIClient`, `Mail`, `Email`, `To`, `Content`)
- `Asyncio`: For asynchronous execution of search and report tasks

