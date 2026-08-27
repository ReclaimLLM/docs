# Source: https://reclaimllm.com/news/hybrid-semantic-search-via-qdrant

[← Back to News](https://reclaimllm.com/news)

update

Mar 26, 2026

# Find Anything with New Hybrid Semantic Search

We've upgraded session search to understand your intent—now you can find past work using natural language queries, even if you don't remember the exact keywords.

## Overview

Finding that one specific session from three weeks ago just got a whole lot easier. We are replacing our basic keyword-based search with a state-of-the-art **Hybrid Semantic Search** pipeline. Instead of just looking for exact word matches, RCLM now understands the _meaning_ and _context_ of your sessions, allowing you to search for things like "debugging auth middleware" or "kubernetes crash loop fixes" even if those exact phrases don't appear in your session titles.

## How it Works

The new search system uses a "hybrid" approach that combines the best of two worlds:

1. **Semantic Understanding (Dense Vectors)**: Every session is now automatically processed through an AI embedding model (OpenAI's `text-embedding-3-small`). This converts your session data into a multi-dimensional numerical representation that captures its semantic meaning.
2. **Keyword Precision (Sparse Vectors)**: At the same time, we still maintain a high-precision keyword index. This ensures that specific terms like tool names, file paths, or unique identifiers are still found exactly as you'd expect.
3. **RRF Fusion Ranking**: When you search, RCLM performs both a semantic and a keyword search simultaneously and then uses a "Reciprocal Rank Fusion" (RRF) algorithm to combine the results. This gives you a single, perfectly ranked list of sessions that match both your intent and your specific terms.
4. **Automatic Background Indexing**: A new background pipeline automatically syncs your sessions to our search engine (Qdrant). This keeps your search index up-to-date without slowing down your session ingestion.
5. **Privacy-First Filtering**: Your searches are always scoped to your user ID at the search engine level, ensuring that your data remains private and secure.

## Why We Made This Change

As our users' session histories grew from dozens to hundreds or even thousands of entries, basic keyword search (`ILIKE` matching) became increasingly frustrating. If you didn't remember the exact words used in a session summary, you couldn't find it. By moving to a semantic search model, we're making your entire RCLM history truly discoverable and useful for long-term knowledge management.

## Current Limitations and Feedback

We're rolling this out as a significant upgrade, but there are a few things to note:

- **Indexing Delay**: Because sessions are processed in the background, it may take up to an hour for a newly completed session to appear in semantic search results.
- **Ranking over Pagination**: Semantic search currently returns the most relevant results first rather than a simple paginated list. We are monitoring how users interact with these results to decide if traditional "Load More" pagination is still needed.
- **Cost Efficiency**: We use the OpenAI Batch API to process your sessions at a 50% discount, helping us keep RCLM's storage and search features sustainable.

This is one of our most requested features, and we're excited to see how it changes the way you interact with your past AI coding sessions. If you find a search that _should_ have matched but didn't, please let us know!

Share[Share on X](https://x.com/intent/post?url=https%3A%2F%2Freclaimllm.com%2Fnews%2Fhybrid-semantic-search-via-qdrant&text=Find%20Anything%20with%20New%20Hybrid%20Semantic%20Search) [Share on LinkedIn](https://www.linkedin.com/sharing/share-offsite/?url=https%3A%2F%2Freclaimllm.com%2Fnews%2Fhybrid-semantic-search-via-qdrant) [Share on Facebook](https://www.facebook.com/sharer/sharer.php?u=https%3A%2F%2Freclaimllm.com%2Fnews%2Fhybrid-semantic-search-via-qdrant)