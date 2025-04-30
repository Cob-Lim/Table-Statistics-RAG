# PDF-to-QA Chatbot using RAG (LangChain + Guardrails + RAGAS)

This project builds a **Question Answering (QA) chatbot** from a PDF document that contains tables and text. It uses a **Retrieval-Augmented Generation (RAG)** pipeline powered by **LangChain**, **Guardrails**, and evaluated using **RAGAS**. The core components are OpenAI’s `gpt-4o-mini` for generation and `text-embedding-3-small` for embeddings.

---

## Pipeline Overview

### 1. PDF Preprocessing
- Extracted structured and unstructured content from PDF (including **tables** and **paragraphs**).
- Used `camelot` to retain table structure as **flattened Markdown or CSV-style text** for improved chunking and parsing.
- The dataset originated from a worksheet I encountered while learning Chinese in Taiwan. It contained both tabular and textual information relevant to multiple countries, which made it an ideal candidate for QA transformation.


### 2. Chunking Strategy
- After extracting text and tables from the PDF, we **split the tables by row**, especially for datasets involving multiple countries.
- This row-wise splitting preserves the **semantic granularity** of each country's information and improves retrieval accuracy.

### 3. Embedding
- Generated vector embeddings using OpenAI's **`text-embedding-3-small`** model.
- Stored embeddings in a vector database (Pinecone) for fast similarity-based retrieval.

### 4. Retrieval-Augmented Generation (RAG)
- Built a **LangChain RAG pipeline**:
  - Query is embedded and used to retrieve top-k relevant chunks.
  - Retrieved chunks are passed to **OpenAI’s `gpt-4o-mini`** for answer generation.
  - Guardrails were added for **output validation**.

### 5. Evaluation with RAGAS
- Evaluated the chatbot using **RAGAS** metrics:
  - **Faithfulness**
  - **Answer Relevance**
  - **Context Precision**
  - **Context Recall**
- Ground truth and generated answers were compared to identify hallucinations or incomplete retrievals.

---

## Tech Stack

| Component       | Tool/Service               |
|----------------|----------------------------|
| PDF Extraction  | `camelot`                  |
| LLM             | `gpt-4o-mini`              |
| Embeddings      | `text-embedding-3-small`   |
| RAG Framework   | LangChain                  |
| Vector Store    | Pinecone                   |
| Evaluation      | RAGAS                      |
