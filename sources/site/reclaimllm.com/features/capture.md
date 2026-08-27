# Source: https://reclaimllm.com/features/capture

Feature

# Capture AI interactions 
wherever they happen.

RCLM captures sessions from local API traffic, enterprise gateway traffic, coding agents, and browser chats. Start with one method, or combine them into a single searchable history. When session encryption is enabled, detailed session blobs are encrypted while metadata remains available for fast product workflows.

[View install guide→](https://reclaimllm.com/install) [Create free account](https://reclaimllm.com/register)

Capture methods

## Four entry points, one system.

Each capture path fits a different workflow, but they all feed the same RCLM account and the same browsing experience.

01rclm-proxy

### API Proxy (local)

Run rclm-proxy on your own machine.

Run the local rclm-proxy between your app and providers like Anthropic, OpenAI, Azure OpenAI, and Gemini. Point your requests at a local base URL and every request and response is captured automatically.

- —No application code changes beyond endpoint configuration
- —Runs locally for individual developers and local services
- —Captures every request, response, model, and token count
- —Best fit when you control the app runtime and provider keys

02rclm-hooks

### Native Hooks

The richest capture path for coding workflows.

Install directly into Claude Code and Gemini CLI so RCLM records the full session: messages, tool calls, results, token usage, and every file the agent created or changed.

- —Captures structured agent lifecycle events
- —Includes paired tool inputs and outputs
- —Records before-and-after file diffs for changed files
- —Best fit for debugging and replaying AI-assisted coding work

03rclm-browser-ext

### Browser Extension

Capture web chat sessions with no API setup.

Use the Chrome extension to capture conversations from ChatGPT, Claude.ai, and Gemini in the background while you continue using your normal accounts and workflows.

- —Works with existing browser-based AI accounts
- —No API keys or provider reconfiguration required
- —Runs passively once installed and signed in
- —Makes casual browser usage searchable alongside everything else

04enterprise

### Enterprise LLM Gateway

Governed proxy capture for organization traffic.

Enterprise admins can issue team-scoped gateway keys, restrict providers or models, and route production or team API traffic through a hosted gateway. Calls are logged under the correct organization and team.

- —Enterprise-only capture path for governed LLM API access
- —Uses org-managed provider credentials instead of developer-owned keys
- —Supports team-scoped access by provider and model policy
- —Lets admins and team leads review gateway traffic centrally

[See how it works →](https://reclaimllm.com/features/enterprise-llm-gateway)

Normalized record

## Different sources, same session model.

The important design choice is not just capturing data. It is capturing it into a consistent shape. That is what lets browser chats, local API traces, enterprise gateway calls, and coding-agent sessions live in one dashboard instead of feeling like separate products.

Richer sources contribute richer data, but every capture method still becomes a session you can search, inspect, and export. The detailed blob and the metadata are intentionally separated: encrypted blobs protect raw content, while unencrypted metadata keeps search, summaries, stats, and filters responsive.

MessagesPrompts, replies, and conversation turns

Provider contextProvider, model, timestamps, token usage, and cost metadata

Tool activityTool calls and results when the source supports them

File diffsBefore/after snapshots for coding-agent changes via hooks

Session metadataTitles, descriptions, tags, and searchable indexing

Storage boundaryFull session blobs are encrypted when enabled; metadata remains unencrypted for search, stats, and summaries

Sensitive content flagsCredentials, tokens, and other risky content highlighted for review

Why capture matters

## Capture is the prerequisite for everything else.

### One history across every interface

Most people use AI in fragments: browser tabs, coding agents, local API calls, and enterprise gateway traffic. Capture unifies those disconnected traces into one searchable account instead of separate silos.

### The source determines the richness

Not every capture path can see the same data. Browser sessions capture conversation content. Hooks capture the full agent execution trace. The UI stays consistent while the data gets richer where the source allows it.

### You can start narrow and expand later

Use only the extension if that is where you work today. Add the local proxy when you build with APIs. Add hooks when you want coding-session diffs. Add the enterprise gateway when org-level access control matters.

Workflow

## From raw interaction to usable history.

01

### Capture

RCLM intercepts or receives the session from the local proxy, enterprise gateway, hook, or extension.

02

### Normalize

Different raw sources are shaped into a common session record so they can be browsed together.

03

### Store

Metadata is indexed for search, stats, and summaries. Full session blobs are stored separately and encrypted when session encryption is enabled.

04

### Use

Search, replay, analyze, export, reuse as context, or govern the session in an enterprise workspace.

Where to start

## Pick the capture path that matches how you already work.

RCLM does not require a wholesale workflow change. The right first step is usually the one that captures your highest-volume AI usage with the least friction.

Once that is in place, you can layer on the other sources and keep building a more complete record over time.

You build with APIsStart with local rclm-proxy

You manage LLM access for a companyStart with the Enterprise LLM Gateway

You use Claude Code or Gemini CLI dailyStart with Native Hooks

You mostly use ChatGPT, Claude.ai, or Gemini in the browserStart with the Browser Extension

You want the fullest pictureUse all available capture paths together

## Start capturing in the interface you already use.

Install the proxy, hooks, or extension now, then bring the rest online when you need a fuller picture. Enterprise teams can add the hosted gateway when they need org-level control over API traffic.

[See installation steps →](https://reclaimllm.com/install) [Read the full product overview](https://reclaimllm.com/about)