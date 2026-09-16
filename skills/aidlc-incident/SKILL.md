---
name: aidlc-incident
description: Use when a production metric moves outside its baseline, an alert fires, or an incident needs diagnosing and turning into follow-up work. Triggers on error-rate spikes, latency regressions, failed deploys, and post-incident writeups.
---

# Close the loop

Stage 6 of the aidlc loop, and the reason it is a loop. An anomaly in production is
diagnosed and then written back out as a new intent, so the fix re-enters the pipeline
through the same gates as any other change.

## Inputs

An anomaly: a metric outside its baseline, an alert, a failed deploy, or an incident note.

Pick metrics that have a stable rolling baseline — CI test failure rate, post-deploy 5xx
rate, PR cycle time. A metric with no baseline produces noise, not signals.

## Detection is not your job

Detection runs as a deterministic script against statistical rules, with no model involved.
The agent is called in for the response, and how far it may go depends on the deviation:

| Deviation | Response |
|---|---|
| 1 sigma | Log only. No agent. |
| 2 sigma | Diagnose with read-only access. No fixes. |
| 3 sigma | May propose a fix as a PR, or execute a pre-approved runbook. |

Never exceed the tier the deviation warrants, and say which tier you are operating under.

## Steps

1. **Diagnose read-only.** Correlate the anomaly with recent deploys, config changes, and
   logs. State what you can prove and what you are inferring.
2. **Write the finding as a new intent** using `templates/intent.md` in this skill's
   directory. Stage 1 format, no exceptions — the problem is the anomaly, the proposed
   outcome is the fix, the constraints include anything the incident revealed. Write it to
   `docs/features/<slug>/intent.md` with a slug like `2026-09-checkout-5xx-spike`.
3. **Commit it** — `aidlc(<slug>): intent — <one line>` — and hand it to a human to triage
   and approve. It now goes through the pipeline like anything else.
4. **Turn the incident into a regression test.** A production incident becomes a permanent
   eval case owned by the team responsible, so the same failure cannot pass review twice.

## Output

`docs/features/<slug>/intent.md`, committed, plus a named eval case to add.

## Next

`aidlc-intent` approval, then the loop runs again from Stage 2.
