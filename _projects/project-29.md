---
title: "Interview Preparations - Agent and MCP Cheatsheet 2"
excerpt: "When I have only 1 day before the interview...😅 <br/><img src='/images/Agent_Course_36.png'>"
collection: projects
date: 2025-07-15
category: LLM
description: "Notes from online materials."
tags:
  - LangChain
  - CrewAI
  - AutoGen
  - OpenAI
  - LangGraph
---


# AutoGen

![image](/images/Agent_Course_36.png)
![image](/images/Agent_Course_37.png)
![image](/images/Agent_Course_38.png)
![image](/images/Agent_Course_39.png)
![image](/images/Agent_Course_40.png)
![image](/images/Agent_Course_41.png)
![image](/images/Agent_Course_42.png)
![image](/images/Agent_Course_43.png)
![image](/images/Agent_Course_44.png)

## AutoGen Core

Something a little different.

This is agnostic to the underlying Agent framework

You can use AutoGen AgentChat, or you can use something else; it's an Agent interaction framework.

From that point of view, it's positioned similarly to LangGraph.

### The fundamental principle

Autogen Core decouples an agent's logic from how messages are delivered.  
The framework provides a communication infrastructure, along with agent lifecycle, and the agents are responsible for their own work.

The communication infrastructure is called a Runtime.

There are 2 types: **Standalone** and **Distributed**.





# Acknowledgements

This overview was created with the help of deep and manual research, drawing inspiration and information from the excellent resource:

[The Complete Agentic AI Engineering Course (2025)](https://www.udemy.com/course/the-complete-agentic-ai-engineering-course/)
