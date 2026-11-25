# Contributing

Thanks for your interest in improving the Autonomous Research Agent.

## Before opening an issue

- Include the relevant error message and the cell or workflow where it occurred.
- Remove API keys, personal data, and private URLs from logs and screenshots.
- State whether the issue is related to model access, retrieval, ranking, or YouTube caption access.

## Pull requests

1. Keep changes focused and explain the motivation.
2. Never commit API keys, `.env` files, private transcripts, or private datasets.
3. Update the documentation when behavior or setup changes.
4. Run Python syntax checks for changed Python code and validate notebook JSON if a notebook is added.
5. Do not claim YouTube video analysis works for all videos; caption availability and network access vary.

## Notebook contributions

If you add or update the notebook, restart the runtime and run all cells from top to bottom before describing it as verified. Clear outputs that may contain private information before committing.
