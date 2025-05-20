---
title: "RAG Pipeline"
excerpt: "An End-to-End Retrieval Augmented Generation (RAG) System for Answering Questions. <br/><img src='/images/RAG.png'>"
collection: projects
date: 2025-04-08
category: python
description: "A Retrieval Augmented Generation (RAG) system designed to answer questions about Pittsburgh and CMU."
tags:
  - Retrieval-Augmented-Generation
  - Llama-3.1-8B-Instruct
  - Gemini-2.0-flash-thinking
  - SQuAD
  - FAISS
  - CHROMA
  - Langchain
  - Hugging Face
  - sentence-transformers
  - beautifulsoup4
  - pypdf2
  - PyMuPDF
  - pdf2image
  - pytesseract
---


**[RAG System (Live Preview)](https://github.com/ranranrunforit/End-to-End-Retrieval-Augmented-Generation-system)**


## Project Specification

The project focuses on factual question-answering (QA) ([Touvron et al., 2023](https://arxiv.org/abs/2307.09288)) specifically related to Pittsburgh and CMU. The system retrieves relevant documents and generates answers based on those documents. Large language models (LLMs), such as `meta-llama/Llama-3.1-8B-Instruct` and `gemini-2.0-flash-thinking`, are used to enhance the knowledge base with relevant documents, improving the accuracy of answers related to history, culture, trivia, and upcoming events.

## RAG Workflow

The project workflow includes four major components: data acquisition, QA-pair annotation, RAG pipeline implementation, and evaluation analysis.

### Data Acquisition

Relevant URLs were manually collected to construct the knowledge base. For each web page, one layer of sub-links was also collected. The crawled data from the manually collected URLs was used for both the RAG document base and QA pair generation, while the crawled data from sub-links was used exclusively for the RAG document base.

### QA-Pair Annotation

The QA-pair annotation process was automated using a large language model (LLM) and prompt engineering to extract question-answer pairs from each document. The generated QA pairs were manually evaluated using an inter-annotator agreement (IAA) pipeline to ensure quality. These QA pairs were used to evaluate the performance of the QA system with the RAG pipeline.

### RAG Pipeline Implementation

The RAG pipeline was implemented using a few-shot learning approach with an LLM. This involved selecting a base LLM, designing document embedding and retrieval methods, and creating effective prompts for the system.

### Evaluation Analysis

The evaluation process adopted three metrics from the SQuAD paper ([Rajpurkar et al., 2016](https://arxiv.org/abs/1606.05250)): Exact Match, Answer Recall, and F1 score. The questions from the generated QA pairs were fed into the QA system with the RAG pipeline. The generated results were compared with the answers from the QA pairs to calculate the three metrics.

## Key Points

- **Raw Data Preparation:** Compiled a knowledge resource of relevant documents from various sources.
- **Data Annotation:** Annotated data for both testing/analysis and training purposes.
- **RAG System Development:** Developed a retrieval augmented generation system using open-source models and libraries.
- **Result Generation:** Generated answers using the constructed system and validated against reference answers.

## Data Format

- **Input:** A text file `questions.txt` containing one question per line.
- **Output:** A text file `system_output.txt` containing system-generated answers corresponding to the questions.
- **Reference:** A text file `reference_answers.txt` with correct answers for evaluation.

## Data Preparation

### Knowledge Resource Compilation

The knowledge resource included a mix of HTML pages, PDFs, and plain text documents from recommended sources such as:

- Wikipedia pages on Pittsburgh and its history.
- The City of Pittsburgh official webpage.
- Event calendars from Visit Pittsburgh and CMU.
- Information regarding music, culture, and sports in Pittsburgh.

### Data Collection

Created a clean dataset for model development:

- Processed HTML pages using `beautifulsoup4`
- Parsed PDF documents with `pypdf2`, `PyMuPDF`, `pdf2image`, and `pytesseract`

## Data Annotation

Annotated question-answer pairs:

- Curated a diverse set of questions relevant to Pittsburgh and CMU, ensuring high quality and domain relevance.
- Annotated examples manually and explored existing datasets for transfer learning.

### Quality Estimation

Measured inter-annotator agreement (IAA) to assess the quality of annotations, ensuring reliability in the test dataset.

## RAG System Development

Developed the RAG system with the following components:

1. **Document & Query Embedder**
2. **Document Retriever**
3. **Document Reader (Question-Answering System)**

Utilized [langchain&#39;s RAG stack](https://python.langchain.com/docs/use_cases/question_answering/local_retrieval_qa) to construct the system.

### RAG Strategies

Experimented serveral RAG strategies to improve performance:

- **Custom Text Splitters:** Experimented with different types of splitters, including `recursive`, `character`, `token`, and `semantic` splitters.
- **Embedding Models:** Compared multiple embedding models, such as `sentence-transformers/all-MiniLM-L6-v2` and `sentence-transformers/all-mpnet-base-v2`.
- **LLM Models:** Evaluated the performance of four backbone models (`meta-llama/Llama-3.2-3B-Instruct`, `meta-llama/Llama-3.1-8B-Instruct`, `gemini-2.0-flash`, and `gemini-2.0-flash-thinking-exp-01-21`) without the RAG pipeline. 
- **Document Retrieval Methods:** Evaluated `FAISS` and `CHROMA` retrievers with different algorithms, such as similarity search and MMR.
- **Reranking:** Implemented reranking using models like `ms-marco-MiniLM-L-12-v2` and `ms-marco-MultiBERT-L-12`.
- **Hypothetical Document Embeddings (HyDE):** Used hypothetical document embeddings ([Gao et al., 2022](https://arxiv.org/abs/2212.10496)) to improve retrieval quality.

## Result Generation

Executed the system on an unseen test set to evaluate its performance. 

### Evaluation Metrics

Results were evaluated based on standard metrics including answer recall, exact match, and F1 score, following the methodology outlined in section 6.1 of the [original SQuAD paper](https://arxiv.org/abs/1606.05250) .

## Usage Instructions

### Hardware Requirements
The project required a machine with:

- GPU memory >= 20GB
- CUDA support
- At least 50GB available disk space

### API Keys

Set up the API keys for [LangChain](https://www.langchain.com), [Hugging Face](https://huggingface.co/models), and [Google AI Studio](https://aistudio.google.com), to access Llama-3.1 and Gemini.

## References

+ Lewis et al., 2021. [Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks](https://arxiv.org/abs/2005.11401).
+ Touvron et al., 2023. [Llama 2: Open Foundation and Fine-Tuned Chat Models](https://arxiv.org/abs/2307.09288).
+ Vu et al., 2023. [FreshLLMs: Refreshing Large Language Models with Search Engine Augmentation](https://arxiv.org/abs/2310.03214).
+ Gao et al., 2022. [Precise zero-shot dense retrieval without relevance labels](https://arxiv.org/abs/2212.10496).
+ Rajpurkar et al., 2016. [SQuAD: 100,000+ Questions for Machine Comprehension of Text](https://arxiv.org/abs/1606.05250).


