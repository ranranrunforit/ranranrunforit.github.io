---
title: "RAG with Langchain NVIDIA NIM"
excerpt: "Retrieval-Augmented Generation (RAG) applications using NVIDIA NIM microservices and LangChain. <br/><img src='/images/RAG_NIM_1.png'>"
collection: projects
date: 2025-06-08
category: python
description: "Retrieval-Augmented Generation (RAG) applications using NVIDIA NIM microservices and LangChain."
tags:
  - Retrieval-Augmented-Generation
  - FAISS
  - LangChain
  - ChatNVIDIA
  - Docker
---

[RAG with Langchain NVIDIA NIM (Code Preview)](https://github.com/ranranrunforit/RAG/tree/main/RAG%20with%20Langchain%20NVIDIA%20NIM)


This project developes Retrieval-Augmented Generation (RAG) applications utilizing NVIDIA NIM microservices and LangChain. The project highlight the use of NVIDIA AI Endpoints for tasks such as document retrieval, embedding generation, and conversational AI, with practical workflows for both cloud-hosted and locally deployed NIM models. 


![image](/images/RAG_NIM_1.png)
![image](/images/RAG_NIM_2.png)
![image](/images/RAG_NIM_3.png)
![image](/images/RAG_NIM_4.png)


Two primary notebooks are included: 

RAG_for_HTML_docs_with_Langchain_NVIDIA_AI_Endpoints.ipynb, which constructs a RAG pipeline for NVIDIA Triton documentation, covering HTML processing, embedding generation with NV-Embed-QA, FAISS storage, and conversational retrieval chains with Mixtral and Llama3

RAG_Langchain_with_Local_NIM.ipynb, demonstrates a RAG pipeline with a locally hosted NIM microservice, including local NIM container setup, cloud-based embedding generation, FAISS document management, and LangChain integration for conversational QA. 
