# Source: https://reclaimllm.com/features/model-switching

Feature

# Switch models 
without losing the thread.

Start a task in Claude Code, continue it in Gemini CLI, hand it to Codex CLI, or move it into a different model. RCLM turns captured sessions into target-specific Markdown context so the next assistant starts with the state that matters.

[Install hooks→](https://reclaimllm.com/install) [See capture methods](https://reclaimllm.com/features/capture)

Workflow

## One captured session, many possible next models.

01

Capture

### Start in any supported tool

RCLM hooks capture the original coding session from Claude Code, Gemini CLI, or Codex CLI: messages, tool calls, file diffs, and session metadata.

02

Export

### Generate a continuation brief

Use the dashboard or \`rclm convert-session <session\_id> <target\_tool>\` to produce a Markdown context document for the tool you want to switch into.

03

Continue

### Open the next model with context

Paste or save the generated context as the first instruction file for the next assistant. The new model starts with decisions, pending work, and relevant file changes.

Export targets

## Context shaped for the next assistant.

RCLM does not try to reverse-engineer fragile private transcript formats. It creates a readable continuation document that captures what happened, what changed, and what still needs attention.

That makes the handoff stable across tool updates and useful even when you move into a model or interface RCLM has never seen before.

Claude CodeCLAUDE.md-ready context

Gemini CLI.gemini context document

Codex CLIAGENTS.md-ready handoff

Generic toolsPlain Markdown continuation brief

Why Markdown

## Format compatibility should not decide where you work next.

### No vendor lock-in

Your session history is normalized by RCLM, so the handoff is not tied to one provider's private transcript format.

### Fast by default

Annotated sessions reuse existing titles, summaries, insights, and file-diff summaries, so most exports avoid a fresh LLM call.

### Regenerate when needed

For difficult handoffs, force regeneration to produce continuation-specific framing around pending work and decisions.

### Plain Markdown survives tool changes

The output is readable, editable, and usable by any assistant, even when CLI transcript formats change.

Honest boundary

## It continues the work, not the private vendor session.

The next tool starts a new session with high-quality context. That is more reliable than trying to place synthetic transcript files into vendor-owned state directories.

What it isA context handoff that lets another model continue with the important state.

What it is notNative transcript resume inside another vendor's private session store.

Common pathInstant export from existing annotations when summaries and diff context already exist.

Long-session controlDiff line limits and file caps keep generated context inside target model windows.

## Use the best model for the next step.

Capture the original session, export context, and continue in the assistant that fits the work in front of you.

[Set up capture →](https://reclaimllm.com/install) [Search and replay sessions](https://reclaimllm.com/features/search-replay)