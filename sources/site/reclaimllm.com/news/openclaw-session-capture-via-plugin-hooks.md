# Source: https://reclaimllm.com/news/openclaw-session-capture-via-plugin-hooks

[← Back to News](https://reclaimllm.com/news)

feature

Apr 27, 2026

# OpenClaw session capture via plugin hooks

RCLM now captures OpenClaw sessions through a lightweight plugin that forwards session, LLM, and tool events into the existing hook ingestion pipeline.

RCLM now supports OpenClaw session capture through plugin hooks. OpenClaw sessions can be recorded alongside Claude Code, Gemini CLI, and Codex CLI sessions, using the same RCLM ingestion path for messages, tool calls, session timing, and upload behavior.

The integration is capture-only in its first version. It observes OpenClaw activity, accumulates the relevant events locally, and uploads the completed session when OpenClaw reports the session has ended. It does not try to control or block OpenClaw behavior.

## How it works

OpenClaw exposes more than one hook surface. RCLM uses OpenClaw plugin hooks instead of the lower-level internal command automation hooks.

At a workflow level, users install RCLM hooks as usual:

```bash
rclm-hooks-install --openclaw
```

The installer creates a small OpenClaw plugin under the user's OpenClaw extensions directory and patches OpenClaw configuration so the plugin is loaded. The plugin is a TypeScript shim. Its job is intentionally narrow: listen for selected OpenClaw plugin events and forward them to the Python hook handler.

The Python side is exposed through:

```bash
rclm-openclaw-hooks
```

That keeps OpenClaw capture inside the same Python infrastructure used by the rest of RCLM:

- saved RCLM configuration
- local session event storage
- upload retry behavior
- redaction-on-upload
- shared session record shape
- existing hook tests and parser patterns

The plugin forwards selected event types that are useful for reconstructing an AI coding session:

- `session_start`
- `session_end`
- `llm_input`
- `llm_output`
- `before_tool_call`
- `after_tool_call`

RCLM stores those events during the session. At `session_end`, it builds one session record and uploads it. That avoids uploading on every event and keeps the first version simple.

Captured OpenClaw sessions follow the same general shape as other RCLM hook sessions:

- user messages
- assistant messages
- tool calls
- tool results
- working directory when available
- model name when available
- start and end timing
- session duration

This makes OpenClaw sessions usable in the same dashboard and downstream workflows as other captured tools.

## Why plugin hooks

The main decision was whether to integrate through OpenClaw's internal hooks, plugin hooks, or both.

Internal hooks are useful for automation-style behavior, but they are too shallow for reliable session capture. RCLM needs lifecycle events, LLM input and output, and tool-call data. Plugin hooks expose the right level of context for that.

Building a TypeScript-only uploader inside the OpenClaw plugin was rejected. It would duplicate logic RCLM already has in Python: upload behavior, config loading, redaction, session state, and tests. A small TypeScript forwarding plugin is easier to reason about and keeps the capture implementation in one place.

Implementing both hook paths immediately was also rejected. It would add complexity before real payload samples prove that the second path is needed.

The resulting architecture is additive. It does not require refactoring Claude, Gemini, or Codex capture, and it lets OpenClaw support evolve as payload shapes become clearer.

## Current limitations

OpenClaw support is still based on selected plugin events. If OpenClaw changes its plugin API or event payload structure, RCLM may need parser updates.

The installer also has to modify OpenClaw's local plugin configuration. If a config file is not strict JSON, RCLM cannot safely patch it automatically. In that case, manual setup may be needed.

There is also a performance tradeoff. The plugin forwards events to a Python process. To keep overhead low, the first version forwards only selected lifecycle, LLM, and tool events, then uploads once at the end of the session. If process-spawn overhead becomes noticeable in real usage, the integration may need to move toward a long-running local daemon.

Feedback is most useful around payload fidelity: missing messages, missing tool results, incorrect working directories, or model names that do not match what OpenClaw showed during the session.

Share[Share on X](https://x.com/intent/post?url=https%3A%2F%2Freclaimllm.com%2Fnews%2Fopenclaw-session-capture-via-plugin-hooks&text=OpenClaw%20session%20capture%20via%20plugin%20hooks) [Share on LinkedIn](https://www.linkedin.com/sharing/share-offsite/?url=https%3A%2F%2Freclaimllm.com%2Fnews%2Fopenclaw-session-capture-via-plugin-hooks) [Share on Facebook](https://www.facebook.com/sharer/sharer.php?u=https%3A%2F%2Freclaimllm.com%2Fnews%2Fopenclaw-session-capture-via-plugin-hooks)