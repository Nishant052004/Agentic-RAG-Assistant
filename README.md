<p align="center">
  <img src="https://img.shields.io/badge/Python-3.12+-3776AB?style=for-the-badge&logo=python&logoColor=white" />
  <img src="https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white" />
  <img src="https://img.shields.io/badge/Streamlit-FF4B4B?style=for-the-badge&logo=streamlit&logoColor=white" />
  <img src="https://img.shields.io/badge/Google_Gemini-4285F4?style=for-the-badge&logo=google&logoColor=white" />
  <img src="https://img.shields.io/badge/LangChain-1C3C3C?style=for-the-badge&logo=langchain&logoColor=white" />
  <img src="https://img.shields.io/badge/ChromaDB-FF6F00?style=for-the-badge&logoColor=white" />
</p>

<h1 align="center">🤖 Agentic RAG Assistant</h1>

<p align="center">
  <b>A Multi-Agent Retrieval-Augmented Generation system with built-in hallucination verification</b>
</p>

<p align="center">
  Upload documents → Ask questions → Get verified, source-grounded answers
</p>

---

## 🌟 Overview

**Agentic RAG Assistant** is an intelligent knowledge assistant that goes beyond simple RAG. It employs a **multi-agent pipeline** where:

1. A **Retrieval Agent** fetches the most relevant document chunks from a vector database
2. A **Reasoning Agent** generates a comprehensive, well-structured answer using retrieved context
3. A **Verification Agent** cross-checks the answer against source documents to detect hallucinations

This three-agent architecture ensures answers are not just relevant, but **factually grounded** in your uploaded documents.

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    Streamlit Frontend                        │
│              (Document Upload + Chat Interface)              │
└──────────────────────┬──────────────────────────────────────┘
                       │ HTTP (REST API)
                       ▼
┌─────────────────────────────────────────────────────────────┐
│                   FastAPI Backend (/api)                      │
│                                                              │
│  ┌──────────────┐  ┌───────────────┐  ┌──────────────────┐  │
│  │  Retrieval    │→│  Reasoning     │→│  Verification     │  │
│  │  Agent        │  │  Agent         │  │  Agent           │  │
│  │              │  │                │  │                   │  │
│  │ ChromaDB     │  │ Google Gemini  │  │ Hallucination     │  │
│  │ Vector Search│  │ LLM Response   │  │ Check (JSON)      │  │
│  └──────────────┘  └───────────────┘  └──────────────────┘  │
│                                                              │
│  ┌──────────────────────────────────────────────────────┐   │
│  │  Document Ingestion Pipeline                          │   │
│  │  PDF / DOCX / TXT / CSV → Chunking → Embedding → DB  │   │
│  └──────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
```

---

## ✨ Key Features

| Feature | Description |
|---------|-------------|
| 📄 **Multi-format Ingestion** | Upload PDF, DOCX, TXT, and CSV files |
| 🧠 **Agentic Pipeline** | Three specialized agents work in sequence |
| 🛡️ **Hallucination Detection** | Verification agent flags unsupported claims |
| 🔍 **Semantic Search** | Sentence-transformer embeddings with ChromaDB |
| 🔑 **Bring Your Own Key** | Enter your Gemini API key directly in the UI |
| ⚡ **Quick Actions** | One-click summarize, explain concepts, or create study plans |
| 🐳 **Docker Ready** | Containerized deployment with a single Dockerfile |

---

## 🚀 Getting Started

### Prerequisites

- **Python 3.12+**
- **Google Gemini API Key** — Get one free at [Google AI Studio](https://aistudio.google.com/apikey)

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/Nishant052004/Agentic-RAG-Assistant.git
cd Agentic-RAG-Assistant

# 2. Create and activate virtual environment
python -m venv venv

# Windows
venv\Scripts\activate

# macOS / Linux
source venv/bin/activate

# 3. Install dependencies
pip install -r requirements.txt

# 4. Configure environment
cp .env.example .env
# Edit .env and add your GEMINI_API_KEY
```

### Running the App

```bash
# Option 1: Launch both backend & frontend with one command
python run.py

# Option 2: Run services individually
# Terminal 1 - Backend
uvicorn app.main:app --host 127.0.0.1 --port 8000

# Terminal 2 - Frontend
streamlit run streamlit_app.py --server.port 8501
```

Once running, open **http://127.0.0.1:8501** in your browser.

### Docker

```bash
docker build -t agentic-rag .
docker run -p 8000:8000 --env-file .env agentic-rag
```

---

## 📁 Project Structure

```
Agentic-RAG-Assistant/
├── app/
│   ├── __init__.py
│   ├── main.py              # FastAPI app entry point
│   ├── api.py               # REST API routes (/chat, /upload)
│   ├── agent_pipeline.py    # Multi-agent orchestration logic
│   ├── config.py            # Pydantic settings & env loading
│   ├── embeddings.py        # Sentence-transformer embedding setup
│   ├── ingestion.py         # Document chunking & indexing
│   ├── vectorstore.py       # ChromaDB vector store operations
│   └── utils.py             # File parsing (PDF, DOCX, TXT, CSV)
├── streamlit_app.py          # Streamlit frontend UI
├── run.py                    # Unified launcher script
├── requirements.txt          # Python dependencies
├── Dockerfile                # Container configuration
├── .env.example              # Environment variable template
└── README.md
```

---

## ⚙️ Configuration

All settings are managed via environment variables (`.env` file):

| Variable | Default | Description |
|----------|---------|-------------|
| `GEMINI_API_KEY` | *required* | Your Google Gemini API key |
| `EMBEDDING_MODEL` | `all-MiniLM-L6-v2` | Sentence-transformer model for embeddings |
| `LLM_MODEL` | `gemini-1.5-flash` | Gemini model to use for generation |
| `CHUNK_SIZE` | `500` | Characters per document chunk |
| `CHUNK_OVERLAP` | `50` | Overlap between consecutive chunks |
| `CHROMA_HOST` | `localhost` | ChromaDB host address |
| `CHROMA_PORT` | `8001` | ChromaDB port |

---

## 🎯 How It Works

1. **Upload** a document via the sidebar — it gets parsed, chunked, embedded, and stored in ChromaDB
2. **Ask a question** — the Retrieval Agent performs semantic search to find the top-k relevant chunks
3. **Reasoning Agent** constructs a detailed answer grounded in the retrieved context using Google Gemini
4. **Verification Agent** independently checks the answer against the source context and returns a JSON verdict:
   - ✅ **Verified Supported** — answer is well-grounded in the documents
   - ⚠️ **Inconclusive** — partial support found
   - ❌ **Hallucination Risk** — answer may contain unsupported claims
5. **Results** are displayed with the answer, verification badge, and source references

---

## 🛠️ Tech Stack

- **Backend**: FastAPI + Uvicorn
- **Frontend**: Streamlit
- **LLM**: Google Gemini (via OpenAI-compatible API)
- **Embeddings**: Sentence Transformers (`all-MiniLM-L6-v2`)
- **Vector DB**: ChromaDB (local persistent storage)
- **Orchestration**: LangChain
- **Config**: Pydantic Settings + python-dotenv

---

## 🤝 Contributing

Contributions are welcome! Feel free to:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

---

## 📝 License

This project is open source and available under the [MIT License](LICENSE).

---

<p align="center">
  Built with ❤️ by <a href="https://github.com/Nishant052004">Nishant</a>
</p>
