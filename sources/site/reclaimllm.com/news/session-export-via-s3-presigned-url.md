# Source: https://reclaimllm.com/news/session-export-via-s3-presigned-url

[← Back to News](https://reclaimllm.com/news)

update

Mar 19, 2026

# Session Export Delivery via S3 Presigned URL

We've upgraded our session export process to handle larger datasets by moving from direct email attachments to secure, time-limited S3 download links.

## Overview

RCLM users can now export their entire session history, including full metadata and raw S3 blobs, regardless of the total size. Previously, exports were sent as direct email attachments, which were capped at 40MB by AWS SES limits. We've transitioned to a more robust delivery method using secure, presigned S3 URLs.

## How it Works

When you request a session export, RCLM now performs the following steps in the background:

1. **Asynchronous Processing**: The export starts immediately as a background task, allowing you to continue using the application.
2. **ZIP Assembly**: All your captured LLM sessions and their associated data are gathered into a single ZIP archive.
3. **Secure Storage**: The ZIP archive is uploaded to a dedicated, private section of our S3 storage.
4. **Presigned URL Generation**: A unique, cryptographically signed download link is generated for your specific export.
5. **Email Notification**: You receive an email containing this secure link rather than a bulky attachment.

The download links are valid for **24 hours**. After this period, the link will expire, and you will need to trigger a new export if you haven't downloaded your data.

## Why We Made This Change

As our users' session histories grew, we encountered a hard technical limit. Direct email attachments through AWS SES are restricted to 40MB. For active users, a full export of their LLM interactions frequently exceeded 60MB, causing export failures and "HTTP content length exceeded" errors.

By moving to S3-based delivery, we've eliminated this size ceiling. This approach is not only more reliable for large datasets but also more efficient, as it avoids the overhead of processing large MIME attachments through email servers.

## Current Limitations and Feedback

While this update solves the immediate size issue, there are a few things to keep in mind:

- **24-Hour Expiration**: Ensure you download your export within a day of receiving the email.
- **Direct Link Access**: Anyone with the link can download the ZIP during the 24-hour window. We send these links only to your verified account email to minimize risk.
- **Memory Usage**: For extremely large session counts, the export process currently assembles the entire ZIP in memory. We are monitoring performance and may move to a streaming upload model if needed.

We'd love to hear your feedback on this new export workflow. If you encounter any issues with the download links or have suggestions for the export format, please let us know.

Share[Share on X](https://x.com/intent/post?url=https%3A%2F%2Freclaimllm.com%2Fnews%2Fsession-export-via-s3-presigned-url&text=Session%20Export%20Delivery%20via%20S3%20Presigned%20URL) [Share on LinkedIn](https://www.linkedin.com/sharing/share-offsite/?url=https%3A%2F%2Freclaimllm.com%2Fnews%2Fsession-export-via-s3-presigned-url) [Share on Facebook](https://www.facebook.com/sharer/sharer.php?u=https%3A%2F%2Freclaimllm.com%2Fnews%2Fsession-export-via-s3-presigned-url)