---
description: Run the step-closing ritual — one full sweep over SPEC.md for remaining reduction candidates, a behavior-preservation check against the raw spec, then the final question "Is this the simplest design that preserves the behavior?". Close only on the user's explicit yes, recording SPEC.md as the source of truth.
allowed-tools: Read, Edit, Write, Grep, Glob
---

Run the step-closing ritual:

1. Full sweep: go through every SPEC.md section once more hunting for reduction candidates. If any are found, present them — the step is not ready to close; go back to /simplify-spec.
2. Preservation check against ../step_02_spec_extraction/SPEC.md: every behavior of the raw spec must be either present in the simplified spec or accounted for by an entry in REDUCTIONS.md (including PRODUCT entries for user-decided changes). List anything unaccounted for and resolve it with the user, one by one.
3. Then ask the final question, verbatim: "Is this the simplest design that preserves the behavior?"
4. Only if the user explicitly answers yes:
   - set the Status line to: Status: agreed by the user on <date> — source of truth
   - tell the user step 3 is complete, this SPEC.md is now the source of truth, and step 4 can start in ../step_04_implementation.
5. On any other answer: log what remains and continue simplifying. Never set the status yourself.
