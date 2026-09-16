---
name: aidlc-spec
description: Use when turning an approved intent into a requirements and design spec, or when asked to spec out a change, write requirements, or produce a design document for work that already has an intent.md.
---

# Requirements and design

Stage 2 of the aidlc loop. Requirements and design are produced in one pass, constrained by
the organisation's policies up front rather than discovered in a late review.

## Inputs

`docs/features/<slug>/intent.md`, approved.

If it is missing, stop and produce it first (the `aidlc-intent` skill covers this). If it
exists but is not approved, say so and ask whether to proceed anyway — that is the product
owner's call, not yours.

## Steps

1. **Load the policies.** Note which policy skills or documents are available in this
   session — security, brand, UX, compliance, API conventions. If none are present, say so
   plainly in the spec rather than inventing policy.
2. **Read the intent and the codebase.** The spec designs against what exists, so find the
   modules the change actually lands in before describing the design.
3. **Produce the spec** using `templates/spec.md` in this skill's directory:
   requirements and design for integrating the intent into the existing codebase, conforming
   to the policies available to you, documented fully enough to hand to an engineer.
4. **Flag rather than resolve.** Where policies contradict each other, or where a
   requirement cannot be satisfied within policy, write it under `## Flagged concerns`
   and name the decision needed. Do not quietly pick a side.
5. **Carry the open questions forward.** Every open question in the intent is either
   answered in the spec or restated as still open.
6. **Write and commit** `docs/features/<slug>/spec.md` —
   `aidlc(<slug>): spec — <one line>`. Record which policies were in force, so the commit
   is an auditable record of the decision.

## Output

`docs/features/<slug>/spec.md`, committed.

## The gate

Flagged concerns are resolved with their policy owners before engineering starts. A product
owner approves the spec; high-risk items also need a technical lead.

## Next

`aidlc-plan`.

## Metrics

- Leading: elapsed time between the intent and spec commits.
- Lagging: spec commits made after the first plan commit for the same change — that is
  requirements rework after build started.
