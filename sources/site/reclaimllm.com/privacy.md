# Source: https://reclaimllm.com/privacy

Legal

# Privacy Policy

Last updated: June 2026

RCLM is built on the premise that your AI interactions belong to you. This policy explains what data we collect, how we store it, and what we do and don't do with it. We've written it in plain English — not legalese.

## What we collect

When you use RCLM, we collect the following:

- **AI session data** — the messages, tool calls, file diffs, model names, and token counts captured by whichever capture method you use (API proxy, native hooks, or browser extension).
- **Account information** — your email address and OAuth profile (name and avatar) from GitHub or Google when you sign up.
- **Usage metadata** — which models you use, session timestamps, token counts, and session durations. This powers your analytics dashboard.
- **Browser telemetry** — basic page analytics (page views, navigation events) via PostHog, if you haven't opted out.

We do not collect passwords. Authentication is handled entirely via GitHub or Google OAuth.

## How we store it

Session content (messages, tool calls, file diffs) is stored as JSON blobs in S3-compatible object storage. Session metadata (timestamps, model names, token counts, titles) is stored in PostgreSQL.

- **Encryption at rest:** AES-256 for stored data.
- **Encrypted raw session storage:** Paid users and Enterprise organizations can enable an additional encryption layer for raw session details, including full captured transcripts and session blobs.
- **Recovery keys:** When encrypted session storage is enabled, the recovery key is downloaded once by the user or org admin. It is never emailed, and we do not store the plaintext key.
- **Encryption in transit:** TLS for all data transfers between your device and our servers.
- **Data residency:** Free plan data is stored in our default region. Paid plan users can choose US or EU. Enterprise customers can specify any region or deploy on their own infrastructure.

## Encrypted session storage

Raw AI session details can include source code, prompts, terminal output, file paths, tool results, and other development context. Paid users and Enterprise organizations can enable encrypted raw session storage to add another boundary around that full session content.

- **Paid users:** encryption is controlled from account settings. When enabled, the user downloads a recovery key once and is responsible for storing it safely.
- **Enterprise:** encryption is configured org-wide by an org admin. The org recovery key is downloaded once by the enabling admin, and decrypt access for member session details is controlled by organization policy.
- **What remains usable:** high-level metadata, summaries, stats, filters, and search signals can continue to power dashboards and aggregate views without opening every full raw transcript.

Encrypted session storage does not replace access control, redaction, or careful handling of secrets. It is an additional storage-level protection for full session content.

## What we don't do

- We do not train any AI model on your session data — ever. Your interactions are yours, not ours.
- We do not sell your data to third parties.
- We do not share your sessions with anyone without an explicit action from you, such as creating a share link or exporting your data.
- We do not use your data for advertising.

## Sensitive data

AI sessions frequently contain sensitive content: API keys, tokens, passwords, connection strings, and personal information. RCLM handles this carefully.

- On the **Free plan**, you can manually review and redact sessions before they're stored, shared, or exported.
- On the **Paid plan**, RCLM automatically scans every session for common sensitive patterns (API keys, tokens, PII) and flags them in your dashboard for review. You can also configure auto-redaction rules that apply before storage.
- On **Enterprise**, org-wide redaction policies can be enforced centrally.

Sensitive-data scanning uses pattern matching and heuristics locally or server-side depending on your plan. We do not send session content to external AI services for this scan.

## Browser extension

The browser extension captures conversation content directly from ChatGPT, Claude.ai, and Gemini in your browser. A few specifics worth knowing:

- **What is captured:** conversation messages, the model name, and the conversation title. The extension does not capture anything outside of a supported conversation page.
- **Images:** user-uploaded images and AI-generated images embedded in conversations are fetched locally by the extension's background worker (not by our servers) and converted to base64 before being included in the session record. Images are only fetched from first-party CDN domains (Google, OpenAI).
- **Credentials:** the extension stores your RCLM access token in `chrome.storage.local` — local to your browser profile and not accessible to other extensions or websites.
- **Pause control:** you can pause capturing for any supported site at any time from the extension popup. While paused, no data is captured or uploaded for that site.
- **Historical crawl:** if you choose to sync your conversation history, the extension opens past conversations in background tabs to capture them. This runs only when you initiate it and stops automatically when complete.
- **Extension analytics:** the extension sends anonymous error events (e.g. upload failures, scrape errors) to PostHog solely for debugging purposes. These events contain no session content — only error type and provider name.

## Sharing and export

If you choose to share or export sessions, the following applies:

- Shared session links are explicit actions and can be revoked.
- Before sharing sensitive sessions, RCLM can flag credentials, secrets, PII, and other review-worthy content.
- You control what you export. Export is always an explicit action, and exported files are yours to manage.
- Enterprise workspaces can add role-based access, retention, and audit controls around shared organizational history. If encrypted session storage is enabled, opening full encrypted session details still requires decrypt access.

## Payments

Payments are processed by **Stripe**. We do not store your credit card details — they never touch our servers. Stripe handles all payment data under their own PCI-compliant infrastructure.

We receive from Stripe: your subscription status, last-4 of card (for your receipts), and billing email. That's it.

## Your rights and deletion

You have full control over your data:

- **Delete individual sessions** from your dashboard at any time. Deletion removes both the metadata from our database and the blob from object storage.
- **Delete your entire account** from Settings → Account. This removes all your sessions, metadata, API keys, and OAuth connections. We don't retain anything.
- **Export your data** at any time in JSON format. Paid users can bulk-export as a zip. Encrypted session storage affects how raw session details are stored; exported files are still yours to protect after download.
- **Data subject requests** (GDPR / CCPA): email us and we'll respond within 30 days.

## Cookies and tracking

We use a session cookie for authentication (managed by Supabase Auth). We use PostHog for product analytics — page views and navigation events only. No behavioral tracking, no cross-site tracking, no advertising cookies.

You can opt out of PostHog analytics from Settings → Privacy.

## Changes to this policy

If we make material changes to this policy, we'll notify you by email at least 14 days before the change takes effect. Minor clarifications may be made without notice. The “Last updated” date at the top of this page always reflects the current version.

## Contact

Questions about your data or this policy? Email us at [privacy@reclaimllm.com](mailto:privacy@reclaimllm.com).