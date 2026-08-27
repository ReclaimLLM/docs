# Source: https://reclaimllm.com/news/session-statistics-via-background-task

[← Back to News](https://reclaimllm.com/news)

update

Mar 18, 2026

# Reliable Session Statistics via Background Task

We've introduced a server-side background task to compute consistent token counts, tool usage, and code impact metrics for every session across all capture sources.

## Overview

RCLM now automatically calculates detailed statistics for every captured LLM session, regardless of whether you're using the browser extension, CLI, or automated hooks. Previously, session metadata like token counts and tool usage were only available if the source client explicitly provided them. This often led to incomplete dashboards and unreliable usage analytics. Our new background task ensures that every session is analyzed for its full impact.

## How it Works

The core of this update is a new background service that identifies sessions with missing statistics and processes them systematically:

1. **Server-Side Analysis**: Instead of relying on client-side logic, the RCLM server now reads the full session data and computes statistics directly. This ensures consistency across all platforms.
2. **Accurate Token Counting**: We've integrated the `tiktoken` library using the `cl100k_base` encoding. This provides highly accurate token counts for GPT-4 class models and serves as a reliable industry-standard approximation for other providers like Claude and Gemini.
3. **Expanded Metrics**: Beyond simple token counts, we now track several new data points for every session:
 - **Message and Turn Counts**: Understand the depth of each interaction.
 - **Tool Distribution**: A detailed breakdown of which tools (like `grep_search`, `read_file`, or custom tools) were used most frequently.
 - **Code Impact**: Automated tracking of `lines_added`, `lines_removed`, and the programming languages involved in each session.
4. **Automated Cleanup**: To keep your workspace clean, any sessions that are found to have zero input and output tokens after analysis are automatically deleted.

## Why We Made This Change

As RCLM has grown to support more ways of capturing interactions, maintaining consistent data became a challenge. Client-side computation is prone to inconsistency and adds overhead to the ingestion process. By moving this logic to a background task, we keep the initial session capture fast while ensuring that enterprise-grade analytics are eventually consistent and accurate. This approach also allows us to backfill statistics for historical sessions that were captured before these metrics were introduced.

## Current Limitations and Feedback

While this significantly improves data reliability, there are a few considerations:

- **Processing Delay**: Statistics are calculated in the background, so there may be a short delay (typically a few minutes) before they appear in your dashboard after a session is first captured.
- **Tokenization Variance**: While `cl100k_base` is an excellent general-purpose tokenizer, there may be a 5-15% variance compared to the exact counts reported by non-OpenAI providers like Anthropic.
- **Cost Derivation**: At this stage, we are focusing on providing accurate raw primitives (tokens, lines, tools). We haven't yet introduced direct cost estimation in USD, as provider pricing changes frequently.

We are closely monitoring the performance of this background task as session volumes increase. If you notice any discrepancies in your usage metrics or have specific statistics you'd like to see tracked, please reach out with your feedback.

Share[Share on X](https://x.com/intent/post?url=https%3A%2F%2Freclaimllm.com%2Fnews%2Fsession-statistics-via-background-task&text=Reliable%20Session%20Statistics%20via%20Background%20Task) [Share on LinkedIn](https://www.linkedin.com/sharing/share-offsite/?url=https%3A%2F%2Freclaimllm.com%2Fnews%2Fsession-statistics-via-background-task) [Share on Facebook](https://www.facebook.com/sharer/sharer.php?u=https%3A%2F%2Freclaimllm.com%2Fnews%2Fsession-statistics-via-background-task)