# aidlc

Skills that run a change through the AI-native SDLC loop. Each stage ends by committing an
artifact, and that commit starts the next stage.

Based on Anthropic's [AI-Native SDLC Playbook](https://academy.claude.com/courses/ai-native-sdlc-playbook/introduction).

## The loop

| Stage | Artifact | Skill |
|---|---|---|
| 1 Plan | `intent.md` | `aidlc-intent` |
| 2 Design | `spec.md` | `aidlc-spec` |
| 3 Build | `plan.md`, then code | `aidlc-plan` |
| 4 Test | build/test/lint output | `aidlc-verify` |
| 5 Deploy | reviewed PR | `aidlc-review` |
| 6 Maintain | anomaly written back out as a new `intent.md` | `aidlc-incident` |

Stage 6 feeds stage 1, which is what makes it a loop. `aidlc` routes between stages, and
`aidlc-setup` bootstraps the config a repo needs once.

Artifacts live in `docs/features/<slug>/`, one folder per change, slugged
`<yyyy-mm>-<kebab-topic>`.

## Install

As a Claude Code plugin:

```
/plugin marketplace add ~/projects/aidlc
/plugin install aidlc@aidlc
```

Or with the [`skills`](https://github.com/vercel-labs/skills) CLI, which installs into any
of its supported agents:

```bash
npx skills add kevindurb/aidlc --list     # see what is available
npx skills add kevindurb/aidlc            # pick interactively
```

The plugin path also installs the `verifier` subagent. The CLI path installs skills only;
`aidlc-verify` falls back to a clean session for its final check.

## Design notes

Skill bodies are agent-agnostic — they say "plan mode" and "the project instructions file
(`AGENTS.md` or `CLAUDE.md`)", and vendor commands appear only as labelled examples. The
`.claude-plugin/` manifests and `agents/verifier.md` frontmatter are the two host-specific
files.

Each skill directory is self-contained, so installing one skill without its neighbours
works. Skills reference each other by name, never by path.
