## Diagnosis

At verified commit `f89c06f`, static inspection reproduces a documentation mismatch in the environment setup instructions. `README.md` lines 24–25 tell contributors to add `OPENROUTER_API_KEY` to `.env` after copying `.env.example`, but the complete `.env.example` instead lists `mock` and `openai` as `LLM_PROVIDER` options and includes only `OPENAI_API_KEY`.

`core/config.py` declares both `openai_api_key` and `openrouter_api_key`, confirming that configuration fields exist for both providers. The problem is therefore inconsistent setup guidance across the documentation files, not a demonstrated application startup failure. This repro does not establish which configuration option should be authoritative or whether an API key is required when `LLM_PROVIDER=mock`.

## Scope

This change will align the environment-setup guidance in `README.md` and `.env.example` so a new contributor sees the same instructions for the documented OpenRouter configuration in both places.

In scope:

- Updating the Quick Start environment-configuration text in `README.md`.
- Updating the LLM-provider comments and key placeholders in `.env.example` so they no longer conflict with the README.

Out of scope:

- Changing `core/config.py`, provider-selection behavior, or the default `LLM_PROVIDER`.
- Testing application startup, making API requests, or changing secrets or example values beyond the documentation/template guidance.

## Files to touch

- `README.md` — update the Quick Start environment-configuration instructions so they describe the same OpenRouter setup reflected in the environment template.

- `.env.example` — update the LLM-provider comments and API-key placeholders so they include the OpenRouter guidance referenced by the README.

## Approach

1. Before editing `.env.example`, search the repository for `openrouter_api_key`, `llm_provider`, and `openrouter`. If a code path selects OpenRouter through `LLM_PROVIDER=<value>`, add that value to the Options comment. Otherwise, leave the Options comment as `mock` and `openai`. In either case, add an `OPENROUTER_API_KEY` placeholder next to `OPENAI_API_KEY`, labeled only as an optional OpenRouter configuration setting; do not claim that a runtime path currently uses it.

2. Rewrite the Quick Start comment in `README.md` so it no longer tells contributors to add a key before the first run. Cite the template’s existing statement that mock is the default and needs no API key, then describe `OPENROUTER_API_KEY` as an optional configuration setting. Do not claim that the application runs successfully with either configuration.

3. Compare the two files line by line. Every variable the README names must appear in `.env.example`, and neither file should say an API key is required while `LLM_PROVIDER=mock`. `docs/SETUP.md` line 47 still calls the key “required for AI features”; it is outside this two-file scope, so I will report it as a follow-up rather than edit it.

## Test plan

1. Run `sed -n '20,32p' README.md`.

   Expected: the instructions say that copying `.env.example` preserves `LLM_PROVIDER=mock` as the default and that `OPENROUTER_API_KEY` is optional rather than required before a first run.

2. Run `sed -n '1,35p' .env.example`.

   Expected: the LLM section keeps `LLM_PROVIDER=mock` as the default, preserves the `OPENAI_API_KEY` placeholder, and adds an `OPENROUTER_API_KEY` placeholder with neutral optional guidance. The Options comment includes an OpenRouter value only if the planned repository search establishes that `LLM_PROVIDER` selects it; otherwise, it remains `mock` and `openai`.

3. Run `grep -nE 'OPENROUTER_API_KEY|OPENAI_API_KEY|LLM_PROVIDER' README.md .env.example`.

   Expected: every variable named by the README exists in `.env.example`, and neither file says an API key is required while the mock provider is active.

4. Run `git diff --check`.

   Expected: no output and a zero exit status. No application run or API request is needed because the reproduced defect is inconsistent static setup guidance.

## Risks and unknowns

- I have not confirmed whether a runtime path consumes `openrouter_api_key` or selects OpenRouter through `LLM_PROVIDER`. The repository search in the approach will decide whether the Options comment changes; this documentation-only change will not claim that the key activates a working runtime provider.
- `docs/SETUP.md` has related “required for AI features” wording, but it is outside this two-file scope. I will mention it as a follow-up rather than expand this change.
- The main documentation risk is implying that an API key is required for the default mock configuration. The updated README and template will explicitly preserve `LLM_PROVIDER=mock` as the no-key default.

## Deviations

No deviations from the accepted plan. The implementation kept `LLM_PROVIDER=mock`, left the Options comment as `mock` and `openai`, added a neutral optional `OPENROUTER_API_KEY` placeholder, and updated the Quick Start wording. No runtime code changed.
