---
description: Run one verification pass — pick unverified specification items, adversarially check each against the implementation (delegating bounded checks to the spec-verifier agent), verify every finding before recording it, and write the verdicts to VERIFICATION.md.
argument-hint: [optional: which spec section or dimension to verify]
allowed-tools: Read, Edit, Write, Grep, Glob
---

Run one verification pass:

1. On first run: check the preconditions — ../step_03_spec_simplification/SPEC.md Status reads "agreed … source of truth", and ../step_04_implementation/SPECGAPS.md contains the CLOSED entry. Stop and name the unclosed phase if not. Then create VERIFICATION.md (structure from CLAUDE.md, Status: in progress, the `Implementation:` line copied from SPECGAPS.md) with the list of every specification item to verify, all pending.
2. Pick the focus: $ARGUMENTS if given, otherwise the next unverified items.
3. Check each item against the implementation: read the code, run the tests, observe the behavior where needed. Delegate bounded per-item checks to the spec-verifier agent, passing it the implementation's location.
4. Verify every finding before recording it — a DEVIATES or MISSING verdict must be reproducible, with concrete evidence.
5. Record the verdicts in VERIFICATION.md. Put AMBIGUOUS items to the user as explicit questions.
6. Report progress: verified and remaining counts, and the current list of deviations.

Never modify the implementation, nor anything under ../step_03_spec_simplification/ or ../step_04_implementation/. Write only VERIFICATION.md in this folder.
