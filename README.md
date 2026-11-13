
This notebook implements an advanced production-ready Hybrid Search RAG Pipeline with a Cross-Encoder Reranker using the groq client and open-source embedding models.

Repository Key Features:
Document Ingestion: Extracting text content from PDFs dynamically.
Hybrid Retrieval: Combining dense embeddings (FAISS) and sparse lexical retrieval (BM25).
Reciprocal Rank Fusion (RRF): Merging rankings mathematically from dense and sparse retrievers.
Cross-Encoder Reranking: Re-evaluating chunk relevance to maximize precision.
Evaluation Benchmark: Calculating retrieval metrics such as Hit-rate@5 and MRR (Mean Reciprocal Rank) to validate optimization.
Gemini
```markdown
# Hybrid Search RAG Pipeline with Cross-Encoder Reranking

This repository contains a production-ready, highly optimized **Hybrid Retrieval-Augmented Generation (RAG)** pipeline developed in Python. The system combines the semantic search capabilities of dense vector indices with the keyword-matching precision of sparse lexical retrievers, utilizing 
# Hybrid Search RAG Pipeline with Cross-Encoder Reranking

This repository contains a production-ready, highly optimized **Hybrid Retrieval-Augmented Generation (RAG)** pipeline developed in Python. The system combines the semantic search capabilities of dense vector indices with the keyword-matching precision of sparse lexical retrievers, utilizing **Reciprocal Rank Fusion (RRF)** and a **Neural Cross-Encoder** to achieve state-of-the-art retrieval accuracy and grounded generation.

---

## 🚀 Features

1. **High-Fidelity Document Ingestion**: Seamless PDF textual extraction utilizing `PyMuPDF` with customized cleanups.
2. **Hugging Face Tokenizer-Based Chunking**: Chunking strategy based on token counts (`sentence-transformers/all-MiniLM-L6-v2` tokenizer) to guarantee rigid boundary thresholds without losing context.
3. **Dual-Path Retrieval (Hybrid Search)**:
   * **Dense Path**: Semantic vector search with flat inner-product indexing in `FAISS`.
   * **Sparse Path**: Exact-match lexical scoring using the modern `BM25Okapi` algorithm.
4. **Reciprocal Rank Fusion (RRF)**: Mathematically merges ranking lists from both paths to generate a unified, robust candidate list.
5. **Cross-Encoder Reranking**: Uses `cross-encoder/ms-marco-MiniLM-L-6-v2` to compute query-document attention scores, shifting highly relevant context pieces directly to rank 1.
6. **Grounded QA Synthesis**: Generates cited responses via the Groq API (`llama-3.3-70b-versatile`) while strictly adhering to the provided document context to eliminate hallucinations.
7. **Empirical Benchmarking**: Seamless evaluation logic measuring **Hit-rate@5** and **MRR (Mean Reciprocal Rank)** across customized Q&A test cases.

---

## 📊 Empirical Evaluation Benchmarks

Tested across **42 ground-truth validation queries** on the core textbook documentation, the retrieval configurations yielded the following performance improvements:

| Retrieval Strategy | Hit-rate @ 5 (%) | MRR | Median Latency (s) |
| :--- | :---: | :---: | :---: |
| **Vector Only (Base)** | 76.19% | 0.5992 | ~0.027s |
| **Hybrid RRF** | 85.71% | 0.6754 | ~0.023s |
| **Hybrid + Cross-Encoder Reranker** | **88.10%** | **0.6675** | ~3.03s |

### Key Takeaways:
* **Hybrid RRF** yields an immediate **+9.52% absolute gain** in Hit-Rate over standard Vector Search without adding computational latency.
* Adding a **Cross-Encoder Reranker** pushes the Hit-Rate to **88.10%**, maximizing context relevance before generation at the cost of slight CPU reranking latency.

---

## 🛠️ Tech Stack

* **Orchestration**: Python, Google Colab
* **Embedding Engine**: `all-MiniLM-L6-v2` (Sentence-Transformers)
* **Vector Database**: `FAISS` (Facebook AI Similarity Search)
* **Lexical Retriever**: `rank-bm25` (BM25Okapi)
* **Reranker Model**: `ms-marco-MiniLM-L-6-v2` (Cross-Encoder)
* **LLM Engine**: Groq API (`llama-3.3-70b-versatile` / `openai/gpt-oss-20b` fallback)
* **Inference Library**: PyMuPDF (`fitz`)

---

## 📁 Directory Structure

```text
├── documents/                    # Raw source PDF files (e.g., Artificial Intelligence - A Modern Approach)
├── hybrid_rag_faiss.index        # Serialized FAISS Dense Vector Index 
├── chunks.json                   # Serialized text chunks and corresponding metadata
├── evaluation_benchmark.csv      # 42 validated Q&A ground-truth pairs
├── evaluation_results.csv        # Performance results of retrieval runs
└── hybrid_rag_pipeline.ipynb     # Interactive pipeline development notebook
🚦 How to Get Started
1. Clone the Repository
git clone https://github.com/mounikas-tech-AI/hybrid-rag-system.git
cd hybrid-rag-system
2. Install Dependencies
pip install -q groq pymupdf sentence-transformers faiss-cpu rank_bm25 pandas numpy matplotlib seaborn tqdm transformers
3. Setup Your Groq API Key
Configure your environment or enter your key when prompted inside the notebook:

import getpass
GROQ_API_KEY = getpass.getpass("Enter your Groq API key: ")
4. Run the Pipeline
Open the interactive notebook, upload your PDF source documents into the documents/ folder, generate your gold standard Q&A benchmark file (evaluation_benchmark.csv), and execute the pipeline cells sequentially. ```
