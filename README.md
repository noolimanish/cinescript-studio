# CineScript Studio

A RAG-powered AI screenwriting platform that generates genre-specific screenplay content — not generic LLM output.

Built end-to-end in Python, from the retrieval pipeline to the Streamlit interface.

---

## The Problem

Most AI screenwriting tools produce generic output. You ask for a thriller scene, you get something that could be a rom-com with different names. The model doesn't know *how* a thriller is structured, *what* voice it uses, or *which* mechanical rules it follows.

CineScript Studio solves this by injecting genre-specific rules into every LLM prompt — retrieved from a knowledge base built from real scripts.

---

## Architecture

The core is a **four-layer retrieval pipeline**:

| Layer | What it retrieves | Why it matters |
|---|---|---|
| **Voice** | Tone, diction, sentence rhythm | Makes the output sound like the genre |
| **Structure** | Act breaks, beat placement, pacing | Makes the scene *work* as a scene |
| **Mechanics** | Formatting rules, slug lines, transitions | Makes it producible |
| **Philosophy** | Thematic concerns, genre conventions | Makes it *feel* like the genre |

Each layer queries a separate vector index. The top-3 most relevant chunks from each layer are injected into the prompt before generation.

---

## How it works

1. **Ingestion** — Scripts are chunked (~2,500 words each), embedded using a multilingual Indic model, and stored in a local vector index.
2. **Extraction** — A Gemini API call extracts style rules from each chunk and routes them into genre-specific JSON knowledge stores.
3. **Retrieval** — At generation time, the user's query is embedded and matched against all four layers using cosine similarity.
4. **Generation** — The retrieved rules are assembled into a multi-layer prompt and sent to Gemini.
5. **Review** — A second LLM call critiques the output for continuity and style violations before it reaches the user.
6. **Feedback** — User edits are captured in a "Decision Journal" that evolves prompts over time.

---

## Screenshots

*(Add 2–3 screenshots here: the main interface, the retrieval transparency dashboard showing cosine similarity scores, and a sample generated scene with the reviewer feedback.)*

---

## Tech Stack

- **Language:** Python
- **UI:** Streamlit
- **LLM:** Google Gemini API
- **Embeddings:** Sentence-Transformers (multilingual Indic model)
- **Vector Search:** Cosine similarity (local index)
- **Storage:** SQLite, JSON
- **Data Processing:** Custom chunking, extraction, and validation pipelines

---

## What I learned building this

- Chunking scripts without losing structural context is harder than it looks. 2,500 words was the sweet spot — smaller chunks lost scene boundaries, larger chunks diluted retrieval precision.
- A single retrieval layer produces generic output. The four-layer design was a direct response to that failure.
- The reviewer loop was an afterthought that became essential. Without it, the model repeated itself across scenes.
- Retrieval transparency (showing cosine scores to the user) built trust. Users could see *why* a rule was injected.

---

## Code Access

The source code is private.

If you'd like to review it for hiring or collaboration, reach out:

📧 **nmanish10@gmail.com**
🔗 **linkedin.com/in/noolimanish**

---

## Status

Active development. Demo deployment in progress.
