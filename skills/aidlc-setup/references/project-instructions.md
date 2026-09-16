# Project instructions reference

Goes in `AGENTS.md` or `CLAUDE.md` at the repo root. Extend whichever already exists; if
neither does, ask which the team wants.

The test for what belongs here: would a competent developer joining on Monday need it on
day one? If not, leave it out. Stay under one page — every line costs context on every
task.

## Four categories, nothing else

```markdown
# <project name>

One or two lines on what this service does and who uses it.

## Commands

- Build: `make build` (finishes with "Build succeeded")
- Test: `make test` (all green)
- Lint: `make lint` (zero warnings)
- Run locally: `make dev` (serves on :3000)

## Conventions

- Go 1.23; no new dependencies without discussion.
- Money is always minor units as int64, never float.
- Every exported function has a test.
- Errors wrap with `fmt.Errorf("...: %w", err)`.

## Architecture

- `cmd/` entrypoints, `internal/` everything else, `internal/store/` is the only package
  that talks to the database.
- HTTP handlers are thin; logic lives in `internal/<domain>/`.

## Common mistakes

- Do not add migrations by hand; use `make migrate-new`.
- The staging config is not a copy of production; check `config/staging.yaml`.

## Verifying your work

Run build, test, and lint before reporting any task complete, and paste the output.
If a test fails, fix the code, not the test.
```

## Keeping it useful

Generate a first draft with whatever init or scaffold command your agent provides, then cut
it down — generated versions are usually three times too long.

The working rule: when the agent makes the same mistake twice, the correction goes in the
Common mistakes section. Twice, not once; once is noise.

Check it into git at the repo root so changes are reviewed in PRs like any other code, with
code owners on the file.
