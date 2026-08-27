# Source: https://reclaimllm.com/news/cursor-capture-now-uses-transcripts-as-the-source-of-truth

[← Back to News](https://reclaimllm.com/news)

feature

May 6, 2026

# Cursor capture now uses transcripts as the source of truth

RCLM now captures Cursor sessions by reading the Cursor transcript on completion, while using hook events for file edits, lifecycle data, and fallback coverage.

RCLM now has first-class Cursor capture that treats Cursor's transcript file as the primary source for complete session data. When a Cursor session ends, RCLM reads the transcript path provided by Cursor and uploads a normalized session with messages, tool calls, file diffs, model metadata, working directory, timing, and transcript path.

This matters because Cursor hook stop events can arrive with little or no message content. In those cases, relying only on live hook payloads creates incomplete records. The transcript contains the richer conversation state, so RCLM now uses that file first and keeps hook events as supporting data.

## How it works

Cursor hooks are installed through the same hook installer used for the other supported coding tools. Cursor uses a `.cursor/hooks.json` file with a flat event-to-command structure, so RCLM now writes Cursor hook commands using Cursor's hook schema instead of the Claude/Codex nested hook shape.

At a workflow level:

```bash
rclm-hooks-install --cursor
```

or, if you install all supported local hooks:

```bash
rclm-hooks-install
```

The installer registers the documented Cursor hook events, including prompt, tool, shell, MCP, file edit, Tab edit, session, and compaction hooks. Those hook events let RCLM observe what Cursor is doing during the session, but final ingestion does not depend on every event carrying a full conversation payload.

On session completion, RCLM looks for a transcript path from Cursor's stop or session-end payload. If that path exists, RCLM parses the Cursor JSONL transcript and builds the normalized session record from it.

The captured session is shaped like other RCLM hook sessions:

- user and assistant messages
- tool calls
- file diffs
- working directory
- model name when available
- start and end timing
- transcript path
- token counts when Cursor transcript data includes them

File edits get special handling. Cursor exposes file edit hooks for both agent edits and Tab edits. RCLM captures those edits and converts them into file diffs, then merges them with transcript-derived diffs. That gives better coverage for cases where the transcript and hook payload each contain part of the picture.

Cursor historical sync uses the same transcript parser. That keeps live capture and backfill behavior aligned, instead of maintaining separate normalization logic for the two paths.

## Why transcript-first

The initial question was whether Cursor sessions should be built only from hook events, only from transcripts, or from both.

Hook-only capture is attractive because it gives immediate event-level visibility. The problem is that Cursor's hook payloads are not yet proven to be stable or complete enough to reconstruct every session. Some observed stop events provide no messages and only point to a local transcript file.

Transcript-only capture is cleaner, but it would lose useful live event context. Hook events still matter for file edits, lifecycle timing, debugging payload shape changes, and fallback behavior when a transcript is missing or unreadable.

The chosen approach is hybrid but transcript-first:

1. Install the Cursor hooks.
2. Record useful hook data during the session.
3. Use the transcript path at completion as the primary source of normalized session content.
4. Fall back to accumulated hook events only when transcript parsing cannot produce data.

This follows the pattern already used for richer hook integrations like Claude and Codex: the transcript is the source of truth, and hook events provide lifecycle and supplemental data.

## Current limitations

Cursor support is still being validated against real-world Cursor payloads. The main thing we are watching is whether Cursor changes transcript structure, hook field names, or the location convention for agent transcripts.

There is also an operational edge case: if multiple Cursor hook commands are installed in `.cursor/hooks.json`, Cursor may call more than one RCLM binary for the same event. That can create duplicate ingests or inconsistent records if different installed versions are present. If you see duplicate Cursor sessions, check the Cursor hook config and remove stale hook command entries.

For now, temporary hook payload logging may be used during debugging to understand exactly what Cursor sends for each event. That logging should become configurable or be removed once the event shapes are confirmed.

Feedback is useful here. If a Cursor session uploads with missing messages, missing file diffs, or an unexpected model name, the most useful detail is the shape of the stop/session-end payload and whether the transcript path exists locally.

Share[Share on X](https://x.com/intent/post?url=https%3A%2F%2Freclaimllm.com%2Fnews%2Fcursor-capture-now-uses-transcripts-as-the-source-of-truth&text=Cursor%20capture%20now%20uses%20transcripts%20as%20the%20source%20of%20truth) [Share on LinkedIn](https://www.linkedin.com/sharing/share-offsite/?url=https%3A%2F%2Freclaimllm.com%2Fnews%2Fcursor-capture-now-uses-transcripts-as-the-source-of-truth) [Share on Facebook](https://www.facebook.com/sharer/sharer.php?u=https%3A%2F%2Freclaimllm.com%2Fnews%2Fcursor-capture-now-uses-transcripts-as-the-source-of-truth)