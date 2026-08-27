# Source: https://reclaimllm.com/news/local-redaction-of-sensitive-information

[← Back to News](https://reclaimllm.com/news)

feature

Apr 27, 2026

# Local Redaction of Sensitive Information

RCLM hooks can now apply cached substitution rules, local-only redactions, and folder exclusions before session uploads leave the machine.

RCLM hooks now support local redaction before captured sessions are uploaded. When configured, hook uploads apply substitution rules on the machine first, so matched strings in session messages, tool calls, and file diffs are replaced before the payload is sent to ReclaimLLM.

This change addresses a privacy gap in hook-based capture. Hooks run close to the developer workflow, and they can see raw context before server-side processing happens. If a team has configured substitutions for secrets, identifiers, internal names, or other sensitive strings, those rules should be able to run before ingestion, not only after data reaches the backend.

## How it works

Hook redaction is enabled by default. During hook installation and during `rclm-update`, RCLM fetches the account’s remote substitution rules and stores a local cached copy in the machine’s RCLM config.

The local redaction config includes:

- `redaction.enabled`
- `redaction.remote_substitutions`
- `redaction.local_substitutions`
- `redaction.exclude_folders`
- `redaction.last_sync`

At upload time, hooks compute an effective substitution map from remote substitutions plus local substitutions. If the same source string exists in both places, the local value wins. This lets account-level substitutions cover shared defaults while still allowing machine-specific replacements that should not be pushed into the remote app.

Before upload, the hook uploader also checks folder exclusions. If the captured session’s working directory or transcript path is inside an excluded folder, the record is skipped instead of uploaded. This is meant for local projects that should never leave the machine.

Substitutions are applied to the serialized upload payload using deterministic longest-first string replacement. Longest-first replacement avoids a shorter key replacing text inside a longer key before the longer one gets a chance to match.

## Updating local rules

Shared substitution rules are managed in the ReclaimLLM app. After adding, editing, or removing shared substitutions, run:

```bash
rclm-update
```

That refreshes the local hook redaction cache on the current machine. Run it on each machine where hooks are installed.

Local substitutions are for machine-specific values: local usernames, absolute paths, workstation-only secrets, private service aliases, or any value that should not become an account-wide rule. Add them under `redaction.local_substitutions` in the local RCLM config as a string-to-string map.

Folder exclusions are also local. Add project or directory paths under `redaction.exclude_folders`. When a captured session has a working directory or transcript path inside one of those folders, the hook skips upload entirely. This is stricter than substitution: nothing from that captured record is sent.

A typical workflow is:

1. Configure shared substitutions in the app.
2. Run `rclm-update` on machines that have hooks installed.
3. Add local-only substitutions for machine-specific values.
4. Add excluded folders for repositories that should never be uploaded.
5. Let hooks redact locally during normal session capture.

## Tradeoffs and limitations

This implementation intentionally does not fetch remote substitutions on every upload. Doing so would keep settings fresher, but it would add network latency and another failure path to every hook upload. The current design keeps the upload path local and predictable.

The tradeoff is staleness. If you change substitutions in the app, already-installed hooks will not see those changes until `rclm-update` runs again. The app should remind users to run `rclm-update` after substitution changes.

This is also string replacement, not semantic sensitive-data detection. It only replaces strings that match configured substitution keys. If a key is too broad, it may over-redact. If a sensitive value is not configured, local hook redaction will not infer it automatically.

## Future direction

The next direction is to make substitution updates closer to real time, so local hook caches can update after remote substitution changes without relying only on manual `rclm-update`. The goal is to keep the upload path fast while reducing the chance of stale local rules.

We also plan to add server-side redaction as defense in depth. Local redaction should prevent sensitive values from leaving the machine in normal hook uploads, but the server should still apply redaction when local clients are old, disabled, misconfigured, or miss a value.

## What we are watching

The main question is whether manual sync is enough for the first version. If users often forget to run `rclm-update`, real-time or conditional sync becomes more important.

We are also watching edge cases around malformed local config, folder exclusion matching, and overly broad substitution keys. Feedback is especially useful if you rely on hook capture in repositories with stricter privacy boundaries or machine-specific secrets.

Share[Share on X](https://x.com/intent/post?url=https%3A%2F%2Freclaimllm.com%2Fnews%2Flocal-redaction-of-sensitive-information&text=Local%20Redaction%20of%20Sensitive%20Information) [Share on LinkedIn](https://www.linkedin.com/sharing/share-offsite/?url=https%3A%2F%2Freclaimllm.com%2Fnews%2Flocal-redaction-of-sensitive-information) [Share on Facebook](https://www.facebook.com/sharer/sharer.php?u=https%3A%2F%2Freclaimllm.com%2Fnews%2Flocal-redaction-of-sensitive-information)