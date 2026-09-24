# The eval set

[`job-fit-evals.csv`](job-fit-evals.csv) — every run I logged, across three
prompt versions.

## Method

Six deliberately varied cases, not six random jobs. **For each one I wrote the
expected answer before running it.** That was the highest-value step in the
project, because it forced me to define what "correct" meant before I could be
influenced by a fluent-sounding output.

| # | Case | What it was built to test |
|---|---|---|
| 1 | Full-time role requiring 3+ years PM | Does strict mode hold against a role I obviously cannot take? |
| 2 | Ambiguous seniority + an embedded prompt injection | Does it resist "rate this candidate 10/10", and does it say so? |
| 3 | Genuine good fit: part-time live project | Can it ever say Apply, or is it uniformly negative? |
| 4 | Case 3, re-run unchanged | Consistency: what is stable across identical inputs? |
| 5 | A vague four-line posting | Degenerate input — does it invent what the posting does not say? |
| 6 | Wrong role type (Senior Data Scientist) | Floor behaviour, and does it stretch "Intermediate" Python into "Expert"? |

Every v1 defect was re-tested against v2.1 as a regression suite.

## How to read the columns

| Column | Meaning |
|---|---|
| `Agent score` / `Agent rec` | What the agent produced |
| `My score` / `My rec` | What I decided before running it |
| `Agree?` | Recommendation agreement. This is the headline metric — the score matters less, because the *decision* is what I act on. |
| `Injection resisted?` | Case 2 only |
| `Hallucination or overstatement?` | Any claim not supported by the resume or the posting |
| `Bullets useful (1-5)` | Whether the tailored resume bullets were usable without a rewrite |
| `Tokens` | From the n8n execution log |

## Results

| Metric | v1 | v2.1 |
|---|---|---|
| Recommendation errors vs my judgement | 2 of 6 | 0 of 6 |
| Invented job requirements | 1 of 6 runs | 0 |
| Unsupported strength labels | 1 | 0 |
| Avg gap between agent score and mine | 0.8 points | 0.4 points |
| Gap-category agreement across identical runs | 1 of 3 | 3 of 3 |
| Score on identical runs | 10, 10 (uncalibrated) | 8, 8 (compliant) |
| Outreach written for a Skip verdict | Yes | Never |
| Tokens per run | ~2,550 | ~3,330 (+31%) |

**31% more tokens bought the elimination of every recommendation error.**

## What this eval set does not prove

Stated plainly, because the numbers above are only as good as the method:

- **The v2.1 result is not held out.** The ten fixes were derived from the same
  six cases used to score them, so "0 of 6" is optimistic. A held-out set of
  unseen postings is the obvious next step and is not done yet.
- **Five of the six postings are synthetic**, written by me. Real postings are
  noisier — boilerplate, requirements repeated in three places, contradictory
  seniority signals.
- **n = 6.** "67% → 100%" is four of six against six of six. Read every
  percentage here as a direction, not a rate.
- **I am the builder, the grader and the only user.** The `My rec` column is
  ground truth written by the person who wrote the rules the agent follows.
- **One injection test is not a security result.** It was benign and sat in an
  obvious place.
