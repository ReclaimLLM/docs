# Source: https://reclaimllm.com/features/search-replay

Feature

# Search and replay 
every AI session.

Find old solutions fast, reopen full conversations, and inspect the exact steps an agent took. Your AI history stops being disposable once it becomes searchable.

[Start building your history→](https://reclaimllm.com/register) [See capture methods](https://reclaimllm.com/features/capture)

Search layer

## Built for retrieval, not just storage.

⌕

### Full-text search across providers

Search your ChatGPT, Claude, Gemini, proxy, and coding-agent sessions from one place instead of remembering which tool you used last time.

◈

### Replay full execution traces

See the whole session, not just the final answer: prompts, responses, tool calls, results, and file changes when the source supports them.

◇

### AI-generated organization

Sessions are titled and described automatically, with tags and other metadata that make browsing easier as your history grows.

◌

### Usage context alongside content

Search by topic, then inspect the model, time, token usage, and cost context around the session you are reopening.

Replay view

## Reopen the session with its full context.

Search is only useful if opening a result gives you enough detail to act on it. RCLM keeps the session detail view rich enough to answer, "What actually happened here?"

That matters most for coding and debugging workflows, where the missing detail is usually not the final answer. It is the path the agent took to get there.

TranscriptRead the full back-and-forth, not just the remembered summary

Tool callsInspect what the agent actually invoked and what came back

File diffsReview before-and-after changes for hooks-captured coding sessions

Session metadataModel, provider, timestamps, tags, tokens, and cost

Sensitive flagsSee which sessions need extra review before sharing or exporting

Export pathTurn found sessions into datasets, archives, or further analysis

Why it matters

## Retrieval is the practical value users feel every week.

### Recover a solution you already found

The practical win is simple: stop re-solving the same problem because a provider-specific chat history buried the answer.

### Understand why an agent went wrong

When a coding session goes sideways, replay shows the sequence of prompts, tool usage, and file changes that led there.

### Trace repeated patterns in your work

Search history turns AI usage from disposable chat into a reusable body of work you can inspect over time.

Workflow

## From remembered problem to reusable session.

01

### Find

Search by phrase, topic, provider, model, date, or other metadata.

02

### Open

Jump into the session detail view and inspect the exact conversation context.

03

### Replay

Review transcripts, tool activity, and file diffs to understand what happened.

04

### Reuse

Export, reference, refine, or add the session to a dataset or workflow.

MCP recall

## Bring search into your coding assistant.

The ReclaimLLM MCP server lets tools like Claude, Gemini, Cursor, and Codex ask your ReclaimLLM backend for prior sessions without leaving the current workflow.

It is intentionally conservative: search returns short titles and highlights first. A session is only summarized into the current conversation when you explicitly ask to use it.

`rclm-hooks-install --with-mcp`

01

### Install

Run rclm-hooks-install --with-mcp so capture hooks and local MCP recall use the same account configuration.

02

### Ask

In a coding assistant, ask whether similar prior work exists or request history for a specific file or folder.

03

### Choose

Review the returned titles and highlights, then decide whether any session is worth opening or summarizing.

search\_sessions

Find prior bug fixes, performance work, or similar tasks with optional project and file filters

search\_by\_filename

Find sessions that touched a file or folder when the question is about file history

get\_session

Open a specific session summary and link after you provide a session ID

summarize\_session

Turn a selected session into reusable context only when you ask for it

Outcomes

## Better memory, better debugging, better reuse.

Search and replay is where RCLM stops feeling like a storage product and starts feeling like infrastructure for real work.

Once sessions are easy to recover and inspect, they become reusable knowledge instead of expired chat history.

For individual usersA personal memory layer for every AI problem you already solved

For coding workflowsA debugging trail for sessions that changed files or used tools

For teamsA shared record that supports audits, reviews, and cost visibility

For data creationA clean path from useful session to exportable training artifact

## Make past AI work easy to find again.

Capture first, then search and replay every useful session instead of starting from scratch each time.

[Get started free →](https://reclaimllm.com/register) [Back to capture](https://reclaimllm.com/features/capture)