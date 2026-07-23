---
description: Run the step-closing ritual — resolve any still-open spec gaps, walk the implementation against every specification section, run the full test suite for real, then ask the final question "Does this implementation cover the specification, with passing tests?". Close only on the user's explicit yes, recorded as the CLOSED entry in STEP4_IMPL_SPEC_GAPS.md.
allowed-tools: Read, Edit, Write, Grep, Glob, Bash
---

Run the step-closing ritual:

1. Read STEP4_IMPL_SPEC_GAPS.md: every GAP must be RESOLVED. If any are open, resolve them with the user first (the /spec-gap flow).
2. Coverage walkthrough: go through the source-of-truth specification section by section; show where each is implemented and which tests cover it. List anything not covered.
3. Run the full test suite and show the real result. If tests fail, the step cannot close — say so plainly and fix before returning here.
4. Then ask the final question, verbatim: "Does this implementation cover the specification, with passing tests?"
5. Only if the user explicitly answers yes:
   - append the CLOSED entry to STEP4_IMPL_SPEC_GAPS.md (accepted leftovers listed, or "none");
   - tell the user step 4 is complete and step 5 can start in ../step_05_verification.
6. On any other answer: log the missing points and continue. Never close on your own judgment.
