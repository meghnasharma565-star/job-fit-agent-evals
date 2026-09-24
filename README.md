# Job-Fit Analyzer — prompts and eval set

An AI agent that scores AI Product Manager job descriptions against my resume.
Built in n8n on free model tiers, measured against a hand-built eval set, and
iterated through three versions.

**This repo is the evidence behind the case study:** the system prompts, the
eval log, and the workflow. The write-up lives here →
**[Full case study](ADD_YOUR_NOTION_LINK_HERE)**

---

## At a glance

| | |
|---|---|
| Recommendation errors against my own judgement, v1 → v2.1 | **2 of 6 → 0 of 6** |
| What that fix cost | **+31% tokens per run** |
| What full autonomy (v3 agent) cost | **~3x tokens, ~5x latency** |
| What full autonomy bought | **One extra output field. Same verdict.** |
| Shipped | **The single call, not the agent** |

The conclusion I did not expect: **the most autonomous version is the one I
would not ship.**

---

## What's in here

| Path | What it is |
|---|---|
| [`prompts/CHANGELOG.md`](prompts/CHANGELOG.md) | **Start here.** Every prompt change, and the measured failure that caused it. |
| [`prompts/v1-system-prompt.txt`](prompts/v1-system-prompt.txt) | First version. Six eval runs found five distinct defects. |
| [`prompts/v2.1-system-prompt.txt`](prompts/v2.1-system-prompt.txt) | The shipped version. Ten fixes, plus the one-line correction that mattered most. |
| [`prompts/v3-tool-use-addendum.txt`](prompts/v3-tool-use-addendum.txt) | What turned it into a real agent. Measured, then rejected. |
| [`evals/job-fit-evals.csv`](evals/job-fit-evals.csv) | The raw log. Agent verdict next to mine, run by run. |
| [`evals/README.md`](evals/README.md) | How the eval set was designed, how to read it, and what it does **not** prove. |
| [`workflow/`](workflow/) | n8n architecture and how to reproduce it. |

---

## The two findings worth your time

**1. Write guardrails against observable structure, not adjectives.**
My scoring rule read *"if you list any material gap, the score cannot be 9 or
10."* It was violated twice, because "material" is a judgement the model makes
in its own favour. Rewritten as *"if the gaps section contains any entry, the
score cannot exceed 8,"* it held immediately and the score variance across
identical runs disappeared. One word was the entire bug.

**2. Autonomy multiplies your exposure to every dependency.**
Mid-project the free-tier endpoint started returning 503s. The single call
mostly survived; the five-call agent loop did not — and that is arithmetic, not
bad luck. At 90% per-call reliability, one call succeeds 90% of the time and
five calls succeed 59% of the time.

---

## Honest limitations

The v2.1 numbers are **not held out** — the fixes were derived from the same six
cases used to score them. Five of six postings were written by me. n = 6. I am
the builder, the grader and the only user. Full list in
[`evals/README.md`](evals/README.md#what-this-eval-set-does-not-prove).

---

## Stack

n8n (self-hosted) · Gemini Flash free tier · OpenRouter (fallback) · Tavily
(web search, v3 only). Total cost: ₹0.

---

*The prompts embed my own resume by design — it is the ground truth the agent
scores against. Contact details are removed.*
