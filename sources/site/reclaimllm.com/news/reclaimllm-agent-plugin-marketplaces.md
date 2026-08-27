# Source: https://reclaimllm.com/news/reclaimllm-agent-plugin-marketplaces

[← Back to News](https://reclaimllm.com/news)

feature

Jun 10, 2026

# ReclaimLLM Agent Plugin Marketplaces

ReclaimLLM now ships local plugin marketplace entries for Codex, Claude, and Cursor, backed by the existing rclm MCP server.

ReclaimLLM now ships local plugin marketplace entries for Codex, Claude, and Cursor. These plugins expose captured ReclaimLLM sessions as persistent memory for AI agents, using the same local MCP server that already powers session search and context export.

The goal is simple: if an agent can use MCP and supports a plugin marketplace, it should be able to search your prior captured sessions without a custom integration. That includes arbitrary memory searches such as prior implementation decisions, file history, debugging sessions, browser conversations, and proxy-captured interactions.

## How it works

The plugin package lives with the RCLM data-capture package, because that package owns the local MCP server and installer. The plugin folder contains host-specific manifests for Codex, Claude, and Cursor, plus shared skill guidance for when and how agents should search memory.

Each marketplace entry points at the same ReclaimLLM plugin package. The plugin then starts the local MCP server, which exposes the existing ReclaimLLM memory tools:

| Tool | Purpose | |------|---------| | `search_sessions` | Semantic search across captured sessions. | | `search_by_filename` | Find prior sessions that touched a file or folder. | | `list_projects` | Discover project filters. | | `get_session` | Retrieve metadata and a link for a known session. | | `summarize_session` | Export a reusable Markdown context document for a known session. |

The important design choice is that the plugin does not reimplement memory search. The MCP server still calls the ReclaimLLM backend APIs, so search ranking, project filtering, session metadata, and context export stay in one place.

That means the plugin behaves like a distribution layer over the existing memory system, not a second memory backend.

## Using it locally

Install and authenticate RCLM first:

```bash
pip install rclm
rclm-hooks-install --with-mcp
```

Then add the marketplace for your agent.

For Codex, add the `DC-hooks-proxy` package directory as a marketplace source and install the `reclaimllm` plugin from it:

```bash
codex plugin marketplace add /path/to/DC-hooks-proxy
codex plugin add reclaimllm@reclaimllm-plugins
```

For Claude and Cursor, use the matching marketplace files in the repository:

```text
DC-hooks-proxy/.claude-plugin/marketplace.json
DC-hooks-proxy/.cursor-plugin/marketplace.json
```

After installing, start a new agent thread and confirm that both the `reclaimllm` plugin and the `reclaimllm` MCP server are enabled.

Once enabled, prompts like these should route through ReclaimLLM memory:

```text
Search my ReclaimLLM memory for prior work on enterprise proxy deployment.
```

```text
Find sessions that touched rclm/mcp_server.py.
```

```text
Use this ReclaimLLM session as context.
```

## Why this is packaged this way

ReclaimLLM already had a local MCP server for recall. It also already had hooks and installers for capturing sessions from coding agents and other AI tools. The missing piece was a shareable plugin shape that lets agents discover and load that memory layer through their own plugin systems.

We chose to keep the plugin inside `DC-hooks-proxy` rather than creating a separate plugin repository. That keeps the plugin metadata, MCP command, package dependencies, installer, and tests under the same version boundary.

This also avoids a common failure mode: shipping a plugin that describes a tool but is disconnected from the executable that actually runs it.

## Current limitations

The plugin still requires the local `rclm` package to be installed and authenticated. If the MCP server command is not available to the agent process, the plugin may install but fail to start.

Codex, Claude, and Cursor also use different marketplace and plugin manifest shapes, so ReclaimLLM keeps separate host metadata files. That creates some duplication, but it keeps each host integration explicit and easier to debug.

Gemini is not included in the plugin marketplace packaging yet. Gemini can still use the same ReclaimLLM MCP server through the existing MCP installer path.

We are watching for schema changes in agent plugin systems, better shared marketplace conventions, and safer read-scoped credentials for memory retrieval. If you try the plugin in a local or team setup and hit an edge case, send the exact host, install path, and MCP startup error so we can tighten the integration.

Share[Share on X](https://x.com/intent/post?url=https%3A%2F%2Freclaimllm.com%2Fnews%2Freclaimllm-agent-plugin-marketplaces&text=ReclaimLLM%20Agent%20Plugin%20Marketplaces) [Share on LinkedIn](https://www.linkedin.com/sharing/share-offsite/?url=https%3A%2F%2Freclaimllm.com%2Fnews%2Freclaimllm-agent-plugin-marketplaces) [Share on Facebook](https://www.facebook.com/sharer/sharer.php?u=https%3A%2F%2Freclaimllm.com%2Fnews%2Freclaimllm-agent-plugin-marketplaces)