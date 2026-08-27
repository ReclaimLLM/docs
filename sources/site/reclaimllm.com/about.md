# Source: https://reclaimllm.com/about

About

# Your AI work belongs to you.

Every debugging session, architecture discussion, and code review you have with an AI tool produces valuable data. Today it is scattered across tools, hard to search, and usually locked in vendor formats. RCLM captures it, stores it, and gives it back to you.

The problem

## We've seen this before.

Social media platforms built billion-dollar businesses on the content and behavior data their users generated, while users got the product for free and nothing else. We normalized giving away our data in exchange for access.

LLMs are heading down the same path, faster and with higher stakes. The interactions you have with AI tools today are exactly the kind of data that trains the next generation of models. You're contributing to that whether you know it or not.

Your own history — the regex you worked out last month, the architecture discussion that shaped a project, the debugging session you want to show a teammate — is spread across half a dozen tools and formatted in ways only vendors can easily read.

Our premise

## Own it. Search it. Govern it.

RCLM is a capture and observability layer for AI work. It records interactions across providers and tools, then gives individuals and organizations search, usage analytics, compliance controls, and an audit trail.

Sessions you capture belong to you. The immediate value is practical: find old work, understand AI-assisted decisions, audit what happened, and govern AI usage across an organization without changing individual capture tools.

Ownership is the precondition. You cannot choose to keep, move, share, or delete work you do not control. RCLM makes you the owner first.

Capture

## Capture from the tools you already use.

CLI-based coding assistants produce the richest structured data, but each capture method works independently and produces the same normalized session record.

01rclm-hooks

### Native Hooks

The richest capture: messages, tool calls, file diffs.

Hooks that plug directly into Claude Code, Gemini CLI, and Codex CLI lifecycle events. Captures full structured sessions: messages, reasoning, tool calls, unified file diffs, and precise token usage.

- —Claude Code, Gemini CLI, and Codex CLI supported
- —Paired tool inputs + results captured
- —Secret values redacted before model context
- —File diffs and per-session token analytics

02rclm-proxy

### API Proxy

Zero code changes. Every API call captured.

A local proxy powered by LiteLLM sits between your code and LLM providers. Point API calls at a different address and every request and response is recorded.

- —Works with any app — just set an env var
- —Runs locally for maximum security
- —Supports all major providers via LiteLLM
- —Async upload — never blocks requests

03rclm-browser-ext

### Browser Capture

Capture ChatGPT, Claude.ai, Gemini — no API key needed.

A limited-beta Chrome extension captures conversations from ChatGPT, Claude.ai, and Gemini in the browser. CLI capture remains the highest-fidelity path.

- —Works on ChatGPT, Claude.ai, Gemini
- —No API keys or account access required
- —Captures in the background, syncs automatically
- —Sensitive content flagged before upload

For individuals

## Immediate value from work you already did.

A permanent, searchable record of your AI interactions has practical value every day: reuse context, debug agent behavior, and carry work across tools.

⊙

### Search your history

Every conversation is searchable across providers, tools, and time. Find that architecture discussion from last month, or the exact prompt that produced a working solution. Full-text search across all your sessions.

◈

### Debug and replay securely

When an AI-assisted workflow goes wrong, the full session log shows you exactly what happened — every step, every file change, every tool the agent used. Paid and enterprise users can encrypt raw session details while keeping useful metadata searchable.

◌

### Monitor usage

See which models you use, how often, and for what kinds of tasks. For organizations, this rolls up into cross-developer analytics by org and team.

⌕

### Hybrid search

Search by meaning, exact terms, error messages, model, or a mix of all of them. RCLM fuses semantic and keyword results so retrieval matches how you actually remember work.

◇

### AI-assisted organization

Sessions are automatically titled, described, tagged, and made ready to browse from the moment they are captured — no manual labeling pass required.

Your data, your rules

## Private by default. Yours to export or delete.

Nothing is shared, sold, or used for any purpose without an explicit action from you. RCLM does not train on your data.

Export your full history in standard formats, delete individual sessions or your account, and choose region-specific storage when you need it. Credentials and tokens can be flagged or redacted before storage.

Paid users and enterprise organizations can also encrypt raw session details at storage level. Recovery keys are downloaded once, never emailed, and not stored as plaintext. Session summaries, stats, and high-level search metadata stay usable without opening full transcripts.

Default visibilityPrivate

Data used for trainingNever

ExportFull history or individual sessions, standard formats

DeletionAny session or full account, on demand

EncryptionEncrypted raw session storage on Paid and Enterprise

Recovery keysDownloaded once · never emailed · plaintext not stored

Data residencyAccount-level region selection · on-prem for enterprise

Sensitive dataFlags, optional redaction, hook-level secret protection

Controlled sharingEmail-bound, expiring links with revoke and view tracking

Enterprise

## AI coding assistants are now standard. The governance layer isn't.

The enterprise layer groups users into organizations and teams, tags sessions at ingest, gives admins a cross-developer view, and lets org admins enable encrypted raw session storage without changing the local capture tools each developer already uses.

### You don't know what's leaving your network

Developers paste code into ChatGPT, share database schemas with Claude, describe internal infrastructure to Gemini. Some is fine. Some is proprietary source code, API keys, connection strings, or customer data that was never meant to leave your systems. Without visibility, you find out about leaks after the fact — if at all.

### You don't know what it's costing you

LLM usage can balloon fast with coding agents. A misconfigured agent running overnight or a developer sending huge context on every request can create real spend risk, but most teams lack usage breakdowns by developer, team, model, or project.

### You have no observability into how AI is being used

Which models are your developers using? Are they coding, debugging, writing docs? Are some teams heavy users while others barely touch it? Is AI improving output quality or adding noise? Without data, these are guesses.

### You have no audit trail

For regulated industries or incident response, the inability to answer "what did our developers share with external AI providers over the past 90 days?" is a significant gap. Without a capture layer, that question is simply unanswerable.

What RCLM gives your organization

Org visibilityDevelopers join an org; sessions are tagged with durable org and team attribution server-side

Usage analyticsCross-developer dashboard, time-series usage, and session lists backed by materialized views refreshed every 15 minutes

Historical accessWhen a user joins an org, their existing sessions can be backfilled so the enterprise view covers past and future activity

Admin controlsMember management, teams, org API keys, retention policy, encryption settings, and SSO configuration under the enterprise portal

Access controlAdmins manage the full org; team leads see only their teams; developers do not get enterprise portal access

Retention policiesConfigurable retention windows with legal hold; a background task enforces deletions across sessions and blobs

Session encryptionOrg-wide encrypted raw session storage with one org recovery key and admin-controlled decrypt access

Auth modelEnterprise APIs use JWT/user context, role-aware org checks, and hashed org API keys for ingestion

ArchitectureAdditive FastAPI enterprise routers and nullable session columns, so existing personal capture keeps working

Current enterprise scope focuses on observability, usage analytics, admin controls, SSO configuration storage, org API keys, encrypted session storage, and retention policy enforcement.

Architecture

## How the pieces connect.

```
  ┌────────────────────────────────────────────────────────────────────┐
  │                         CAPTURE LAYER                              │
  │                                                                    │
  │  rclm-hooks      ←── Claude Code, Gemini CLI, Codex CLI hooks      │
  │  rclm-proxy      ←── local LiteLLM proxy, set env var             │
  │  rclm-browser-ext ←── limited-beta Chrome extension               │
  │  rclm convert-session ←── move context between assistants          │
  └─────────────────────────────┬──────────────────────────────────────┘
                                │
                         POST /api/ingest
                      (X-API-Key or Bearer JWT)
                                │
  ┌─────────────────────────────▼──────────────────────────────────────┐
  │                         SERVER LAYER                               │
  │                                                                    │
  │  rclm-server (FastAPI)                                             │
  │  ├── PostgreSQL  metadata, flags, hybrid search index              │
  │  └── S3 / R2    full session blobs, encrypted when enabled         │
  └─────────────────────────────┬──────────────────────────────────────┘
                                │
              ┌─────────────────┴─────────────────┐
              │                                   │
  ┌───────────▼───────────┐           ┌───────────▼────────────┐
  │   Individual users    │           │    Enterprise orgs      │
  │                       │           │                         │
  │   Dashboard / search  │           │   Usage analytics       │
  │   Session detail      │           │   Cost attribution      │
  │   Export / share      │           │   Retention + audit     │
  └───────────────────────┘           └─────────────────────────┘
```

## Start with what you need.

Free for individuals. Each capture method works standalone. No forced pipelines.

[Get started free](https://reclaimllm.com/register) [See pricing](https://reclaimllm.com/pricing)