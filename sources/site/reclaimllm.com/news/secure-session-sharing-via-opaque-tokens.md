# Source: https://reclaimllm.com/news/secure-session-sharing-via-opaque-tokens

[← Back to News](https://reclaimllm.com/news)

feature

Apr 21, 2026

# Introducing Secure Session Sharing via Opaque Tokens

Share your LLM sessions with clients and collaborators effortlessly—no account required, with full control over revocation and expiration.

## Overview

We are excited to launch a significant update to how you share your work in RCLM. You can now share individual sessions with anyone—clients, teammates, or external partners—using secure, per-recipient links. Recipients can view the full session transcript and metadata directly in their browser without ever needing to sign up for a ReclaimLLM account.

## How it Works

This new sharing mechanism replaces the simple "is\_public" toggle with a more robust, auditable system based on 256-bit secure bearer tokens:

1. **Per-Recipient Links**: When you share a session, you can specify an email address for the recipient. RCLM generates a unique, cryptographically secure link specifically for that share.
2. **No Account Required**: The recipient simply clicks the link to view the session. There’s no login wall, making it perfect for sharing insights with external stakeholders.
3. **Full Owner Control**: You have a centralized view of every share you've created. You can see when a link was first viewed, how many times it’s been accessed, and you can revoke any link instantly, cutting off access immediately.
4. **Automatic Expiration**: All shared links have a built-in time-to-live (TTL). Once a link expires, it is automatically invalidated by our database, ensuring your data isn't exposed indefinitely.
5. **Email Invitations**: RCLM can send the secure link directly to your collaborator via email, including the session title and a direct access button.

## Why We Made This Change

Previously, sharing a session was an "all or nothing" decision. Turning on public sharing made the session world-readable to anyone with the ID, with no way to track who was viewing it or set an expiration date. Our users needed a more professional way to share their work—one that provides a clear audit trail and doesn't force their collaborators to go through a signup process just to view a single interaction.

## Current Limitations and Feedback

We've designed this first iteration to balance security and ease of use, but there are a few things to keep in mind:

- **Link Forwarding**: Currently, a shared link is a "bearer token." This means anyone who has the link can view the session. We recommend sharing links only with trusted collaborators.
- **Daily Sharing Cap**: To prevent abuse, there is a soft limit of 50 new shares per day per user.
- **Plaintext Tokens**: For performance reasons, tokens are currently stored as-is in our database. We are evaluating a hashed-lookup approach for future security hardening as our enterprise user base grows.
- **Redaction**: The shared view currently shows the full session as you see it. We are exploring "redacted sharing" options that would automatically hide sensitive information based on AI-detected patterns.

We'd love to hear how you're using session sharing in your workflow. If you have suggestions for the viewer interface or need more granular access controls, please reach out.

Share[Share on X](https://x.com/intent/post?url=https%3A%2F%2Freclaimllm.com%2Fnews%2Fsecure-session-sharing-via-opaque-tokens&text=Introducing%20Secure%20Session%20Sharing%20via%20Opaque%20Tokens) [Share on LinkedIn](https://www.linkedin.com/sharing/share-offsite/?url=https%3A%2F%2Freclaimllm.com%2Fnews%2Fsecure-session-sharing-via-opaque-tokens) [Share on Facebook](https://www.facebook.com/sharer/sharer.php?u=https%3A%2F%2Freclaimllm.com%2Fnews%2Fsecure-session-sharing-via-opaque-tokens)