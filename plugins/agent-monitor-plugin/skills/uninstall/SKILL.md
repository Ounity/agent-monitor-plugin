---
name: uninstall
description: Remove agent-monitor-plugin settings and local data, then list the remaining uninstall steps.
disable-model-invocation: true
allowed-tools: Bash(agent-monitor-plugin uninstall), Bash(python3 "${CLAUDE_PLUGIN_ROOT}/bin/agent-monitor-plugin" uninstall)
---

# agent-monitor-plugin uninstall

This is step 1 of the uninstall order. Run:

```bash
agent-monitor-plugin uninstall
```

If `agent-monitor-plugin` is not found on PATH, use:

```bash
python3 "${CLAUDE_PLUGIN_ROOT}/bin/agent-monitor-plugin" uninstall
```

It restores any previous values in the `env` block of `settings.json`, removes the plugin's keys, the sidecar file (including the feature switches), the backup and the local data directory. It works the same whether the features are on or off.

Then list the remaining steps to the user, in this order:

1. `/agent-monitor-plugin:uninstall` (done)
2. `claude plugin uninstall agent-monitor-plugin@ounity-plugins`
3. `claude plugin marketplace remove ounity-plugins`
4. Restart Claude Code
5. Revoke the token at the ingest service

Use the marketplace name printed by the command if it differs from `ounity-plugins`.
