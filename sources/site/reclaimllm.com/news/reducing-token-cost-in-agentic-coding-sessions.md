# Source: https://reclaimllm.com/news/reducing-token-cost-in-agentic-coding-sessions

[← Back to News](https://reclaimllm.com/news)

feature

Jul 15, 2026

# Reducing token cost in agentic coding sessions

New opt-in hooks cut redundant tokens in captured sessions, cache-token measurement is now accurate, and a shadow mode lets you see estimated savings before anything is enforced.

Every tool result in an agentic coding session gets re-sent to the model on every subsequent turn. A file read at turn 5 of a 200-turn session doesn't cost what it looks like once — it costs what it looks like times roughly 195. That resend multiplier, more than any single large tool call, is where most avoidable token spend in a coding session actually comes from.

RCLM now measures that directly and ships a set of opt-in hooks that reduce it, along with a project-level dashboard to see where the tokens are going.

## What's new

A handful of independent, opt-in mechanisms run at the hook layer — the same layer RCLM already uses to capture sessions and to redact secrets before they reach the model:

- **Read cache with diff-on-change.** Re-reading a file you already read earlier in the session gets replaced with either a short "unchanged" notice or a diff against what you saw last, instead of the full content again. Covers both the native Read tool and shell reads (`cat`, `sed`, `Get-Content`, etc).
- **Search-result shaping.** Broad greps are a large share of avoidable tokens in most sessions we looked at. Search results are shaped to files-plus-match-counts first; full match content is still one call away if you actually need it.
- **Exec-output compaction.** ANSI codes stripped, repeated lines (progress bars, duplicate log lines) collapsed, and long output capped to head+tail.
- **Loop breaker.** Repeated identical tool calls or repeated failures on the same file get flagged, and — past a threshold — the agent is asked before it's allowed to keep retrying blindly.
- **Session-start context pack and handoff.** New sessions can be seeded with highlights from recent RCLM sessions in the same project. A `handoff` MCP tool packages the current session's state into a document you can paste into a fresh session — useful once a session has grown large enough that the resend multiplier itself is the problem, not any individual tool call.
- **`file_brief` MCP tool.** A distilled summary of prior sessions that touched a given file, for orientation before an edit, instead of a full read.

Every mechanism is off by default and enabled independently — there's no bundle, and enabling one doesn't enable the others.

## Shadow mode

We don't think you should have to trust a savings estimate you can't see. A new `shadow_mode` setting makes every enabled mechanism run its detection and measurement as normal, but skip the actual rewrite — you get the estimated tokens saved recorded against the session without anything about your agent's behavior changing. It's the way we'd want to evaluate a change like this ourselves before turning it on for real.

## Honest measurement

Separately from the mechanisms above, we found we were undercounting real usage. Per-message cache-read and cache-creation token counts were being dropped during capture, and the org dashboard had no per-project breakdown — only raw totals. Both are fixed: cache tokens are now captured directly from provider usage data, and each session is tagged with whether its numbers come from real provider-reported usage or an older modeled estimate.

## Where to find this

The project-level token view is in the enterprise dashboard: **Enterprise → your org → Tokens**, next to the existing Usage tab. It breaks down token volume by project, team, user, or model, with the usage-source badge mentioned above, and a few early efficiency ratios (tokens per session, tokens per line changed, cache-hit ratio) shown against your org's median. It needs real session data to populate — a freshly created org will show an empty state until sessions have been captured and the usage rollup has run.

The reduction mechanisms and shadow mode aren't in the dashboard or a settings page yet — they're enabled per install via the same CLI installer that sets up hooks, for example:

```
rclm-hooks-install --read-cache --loop-breaker --compress --shadow-mode
```

Each flag is independent, so turn on only what you want to try. There's no web UI toggle for any of this today; it's local configuration, same as the existing `--dlp` flag.

## What's not here yet

This is deliberately scoped. A few things we're aware of and not pretending are done:

- Savings telemetry is per-session right now, not per-tool-call. We can tell you a session saved roughly N tokens across which mechanisms, not which specific tool call it was. Per-call attribution is a larger change we're holding off on until we're sure it's worth the complexity.
- `shadow_mode` is a single switch — you can't shadow-test one mechanism while enforcing another yet.
- The dashboard doesn't yet show tokens-per-turn or a repeat-read ratio; both need data we haven't wired up.
- There's no automated recommendation engine yet — no "here's what to enable and why." That's next, once we've seen enough real telemetry from shadow mode to know it's trustworthy.

If you turn any of this on and something looks wrong — a diff that doesn't make sense, a search result that's too aggressively trimmed, a loop-breaker warning that fires when it shouldn't — we want to hear about it. This is exactly the kind of feature where a wrong compression costs more than it saves, and we'd rather find that out from real usage than assume we got the tradeoffs right on the first pass.

Share[Share on X](https://x.com/intent/post?url=https%3A%2F%2Freclaimllm.com%2Fnews%2Freducing-token-cost-in-agentic-coding-sessions&text=Reducing%20token%20cost%20in%20agentic%20coding%20sessions) [Share on LinkedIn](https://www.linkedin.com/sharing/share-offsite/?url=https%3A%2F%2Freclaimllm.com%2Fnews%2Freducing-token-cost-in-agentic-coding-sessions) [Share on Facebook](https://www.facebook.com/sharer/sharer.php?u=https%3A%2F%2Freclaimllm.com%2Fnews%2Freducing-token-cost-in-agentic-coding-sessions)