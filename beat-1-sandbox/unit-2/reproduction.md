# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

ocabezas95

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/73#issuecomment-5848303379

I'm a new contributor working through a course, and I'd like to take this one.

Restating the disagreement in my own words so it's clear what I'll be checking: the setup step in README.md says to add OPENROUTER_API_KEY to .env, but .env.example doesn't list that variable at all, and its LLM_PROVIDER comment offers only mock and openai. core/config.py defines both keys, so the two docs point a new setup in different directions depending on which one you follow first.

Next I'm reading those three files against each other to work out which side is authoritative before proposing any change. I'll post a reproduction report with what I find; I'm not putting a date on it.



**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/73#issuecomment-5850104826

# Reproduction Report for Issue #73

## Environment

- OS: macOS 27.0
- Python: 3.13.13
- Repository: `ocabezas95/pathreview-ai301-fa26-s1`.
- Verified checkout: `f89c06fc3ff292df2a04a39ac51319d32a76b779` (`f89c06f`) on `main`, with a clean working tree.
- Reproduction method: static inspection of the three files below. Running the application or making API requests is not needed to observe this documentation mismatch.

## Steps to Reproduce

1. Open a clone of `ocabezas95/pathreview-ai301-fa26-s1` at commit `f89c06fc3ff292df2a04a39ac51319d32a76b779`.
2. Read `README.md`, lines 24–25, in **Quick Start**. It tells the reader to add `OPENROUTER_API_KEY` to `.env` and copy `.env.example` to `.env`.
3. Read the complete `.env.example` (lines 1–27). It contains `OPENAI_API_KEY`, but no `OPENROUTER_API_KEY`. Its provider comment lists only `mock` and `openai`.
4. Read `core/config.py`, lines 17–22. The `Settings` class declares both `openai_api_key` and `openrouter_api_key`, along with an OpenRouter base URL and model.
5. Compare the README instruction with the supplied environment template. The key named by the README is missing from the template users are instructed to copy.

## Expected Behavior

The Quick Start instructions and `.env.example` should agree about configuring the provider and API key. If Quick Start directs users to supply `OPENROUTER_API_KEY`, the template should include that variable or clearly explain how to add it.

## Actual Behavior and Evidence

All excerpts below are from the verified commit above.

### A. README instructs users to add an OpenRouter key

`README.md`, lines 24–25:

```bash
# Configure environment (add your OPENROUTER_API_KEY to .env)
cp .env.example .env
```

### B. The supplied template omits that key

Complete `.env.example`, lines 1–27 (no omitted lines):

```dotenv
# =============================================================================
# PathReview — Environment Variables
# =============================================================================
# Copy this file to .env and fill in the values.
#   cp .env.example .env

# Database
DATABASE_URL=postgresql+asyncpg://pathreview:pathreview@localhost:5433/pathreview_dev

# Redis
REDIS_URL=redis://localhost:6379/0

# Vector store (ChromaDB)
VECTOR_DB_URL=http://localhost:8001

# LLM provider
# Options: "mock" (default, no API key needed), "openai"
LLM_PROVIDER=mock
OPENAI_API_KEY=sk-your-key-here

# App settings
APP_ENV=development
SECRET_KEY=dev-secret-key-change-in-production
LOG_LEVEL=INFO

# GitHub API (optional — only needed for testing GitHub analysis tools with real repos)
GITHUB_TOKEN=ghp_your-token-here
```

The complete template contains no `OPENROUTER_API_KEY`. Lines 17–19 list `mock` and `openai` as provider options, default to `mock`, and provide an `OPENAI_API_KEY` placeholder. The values above are the repository's example values.

### C. Configuration declares fields for both providers' keys

`core/config.py`, lines 17–22, inside `Settings`:

```python
    # LLM Configuration
    llm_provider: str = Field(default="mock")
    openai_api_key: str = Field(default="")
    openrouter_api_key: str = Field(default="")
    openrouter_base_url: str = Field(default="https://openrouter.ai/api/v1")
    openrouter_model: str = Field(default="google/gemma-3-27b-it:free")
```

These are the actual lowercase field names in the configuration source; the excerpt establishes their presence, not successful runtime provider behavior.

## Conclusion

The documentation mismatch is reproduced by file inspection at `f89c06f`: the README names `OPENROUTER_API_KEY` (A), but the complete environment template omits it and lists only `mock` and `openai` (B). The configuration source also contains an `openrouter_api_key` field (C). This report demonstrates inconsistent setup guidance; it does not establish an application startup failure or whether an API key is required when using the default mock provider.


## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

- 19,20
- agreement: 20/20 scored items  (bar: 18/20: PASS)

**Package analysis**
`pkg-20` was initially graded as accept by my rubric, whereas the gold label evaluated it as reject. The issue was that my rubric's check for specific-not-boilerplate was too lenient when evaluating generic summaries and inline references. After tightening the check criteria to strictly require concrete file excerpts and exact variable names rather than high-level prose, my rubric correctly evaluated `pkg-20` as reject, aligning with the gold label.

**Check rationale**

"- specific-not-boilerplate: Draft names specific filenames, variables, or exact code blocks rather than using generic template language."

This check was refined to ensure claim and reproduction reports contain issue-specific technical details (such as explicit filenames like README.md, .env.example, or core/config.py) rather than boilerplate placeholders, while avoiding unnecessary rejections when exact lines are quoted directly.

**Trade-offs**

Making the specific-not-boilerplate check stricter prevents low-effort boilerplate comments from passing, but it risks falsely rejecting valid reports that clearly describe the bug using concise inline references instead of full file snippets.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
