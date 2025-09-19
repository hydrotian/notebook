---
layout: post
title: Building a Local RAG System for My Research
date: 2025-09-18 17:45:00 # Adjust date/time as needed
author: Tian # Or your name
comments: true
categories: [Programming, Unix, AI] # Example categories
---

I've been working on a practical application of local language models: building a Retrieval-Augmented Generation (RAG) system to interact with my personal collection of hydrology research papers. The goal is to create a tool that can accurately answer questions based on a curated set of documents, moving beyond the general knowledge of a standard LLM. This document outlines the process, the tools I used, and my understanding of how it all works.

## The Architecture: The Open-Source Stack

Running everything locally ensures privacy and control. Here are the components I used for this project:

* **Orchestration:** **LangChain** is the framework I used to connect all the different components of the pipeline, from data loading to interacting with the LLM. A popular alternative is LlamaIndex, which is also excellent for RAG-focused projects.
* **Embedding Model:** For converting text to vectors, I used **Nomic's `embed-text-v1.5`**. It's a strong open-source model. Other great options are available on Hugging Face, like `BGE-large-en-v1.5` or the lightweight `all-MiniLM-L6-v2`.
* **Vector Database:** I chose **ChromaDB** for storing the vectors and text chunks because it's easy to set up and run locally. For larger-scale projects, one might look into FAISS for raw speed or Qdrant for more advanced filtering capabilities.
* **The LLM:** The core of the system is **`meta-llama-3-8b-instruct`**, which I run locally using **LM Studio**. LM Studio provides a simple UI and a local server. Ollama is another excellent tool for managing and running local models via the command line.

## Phase 1: Building the Knowledge Base

This is the data preparation phase. The goal is to take a folder of PDFs on a topic, like groundwater modeling, and make it searchable and understandable for the system.

1.  **Document Loading and Chunking:** The process begins by loading the documents. Using LangChain, I can load PDFs and then apply a text splitter. This "chunking" process breaks down the long documents into smaller, overlapping paragraphs. This is a critical step because an LLM has a limited context window, and searching for smaller, specific chunks yields more accurate results than searching a whole document.

2.  **Embedding:** Each text chunk is then passed to the Nomic embedding model. The model converts the chunk into a numerical vector, which represents its semantic meaning. This vector is essentially a coordinate on a high-dimensional "map of concepts." One chunk = one vector.

3.  **Storage and Indexing:** Finally, each vector is stored in the ChromaDB database. The database stores not just the vector, but also the original text chunk it represents and any useful metadata, like the source filename and page number. This completes the index, creating a searchable library for the papers.

## Phase 2: The Query Workflow

Once the knowledge base is indexed, I can begin asking questions. Let's use a sample query: "What are the common assumptions for applying the MODFLOW model in arid regions?"

1.  **Embed the Query:** The user's question is passed through the *same* Nomic embedding model to create a query vector. Using the same model for indexing and querying is essential for the results to be meaningful.

2.  **Retrieve Relevant Chunks:** This query vector is used to perform a similarity search in ChromaDB. The database finds the vectors in its index that are closest to the query vector and returns their corresponding text chunks. These are the pieces of text from my original documents that are most semantically related to my question.

3.  **Augment the Prompt:** The system then constructs a detailed prompt for the LLM. It combines the original question with the retrieved text chunks as context. The structure looks something like this:

    ```
    Based on the following context, please answer the user's question.

    Context:
    [Text chunk 1 from a retrieved document with DOI...]
    [Text chunk 2 from another document with DOI...]
    [Text chunk 3...]

    User Question: What are the common assumptions for applying the MODFLOW model in arid regions?
    ```

4.  **Generate the Answer:** This final, text-only prompt is sent to the Llama 3 model running in LM Studio. The LLM reads the provided context and synthesizes an answer based only on that information.

## How This Approach Mitigates Hallucination

A recently [published paper from OpenAI](https://openai.com/index/why-language-models-hallucinate/) explained the reason why LLMs hallucinate, which is due to the fact it's more rewarding by making something up than saying "I don't know". Then why RAG system could mitigate hallucination with the same LLM? Because this isn't about the model itself, but about the process. The core benefit of RAG is that it **grounds** the LLM.

Without RAG, an LLM answers questions from its vast, internal knowledge, which is a blend of all the data it was trained on. It's an open-ended recall task, and if it doesn't know the answer, its programming to be helpful can cause it to "hallucinate" by generating plausible but incorrect information.

RAG changes the task fundamentally. It's no longer a test of the LLM's memory. Instead, it becomes a task of reading comprehension and synthesis. By providing a small, specific, and relevant context with the prompt, we instruct the model to base its answer on the provided facts. If the information isn't in the retrieved chunks, a well-implemented RAG system is far more likely to state that the answer isn't available in the documents, rather than making something up. This provides a verifiable and more trustworthy way to interact with my specific knowledge base.
