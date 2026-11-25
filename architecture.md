# Architecture

## Overview

The project has two main paths: general research and YouTube transcript analysis. A LangGraph ReAct agent routes user requests to the available tools.

## General research path

1. **Question expansion:** the LLM proposes focused subqueries for a broad question.
2. **Parallel retrieval:** available sources are queried concurrently. Tavily is optional; ArXiv and Wikipedia are configured as additional sources.
3. **Normalization and deduplication:** retrieved documents are converted to a consistent structure and duplicate evidence is reduced.
4. **Hybrid retrieval:** BM25 captures lexical matches, while sentence-transformer embeddings capture semantic similarity.
5. **Reranking:** a cross-encoder reranks candidate passages for relevance to the original question.
6. **Iterative retrieval:** the system identifies missing aspects and issues a small number of follow-up searches.
7. **Answer generation:** the agent produces a response grounded in the retrieved evidence and available source references.

## Agent tools

- `deep_research_search`: runs the enhanced multi-source retrieval pipeline.
- `arxiv_academic_search`: searches academic papers and preprints.
- `wikipedia_search`: retrieves background information and definitions.
- `youtube_video_research`: retrieves available captions and analyzes timestamped transcript text.

## LLM configuration

The supplied working notebook is configured for Groq using the model ID `openai/gpt-oss-120b`. The API key is loaded at runtime from `GROQ_API_KEY`. Model availability, account eligibility, and rate limits are controlled by the provider and may change.

## Optional web search

Tavily is enabled only when `TAVILY_API_KEY` is configured. Without this key, the notebook installs a placeholder web-search tool that reports web search as disabled; the other configured research sources remain available.

## Reproducibility notes

- Record dependency versions when preparing a release or reproducing an experiment.
- The embedding and cross-encoder models may be downloaded on first use.
- Ranking scores are relative ranking signals, not probabilities.
- External sources, APIs, and model endpoints can change independently of this repository.
