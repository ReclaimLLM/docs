# Source: https://reclaimllm.com/news/session-category-classification

[← Back to News](https://reclaimllm.com/news)

feature

May 6, 2026

# Session Category Classification

CLI sessions now carry a maintenance category so users can filter and review fix, feature, and performance work.

CLI sessions now include a maintenance category: `fix`, `feat`, or `perf`.

The category gives each coding session a compact label for the kind of engineering work it represents. Instead of relying only on title search, tags, project names, or dataset membership, you can now filter sessions by whether they were mainly about fixing behavior, adding capability, or improving performance.

This is meant for review and discovery. If you are scanning a project history, preparing a changelog, checking what kind of work dominated a week, or finding sessions related to regressions, the category gives you another way to narrow the list.

## What the categories mean

`fix` means the session was primarily corrective work.

Use this for sessions focused on repairing broken behavior, resolving errors, fixing regressions, correcting incorrect output, restoring a failing workflow, or addressing a bug reported by a user or test.

Examples:

- Fixing a dashboard filter that returned the wrong sessions
- Repairing an API route that failed for valid input
- Correcting a parser that missed part of a transcript
- Updating UI behavior that broke on a specific viewport

`feat` means the session was primarily new or changed capability.

Use this for sessions where the main outcome is something users, developers, or operators can now do that they could not do before. This can include new UI controls, new API behavior, new metadata, new workflow support, or a meaningful expansion of an existing feature.

Examples:

- Adding a new filter to the sessions dashboard
- Supporting a new dataset workflow
- Adding editable metadata to a session detail page
- Introducing a new provider integration

`perf` means the session was primarily performance or efficiency work.

Use this for sessions focused on making something faster, cheaper, lighter, or more scalable. This includes reducing latency, cutting token usage, avoiding unnecessary work, improving query patterns, reducing memory use, or making a batch job safer to run at scale.

Examples:

- Avoiding unnecessary regeneration for historical sessions
- Reducing repeated requests in a dashboard
- Adding throttling to a backfill process
- Improving a query path that was too slow

## How to use category filters

On session list pages, use the Category filter to narrow CLI sessions by `Fix`, `Feature`, or `Performance`.

Selecting a category automatically scopes the results to CLI sessions, because these labels describe coding-session maintenance work. API proxy records and browser chat records are not categorized with this system.

Session rows also show the category near the title, so you can scan a list without opening every session. When a project name is available, it appears beside the category, which makes it easier to read session history as:

```text
type → category → project → title
```

On the session detail page, the category appears near project and dataset membership. Hovering over it shows a short reason for the selected category. You can update the category manually when the label does not match how you want to classify the work.

## Why this matters

Search answers “which sessions mention this?”

Projects and datasets answer “where does this session belong?”

Category answers “what kind of maintenance work was this?”

Those are different questions. A session titled “Update session filters” could be a bug fix, a feature, or a performance improvement depending on the work done. The category gives that intent a first-class place in the UI.

This should make it easier to:

- Review recent work by maintenance type
- Separate bug-fix sessions from feature-building sessions
- Find performance work without relying on inconsistent tags
- Audit what kind of work happened inside a project
- Build cleaner summaries or changelogs from session history

## Current limitations

Each session has one category. Some sessions include mixed work, such as fixing a bug while also adding a small feature. In those cases, the category should represent the dominant intent of the session.

The category is meant to help with scanning and filtering, not to be a perfect taxonomy. If a label is wrong or too debatable, edit it directly on the session detail page.

Feedback is especially useful around mixed sessions, category wording, and whether teams need multi-category sessions in the future.

Share[Share on X](https://x.com/intent/post?url=https%3A%2F%2Freclaimllm.com%2Fnews%2Fsession-category-classification&text=Session%20Category%20Classification) [Share on LinkedIn](https://www.linkedin.com/sharing/share-offsite/?url=https%3A%2F%2Freclaimllm.com%2Fnews%2Fsession-category-classification) [Share on Facebook](https://www.facebook.com/sharer/sharer.php?u=https%3A%2F%2Freclaimllm.com%2Fnews%2Fsession-category-classification)