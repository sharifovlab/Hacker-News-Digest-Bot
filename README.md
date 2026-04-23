# 🗞️ Hacker News Digest Bot

> An end-to-end NLP + LLM + RAG pipeline that fetches, classifies, summarizes, and answers questions about today's top Hacker News stories.

---

## 🚀 What It Does

Every run, this project:

1. **Fetches** top stories from the Hacker News API (async, concurrent)
2. **Scrapes** full article text from each story URL
3. **Classifies** each article by topic — AI, crypto, politics, science
4. **Summarizes** each article into 3 key sentences (extractive NLP)
5. **Answers** your natural language questions using RAG + Claude API

```
[HN API] → fetch IDs → fetch metadata (async)
         → scrape article text (rate-limited)
         → classify topic (weighted keywords)
         → extractive summary (sentence scoring)
         → embed articles (sentence-transformers)
         → RAG: find relevant articles (cosine similarity)
         → LLM answers your question (Claude API)
```

---

## 📸 Sample Output

```
=================================================================
  🗞️  HACKER NEWS DIGEST — TOP 10 STORIES
=================================================================

 1. [AI        ] OpenAI announces new reasoning model
    🔗 https://openai.com/...
    ⭐ Score: 847
    📝 The model demonstrates improved performance on complex reasoning tasks...

 2. [SCIENCE   ] Researchers discover new exoplanet in habitable zone
    🔗 https://nature.com/...
    ⭐ Score: 634
    📝 Scientists using the James Webb telescope identified a rocky planet...

=================================================================
  🤖 RAG ANSWER
=================================================================
❓ Question: What is happening with AI today?
💡 Answer: Based on today's articles, OpenAI released a new reasoning
   model focused on multi-step problem solving...
```

---

## 🧠 Concepts Covered

| Stage | Concept | What You Learn |
|-------|---------|----------------|
| 1 | **Async programming** | `asyncio`, `aiohttp`, `asyncio.gather`, Semaphore |
| 2 | **Web scraping** | HTML extraction, `trafilatura`, handling failures |
| 3 | **Text classification** | Bag-of-words, weighted keyword matching |
| 4 | **Summarization** | Stopwords, extractive vs abstractive |
| 5 | **RAG** | Embeddings, cosine similarity, LLM prompt grounding |

---

## ⚙️ Setup

### 1. Clone the repo

```bash
git clone https://github.com/your-username/hn-digest-bot.git
cd hn-digest-bot
```

### 2. Install dependencies

```bash
pip install aiohttp trafilatura sentence-transformers scikit-learn anthropic nest_asyncio
```

### 3. Set your Anthropic API key

Get a free key at [console.anthropic.com](https://console.anthropic.com) → API Keys.

```bash
export ANTHROPIC_API_KEY="your-key-here"
```

Or set it directly in the notebook (don't commit this to GitHub).

### 4. Open in Google Colab

Upload `HN_Digest_Bot.ipynb` to [colab.research.google.com](https://colab.research.google.com) and run cells top to bottom.

---

## 📁 Project Structure

```
hn-digest-bot/
│
├── HN_Digest_Bot.ipynb   # Main notebook — all 5 stages
└── README.md
```

---

## 🔧 How Each Stage Works

### Stage 1 — Async Fetching
Uses `aiohttp` + `asyncio.gather` to fetch all 10 story metadata requests **concurrently** instead of one by one. A `Semaphore` limits article scraping to 3 concurrent requests to avoid rate-limiting.

### Stage 2 — Article Scraping
`trafilatura` extracts clean readable text from raw HTML — stripping ads, navigation, and clutter automatically. Chosen over `newspaper3k` for better extraction quality on varied HTML structures.

### Stage 3 — Topic Classification
Weighted keyword matching: each keyword has a trust score (1–3). Specific phrases like `"neural network"` score higher than generic ones like `"ai"`. The topic with the highest total score wins.

### Stage 4 — Extractive Summarization
Scores each sentence by counting non-stopword words, then returns the top 3 in their **original order** to preserve narrative flow. No ML required.

### Stage 5 — RAG
`sentence-transformers` embeds all articles and the user's question into 384-dimensional vectors. Cosine similarity finds the most relevant articles. They're injected into a Claude prompt as context — grounding the answer in real, current information.

---

## 📚 Resources

| Topic | Link |
|-------|------|
| Async Python | https://realpython.com/async-io-python/ |
| trafilatura | https://trafilatura.readthedocs.io |
| Sentence Transformers | https://www.sbert.net |
| TF-IDF | https://scikit-learn.org/stable/modules/feature_extraction.html |
| Cosine Similarity | https://developers.google.com/machine-learning/clustering/similarity/measuring-similarity |
| RAG explained | https://www.pinecone.io/learn/retrieval-augmented-generation/ |
| Anthropic API | https://docs.anthropic.com |

---

## 🔜 What's Next

- [ ] Replace keyword classifier with **TF-IDF** (no hardcoded keywords)
- [ ] Replace extractive summarizer with **abstractive** using Claude API
- [ ] Add **ChromaDB or FAISS** to persist embeddings across runs
- [ ] Build an **RNN/LSTM** for sequence-based text classification
- [ ] Add a **Streamlit UI** for a web interface

---

## 🏗️ Built With

- [Hacker News API](https://github.com/HackerNews/API) — free, no auth required
- [aiohttp](https://docs.aiohttp.org) — async HTTP
- [trafilatura](https://trafilatura.readthedocs.io) — article text extraction
- [sentence-transformers](https://www.sbert.net) — text embeddings
- [scikit-learn](https://scikit-learn.org) — cosine similarity
- [Anthropic Claude](https://docs.anthropic.com) — LLM for RAG answers

---

*Built stage by stage. Committed every step. Shipped. 🚀*
