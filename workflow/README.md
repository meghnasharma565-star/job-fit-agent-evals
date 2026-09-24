# n8n workflow

The agent runs in [n8n](https://n8n.io), self-hosted and free.

**`job-fit-analyzer.json` is not committed yet** — export it from n8n and drop
it in this folder (see below).

## Architecture

```
Chat trigger
     │
     ▼
AI Agent node ──── Chat Model:     Gemini Flash (free tier)
     │        ├─── Fallback Model: OpenRouter free model   ← v3 only
     │        ├─── Memory:         Simple Memory (RAM)
     │        └─── Tool:           HTTP Request → Tavily search  ← v3 only
     ▼
  Output
```

The shipped version (v2.1) is the same graph **without** the tool and without
the fallback: one model call with the system prompt from
[`../prompts/v2.1-system-prompt.txt`](../prompts/v2.1-system-prompt.txt).

Settings worth noting:

- **Retry On Fail** lives on the AI Agent node's *Settings* tab, not on the
  model sub-node. Three attempts.
- The search tool's query is filled by the model through n8n's
  `{{ $fromAI('searchQuery', '...') }}` expression in the request body.

## Exporting it yourself

1. Open the workflow in n8n
2. Top-right **⋯** menu → **Download**
3. Save the file into this folder as `job-fit-analyzer.json`

## Before you commit the export

The export contains **credential references** (IDs and names), not the secrets
themselves — but check anyway:

```
grep -i -E "apiKey|token|secret|sk-|AIza" job-fit-analyzer.json
```

If that returns anything resembling a real key, delete the value before
committing. Rotate any key that has ever been committed or appeared in a
screenshot.

## Reproducing it

1. Install n8n (Node 22; newer versions fail to build `isolated-vm`)
2. `n8n start`, then open `http://localhost:5678`
3. Import `job-fit-analyzer.json`
4. Add your own Gemini / OpenRouter / Tavily credentials
5. Replace the resume block in the system prompt with your own
