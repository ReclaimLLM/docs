# Source: https://reclaimllm.com/install

Get started

# Up and running 
in 2 minutes.

Choose the capture method that fits how you work. You can use all three at once — they produce the same normalized session record.

[Native Hooks →](https://reclaimllm.com/install#hooks)·[Browser Extension →](https://reclaimllm.com/install#browser)·[API Proxy →](https://reclaimllm.com/install#proxy)·[Videos →](https://reclaimllm.com/install#videos)

Video walkthroughs

## Watch the setup, then inspect your first session.

[ReclaimLLM channel →](https://www.youtube.com/@ReclaimLLM)

### Install ReclaimLLM

Set up RCLM and start capturing sessions.

### Use session details

Read captured messages, tool calls, file diffs, and metadata.

### ReclaimLLM walkthrough

A quick product walkthrough from the ReclaimLLM channel.

Before you start

You need a free RCLM account and an API key. Create your account, then find your API key in Settings → API Keys.

[Create free account →](https://reclaimllm.com/register)

rclm-hooksMethod 1

## Native Hooks

The richest capture method. Hooks that plug directly into Claude Code, Gemini CLI, and Codex CLI's lifecycle events. Captures complete sessions: messages, every tool call and result, every file the agent created or modified, and token usage. One install command, then it runs invisibly.

- ✓Claude Code, Gemini CLI, and Codex CLI supported
- ✓Paired tool inputs + results captured
- ✓File diffs: before/after for every changed file
- ✓Per-session token and cost analytics

1

Install the package

install

$ pip install rclm

2

Install hooks for all providers

install hooks

$ rclm-hooks-install

Opening browser to create your API key…

✓ Saved credentials to ~/.reclaimllm/config.json

✓ Hooks installed in ~/.claude/settings.json

✓ Hooks installed in ~/.gemini/settings.json

✓ Hooks installed in ~/.codex/hooks.json

Opens a browser to reclaimllm.com/settings to create your API key — no copy-paste needed. Pass \--api-key=<key> to skip the browser.

3

Or install for a single provider

single-provider install

\# Pick one:

$ rclm-hooks-install --claude

$ rclm-hooks-install --gemini

$ rclm-hooks-install --codex

─────────────────────────────────────────────────

\# Project-local install (default is global):

$ rclm-hooks-install --local

4

Verify it works

verify

\# Start any Claude Code, Gemini, or Codex session

\# Session appears in your dashboard within seconds

─────────────────────────────────────────────────

\# Or check the session spool directly:

$ ls ~/.reclaimllm/sessions/

rclm-browser-extMethod 2

## Browser Extension

Capture conversations from ChatGPT, Claude.ai, and Gemini directly in the browser. Works with your existing accounts — no API keys or extra configuration required. Just install and go.

- ✓Works on ChatGPT, Claude.ai, Gemini
- ✓No API keys or account access required
- ✓Captures in the background, syncs automatically
- ✓Sensitive content flagged before upload

⏳

The extension is awaiting Chrome Web Store review. In the meantime, you can install it manually in developer mode using the steps below — it takes under a minute.

1

Download the extension

[↓ Download reclaimllm-extension.zip](https://reclaimllm.com/reclaimllm-extension.zip)

Unzip the file after downloading — you'll need the extracted folder in the next step.

2

Enable Developer mode in Chrome

Open chrome://extensions in your browser. Toggle Developer mode on using the switch in the top-right corner of the page.

3

Load the extension

Click Load unpacked and select the folder you unzipped in step 1. The ReclaimLLM icon will appear in your Chrome toolbar.

4

Sign in with your RCLM account

Click the ReclaimLLM icon in your toolbar and sign in with the same GitHub or Google account you use for your RCLM account. Capturing starts automatically once you're signed in.

rclm-proxyMethod 3experimental

## Local API Proxy

A local proxy that sits between your code and LLM providers. Every request and response is recorded — Anthropic, OpenAI, Azure OpenAI, and Gemini. No code changes required; just point your environment variables at it.

- ✓Works with any app — just set one env var
- ✓Runs locally for maximum security
- ✓Supports all major providers via LiteLLM
- ✓Async upload — never blocks your requests

⚗️

The local proxy is experimental. Expect rough edges — please report any issues you run into.

1

Install the package

install

$ pip install rclm

\# or with proxy support (LiteLLM)

$ pip install 'rclm\[proxy\]'

2

Start the proxy

start proxy

$ export RECLAIMLLM\_API\_KEY=rclm\_your\_key\_here

$ rclm-proxy

✓ ReclaimLLM proxy listening on :9800

✓ Forwarding to https://api.anthropic.com

3

Point your app at the proxy

your app .env

\# Anthropic

$ export ANTHROPIC\_BASE\_URL=http://localhost:9800

─────────────────────────────────────────────────

\# OpenAI

$ export OPENAI\_BASE\_URL=http://localhost:9800

Your existing API keys stay in place — the proxy forwards them transparently.

Verify

## Check that it's working.

01

Run a session

Start a Claude Code session, send a request through the proxy, or open ChatGPT with the extension installed.

02

Open your dashboard

Go to your RCLM dashboard. A new session should appear within a few seconds of the session ending.

03

Browse the capture

Click into the session to see messages, tool calls, file diffs (hooks only), and token usage.

## No account yet?

Sign up free — no credit card required. Start capturing up to 200 sessions at no cost.

[Create free account→](https://reclaimllm.com/register)