---
title: "RAG Agents with LLMs"
excerpt: "Develops and evaluates Retrieval-Augmented Generation (RAG) systems powered by large language models (LLMs). <br/><img src='/images/RAG_LLM_2.png'>"
collection: projects
date: 2025-06-18
category: python
description: "Developes and evaluates of Retrieval-Augmented Generation (RAG) systems powered by large language models (LLMs)."
tags:
  - Retrieval-Augmented-Generation
  - Agent
  - FAISS
  - LangChain
  - LangServe
  - asyncio
  - gradio
  - FastAPI
  - Docker
  - ChatNVIDIA
---

[RAG Agents with LLMs (Code Preview)](https://github.com/ranranrunforit/RAG/tree/main/RAG%20Agents%20with%20LLMs)


Project Notes

![image](/images/RAG_LLM_1.png)
![image](/images/RAG_LLM_2.png)
![image](/images/RAG_LLM_3.png)
![image](/images/RAG_LLM_4.png)
![image](/images/RAG_LLM_5.png)
![image](/images/RAG_LLM_6.png)
![image](/images/RAG_LLM_7.png)
![image](/images/RAG_LLM_8.png)
![image](/images/RAG_LLM_9.png)
![image](/images/RAG_LLM_10.png)
![image](/images/RAG_LLM_11.png)
![image](/images/RAG_LLM_12.png)
![image](/images/RAG_LLM_13.png)
![image](/images/RAG_LLM_14.png)


This project developes and evaluates of Retrieval-Augmented Generation (RAG) systems powered by large language models (LLMs). 

Key areas covered include:

* **Conversation State Management:** Maintaining and updating conversation "running state" using a chain approach, including slot-filling and handling schema-conformant outputs.

* **Document Processing:** Working with large documents, covering splitting, processing, generating summaries, managing token context with sliding windows, and processing document chains.

* **Vector Store Integration:** Integrating scalable vector databases for efficient retrieval of document chunks, including embedding generation, document chunking, and vector store querying.

* **Evaluation:** Evaluating RAG systems, guiding users through examining outputs, benchmark evaluations, and considerations for productionalizing retrieval-augmented workflows.

* **Deployment with LangServe:** Deploying the RAG chain via a simple API using LangServe, detailing endpoint setup and basic frontend integration for a production-like chatbot setting.

* **Semantic Guardrailing:** Filtering harmful or irrelevant chatbot responses, including generating synthetic input data, asynchronously embedding queries, training classifiers, and integrating these components into a chatbot event loop to adjust system prompts based on message quality.
