---
description: Log a specification gap — the spec is ambiguous, incomplete, or looks wrong on some point — ask the user to resolve it, and on an agreed spec change amend the source-of-truth STEP3_CLEAN_SPEC.md in step 3. Use the moment the spec fails to answer an implementation question; never improvise around it.
argument-hint: [which spec section, and what is unclear]
allowed-tools: Read, Edit, Write, Grep, Glob
---

Run one gap-resolution exchange:

1. Create STEP4_IMPL_SPEC_GAPS.md if it does not exist yet: the `# Specification gaps` heading. If the project's `.vibe_to_spec.yaml` has no `clean_impl_resources` entry yet, ask the user for the implementation's location and description and add it there.
2. State the gap precisely: which specification section, what it says, and why that is not enough to implement from (ambiguous, incomplete, or wrong). Base it on $ARGUMENTS if given, otherwise on the point just hit.
3. Log it as a GAP entry (open), then ask the user to resolve it. Propose options if helpful, but the decision is the user's.
4. WAIT for the user's decision. Then:
   - log the RESOLVED entry with the user's decision;
   - if the resolution changes the specification: apply the agreed fix to `<artifacts>/STEP3_CLEAN_SPEC.md` and add `Amended on <date>: <one-line summary>` under its Status line;
   - continue implementing from the amended specification.

Do not write to any file other than STEP4_IMPL_SPEC_GAPS.md and `<artifacts>/STEP3_CLEAN_SPEC.md`, and only for the agreed fix.
