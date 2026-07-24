---
description: Run one cleaning pass — find one reduction candidate in the working STEP3_CLEAN_SPEC.md (duplicated concepts, overlapping abstractions, needless options, unclear terms, historical artifacts), propose it with a behavior-preservation justification, critique it with a fresh cleaning-critic subagent, and apply it only on the user's explicit approval, logging it to STEP3_SPEC_OPTIMISATION.md.
argument-hint: [optional: which section or concept to attack]
allowed-tools: Read, Edit, Write, Grep, Glob
---

Run one pass of the cleaning loop:

1. If STEP3_CLEAN_SPEC.md does not exist yet: check that `<artifacts>/STEP2_DIRTY_SPEC.md` has Status "agreed by the user" (stop if not — step 2 is not closed), then copy it to `<artifacts>/STEP3_CLEAN_SPEC.md` with `Status: draft`, and create STEP3_SPEC_OPTIMISATION.md with a `# Reductions` heading.
2. Pick the focus: $ARGUMENTS if given, otherwise scan for the most obvious reduction candidate.
3. Propose ONE reduction: WHAT is removed, merged, or renamed — WHY the behavior is preserved without it.
4. Before showing the user, critique the proposal with the cleaning-critic subagent (fresh context), passing the spec text before and after. If it flags a DISGUISED-PRODUCT-CHANGE, re-present the change to the user as a product decision (a PRODUCT entry), not a cleaning; if it flags BEHAVIOR-NOT-PRESERVED or INVENTED, fix or drop the proposal. Re-run until it returns CLEAN.
5. WAIT for the user's explicit answer. On approval, apply it to STEP3_CLEAN_SPEC.md and log it in STEP3_SPEC_OPTIMISATION.md; on rejection, log the REJECTED entry with the user's reason.
6. Say what you would attack next — or state that you find nothing more, in which case suggest /close-step.

Never touch `<artifacts>/STEP2_DIRTY_SPEC.md`. Do not modify any file other than STEP3_CLEAN_SPEC.md and STEP3_SPEC_OPTIMISATION.md.
