---
name: aidlc-intent
description: Use when capturing a new idea, request, feature, or problem as a written intent before any design or code. Triggers on "I want to build", "we need", "capture this idea", "write an intent", or the start of any change with no intent.md yet.
---

# Capture intent

Stage 1 of the aidlc loop. The originator of an idea works with the agent to produce an
`intent.md` directly, instead of routing it through backlog refinement and losing ownership
at each handoff.

## Inputs

An informal description of the problem. Nothing else — this is the first artifact.

## Steps

1. **Interview before writing.** Ask clarifying questions and wait for answers. Cover:
   the problem as experienced today, who is affected, scope boundaries and non-goals,
   constraints (security, compliance, deadline), and what success would look like.
   Do not draft the file until the answers are in.
2. **Agree the slug.** `<yyyy-mm>-<kebab-topic>`. Confirm it with the originator.
3. **Fill the template** at `templates/intent.md` in this skill's directory. Every section
   gets content or an explicit "none" — never delete a section to hide a gap.
4. **Read it back.** Show the draft and ask the originator to correct it. This is their
   document in their terms; do not smooth their wording into house style.
5. **Write** `docs/features/<slug>/intent.md`.
6. **Commit** it. `aidlc(<slug>): intent — <one line>`.

## Output

`docs/features/<slug>/intent.md`, committed.

## The gate

An intent is not approved by being written. A product owner accepts or rejects it, and that
decision opens Stage 2 or closes the change. State this explicitly when you finish, and set
`Status: awaiting approval`.

## Next

`aidlc-spec`, once the intent is approved.

## Metrics

- Leading: hours from idea to committed `intent.md`.
- Lagging: share of intents approved into Stage 2.
