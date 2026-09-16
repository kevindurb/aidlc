---
name: aidlc-setup
description: Use once per repository to set up the supporting config the aidlc workflow needs — project instructions, review policy, approval gates, evals, CI steps. Also use when asked to bootstrap AGENTS.md or CLAUDE.md, add a review policy, or add an approval gate.
---

# Set up the repo

One-time bootstrap for stages 3 to 5. Everything here is config a repo needs once, not work
done per change.

## Steps

1. **Read what exists first.** List the repo root and `.github/workflows/`; check for
   `AGENTS.md`, `CLAUDE.md`, `REVIEW.md`, existing gate scripts, and an evals directory.
   Never write over a file you have not read.
2. **Ask which pieces the repo needs.** Offer the five below and install only what is
   chosen. Installing all five into a repo that wanted one is not helpful.
3. **Propose a diff, then apply.** For each piece, read the matching reference in this
   skill's `references/` directory, adapt it to this repo's actual commands and stack, and
   show the change before writing it. The references are patterns to adapt, not files to
   copy.

## The five pieces

| Piece | Reference | Produces |
|---|---|---|
| Project instructions | `references/project-instructions.md` | `AGENTS.md` or `CLAUDE.md` at the repo root |
| Review policy | `references/review-policy.md` | `REVIEW.md` at the repo root |
| Approval gates | `references/approval-gates.md` | a gate script plus its host wiring |
| Evals | `references/evals.md` | `evals/` cases and a CI workflow |
| CI steps | `references/ci-steps.md` | agent steps in the existing pipeline |

## Which instructions file

Check for `AGENTS.md` first, then `CLAUDE.md`. Extend whichever exists. If both exist, ask
which is authoritative rather than writing to both. If neither exists, ask which the team
wants — `AGENTS.md` is the portable choice, `CLAUDE.md` if the team is standardised on
Claude Code.

## Start with the commands

If you only do one thing, get the build, test, and lint one-liners into the instructions
file. Everything in stage 4 depends on them, and most repos do not have them written down.

## Next

`aidlc-intent` to start a change, or `aidlc` for the loop overview.
