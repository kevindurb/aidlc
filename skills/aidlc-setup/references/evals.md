# Continuous evals reference

Evals are the stage-gate QA of an agent-assisted codebase: a suite that runs whenever the
agent's configuration changes, checking the agent still performs as expected. Model
versions, instruction files, skills, and gates all change behaviour, and none of them are
covered by the normal test suite.

## Prerequisites

- A project instructions file and a working verification loop, so "did it pass" has a
  mechanical answer.
- CI that can run your agent non-interactively, and an API budget for it.

## Collecting cases

Take 20 to 50 real tasks from work already done, where the expected outcome is known. Real
tasks, not invented ones — invented tasks test the eval author's imagination.

## Case format

`evals/cases/add-status-field.json`:

```json
{
  "id": "add-status-field",
  "prompt": "Add an optional status field to the order response and cover it with a test.",
  "acceptance": [
    "tests pass",
    "lint clean",
    "existing order response fields unchanged",
    "new field documented in the API schema"
  ]
}
```

Acceptance criteria are checkable claims: tests pass, lint clean, behaviour unchanged,
policy followed.

## Running in CI

Trigger on changes to the files that control agent behaviour — the instructions file,
skills, gate config — plus a nightly run to catch model drift.

`.github/workflows/agent-evals.yml`:

```yaml
name: agent-evals
on:
  pull_request:
    paths:
      - "AGENTS.md"
      - "CLAUDE.md"
      - ".claude/skills/**"
      - ".claude/settings.json"
      - "evals/**"
  schedule:
    - cron: "0 2 * * *"

jobs:
  evals:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Run eval suite
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
        run: ./evals/run.sh
```

`evals/run.sh` iterates the cases, runs each prompt non-interactively, checks the
acceptance criteria, and exits non-zero if the pass rate drops below the recorded baseline.

## Gating

A configuration change that moves the pass rate needs review before merge. A drop is not
automatically a veto — sometimes the eval was wrong — but it must be explained.

## Feeding it

Every production incident becomes a permanent case, owned by the team responsible. That is
what stops the same failure recurring.

## Measuring

- Leading: pass-rate trend, and how fast an incident becomes an eval case.
- Lagging: regressions caught in CI versus incidents that reached production.
