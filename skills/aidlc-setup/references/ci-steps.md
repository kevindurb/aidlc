# CI and deployment reference

Running the agent inside the pipeline moves the judgement steps — triaging a failure,
classifying a flaky test — off the human critical path. Autonomy is earned per environment,
not granted once.

## Escalation order

1. **Read-only first.** Triage failed builds, classify flaky tests, draft changelogs. No
   writes, no credentials beyond read access.
2. **Then writes, via PR only.** Lint fixes, documentation updates. The agent's output is a
   pull request; it never commits to the default branch.
3. **Then deploys, tiered by environment.** Development can be fully autonomous. Staging
   sits in between. Production requires a named human authorisation — see
   `approval-gates.md`.

## Sandboxing

Agent jobs run in a container with an explicit network policy, short-lived tokens, and no
production credentials by default. Scope the token to the job, not to the pipeline.

## Example: triage a failed build

*Concrete invocation shown for Claude Code; substitute your agent's non-interactive flag.*

```yaml
- name: Triage failed build
  if: failure()
  run: >
    claude -p "Read the build log at out/build.log. Identify the most
    likely cause, say whether the failure looks flaky or real, and write a
    three-line summary for the PR thread." >> triage.md
```

## Deployment tooling

Expose deploy, status, and rollback as scoped tools — one set per environment — rather than
letting the agent assemble shell commands against production. An MCP server is one way to
do this.

## Rollback

Rollback is a single command, and it is exercised regularly. A rollback path that has not
been run this quarter is a hypothesis, not a control.

## Measuring

- Leading: share of pipeline failures resolved without human intervention.
- Lagging: the DORA metrics the pipeline already emits.
