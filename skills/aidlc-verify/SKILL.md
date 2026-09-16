---
name: aidlc-verify
description: Use before reporting any coding task complete, and when asked to verify a change, prove it works, check the build, or fix a bug. Also use when setting up how the agent checks its own work.
---

# Verify your own work

Stage 4 of the aidlc loop. The agent proves the change works before a human looks at it,
instead of waiting for CI minutes later or QA days later.

## Inputs

The Commands section of the project instructions file — `AGENTS.md` or `CLAUDE.md`,
whichever this repo uses. Check `AGENTS.md` first, then `CLAUDE.md`.

If it names no build/test/lint command, or the commands are multi-step recipes rather than
single invocations, stop and say so: the loop cannot run without them. Fixing that is a
one-time setup job (the `aidlc-setup` skill covers it) — write the one-liners into the
instructions file, each exiting non-zero on failure.

## Steps

1. **Run all three** — build, test, lint — and paste the real output. Not a summary of it.
2. **Name the target before you start.** Verification needs something falsifiable: "all
   tests in `test_status.py` pass", "the endpoint returns 200 with the new field", "the
   screenshot matches the attached mock". "It looks right" is not a target.
3. **Never edit a test to make it pass.** If a test fails, the code is wrong until proven
   otherwise. Deleting or skipping a failing test is not a fix.
4. **Bug fixes go test-first.** Reproduce the bug as a failing test, confirm it fails for
   the stated reason, then change only the code until it passes. The test is not touched
   after it fails.
5. **UI work verifies visually.** Take a screenshot, compare it against the mock, and
   iterate. Two or three rounds is normal; one round is usually not enough.
6. **Hand the final check to a fresh context.** A verifier with no memory of writing the
   code catches what the author misses. Use a delegated subagent if your agent offers one —
   this plugin ships a `verifier` subagent for hosts that support them — otherwise run the
   same commands in a clean session.

## Output

Toolchain output pasted into the task thread: test results, build log, screenshot diff.
Mechanical evidence, not a claim.

## The gate

Make it mandatory in the project instructions file, so it applies to every task rather than
the ones you remember:

```
## Verifying your work

- Build: make build (must finish with "Build succeeded")
- Test: make test (all green; never skip or delete a failing test)
- Lint: make lint (zero warnings)

Run all three before reporting any task complete, and paste the output.
If a test fails, fix the code, not the test.
```

For absolute enforcement, pair it with a pre-tool gate that blocks edits to test files
during a fix task, or check it during review.

## Next

`aidlc-review`.

## Metrics

- Leading: first-pass CI success rate for agent-written changes.
- Lagging: review time per PR, and change failure rate from incident tracking.
