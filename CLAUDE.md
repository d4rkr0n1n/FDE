# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A personal learning log for FDE coursework. There is no application, build system, or test suite. The content is Jupyter notebooks, one per lecture, under `Section-N/Lecture-M.ipynb`. `JOURNAL.md` is a running bullet list of setup steps and progress. Add an entry there when you change the environment, for example by adding a package.

## Environment

- A Python virtualenv exists in `.venv/` (`.venv\Scripts\python.exe`), but the notebook kernel currently runs on the global Python 3.15. Lecture-1's `%pip` output shows packages going to the user site-packages (`AppData\Roaming\Python\Python315`). When a notebook's code is run outside Jupyter for testing, `load_dotenv()` may not find the repo-root `.env`, so load it explicitly with `load_dotenv(".env")` from the repo root.
- There is no `requirements.txt`. Dependencies are installed with `%pip install` in the first cell of each notebook (currently `python-dotenv`, `requests`, `openai`, `ipython`). New dependencies go in that first cell, which is kept for installs only.
- Secrets live in `.env`, which git ignores. `.env.example` is the template and needs `OPENROUTER_API_KEY` (format `sk-or-v1-…`). `.vscode/settings.json` sets `python.terminal.useEnvFile` so VS Code terminals load `.env`.

## Notebook conventions (from Lecture-1)

Cells come in this order: installs → imports → load and check the API key → connectivity check → client setup → calls to the LLM.

- LLM access goes through **OpenRouter** using the OpenAI SDK pointed at a custom base URL:
  ```python
  OpenAI(api_key=openrouter_api_key, base_url="https://openrouter.ai/api/v1", timeout=120, max_retries=5)
  ```
  The default model is `"openrouter/free"`. `max_retries=5` is there because free models are often busy.
- The key is loaded with `load_dotenv()` + `os.getenv("OPENROUTER_API_KEY")`. You can confirm the key works with `GET https://openrouter.ai/api/v1/key`.
- Responses are rendered with `IPython.display.Markdown`.
- Use markdown cells with `#` headings to separate the sections of a notebook.
