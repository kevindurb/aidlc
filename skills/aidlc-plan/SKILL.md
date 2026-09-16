---
name: aidlc-plan
description: Use before writing code for a change that has an approved spec, or when asked to plan an implementation, produce a plan.md, or decide the order of work. Use when someone says "plan this out" or "how should we build this".
---

# Plan before building

Stage 3 of the aidlc loop. Design review happens before any code is generated, while
changing course is still a matter of editing a document.

## Inputs

`docs/features/<slug>/spec.md`, approved. The intent too, for the problem framing.

If the spec is missing, stop — plan from a spec, not from a conversation (the `aidlc-spec`
skill produces one).

## Steps

1. **Go read-only.** Use plan mode if your agent has it; otherwise explore without writing.
   No file may change until the plan is approved.
2. **Explore the real code paths** the spec names. A plan that guesses at file locations is
   worthless.
3. **Draft the plan** using `templates/plan.md` in this skill's directory — files that
   change, order of work, risks, proof — with a header referencing the intent and spec and
   their dates.
4. **Interrogate your own plan** before showing it, and put the answers in the Risks
   section: What could this change break? Which step carries the most risk? What
   alternatives were rejected, and why?
5. **Iterate with the engineer.** Expect the plan to be challenged. Revise the document,
   not the code.
6. **Write and commit** `docs/features/<slug>/plan.md` —
   `aidlc(<slug>): plan — <one line>`. Then implement it.

## Splitting the work

Split by file overlap, using the file list from the plan:

- Tasks that touch the same files run in one session, one after another.
- Tasks with no file overlap can run concurrently, each in its own worktree so the
  checkouts cannot collide. Start with two or three; add more only as fast as the work can
  be reviewed.
- If your agent supports it, launch each in an isolated worktree directly — Claude Code
  does this with `claude --worktree <branch>`.

## Output

`docs/features/<slug>/plan.md`, committed before implementation starts.

## The gate

Routine changes are approved by the engineer. Higher-risk work goes to a tech lead or
architect. The approval is of the plan, before code exists.

## Next

Implement, then `aidlc-verify`.

## Metrics

- Leading: first-pass merge rate and time to merge.
- Lagging: rework cycles per change, and how far the final diff drifted from the committed
  plan.
