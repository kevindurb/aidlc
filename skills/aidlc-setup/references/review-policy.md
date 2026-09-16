# Review policy reference

Goes in `REVIEW.md` at the repo root. It defines the passes every PR gets, so review
coverage does not depend on which reviewer picked it up.

```markdown
# Review policy

Every pull request gets these passes. Findings are ranked by severity and inform the
reviewer; they do not block the merge on their own.

## Passes

1. **Bugs** — logic errors, unhandled cases, race conditions, incorrect error handling.
2. **Security** — authentication and authorisation gaps, injection, secrets in source,
   unvalidated input, data exposure.
3. **Compliance** — data classification, audit logging on state changes, retention rules.
4. **Design principles** — does the change fit the existing architecture, or work around it.

## Severity

**Important** — the change breaks behaviour, or breaches a stated policy. Say what breaks
and under which inputs.

**Nit** — style, naming, and preference. Report at most five per review, and drop the rest.

## Exclusions

- Generated files and vendored dependencies.
- Anything a linter or formatter already enforces in CI.
- Test fixtures, unless the fixture itself is the defect.

## Approval

The author of a change cannot approve it. Findings, fixes, and human sign-off all stay in
the PR history.
```

## Tuning

Adjust the passes to the repo — a library does not need the same compliance pass as a
payments service. Review the ignored-versus-acted-on ratio monthly and cut passes that
produce nothing.
