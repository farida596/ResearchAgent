# Autonomous Research Agent

A LangGraph-powered research assistant that combines multi-source retrieval, hybrid ranking, iterative evidence gathering, and YouTube transcript analysis.

## Features

- **fAgent orchestration:** LangGraph ReAct agent chooses the appropriate research tool for a request.
- **Multi-source research:** optional Tavily web search, ArXiv academic search, and Wikipedia background retrieval.
- **Query expansion:** turns a broad research question into focused subqueries.
- **Parallel retrieval:** queries configured sources concurrently.
- **Hybrid ranking:** combines BM25 keyword relevance with sentence-transformer semantic similarity.
- **Reranking:** uses a cross-encoder to improve the ordering of candidate evidence.
- **Iterative retrieval:** identifies evidence gaps and performs focused follow-up searches.
- **YouTube research:** retrieves available captions, preserves transcript timestamps, and uses an LLM to summarize and explain the video.
- **Failure handling:** retries transient model errors and reports transcript retrieval failures separately from LLM failures.

## Architecture

```text
User question
     |
     v
LangGraph ReAct agent
     |
     +---- Broad research
     |        |
     |        +--> Query expansion
     |        +--> Parallel sources: Tavily (optional), ArXiv, Wikipedia
     |        +--> Deduplication
     |        +--> BM25 + semantic retrieval
     |        +--> Cross-encoder reranking
     |        +--> Evidence-gap analysis and follow-up retrieval
     |
     +---- ArXiv search
     +---- Wikipedia search
     +---- YouTube video research
              |
              +--> Extract video ID
              +--> Fetch available captions
              +--> Preserve segment timestamps
              +--> LLM analysis
     |
     v
Evidence-grounded response
```

See [`docs/architecture.md`](docs/architecture.md) for more detail and [`docs/youtube-workflow.md`](docs/youtube-workflow.md) for YouTube behavior and troubleshooting.

## Technology stack

- Python
- LangChain and LangGraph
- Groq API through `langchain-groq`
- Tavily (optional web search)
- ArXiv and Wikipedia integrations
- `youtube-transcript-api`
- Sentence Transformers, BM25, scikit-learn, and a cross-encoder
- Google Colab for the notebook runtime

## Quick start

### 1. Get the project notebook ready

Place the notebook you personally ran successfully at:

```text
notebooks/Enhanced_Research_Agent.ipynb
```

The notebook is deliberately omitted from this template. Do not upload an untested generated copy in its place.

### 2. Open the notebook in Google Colab

Upload the notebook to Colab, or open it from your GitHub repository after you add it.

### 3. Create API keys

- **Groq:** create an API key in the [Groq Console](https://console.groq.com/keys). The notebook's configured model is `openai/gpt-oss-120b`; access and rate limits depend on your Groq account and current model availability.
- **Tavily (optional):** create a key at [Tavily](https://app.tavily.com/). Without it, web search is disabled, while ArXiv, Wikipedia, and YouTube caption retrieval can still be used.

### 4. Configure secrets

In Google Colab, open **Secrets** (key icon) and add:

| Secret name | Required? | Purpose |
|---|---:|---|
| `GROQ_API_KEY` | Yes | LLM calls |
| `TAVILY_API_KEY` | No | Web search |

Enable notebook access for each secret. Never put real keys in notebook cells, `.env` files committed to Git, screenshots, or this repository.

For local development, copy `.env.example` to `.env` and load the values into environment variables. Keep `.env` untracked.

### 5. Install dependencies

The notebook installs its dependencies in its setup cell. For a local Python environment:

```bash
python -m venv .venv
```

Activate the environment, then run:

```bash
pip install -r requirements.txt
```

The notebook is designed primarily for Google Colab; local execution may require adapting the secret-loading cell and runtime-specific display code.

## YouTube limitations

The YouTube feature analyzes **available captions/transcripts**, not video frames or audio. It cannot guarantee captions exist for every video. YouTube may also block requests from hosted notebook IP addresses (for example, Colab). If captions cannot be retrieved, use a local runtime or provide a transcript export. The agent must not invent transcript content or timestamps.

See [`docs/youtube-workflow.md`](docs/youtube-workflow.md).

## Repository structure

```text
autonomous-research-agent/
├── .github/
│   └── workflows/
│       └── validate-repo.yml
├── docs/
│   ├── architecture.md
│   └── youtube-workflow.md
├── notebooks/
│   └── README.md                 # Add your verified notebook here
├── .env.example
├── .gitignore
├── CONTRIBUTING.md
├── LICENSE
├── README.md
└── requirements.txt
```

## Responsible use and known limitations

- Search results and generated summaries can be incomplete or incorrect. Verify important claims against primary sources.
- Free-tier model quotas, model availability, and provider rate limits can change.
- Automatically generated captions may contain transcription errors.
- Cross-encoder outputs are ranking scores, not calibrated probabilities.
- The first run may download embedding and reranking models and can take several minutes.
- Respect the terms of service and copyright rules of every source you access.

## Contributing

Bug reports and improvements are welcome. Please read [`CONTRIBUTING.md`](CONTRIBUTING.md) before opening a pull request.

## License

This project is released under the MIT License. See [`LICENSE`](LICENSE).
