# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

ocabezas95

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/73#issuecomment-6031799793

I reproduced the documentation mismatch by static inspection at commit `f89c06f`. `README.md` tells contributors to add `OPENROUTER_API_KEY` after copying `.env.example`, but the complete template omits that key, lists only `mock` and `openai` in its provider comment, and defaults to `LLM_PROVIDER=mock`. `core/config.py` declares both key fields.

My plan is limited to `README.md` and `.env.example`: align their environment-setup wording, keep mock as the no-key default, and document `OPENROUTER_API_KEY` as optional rather than a requirement before the first run. I will re-run the static file-inspection repro, compare related references, and run `git diff --check`.

I will not change runtime configuration or provider behavior. Before finalizing the provider wording, I will confirm whether another code path consumes `openrouter_api_key`; I will report the related `docs/SETUP.md` wording as a follow-up rather than expand this change.

---

## Your branch

**Branch**

docs/73-align-openrouter-env-guidance

**Evidence**

Before, from my Unit 2 static-inspection repro at `f89c06f`:

```text
README.md lines 24–25:
# Configure environment (add your OPENROUTER_API_KEY to .env)
cp .env.example .env

.env.example LLM section:
# Options: "mock" (default, no API key needed), "openai"
LLM_PROVIDER=mock
OPENAI_API_KEY=sk-your-key-here

The complete template contained no OPENROUTER_API_KEY.
```

After, on branch `docs/73-align-openrouter-env-guidance`:

```text
1. Command: `sed -n '20,32p' README.md`

    # Configure environment (mock is the default and needs no API key;
    # OPENROUTER_API_KEY is optional)
    cp .env.example .env

2. Command: `sed -n '1,35p' .env.example`

    # Options: "mock" (default, no API key needed), "openai"
    LLM_PROVIDER=mock
    OPENAI_API_KEY=sk-your-key-here

    # Optional OpenRouter configuration
    OPENROUTER_API_KEY=

3. Command: `grep -nE 'OPENROUTER_API_KEY|OPENAI_API_KEY|LLM_PROVIDER' README.md .env.example`

    README.md:25:# OPENROUTER_API_KEY is optional)
    .env.example:18:LLM_PROVIDER=mock
    .env.example:19:OPENAI_API_KEY=sk-your-key-here
    .env.example:22:OPENROUTER_API_KEY=

4. Command: `git diff --check`

    No output; exit status 0.
```

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Full run: 16/20.
2. Targeted retry: pkg-02, 1/1 agreement.
3. Targeted retry: pkg-05,pkg-02, 2/2 agreement.
4. Targeted retry: pkg-08,pkg-02, 2/2 agreement.
5. Final full run: 19/20 agreement.

The final 19/20 score matches the agreement line in `eval-run.txt`.

**Package analysis**

`pkg-02`: the gold label was accept. My first full run rejected it on scope-bounded, but the revised skill accepted it and matched the gold label. The candidate plan made one code change for the reported failure and proposed a limited audit of two similar sites, with anything suspicious reported as a follow-up rather than fixed in the same change. I revised the evidence guide so a bounded read-only audit does not become scope creep when it does not add edits beyond the issue.

**Check rationale**

| `scope-bounded` | Every change the plan proposes (its scope statement, the files or areas it names, and its approach), read against the behavior the issue describes. | Passes if every proposed change is needed to fix this issue's behavior. Fails if the plan includes any refactor, rename, style cleanup, dependency bump, new feature, or fix for an adjacent bug that the plan does not show the fix depends on. Mentioning adjacent problems as a separate follow-up is fine; folding them into this change is not. Judge the change itself, not whether the plan has a "not in scope" section.

I kept this check focused on the changes a plan will actually make. I revised the evidence guide after pkg-02 so a narrowly bounded audit can remain in the plan when it only identifies possible follow-up work and does not add extra edits.

**Trade-offs**

This check can accept a limited audit that discovers adjacent issues, so it may not catch every plan that spends too much investigation effort. The safeguard is that the audit must not modify those adjacent sites and must report them separately. I tested this trade-off by rerunning pkg-02 as a canary; after the clarification, it changed from reject to the gold accept without changing the scope-creep category matches.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
