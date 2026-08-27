# Source: https://reclaimllm.com/news/normalized-session-tool-calls

[← Back to News](https://reclaimllm.com/news)

feature

May 10, 2026

# Normalized session tool calls

ReclaimLLM now records one normalized row per tool call so stats pages can show tool usage, token attribution, failures, and command-family breakdowns across providers.

## Normalized session tool calls

ReclaimLLM now records tool calls as normalized rows instead of only leaving them buried inside session blobs. That makes tool usage visible in stats pages, with per-tool counts, token attribution, command-family breakdowns, failures, compression and redaction flags, and enterprise-scoped views for admins and team leads.

The new data powers the tool-call panels on personal stats and enterprise org stats. If a session contains Claude, Gemini, Codex, Cursor, MCP, browser, or API-proxy tool activity, the stats pipeline can now turn that into a consistent shape and aggregate it the same way across providers.

## How it works

The stats pipeline derives tool-call rows from session blobs after the raw session is fetched and parsed. Each tool call becomes one normalized record with the provider name, the raw tool name, a canonical tool name when one can be identified, and metadata such as status, token estimates, command family, file paths, and timestamps.

That normalized layer is what powers the charts. It supports:

- total normalized call count
- input, output, and total token estimates
- token attribution per canonical tool
- command-family distributions for shell-style calls
- tool category breakdowns
- failure and status analysis
- compression and redaction counts

For users, that shows up as a clearer stats surface: you can see which tools are used most, which ones account for most token consumption, and where errors or redactions are concentrated. For enterprise org views, the same aggregates work at org scope or team scope depending on role.

The normalization is intentionally append-compatible. Provider-specific fields are still preserved when they do not map cleanly into the shared schema, so the system can absorb new tool surfaces without forcing a schema change every time a provider adds a new event shape.

## Why we built it

Before this change, ReclaimLLM had session-level summaries like dominant tool and tool distribution, but that was not enough for the kinds of analysis we actually need.

Those summaries could not answer questions like:

- which tools consume the most tokens
- which tools fail most often
- how much shell activity comes from a specific command family
- how much redaction or compression is happening
- how tool usage differs across providers
- what an admin or team lead should be able to audit in an org-wide or team-scoped view

The normalized row model solves that without pushing analytics into the frontend or into summarization. The raw session blob remains the source of truth, but the stats pipeline now derives deterministic tool-call data from it.

That tradeoff matters. Tool analytics should be reproducible, not inferred by an LLM. It should be consistent across reruns, and it should stay separate from narrative summaries. We also kept the model wide enough to handle future providers and tool shapes without immediate migrations.

## Current limitations

This is a derived layer, so historical sessions need a refill before they appear fully in the new charts.

Recompute is currently delete-then-insert for a session’s tool-call rows. That keeps the data idempotent, but it is not fully atomic. If a recompute fails mid-run, the session may temporarily have no tool-call rows until it is processed again.

Some fields are still estimates when the provider does not emit precise usage. And when a provider changes its tool shape, the normalizer may temporarily classify some rows as unknown until we teach it the new pattern.

If you see a tool name, command family, or status that looks wrong, that is useful feedback. It usually means the raw shape is different from what the normalizer expects, not that the session data itself is missing.

Share[Share on X](https://x.com/intent/post?url=https%3A%2F%2Freclaimllm.com%2Fnews%2Fnormalized-session-tool-calls&text=Normalized%20session%20tool%20calls) [Share on LinkedIn](https://www.linkedin.com/sharing/share-offsite/?url=https%3A%2F%2Freclaimllm.com%2Fnews%2Fnormalized-session-tool-calls) [Share on Facebook](https://www.facebook.com/sharer/sharer.php?u=https%3A%2F%2Freclaimllm.com%2Fnews%2Fnormalized-session-tool-calls)