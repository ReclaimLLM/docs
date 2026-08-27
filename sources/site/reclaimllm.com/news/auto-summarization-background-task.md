# Source: https://reclaimllm.com/news/auto-summarization-background-task

[← Back to News](https://reclaimllm.com/news)

feature

Mar 15, 2026

# Sessions Now Auto-Summarize After Ingest

New sessions are automatically titled, described, and tagged in the background — no manual trigger required.

## Sessions Now Auto-Summarize After Ingest

Sessions ingested into ReclaimLLM now receive a title, description, and tags automatically. Previously this required an explicit API call after ingest. That endpoint still exists, but you no longer need to call it.

## What to Expect

After a session is ingested, a background task wakes periodically, picks up any untitled sessions, and generates summaries via Azure OpenAI. The default interval is 5 minutes, so titles typically appear within a few minutes of ingest under normal load.

Duplicate sessions — those with identical blob content — are detected and cleaned up in the same pass. Only one copy is kept and summarized; the rest are dropped without making any OpenAI calls.

## Limitations

- **Latency is interval-bound, not ingest-bound.** A session ingested right after a batch run will wait up to the full interval before being summarized. If you need a title immediately, the on-demand summarize endpoint is still available.
- **Backlog drains linearly.** After an outage or first deploy, a large backlog of untitled sessions clears at a fixed batch size per cycle. Expect a few cycles before everything is titled.
- **OpenAI outages delay summaries, not ingests.** If Azure OpenAI is unavailable, sessions continue to ingest normally. Summarization resumes automatically once the service recovers — no data is lost.

## What's Next

This is the first version of auto-summarization. The current polling approach is intentionally simple: no new infrastructure, no separate worker process. If tighter latency (sub-minute) or higher ingest volume becomes a requirement, we'll migrate to a queue-based model. That's tracked and will be communicated here when the time comes.

Feedback welcome — if you see sessions stuck untitled longer than expected, open an issue.

Share[Share on X](https://x.com/intent/post?url=https%3A%2F%2Freclaimllm.com%2Fnews%2Fauto-summarization-background-task&text=Sessions%20Now%20Auto-Summarize%20After%20Ingest) [Share on LinkedIn](https://www.linkedin.com/sharing/share-offsite/?url=https%3A%2F%2Freclaimllm.com%2Fnews%2Fauto-summarization-background-task) [Share on Facebook](https://www.facebook.com/sharer/sharer.php?u=https%3A%2F%2Freclaimllm.com%2Fnews%2Fauto-summarization-background-task)