# Source: https://reclaimllm.com/news/mcp-server-installation-hooks

[← Back to News](https://reclaimllm.com/news)

feature

May 7, 2026

# Unified Recall and Capture: Introducing the ReclaimLLM MCP Server

Search and retrieve prior sessions directly from your AI coding tools with a single installation flag.

We have integrated a local Model Context Protocol (MCP) server into the core ReclaimLLM package. This update allows AI coding assistants like Claude, Cursor, and Codex to natively search, retrieve, and summarize your previously captured sessions, providing them with the deep background context necessary for complex engineering tasks.

By bridging the gap between automated capture and active recall, your AI tools can now leverage the full history of your technical decisions and research without manual copy-pasting.

## How it works

The MCP server is included in the standard `rclm` distribution as a new entry point. We’ve updated our installation and update workflows to make activation as simple as adding a single flag.

For new installations:

```bash
rclm-hooks-install --with-mcp
```

For existing users:

```bash
rclm-update --with-mcp
```

Once activated, the installer automatically registers the ReclaimLLM MCP server with your supported local coding tools. The server provides three primary tools to your assistant: `search_sessions`, `get_session`, and `summarize_session`.

## Workflow and scenarios

Adding these tools to your assistant changes the nature of how you interact with your codebase. Instead of trying to remember exactly how a feature was implemented or where a specific bug was discussed, you can simply ask your assistant to find it.

**Debugging with history** 
If you’re facing a regression, you can ask: _"I saw a similar error in a session last Tuesday when I was working on the API. Can you find that session and tell me how I fixed it then?"_ The assistant can use `search_sessions` to find the relevant capture and `get_session` to analyze the previous solution.

**Accelerating research** 
When starting a new feature, you might say: _"Search my history for when I was researching the Stripe integration last month. Summarize the trade-offs we discussed regarding webhooks."_ Using `summarize_session`, the assistant can extract the core logic from a long research thread without blowing through your token budget.

**Onboarding and context-switching** 
If you're returning to a project after a few weeks, the assistant can help you catch up: _"Show me the last three sessions related to the 'auth-refactor' project."_ This allows for a seamless transition back into a complex mental model.

## Unified recall and capture

Historically, setting up an MCP server required independent configuration, often leading to credential drift and installation friction. By integrating the MCP server directly into the capture hook installation path, we’ve unified the lifecycle of your technical context.

This implementation routes all search and retrieval requests through the ReclaimLLM backend. This ensures that the AI assistant benefits from the same high-performance ranking and hybrid search logic used in the web interface, rather than relying on a simplified local implementation.

## Current limitations and feedback

While this unified approach simplifies setup, there are a few considerations for this initial release:

- **Client Restarts**: Most AI coding tools require a full restart or a manual "Reload" of the MCP configuration to recognize the new server after installation.
- **API Key Scope**: The local API key used for capturing hooks now also grants read access to your session history for the MCP server. We are evaluating scoped keys for future updates.
- **Host Support**: We currently support automatic configuration for Claude Desktop, Cursor, and Codex.

We are looking for feedback on the quality of the session summaries and whether additional filtering tools—such as date ranges or project-specific tags—would improve your workflow.

Share[Share on X](https://x.com/intent/post?url=https%3A%2F%2Freclaimllm.com%2Fnews%2Fmcp-server-installation-hooks&text=Unified%20Recall%20and%20Capture%3A%20Introducing%20the%20ReclaimLLM%20MCP%20Server) [Share on LinkedIn](https://www.linkedin.com/sharing/share-offsite/?url=https%3A%2F%2Freclaimllm.com%2Fnews%2Fmcp-server-installation-hooks) [Share on Facebook](https://www.facebook.com/sharer/sharer.php?u=https%3A%2F%2Freclaimllm.com%2Fnews%2Fmcp-server-installation-hooks)