# Approval gates reference

A gate sits in front of an action the agent is about to take and allows it, asks a human, or
blocks it. Gates are how a policy stops being advice and starts being enforced.

## Designing a gate

1. Engineering and compliance name the checkpoints that genuinely require approval. Keep the
   list short; a gate on everything trains people to click through.
2. Each checkpoint becomes a script that exits zero to allow and non-zero to block.
3. The block message must say why it blocked *and* how to get approval. A bare "denied"
   sends the agent looking for a workaround.

## Example: production deploy needs a named release authorisation

`scripts/gate-prod-deploy.sh`:

```bash
#!/usr/bin/env bash
# Production deploys require a named release authorisation.
set -euo pipefail

command=$(cat)

case "$command" in
  *"deploy"*"production"*)
    if [ -z "${RELEASE_APPROVAL:-}" ]; then
      echo "Blocked: production deploys require RELEASE_APPROVAL to name the release manager." >&2
      echo "Ask the on-call release manager to authorise, then re-run with RELEASE_APPROVAL set." >&2
      exit 2
    fi
    ;;
esac
```

## Wiring it up

*If your agent supports pre-tool hooks* — Claude Code does — register the script so it runs
before the tool executes. In `.claude/settings.json`, exit code 0 allows and exit code 2
blocks with the message shown to the agent:

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          { "type": "command", "command": "./scripts/gate-prod-deploy.sh" }
        ]
      }
    ]
  }
}
```

Agents without hooks can run the same script as a CI job or a wrapper around the deploy
command. The script is the policy; the wiring is per-host.

## Where gates live

- Team gates go in the repo's own settings, version-controlled and reviewed in PRs.
- Non-negotiable gates go in admin-managed settings, outside the repo, so a project cannot
  edit its way past them. Claude Code enforces this with `allowManagedHooksOnly`.

Do not put a gate in the repo and call it non-negotiable. Anyone who can open a PR can
change it.

## Measuring

Gate decisions are timestamped events: allow and block counts per gate. The lagging measure
is whether violations of the gated policy still reach production.
