---
name: aidlc
description: Use when someone asks about the aidlc workflow, which stage a change is in, or what to do next with a change under docs/features/. Also use when asked to start a change and it is unclear where to begin.
---

# aidlc

The SDLC as a loop. Each stage ends by committing an artifact, and that commit starts the
next stage. This skill routes; it does not do the work.

## The loop

| Stage | Artifact | Skill |
|---|---|---|
| 1 Plan | `intent.md` | `aidlc-intent` |
| 2 Design | `spec.md` | `aidlc-spec` |
| 3 Build | `plan.md`, then code | `aidlc-plan` |
| 4 Test | passing build/test/lint with output | `aidlc-verify` |
| 5 Deploy | reviewed PR | `aidlc-review` |
| 6 Maintain | an anomaly written back out as a new `intent.md` | `aidlc-incident` |

Stage 6 feeds Stage 1. That is what makes it a loop rather than a pipeline.

## Where artifacts live

`docs/features/<slug>/` — one folder per change, holding `intent.md`, `spec.md`, `plan.md`.

Slug format: `<yyyy-mm>-<kebab-topic>`, e.g. `docs/features/2026-09-passkey-login/`.

## Routing

1. Ask which change is in question, or infer it from the branch name and recent commits.
2. List `docs/features/<slug>/` and report the highest-numbered artifact present.
3. Name the next skill from the table. If the change has no folder yet, it is pre-Stage 1 —
   start with `aidlc-intent`.
4. If an artifact exists but has not been approved (Stage 1 and 2 both need a human
   accept/reject), say so; the gate is the approval, not the file.

## One-time setup

`aidlc-setup` bootstraps the supporting config a repo needs for stages 3-5: the project
instructions file, `REVIEW.md`, approval gates, evals, and CI steps. Run it once per repo.
