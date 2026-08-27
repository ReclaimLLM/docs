# Source: https://reclaimllm.com/news/data-residency-regional-storage-and-migration

[← Back to News](https://reclaimllm.com/news)

feature

Mar 23, 2026

# Data Residency: Regional Storage and Migration

ReclaimLLM now supports regional data residency, allowing users to store their session data in the EU to meet compliance and legal requirements.

## Overview

We are introducing regional data residency for ReclaimLLM. Users can now choose to store their session data in the EU, enabling them to meet strict legal and compliance requirements, such as GDPR. This update includes automatic routing for all new data and a seamless, asynchronous migration process for existing session history.

## How it Works

The data residency feature is integrated directly into your user profile and affects every aspect of how ReclaimLLM handles your data:

1. **Regional Routing**: Once a region is selected, all new session ingests are automatically routed to the corresponding regional S3 bucket. Our backend now uses a unified routing mechanism to ensure that every session read, write, and background process respects your chosen region.
2. **Asynchronous Migration**: If you choose to move your existing data from the US to the EU, ReclaimLLM initiates an idempotent background migration task. This process copies your historical session blobs to the new region and verifies them before removing the old copies. Because this happens in the background, you can continue using ReclaimLLM without interruption.
3. **Resumable Progress**: The migration is designed to be resilient. If the process is interrupted by a server restart or network issue, it automatically picks up where it left off, ensuring that every session is eventually moved to the correct region without data loss.
4. **Full System Integration**: All background services, including session summarization, statistics calculation, and data exports, are fully aware of the regional routing. Your data remains within your chosen region throughout its entire lifecycle.

## Why We Made This Change

Previously, all ReclaimLLM data was stored in a single US-based region. As we've grown, many of our users—particularly those in enterprise and legal sectors—have requested the ability to keep their data within the EU to comply with local regulations. By implementing per-user region routing, we provide the flexibility needed for global compliance while maintaining the high performance of our ingestion pipeline.

## Current Limitations and Feedback

As you plan your transition to regional storage, please keep the following in mind:

- **Migration Duration**: For users with extensive session histories, the migration process may take some time to complete. New sessions will go to the new region immediately, but older sessions will appear as they are moved.
- **Irreversibility**: To ensure data consistency and prevent complex "ping-pong" migrations, the switch to a new region is currently treated as permanent in the user interface. Please confirm your selection before initiating the move.
- **Regional Availability**: We are starting with US and EU regions. We are monitoring demand for additional regions (such as APAC) and will expand our infrastructure as needed.

We are committed to providing robust data sovereignty options. If you encounter any issues during your migration or have specific compliance requirements that aren't met by the current implementation, please let us know.

Share[Share on X](https://x.com/intent/post?url=https%3A%2F%2Freclaimllm.com%2Fnews%2Fdata-residency-regional-storage-and-migration&text=Data%20Residency%3A%20Regional%20Storage%20and%20Migration) [Share on LinkedIn](https://www.linkedin.com/sharing/share-offsite/?url=https%3A%2F%2Freclaimllm.com%2Fnews%2Fdata-residency-regional-storage-and-migration) [Share on Facebook](https://www.facebook.com/sharer/sharer.php?u=https%3A%2F%2Freclaimllm.com%2Fnews%2Fdata-residency-regional-storage-and-migration)