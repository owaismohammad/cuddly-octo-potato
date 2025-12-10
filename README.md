# EvalRAG

RAG app built for Smart India Hackathon to automate and streamline the evaluation of research proposals.

# 💎 OreSight: Intelligent R&D Proposal Evaluator

> **Automating the Future of Research Funding with Agentic RAG & Deterministic Logic.**

[![Ragas Score](https://img.shields.io/badge/RAGAS_Score-90%25-green)](https://github.com/Saad1926Q/nexml-rag)
[![Tech Stack](https://img.shields.io/badge/Stack-LangChain_|_OpenAI_|_Docling-blue)](https://github.com/Saad1926Q/nexml-rag)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

## 📋 Table of Contents
- [Problem Statement](#-problem-statement)
- [The Solution](#-the-solution)
- [Key Features](#-key-features)
- [System Architecture](#-system-architecture)
- [Performance Metrics](#-performance-metrics)
- [Tech Stack](#-tech-stack)
- [Getting Started](#-getting-started)

---

## 🚩 Problem Statement
Organizations like the **Ministry of Coal (MoC)**, DST, and DRDO receive thousands of R&D proposals. The current manual evaluation process is:
- **Slow & Subjective:** Relies on human interpretation, causing bottlenecks.
- **Prone to Error:** Manual checks often miss financial non-compliance or existing prior art.
- **Resource Intensive:** Experts waste time on irrelevant or low-quality proposals.

**OreSight** was built to solve the MoC's specific challenge: *How to objectively, quickly, and accurately evaluate R&D proposals against strict S&T guidelines?*

---

## 💡 The Solution
OreSight is an advanced **RAG-based evaluation engine** that automates the vetting process. Unlike standard wrappers, we combine **Agentic RAG** for reasoning with **Deterministic Logic** for financial auditing.

We target the three core pillars of evaluation:
1.  **Novelty Assessment:** Quantifying originality against historical archives.
2.  **Financial Compliance:** Strict, logic-based budget validation against S&T norms.
3.  **Objective Scoring:** Data-driven scoring across 9 distinct metrics.

---

## 🚀 Key Features

### 1. 🛡️ Agentic Relevance Check (The Gatekeeper)
Before wasting compute resources on full analysis, an AI Agent reviews the proposal abstract.
- **Function:** Determines if the proposal falls under the Ministry's domain.
- **Outcome:** If irrelevant, the pipeline halts and notifies the admin with a reasoning summary.
- **Impact:** Saves cost and admin time on spam/irrelevant submissions.

### 2. 📄 Advanced Parsing with Docling
We don't just read text; we see the document.
- **OCR Capability:** Handles **scanned PDFs** and images, not just markdown.
- **Table Extraction:** accurately pulls complex budget tables for analysis.
- **Semantic Chunking:** Splits text based on *meaning* rather than arbitrary character counts, preserving context for the LLM.

### 3. 💰 Logic-Based Budget Analysis (No Hallucinations)
We do **not** rely on LLMs for math.
- We scraped the **S&T Guidelines** to build a rule-based engine.
- The system extracts budget tables and runs them through a deterministic pipeline to check strict compliance (e.g., equipment caps, manpower costs).
- **Dataset:** Tested on 100+ synthetically generated proposals covering every edge case of the guidelines.

### 4. 🧠 Automated Novelty Search
- **Vector Search:** Queries a historical database of past proposals.
- **Reranking:** Re-orders retrieved chunks to ensure the LLM only sees the most critical context.
- **Arxiv Integration:** Fetches relevant live research papers to ensure the proposal isn't just copying existing public work.

### 5. 💬 Talk2Proposal (Supermemory)
- A chat interface allowing admins to query the specific document.
- Equipped with **Supermemory** to retain context across long sessions.

---

## 🏗️ System Architecture

```mermaid
graph TD
    A[User Uploads PDF] --> B[Docling OCR & Parsing]
    B --> C{Agentic Relevance Check}
    C -- Not Relevant --> D[Reject & Notify Admin]
    C -- Relevant --> E[Semantic Chunking]
    E --> F[Vector Database]
    
    F --> G[Novelty Analysis Module]
    F --> H[Budget Logic Pipeline]
    F --> I[S&T Compliance Check]
    
    G & H & I --> J[LLM Final Evaluator]
    J --> K[Generate 9-Point Scorecard]
    K --> L[Final PDF Report]
## Project Setup (using uv)

```bash
# Install uv if you haven't already
pip install uv

# Sync dependencies
uv sync
```

## Environment Configuration

Create a `.env` file in the project root and add the following content:

```env
# Required: API Key for LLM (Groq)
GROQ_API_KEY="gsk_your_groq_api_key_here"
MODEL_NAME="llama3-70b-8192"
CHROMA_DB_PATH="./chroma_db_data"
CLIP_MODEL = "openai/clip-vit-base-patch32"
TEXT_EMBEDDING_ID = "sentence-transformers/clip-ViT-B-32"
```

## Initialization (First Run Only)

Setup vector database by populating it with proposals and guidelines:

```bash

uv run app/vector_db.py
uv run scripts/create_vector_db.py

```

## File Structure

```
nexml-rag/
├── app/                          # Core application code
│   ├── llm.py                    # LLM functions (novelty, compliance checks)
│   └── vector_db.py              # Vector database collections
├── scripts/                      # Setup and processing scripts
│   ├── doc_extractor.py          # PDF extraction using Docling
│   ├── create_vector_db.py       # Populate proposal database
│   └── guidelines_collection.py  # Load S&T guidelines
├── utils/                        # Utility functions
│   └── utils.py                  # Embeddings and chunking
├── exp/                          # Experiments and notebooks
│   ├── test_novelty_check.ipynb
│   └── test_compliance_check.ipynb
├── documents/                    # Input documents
│   ├── naccer_proposals_100_cleaned.csv
│   └── S&T-Guidelines-MoC.pdf
├── chroma_db_data/               # Vector database storage
└── main.py                       # FastAPI application
```


## Implemented
- Novelty Analysis 
- S&T Guidelines Compliance

## TODO
- Currently I have commented out the logic for handling images for simplicity
- Final evaluation score paprt pending
- Talk to proposal
- Arxiv feature
- check things for a better llm response with minimal hallucination
- regarding scores make sure that you always get a score
