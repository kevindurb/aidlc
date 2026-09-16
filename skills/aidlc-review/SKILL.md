---
name: aidlc-review
description: Use when reviewing a pull request or the current diff against the team's review policy, when asked to review code before merge, or when setting up REVIEW.md. Also use when addressing review feedback left on a change.
---

# Review before merge

Stage 5 of the aidlc loop. Every PR gets the same set of passes, so human attention goes to
intent and risk rather than reading every line of every diff.

## Inputs

`REVIEW.md` at the repo root, defining the passes this team runs.

If it is absent, offer to create it from `templates/REVIEW.md` in this skill's directory,
adapted to the repo. Do not review against an imagined policy.

If your agent already has a review command installed, prefer it and use this skill only for
the policy and the thresholds — there is no value in running two reviewers.

## Steps

1. **Establish the target.** The current diff against the base branch, or a named PR.
2. **Run each pass in `REVIEW.md` separately.** Passes find different things; merging them
   into one read loses the security findings behind the bug findings.
3. **Rank by severity.** Important findings first, each with the failing input or the policy
   clause it breaches. Then nits, capped at five.
4. **Apply the exclusions.** Do not report what CI already enforces, and do not review
   generated files.
5. **Report, do not block.** Findings inform a human decision. Nothing here is a merge gate
   on its own.
6. **Fix on request.** When a reviewer asks for a change, make it and push; keep the
   conversation in the PR thread so the audit trail stays in one place.
7. **Feed repeats back.** A finding that recurs across PRs belongs in the project
   instructions file (`AGENTS.md` or `CLAUDE.md`) as a convention, not in review comment
   number nine.

## The gate

The author cannot approve their own change. Branch protection requires explicit human
sign-off; the review output is evidence for that decision, not a substitute for it.

## Tuning

Once a month, rate the findings that were acted on versus ignored, and adjust `REVIEW.md`.
Rising nit volume means the cap is too high or a linter rule is missing.

## Next

Merge and deploy. Anomalies after deploy go to `aidlc-incident`.

## Metrics

- Leading: time to first review, and share of comments resolved without a human touching
  the branch.
- Lagging: defects and vulnerabilities caught before merge versus those reaching
  production.
