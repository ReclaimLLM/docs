# Source: https://reclaimllm.com/news/extract-project-names-from-cli-sessions

[← Back to News](https://reclaimllm.com/news)

feature

Apr 28, 2026

# Extract Project Names From CLI Sessions

CLI sessions can now be grouped and filtered by a derived project name based on changed file paths.

CLI sessions now get a derived project name from the file paths captured during the session.

That means sessions can be grouped and filtered by project instead of only by date, model, tag, language, or file path. For example, a session that changes files under a `billing-api` project can be labeled as `billing-api`, and a relative path like `mobile-app/src/screens/...` can be labeled as `mobile-app`.

## How it works

ReclaimLLM already stores file diffs for CLI sessions. The new project-name derivation uses those paths as deterministic metadata.

The parser handles common path shapes:

- macOS/Linux absolute paths
- Windows paths
- relative repository paths
- nested workspace paths

When a path includes a known workspace marker such as `Projects`, `repos`, `workspace`, or `Desktop`, ReclaimLLM uses the next path segment as the project name. When the path is relative, it uses the first repo-like path segment.

If a session touches multiple files, ReclaimLLM chooses the most common inferred project across those paths. If there is a tie, it keeps the first stable candidate. If no confident project can be inferred, the project name stays empty instead of guessing.

This runs in the deterministic session statistics background task, not in LLM summarization. Project names do not depend on model availability, prompt output, rate limits, or summary generation.

## Why this belongs in stats

Project name is derived from file paths, so it fits beside other deterministic session metadata like languages, line counts, and file-change tracking.

Putting it in the stats task has a few practical benefits:

- existing sessions can be backfilled
- ingest stays fast
- path parsing stays in one place
- filtering can use a stored session field
- project names can be recomputed if the heuristic improves

The implementation also tracks whether project derivation has already run. That matters because some sessions will not have enough path context to infer a project. Those sessions should remain empty without being retried forever.

## Current limitations

This is intentionally heuristic.

For absolute nested workspace paths, ReclaimLLM currently prefers the outer workspace. For example, a path under `Desktop/workspace/customer-portal/ web-app/...` resolves to `customer-portal`. A relative path beginning with `web-app/...` resolves to `web-app`.

That tradeoff keeps behavior predictable when the absolute path contains a clear workspace root, but it may be too coarse for monorepos where users want both workspace and package names.

Some unusual directory layouts may not produce a project name yet. If your projects live outside common workspace folders, or if you expect package- level grouping inside monorepos, those are useful edge cases to report.

Share[Share on X](https://x.com/intent/post?url=https%3A%2F%2Freclaimllm.com%2Fnews%2Fextract-project-names-from-cli-sessions&text=Extract%20Project%20Names%20From%20CLI%20Sessions) [Share on LinkedIn](https://www.linkedin.com/sharing/share-offsite/?url=https%3A%2F%2Freclaimllm.com%2Fnews%2Fextract-project-names-from-cli-sessions) [Share on Facebook](https://www.facebook.com/sharer/sharer.php?u=https%3A%2F%2Freclaimllm.com%2Fnews%2Fextract-project-names-from-cli-sessions)