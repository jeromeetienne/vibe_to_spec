---
description: Run one simplification pass — find one reduction candidate in the working STEP3_PRODUCTION_SPEC.md (duplicated concepts, overlapping abstractions, needless options, unclear terms, historical artifacts), propose it with a behavior-preservation justification, and apply it only on the user's explicit approval, logging it to STEP3_SPEC_OPTIMISATION.md.
argument-hint: [optional: which section or concept to attack]
allowed-tools: Read, Edit, Write, Grep, Glob
---

Run one pass of the simplification loop:

1. If STEP3_PRODUCTION_SPEC.md does not exist here yet: check that ../step_02_spec_extraction/STEP2_VIBE_SPEC.md has Status "agreed by the user" (stop if not — step 2 is not closed), then copy it here as STEP3_PRODUCTION_SPEC.md with `Status: draft`, and create STEP3_SPEC_OPTIMISATION.md with a `# Reductions` heading.
2. Pick the focus: $ARGUMENTS if given, otherwise scan for the most obvious reduction candidate.
3. Propose ONE reduction: WHAT is removed, merged, or renamed — WHY the behavior is preserved without it.
4. WAIT for the user's explicit answer. On approval, apply it to STEP3_PRODUCTION_SPEC.md and log it in STEP3_SPEC_OPTIMISATION.md; on rejection, log the REJECTED entry with the user's reason.
5. Say what you would attack next — or state that you find nothing more, in which case suggest /close-step.

Never touch ../step_02_spec_extraction/STEP2_VIBE_SPEC.md. Do not modify any file other than STEP3_PRODUCTION_SPEC.md and STEP3_SPEC_OPTIMISATION.md.
