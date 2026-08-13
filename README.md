# 💰 Financial Agent Project

![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Streamlit](https://img.shields.io/badge/Streamlit-FF4B4B?style=for-the-badge&logo=streamlit&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Google Gemini](https://img.shields.io/badge/Google%20Gemini-4285F4?style=for-the-badge&logo=google&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)

---

## 🤔 What Is This?

Ever wished you had a team of financial analysts working for you 24/7 — but without the crazy salary bill? That's basically what this project is.

**Financial Agent Project** is a multi-agent AI system that breaks down complex financial tasks into specialized agents, each doing one thing really well. One agent pulls live market data, another scrapes the news, another does risk analysis — and they all talk to each other to give you a clear, comprehensive picture of the market.

It's powered by **Google Gemini**, connected to **Yahoo Finance**, and ships with a clean **Streamlit dashboard** that you can actually use without being a finance PhD.

> **TL;DR:** Ask it about any stock, get back a full AI-generated market analysis with charts, risk assessment, and earnings predictions.

---

## ✨ What Can It Do?

- 📊 **460+ Global Stocks** — covers US markets, Asia (Korea, Japan, India, Hong Kong), Europe, 98 ETFs, and 20+ cryptocurrencies
- 🤖 **Multi-Agent AI** — specialized agents collaborate like a real research team
- 📈 **Earnings Predictions** — uses machine learning (polynomial regression) to forecast future earnings
- 📰 **Live News** — scrapes financial news automatically and feeds it into the analysis
- 🎯 **Risk Assessment** — calculates your portfolio's exposure and highlights concentration risk
- 📉 **Interactive Charts** — compare stocks side-by-side with real charts, not just raw numbers
- 🎤 **Voice I/O** — yes, you can actually talk to it

---

## 🧠 How the Agents Work Together

Think of it like a small research firm where everyone has a specific job:

```
You ask a question
        │
        ▼
┌─────────────────────────┐
│   FastAPI Orchestrator   │  ← The "manager" who routes your request
└────────────┬────────────┘
             │
     ┌───────┼───────────────────────────┐
     ▼       ▼         ▼        ▼        ▼
  📡 API  📰 News   🔍 RAG   📊 Risk  💬 Gemini
  Agent   Agent    Agent    Agent     Agent
  (data)  (news)  (search) (analyze)  (write)
             │
             ▼
    📱 Streamlit Dashboard
    (your beautiful results)
```

| Agent | What It Does |
|-------|-------------|
| **API Agent** | Grabs real-time stock data from Yahoo Finance with smart caching & retry logic |
| **Scraping Agent** | Fetches financial news from the web using newspaper3k and BeautifulSoup |
| **Retriever Agent** | Searches through documents using FAISS vector similarity (RAG) |
| **Language Agent** | The writer — uses Google Gemini to turn raw data into readable analysis |
| **Analysis Agent** | Calculates portfolio risk, weights, and exposure |
| **Prediction Agent** | Forecasts future earnings using scikit-learn regression models |
| **Graphing Agent** | Builds the charts and comparison visuals using matplotlib |
| **Voice Agent** | Handles speech input/output with SpeechRecognition + gTTS |

---

## 🚀 Getting Started

### What You Need First

- Python 3.10+
- A **Google Gemini API key** — it's free, grab one at [ai.google.dev](https://ai.google.dev/)
- Git

### Step-by-Step Setup

**1. Clone the repo**
```bash
git clone https://github.com/rishabhgsoain/Financial-Agent-Project.git
cd Financial-Agent-Project
```

**2. Create a virtual environment** (keeps your global Python clean)
```bash
python -m venv venv
source venv/bin/activate      # Mac/Linux
# venv\Scripts\activate       # Windows
```

**3. Install dependencies**
```bash
pip install -r requirements.txt
```

**4. Set up your API key**

Copy the example env file and fill in your Gemini key:
```bash
cp .env.example .env
```

Open `.env` and set:
```
GEMINI_API_KEY="your-key-here"
```

**5. Run it!**
```bash
streamlit run app.py
```

Open your browser at **http://localhost:8501** and you're good to go 🎉

---

## 🏃 Running Modes

### Option A — Simple Mode (Recommended for local use)
Everything runs together in one command:
```bash
streamlit run app.py
```

### Option B — Separated Mode (Better for production)
Run the backend and frontend separately for more control:

```bash
# Terminal 1 — Start the API backend
uvicorn orchestrator.orchestrator:app --host 0.0.0.0 --port 8000

# Terminal 2 — Start the frontend
streamlit run streamlit_app/app.py
```

- App: http://localhost:8501
- API docs (Swagger): http://localhost:8000/docs

### Option C — Docker
```bash
docker build -t financial-agent .
docker run -p 8501:8501 -p 8000:8000 --env-file .env financial-agent
```

---

## 💬 Example Queries

Once the app is running, try things like:

- *"Analyze AAPL, MSFT, and GOOGL — what's the current market trend?"*
- *"Compare Samsung (005930.KS) and TSMC performance over the last month"*
- *"What's the risk exposure in a portfolio of BTC-USD, ETH-USD, and AAPL?"*
- *"Show me the earnings prediction for Tesla"*
- *"Summarize the latest news for NVDA"*

---

## 📁 Project Structure

```
Financial-Agent-Project/
│
├── app.py                       # Combined app entry point (FastAPI + Streamlit)
├── requirements.txt             # All Python dependencies
├── .env.example                 # Template for your environment variables
├── Dockerfile                   # Docker configuration
│
├── agents/                      # All AI agents live here
│   ├── analysis_agent.py        # Portfolio risk & exposure analysis
│   ├── language_agent.py        # Gemini-powered text generation
│   ├── retriever_agent.py       # FAISS-based document retrieval (RAG)
│   ├── simple_retriever_agent.py
│   └── voice_agent.py           # Speech input/output
│
├── data_ingestion/              # Data collection layer
│   ├── api_agent.py             # Yahoo Finance data fetching (with caching)
│   └── scrapping_agent.py       # Financial news scraping
│
├── orchestrator/                # FastAPI backend
│   └── orchestrator.py          # Routes requests & coordinates agents
│
├── streamlit_app/               # Streamlit frontend
└── docs/                        # Additional documentation
```

---

## ⚠️ API Rate Limits (Good to Know)

Google Gemini free tier is generous but has limits:

| Model | Requests/min | Requests/day |
|-------|-------------|-------------|
| `gemini-2.0-flash-exp` | 10 | 1,500 |
| `gemini-2.5-pro` | — | 50 |

**Tips to stretch your quota:**
- Use `gemini-2.0-flash-exp` for most queries (much higher daily limit)
- The app automatically falls back to a structured template if you hit rate limits, so it never completely breaks
- For serious or production use, consider upgrading at [ai.google.dev/pricing](https://ai.google.dev/pricing)

---

## 🧰 Tech Stack

| Layer | Technology |
|-------|-----------|
| **Frontend** | Streamlit |
| **Backend** | FastAPI |
| **AI / LLM** | Google Gemini (gemini-2.0-flash-exp, gemini-2.5-pro) |
| **Market Data** | Yahoo Finance via `yfinance` |
| **NLP / RAG** | LangChain + FAISS vector store |
| **ML** | scikit-learn (polynomial regression for earnings forecasting) |
| **Charts** | Matplotlib |
| **News Scraping** | newspaper3k + BeautifulSoup |
| **Voice** | SpeechRecognition + gTTS |
| **Data Processing** | Pandas + NumPy |
| **Deployment** | Docker / Streamlit Cloud / Render |

---

## 🤝 Contributing

Want to add a new agent, improve an existing one, or fix a bug? Contributions are very welcome!

1. Fork this repo
2. Create a new branch: `git checkout -b feature/my-new-feature`
3. Make your changes and test them locally
4. Commit with a clear message: `git commit -m "Add: my new feature"`
5. Push and open a Pull Request

For big changes, please open an issue first so we can talk through the approach before you spend a lot of time on it.

---

## 📄 License

This project is under the **MIT License** — you're free to use it, modify it, and build on top of it. See [LICENSE](LICENSE) for the full text.

---

## 🙏 Thanks To

These open-source tools made this whole thing possible:

- [Yahoo Finance](https://finance.yahoo.com/) for the market data
- [Google Gemini](https://ai.google.dev/) for the AI backbone
- [LangChain](https://www.langchain.com/) for the RAG framework
- [Streamlit](https://streamlit.io/) for making dashboards fun to build
- [FastAPI](https://fastapi.tiangolo.com/) for the blazing-fast backend

---

*If this project helped you or you find it interesting, a ⭐ on GitHub goes a long way — it helps others find it too!*
