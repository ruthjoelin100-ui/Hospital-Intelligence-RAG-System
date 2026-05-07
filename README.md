# 🏥 Hospital Intelligence RAG System
[![Streamlit App](https://static.streamlit.io/badges/streamlit_badge_black_white.svg)](https://hospital-intelligence-rag-system.streamlit.app/)
---

**A Retrieval-Augmented Generation (RAG) Pipeline for Healthcare Analytics**

This repository contains a RAG application for querying and analyzing healthcare datasets  It uses a hybrid retrieval strategy and a cross-encoder re-ranking pipeline to provide more accurate answers to natural language queries regarding patient records, physician performance, and billing logistics.

---

## Project Demo
[![Project Demo](https://img.shields.io/badge/Demo-Watch%20Video-red?style=for-the-badge&logo=youtube)](https://youtu.be/hfv24sco_O4)

---

## Architecture & Technical Stack

The system implements a **Modular RAG Architecture** to ensure data grounding and reduce hallucinations:

* **LLM Engine:** `Llama-3.1-8b-Instant` via **Groq Cloud** for sub-second inference latency.
* **Orchestration:** **LangChain** for stateful conversation management.
* **Hybrid Retrieval:**
    * **Dense Retrieval:** `FAISS` using `all-MiniLM-L6-v2` embeddings for semantic context.
    * **Sparse Retrieval:** `BM25Okapi` for keyword-based exact matching.
* **Re-ranking Layer:** `CrossEncoder (ms-marco-MiniLM-L6-v2)` to surface the most relevant context.
* **Query Transformation:** Integrated **Contextual Query Reformulation** to handle multi-turn conversations and anaphora resolution (e.g., "his", "her").
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
By combining **FAISS** with **BM25** and passing results through a **Cross-Encoder**, the system ensures that qualitative data (like patient reviews) and quantitative data (like billing amounts) are both retrieved with high fidelity.

### **2. Contextual Memory**
Utilizes a "Query Rewriter" pattern. The system analyzes chat history to transform vague follow-up questions into standalone search queries, ensuring the retrieval engine remains focused on the correct subject throughout a session.

### **3. Transparent Data Inspection**
Features a dedicated **Data Inspection Layer**, allowing users to view the raw dataframes utilized by the RAG engine to verify groundedness.

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
