# Step 3 — Specification Cleaning (Vibe to Spec, step 3 of 5)

Goal: reduce the design's complexity without changing behavior. The output of this step — the cleaned STEP3_CLEAN_SPEC.md — becomes the source of truth for everything after.

This is the most important step of the methodology: instead of cleaning code, it cleans the design itself.

## Input — strictly read-only

- `<artifacts>/STEP2_DIRTY_SPEC.md` — the raw specification.

Precondition: the raw specification's Status line must read "agreed by the user". If it does not, stop and tell the user step 2 is not closed.

## The working copy

The first /clean-spec run copies the raw specification into `<artifacts>/STEP3_CLEAN_SPEC.md` with `Status: draft`. All cleaning happens on this working copy. The raw spec from step 2 is never touched — it remains the record of what was actually extracted.

## The cleaning loop

1. Read the working STEP3_CLEAN_SPEC.md and hunt for ONE reduction candidate:
   - duplicated concepts (two names for one thing)
   - overlapping abstractions that can merge
   - configuration options nobody needs
   - unclear terminology that can be unified
   - responsibilities that can be clarified or merged
   - APIs with needless surface
   - historical artifacts left over from exploration
2. Propose ONE reduction, justified in this exact shape: WHAT is removed, merged, or renamed — WHY the behavior is preserved without it.
3. Before showing the user, critique the proposal with the cleaning-critic subagent (see "Self-critique before review" below). Revise, drop, or reclassify it as a product decision until the critic returns CLEAN.
4. The user approves or rejects. Never apply a reduction the user has not explicitly approved.
5. Apply approved reductions to STEP3_CLEAN_SPEC.md and log them in STEP3_SPEC_OPTIMISATION.md.
6. Repeat.

One reduction at a time: small, reviewable, reversible.

## Self-critique before review

Before the user sees a proposed reduction, a cleaning-critic subagent reviews it in a fresh context — blind to the reasoning that produced it. Give it the reduction and the spec text before and after. It hunts the one characteristic failure of cleaning — a behavior change disguised as a reduction — plus a behavior-preservation justification that does not hold, and anything newly invented.

Act on its verdict yourself: if it flags a disguised product change, re-present the change to the user as a product decision (a PRODUCT entry), never as an ordinary cleaning; if it flags behavior-not-preserved or invented, fix or drop the proposal. Re-run until it returns CLEAN. The critic raises the quality of the proposal; it never replaces the user's approval, and it never decides anything only the user may decide.

## STEP3_SPEC_OPTIMISATION.md — the reduction log

Dated sections, one labeled bullet per decision. Entry kinds:

- `MERGED — <a> + <b> → <c>: why they were one thing`
- `REMOVED — <what>: why behavior is preserved without it`
- `RENAMED — <old> → <new>: why the new term is clearer`
- `REJECTED — <proposal>: why the user declined it`
- `PRODUCT — <change>: a behavior change the USER decided (see below)`

This log is the "why" of the cleaning. Later steps can trust the cleaned spec without re-litigating every choice.

## The hard boundary: behavior is preserved

- Never propose a change that alters behavior. This step cleans the DESIGN; it does not reduce the PRODUCT.
- Nothing new is invented: no new features, no new concepts, no new design. Only remove, merge, and clarify.
- If the USER decides during review to drop or change a behavior, that is a product decision, not a cleaning. Record it as a PRODUCT entry in STEP3_SPEC_OPTIMISATION.md and apply it — clearly separated, never mixed into an ordinary reduction.

## Ending the step

Step 3 ends ONLY when a full pass finds nothing further to remove and the user explicitly agrees. Use /close-step. The agreement is recorded in the STEP3_CLEAN_SPEC.md Status line:

    Status: agreed by the user on <date> — source of truth

From that moment this file is the source of truth: steps 4 and 5 read it, and the step 1 prototype no longer matters.
