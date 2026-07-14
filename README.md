# 🧠 PCOS × Neurodivergence — Advanced RAG Assistant (Pinecone + Hybrid Search)

> A Retrieval-Augmented Generation (RAG) pipeline grounded in real clinical research papers.
> Upgrades the baseline FAISS pipeline with Pinecone, BM25 hybrid search, RRF fusion, CrossEncoder reranking, and LLM query expansion.

---

## 📌 What This Does

This notebook builds a **medical question-answering system** that:
- Ingests 5 peer-reviewed research papers on PCOS and neurodivergence
- Lets you ask natural language questions
- Returns answers **cited directly from the papers** (no hallucination)
- Refuses to answer if the papers don't contain relevant information
- Retrieves using **dense (Pinecone) + sparse (BM25) hybrid search**, fused via RRF, reranked with a CrossEncoder — benchmarked against the original FAISS-only baseline

---

## 🗂️ Research Papers Used

> ⚠️ **PDFs are not included in this repo** — they are copyrighted academic papers.
> Download them yourself using the links below and place them in the `papers/` folder.

| Filename | Paper | Access |
|----------|-------|--------|
| `pmos.pdf` | Cherskov et al. (2018) — *PCOS and Autism: A test of the prenatal sex steroid theory.* Translational Psychiatry. DOI: [10.1038/s41398-018-0186-7](https://doi.org/10.1038/s41398-018-0186-7) | 🔓 Open Access |
| `pmos1.pdf` | Redkar & Khan (2025) — *The impact of PCOS on attention: an empirical investigation.* BioPsychoSocial Medicine. DOI: [10.1186/s13030-024-00320-w](https://doi.org/10.1186/s13030-024-00320-w) | 🔓 Open Access |
| `pmos2.pdf` | Berni et al. (2018) — *PCOS is associated with adverse mental health and neurodevelopmental outcomes.* J Clin Endocrinol Metab. DOI: [10.1210/jc.2017-02667](https://doi.org/10.1210/jc.2017-02667) | 🔒 Paywall |
| `pmos3.pdf` | Dubey et al. (2021) — *Systematic review and meta-analysis: maternal PCOS and neuropsychiatric disorders in children.* Translational Psychiatry. DOI: [10.1038/s41398-021-01699-8](https://doi.org/10.1038/s41398-021-01699-8) | 🔓 Open Access |
| `pmos4.pdf` | Chen et al. (2020) — *PCOS or anovulatory infertility and offspring psychiatric disorders: a Finnish population-based cohort study.* Human Reproduction. DOI: [10.1093/humrep/deaa192](https://doi.org/10.1093/humrep/deaa192) | 🔓 Open Access |

> 💡 **Tip:** Papers marked 🔓 Open Access are freely downloadable from the DOI link directly.
> For 🔒 paywalled papers, try [Unpaywall](https://unpaywall.org) or access via your institution.

---

## 🛠️ Tech Stack

| Component | Tool |
|-----------|------|
| PDF Extraction | PyMuPDF (fitz) |
| Chunking | LangChain RecursiveCharacterTextSplitter |
| Embeddings | HuggingFace `all-MiniLM-L6-v2` |
| Dense Vector Store | **Pinecone** (serverless, cosine) |
| Sparse Retrieval | **BM25** (`rank-bm25`) |
| Fusion | **Reciprocal Rank Fusion (RRF)** |
| Reranker | **CrossEncoder** `ms-marco-MiniLM-L-6-v2` |
| Query Expansion | Groq LLaMA 3.3 70B (rephrases query before retrieval) |
| LLM | Groq — LLaMA 3.3 70B Versatile |
| Hallucination Guard | Custom system prompt refusing out-of-context answers |
| Citations | Author name + page number on every answer |
| Interactive UI | ipywidgets (no blocking input loop) |

---

### 4. Run All Cells in Order

Step 0  → Install dependencies
Step 1  → Load API keys (Pinecone + Groq)
Step 2  → Create/connect Pinecone index
Step 3  → Upload PDFs
Step 4  → Extract & chunk text
Step 5  → Embed chunks
Step 6  → Build FAISS baseline pipeline
Step 7  → Upsert vectors into Pinecone
Step 8  → Build BM25 sparse index
Step 9  → Pinecone dense search
Step 10 → Reciprocal Rank Fusion
Step 11 → CrossEncoder reranker
Step 12 → LLM query expansion
Step 13 → Full advanced pipeline
Step 14 → Sanity check (one question, both pipelines)
Step 15 → A/B test: 10 questions, both pipelines
Step 16 → Generate before/after comparison table
Step 17 → Interactive Q&A widget

---

## 💬 Example Questions & Answers

ask("Are children of mothers with PCOS at higher risk of autism?")
# → Yes — studies found 35-60% increased odds [Cherskov 2018, Dubey 2021]

ask("How do elevated androgens in PCOS affect brain development?")
# → Via neuronal spine, synaptic plasticity, brain lateralization pathways

ask("What mental health disorders are most commonly associated with PCOS?")
# → Depression, anxiety, bipolar disorder, eating disorders [Berni 2018]

ask("Does insulin resistance in PCOS contribute to cognitive impairment?")
# → Yes — impairs glucose metabolism and neural processing [Redkar 2025]

---

## 📊 Before / After — A/B Test Results

*Paste your `comparison_table.md` output from Step 16 here.*

---

## 📋 Pipeline Overview

PDFs (research papers)
      ↓
Text Extraction (PyMuPDF) → Chunking (800 chars, 100 overlap) → Embeddings (MiniLM)
      ↓
        Pinecone dense              BM25 sparse
                  └────── RRF Fusion ──────┘
                          ↓
                CrossEncoder Reranking
                          ↓
              LLM (Groq — LLaMA 3.3 70B) → Cited answer

---

## 📁 Repo Structure

pcos-advanced-rag-pinecone/
│
├── Advanced_RAG_Pinecone.ipynb   ← main notebook
├── README.md                      ← this file
├── requirements.txt
├── .gitignore                     ← excludes PDFs
└── papers/
    └── README.txt                 ← instructions to download papers

---

## ⚠️ Disclaimer

This tool is for **research and educational purposes only**.
It is not a medical diagnostic tool. All answers are drawn directly from the uploaded research papers and should not be used as clinical advice. Always consult a qualified healthcare professional.

---

*Built by Preeti Bhardwaj | Day 6 of the 7-Day AI Challenge | PCOS × Neurodivergence Advanced RAG*
