---
description: Run one verification pass — pick unverified specification items, adversarially check each against the implementation (delegating bounded checks to the spec-verifier agent), verify every finding before recording it, and write the verdicts to STEP5_IMPL_VERIFICATION.md.
argument-hint: [optional: which spec section or dimension to verify]
allowed-tools: Read, Edit, Write, Grep, Glob, Bash
---

Run one verification pass:

1. On first run: check the preconditions — ../step_03_spec_cleaning/STEP3_PRODUCTION_SPEC.md Status reads "agreed … source of truth", and ../step_04_implementation/STEP4_IMPL_SPEC_GAPS.md contains the CLOSED entry. Stop and name the unclosed step if not. Then create STEP5_IMPL_VERIFICATION.md (structure from CLAUDE.md, Status: in progress, the `Implementation:` line copied from STEP4_IMPL_SPEC_GAPS.md) with the list of every specification item to verify, all pending.
2. Pick the focus: $ARGUMENTS if given, otherwise the next unverified items.
3. Check each item against the implementation: read the code, run the tests, observe the behavior where needed. Delegate bounded per-item checks to the spec-verifier agent, passing it the implementation's location.
4. Verify every finding before recording it — a DEVIATES or MISSING verdict must be reproducible, with concrete evidence.
5. Record the verdicts in STEP5_IMPL_VERIFICATION.md. Put AMBIGUOUS items to the user as explicit questions.
6. Report progress: verified and remaining counts, and the current list of deviations.

Never modify the implementation, nor anything under ../step_03_spec_cleaning/ or ../step_04_implementation/. Write only STEP5_IMPL_VERIFICATION.md in this folder.
