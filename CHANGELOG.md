# Changelog

All notable changes are documented here. Format: [Keep a Changelog](https://keepachangelog.com/en/1.1.0/). Versioning: [SemVer](https://semver.org/). Releases are tagged `vX.Y.Z`.

## [1.0.0] - 2026-11-10

Initial release.

### Added
- Claude Code hooks that report session `start`, `heartbeat`, `end`, a redacted prompt excerpt and OpenSpec sync/snapshot data to an ingest service you configure (`/v1/otlp`, `/v1/ingest/plugin/*`).
- Two independent, persistent feature switches, both off by default: **traces** (OTEL cost export, session events, redacted prompt excerpt) and **openspec** (change inference, OpenSpec file mirror, snapshot). Skills `/agent-monitor-plugin:send-traces` and `/agent-monitor-plugin:send-openspec` and the matching CLI commands accept only `on`, `off` or `status`. With both off every hook is a silent no-op; openspec-only mode sends exactly one registration `start` per session.
- Skills `/agent-monitor-plugin:setup` (stores URL and token only), `:status`, `:uninstall`, `:send-traces`, `:send-openspec`.
- Optional labels agent name, environment and task (`setup --agent/--environment/--task`, plugin options `AGENT_NAME`, `ENVIRONMENT`, `TASK`, and the per-session `AGENT_MONITOR_TASK` environment variable). Session events carry `user_ref` = `agent:env=<environment>/name=<name>/task=<task>` with unset parts omitted. Each value is 1 to 48 characters from letters, digits, space and `. _ - :`. They are labels, not filters.
- `send-traces on` merges the 9 OTEL keys into user `settings.json` (mode `existing & 600`, backup `settings.json.agent-monitor-plugin.bak`, `--force` rule for conflicting values); `off` and `uninstall` restore it exactly. Uninstall removes only this plugin's own data directories and keeps hand-edited values.
- Offline queue with backoff: HTTP 429 is retried honoring `Retry-After`, HTTP 401 pauses delivery and is shown by `status` as a rejected token, heartbeats are not queued while offline, queued items of a disabled feature are dropped.
- Redaction of secrets in prompt excerpts (JSON-quoted values, `Basic` credentials, identifiers containing a secret word, prefix keys, zero-width characters, NFKC normalization) over an 8000-character window before the excerpt is cut.
- Security posture: plain `http://` to a non-local host is refused unless `--allow-insecure`; redirects are never followed so the bearer token stays on the configured origin; portable lock files, no `fcntl`.
- Opt-outs: `AGENT_MONITOR_NO_PROMPTS` (no prompt excerpts) and `AGENT_MONITOR_NO_SYNC` (no OpenSpec sync).
- `status` and `doctor` show features, labels, the exact `user_ref` sent, queue and last delivery.

### Known limitations / not verified
- Supported on macOS, Linux and WSL. Native Windows is not supported.
- The purpose of `user_ref` on the server side is not documented; the labels are sent there by convention.
- Whether environment variables set for Claude Code are inherited by hook processes (needed for `AGENT_MONITOR_TASK`) has not been verified.
- An end-to-end run on a clean profile has not been done yet.
