# Source: https://reclaimllm.com/news/context-compression-and-session-analytics

[← Back to News](https://reclaimllm.com/news)

update

Mar 15, 2026

# Context Compression and Session Analytics

Stay within your LLM's context window and gain deeper insights into your sessions with our new hook-level compression and usage analytics.

## Overview

One of the biggest challenges in AI-driven development is managing the LLM's context window. Verbose tool outputs—like large `git status` reports, extensive file reads, or massive `grep` results—can quickly consume your context and lead to increased costs and slower responses. We’re excited to introduce **Hook-Level Context Compression**, a powerful new feature that automatically trims redundant information before it ever reaches your AI model.

## How it Works

The new compression engine is built directly into our Python hooks and is available for Claude Code, Gemini CLI, and Codex CLI. When you enable compression with the `--compress` flag, RCLM applies several smart strategies:

1. **Smart File Reading**: For files longer than 500 lines, RCLM automatically injects a read limit, ensuring the AI only sees the most relevant parts of the file unless more is explicitly requested.
2. **Grep Result Capping**: Large search results are automatically capped at a sensible default (like 50 lines), preventing thousands of lines of output from overwhelming the model.
3. **Bash Command Rewriting**: Commands like `git status`, `pytest`, and `ls` are transparently routed through our new `reclaimllm-compress` utility. This utility filters the output to preserve key information while stripping away the fluff, leading to an estimated **70-90% token savings** on common commands.
4. **Integrated Analytics**: Every compressed session now includes detailed analytics. You can track tool call counts, identify the dominant tools in your workflow, and see exactly how many tokens you've saved through compression directly in your RCLM dashboard.

## Why We Made This Change

Previously, RCLM captured everything verbatim. While this was great for record-keeping, it meant the AI model was often processing hundreds of tokens of "noise" that didn't help with the task at hand. By building compression into the capture layer, we help you stay within context limits and significantly reduce your inference costs. We also wanted to give you better visibility into how you're using AI tools, which is why we've added new DB-backed analytics columns for every session.

## Current Limitations and Feedback

Compression is a powerful tool, but it's important to understand its current behavior:

- **Explicit Opt-In**: Compression is off by default to ensure you have full control over your tool behavior. You must explicitly enable it using the `--compress` flag during hook installation or in your config.
- **Subprocess Overhead**: Because compression happens in Python, there is a very slight (sub-millisecond) overhead when running rewritten bash commands.
- **Heuristic Parsing**: Our filters for `git`, `test`, and `shell` outputs use smart heuristics. While highly effective, they may occasionally miss edge cases as these tools update their output formats.
- **Token Estimation**: Savings are currently calculated using a 4-character-per-token heuristic. This is excellent for analytics but should be treated as an approximation for billing purposes.

We’re constantly refining our compression filters to cover more tools and scenarios. If you have a specific command that's eating up your context window, let us know and we'll look into adding a filter for it!

Share[Share on X](https://x.com/intent/post?url=https%3A%2F%2Freclaimllm.com%2Fnews%2Fcontext-compression-and-session-analytics&text=Context%20Compression%20and%20Session%20Analytics) [Share on LinkedIn](https://www.linkedin.com/sharing/share-offsite/?url=https%3A%2F%2Freclaimllm.com%2Fnews%2Fcontext-compression-and-session-analytics) [Share on Facebook](https://www.facebook.com/sharer/sharer.php?u=https%3A%2F%2Freclaimllm.com%2Fnews%2Fcontext-compression-and-session-analytics)