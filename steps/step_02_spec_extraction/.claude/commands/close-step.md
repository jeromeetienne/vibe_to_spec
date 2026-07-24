---
description: Run the step-closing ritual — completeness walkthrough of STEP2_DIRTY_SPEC.md against the prototype and STEP1_VIBE_DECISIONS.md, resolve every open ambiguity, then ask the final question "Does this specification fully account for the prototype?". Close only on the user's explicit yes, recorded in the STEP2_DIRTY_SPEC.md Status line.
allowed-tools: Read, Edit, Write, Grep, Glob
---

Run the step-closing ritual:

1. Completeness sweep — walk the prototype's user-visible behaviors and every VALIDATED entry of `<artifacts>/STEP1_VIBE_DECISIONS.md`; check each is accounted for in STEP2_DIRTY_SPEC.md. List anything that is not.
2. List every ambiguity still unresolved in STEP2_DIRTY_SPEC.md, and every section still marked thin or empty without an explicit "None."
3. Re-read the whole document as if you had never seen the prototype: flag any sentence that only makes sense to someone who watched it run, and any concept name that is vague, ambiguous, or unclear on its own.
4. Resolve each open point with the user: specify it, exclude it, or record it as an assumption or known gap. One by one, explicitly.
5. Then ask the final question, verbatim: "Does this specification fully account for the prototype?"
6. Only if the user explicitly answers yes:
   - set the STEP2_DIRTY_SPEC.md status line to: Status: agreed by the user on <date>
   - tell the user step 2 is complete and step 3 can start in ../step_03_spec_cleaning.
7. On any other answer: record what is missing and continue extracting. Never set the agreed status without the user's explicit yes in this session.
