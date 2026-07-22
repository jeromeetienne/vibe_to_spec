# Step 3 — Specification Simplification (Vibe to Spec, step 3 of 5)

Goal: reduce the design's complexity without changing behavior. The output of this step — the simplified SPEC.md — becomes the source of truth for everything after.

This is the most important step of the methodology: instead of simplifying code, it simplifies the design itself.

## Input — strictly read-only

- ../step_02_spec_extraction/SPEC.md — the raw specification.

Precondition: the raw specification's Status line must read "agreed by the user". If it does not, stop and tell the user step 2 is not closed.

## The working copy

The first /simplify-spec run copies the raw specification into this folder as SPEC.md with `Status: draft`. All simplification happens on this working copy. The raw spec in step 2 is never touched — it remains the record of what was actually extracted.

## The simplification loop

1. Read the working SPEC.md and hunt for ONE reduction candidate:
   - duplicated concepts (two names for one thing)
   - overlapping abstractions that can merge
   - configuration options nobody needs
   - unclear terminology that can be unified
   - responsibilities that can be clarified or merged
   - APIs with needless surface
   - historical artifacts left over from exploration
2. Propose the reduction to the user, justified in this exact shape: WHAT is removed, merged, or renamed — WHY the behavior is preserved without it.
3. The user approves or rejects. Never apply a reduction the user has not explicitly approved.
4. Apply approved reductions to SPEC.md and log them in REDUCTIONS.md.
5. Repeat.

One reduction at a time: small, reviewable, reversible.

## REDUCTIONS.md — the reduction log

Dated sections, one labeled bullet per decision. Entry kinds:

- `MERGED — <a> + <b> → <c>: why they were one thing`
- `REMOVED — <what>: why behavior is preserved without it`
- `RENAMED — <old> → <new>: why the new term is clearer`
- `REJECTED — <proposal>: why the user declined it`
- `PRODUCT — <change>: a behavior change the USER decided (see below)`

This log is the "why" of the simplification. Later steps can trust the simplified spec without re-litigating every choice.

## The hard boundary: behavior is preserved

- Never propose a change that alters behavior. This step simplifies the DESIGN; it does not reduce the PRODUCT.
- Nothing new is invented: no new features, no new concepts, no new design. Only remove, merge, and clarify.
- If the USER decides during review to drop or change a behavior, that is a product decision, not a simplification. Record it as a PRODUCT entry in REDUCTIONS.md and apply it — clearly separated, never mixed into an ordinary reduction.

## Ending the step

Step 3 ends ONLY when a full pass finds nothing further to remove and the user explicitly agrees. Use /close-step. The agreement is recorded in the SPEC.md Status line:

    Status: agreed by the user on <date> — source of truth

From that moment this file is the source of truth: steps 4 and 5 read it, and the step 1 prototype no longer matters.
