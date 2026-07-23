---
description: Run one extraction pass — read the step 1 prototype and STEP1_VIBE_DECISIONS.md, draft or extend one STEP2_DIRTY_SPEC.md section, surface every intentional-or-accidental ambiguity as an explicit question, and review the section with the user before moving on.
argument-hint: [optional: which part of the prototype, or which spec section]
allowed-tools: Read, Edit, Write, Grep, Glob, Bash
---

Run one pass of the extraction loop:

1. If STEP2_DIRTY_SPEC.md does not exist yet: first check that `<artifacts>/STEP1_VIBE_DECISIONS.md` contains the CLOSED entry. If it does not, stop — step 1 is not closed. Otherwise create STEP2_DIRTY_SPEC.md with the full required structure from CLAUDE.md, all sections present, `Status: draft`, and immediately fill "Known gaps" from the accepted gaps in the CLOSED entry.
2. Pick the focus: $ARGUMENTS if given, otherwise the least-covered section of STEP2_DIRTY_SPEC.md.
3. Read the relevant prototype code (run it read-only if observing real behavior helps). Draft or extend that one section: what IS, not what should be.
4. Collect every behavior you could not classify and ask the user explicitly, one by one: intentional, accidental-and-excluded, or a known gap? Record each answer in the right section.
5. Show the section to the user and ask: does this correctly describe what was built? Anything missing or over-stated? Apply corrections.
6. Name the sections still thin, and suggest the next /extract-spec focus.

Never modify the prototype, nor `<artifacts>/STEP1_VIBE_DECISIONS.md`. Do not touch any file other than STEP2_DIRTY_SPEC.md.
