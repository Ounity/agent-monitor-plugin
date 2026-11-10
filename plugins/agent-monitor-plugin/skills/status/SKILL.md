---
name: status
description: Show agent-monitor-plugin configuration and delivery status (URL, last success, errors, queue).
allowed-tools: Bash(agent-monitor-plugin status), Bash(python3 "${CLAUDE_PLUGIN_ROOT}/bin/agent-monitor-plugin" status)
---

# agent-monitor-plugin status

Run:

```bash
agent-monitor-plugin status
```

If `agent-monitor-plugin` is not found on PATH, use:

```bash
python3 "${CLAUDE_PLUGIN_ROOT}/bin/agent-monitor-plugin" status
```

The command locates the data directory used by the hooks on its own (newest `agent-monitor-plugin*` directory under the Claude config dir), so run it as shown without setting variables.

Summarize the output for the user: configured or not, which features (traces, openspec) are enabled or disabled and what each sends, whether managed keys are present (when traces is on), last successful delivery, last error, queue size. NEVER print or ask for the token; the command already hides it.

If it says "Not configured", suggest `/agent-monitor-plugin:setup`. If a feature is disabled, mention `/agent-monitor-plugin:send-traces` or `/agent-monitor-plugin:send-openspec` to enable it. If the last error is a rejected token, suggest re-running `/agent-monitor-plugin:setup` with a new token.
