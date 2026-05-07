# 🏥 Hospital Intelligence RAG System
[![Streamlit App](https://static.streamlit.io/badges/streamlit_badge_black_white.svg)](https://hospital-intelligence-rag-system.streamlit.app/)
---

**A Retrieval-Augmented Generation (RAG) Pipeline for Healthcare Analytics**

This repository contains a RAG application for querying and analyzing healthcare datasets. It uses a hybrid retrieval strategy and a cross-encoder re-ranking pipeline to provide more accurate answers to natural language queries regarding patient records, physician performance, and billing logistics.

---

## Project Demo
[![Project Demo](https://img.shields.io/badge/Demo-Watch%20Video-red?style=for-the-badge&logo=youtube)](https://youtu.be/hfv24sco_O4)

---

## Architecture & Technical Stack

The system uses a modular RAG pipeline to improve retrieval accuracy and reduce hallucinations.

* **LLM Engine:** `Llama-3.1-8b-Instant` via **Groq Cloud** for fast response generation.
* **Framework:** **LangChain** for managing the conversational workflow.
* **Hybrid Retrieval:**
    * **Dense Retrieval:** `FAISS` with `all-MiniLM-L6-v2` embeddings for semantic search.
    * **Sparse Retrieval:** `BM25Okapi` for keyword-based matching.
* **Re-ranking:** `CrossEncoder (ms-marco-MiniLM-L6-v2)` to rank retrieved documents by relevance before generation.
* **Query Reformulation:** Rewrites a vague follow-up question into standalone queries using conversation history.
---

## RAG Pipeline Flow

```text
User Query
   ↓
Contextual Query Reformulation
   ↓
Hybrid Retrieval
(FAISS Dense Search + BM25 Sparse Search)
   ↓
Cross-Encoder Re-ranking
(ms-marco-MiniLM-L6-v2)
   ↓
Top-k Relevant Context
   ↓
Llama-3.1-8b-Instant (Groq)
   ↓
Grounded AI Response
```

# Project Structure

```
├── hospital_data/        # Relational CSV datasets (Patients, Visits, Reviews, etc.)
├── hospital_index/       # Serialized FAISS Vector Index (index.faiss & index.pkl)
├── app.py                # Main application logic & Streamlit UI
├── requirements.txt      # Dependency manifest
└── README.md             # Project documentation
```

## Key Functionalities

### **1. Hybrid Search & Re-ranking**
The system combines **FAISS** semantic search with **BM25** keyword search to retrieve more relevant information from the dataset. Retrieved results are then passed through a **Cross-Encoder** re-ranking model to improve context quality before generation.

### **2. Contextual Memory**
A query reformulation layer rewrites vague follow-up questions into standalone queries using conversation history. This helps the system maintain context across multi-turn conversations.

### **3. Transparent Data Inspection**
Users can inspect the raw dataframes used by the RAG pipeline, making it easier to verify where retrieved information comes from.

---

## Deployment & Usage

### 1. Prerequisites
* **Python 3.9+**
* **Groq API Key** (Obtained from [Groq Cloud](https://console.groq.com/))

### 2. Installation
**a. Clone the repository:**
```bash
git clone [git clone https://github.com/ruthjoelin100-ui/Hospital-Intelligence-RAG-System.git)
cd Hospital-Intelligence-RAG-System
```

**b.Install Dependencies:**
```bash
pip install -r requirements.txt
```

**c.Environment Secrets:**
Configure your GROQ_API_KEY in Streamlit secrets or as an environment variable:
```bash
# .streamlit/secrets.toml
GROQ_API_KEY = "your_actual_key_here"
```

### 3. Run Application
```bash
streamlit run app.py
```
