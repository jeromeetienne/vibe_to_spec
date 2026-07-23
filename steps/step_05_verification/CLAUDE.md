# Step 5 — Verification (Vibe to Spec, step 5 of 5)

Goal: verify that the implementation faithfully realizes the specification.

The specification is the yardstick. Not the prototype (never consulted), not taste, not convention. What the spec says must hold; what it does not say is out of scope.

## Inputs — strictly read-only

- `<artifacts>/STEP3_CLEAN_SPEC.md` — the source of truth, including its amendment lines.
- The implementation under verification — at the external location(s) recorded as `clean_impl_resources` in the project's `.vibe_to_spec.yaml` (a repository link or an absolute path; the code is completely outside this repository). You may read it and run it (and its tests). Never modify it. If the location is a repository link, work from a local clone.

Preconditions: the spec's Status line reads "agreed by the user … source of truth", and `<artifacts>/STEP4_IMPL_SPEC_GAPS.md` contains the CLOSED entry. If either is missing, stop and tell the user which step is not closed.

## The stance: adversarial

You are not here to confirm success; you are here to find non-conformance. Check the five dimensions of the methodology:

- behavior — does it do what the spec says, in the specified situations?
- architecture — is it structured the way the spec prescribes?
- API contracts — do the surfaces match, exactly?
- invariants — do the always-true statements actually always hold?
- completeness — is every specified element present?

## The verification loop

1. Pick the next unverified specification items — or the items the user specifies.
2. Check each item against the implementation across the five dimensions above: read the code, run the tests, observe the behavior where needed. Delegate bounded per-item checks to the spec-verifier agent, passing it the implementation's location.
3. Verify every finding before recording it — a DEVIATES or MISSING verdict must be reproducible, with concrete evidence (see below).
4. Record the verdicts in STEP5_IMPL_VERIFICATION.md. Present AMBIGUOUS items to the user as explicit questions — only the user resolves them.
5. Report progress: verified and remaining counts, and the current list of deviations.
6. Repeat until every specification item has a verdict.

Use /verify to run one pass of this loop.

## Findings are verified before they are reported

Every verdict needs concrete evidence: a file read, a test run, a behavior observed. A DEVIATES or MISSING verdict must be reproducible. No unconfirmed claims enter STEP5_IMPL_VERIFICATION.md.

Delegate bounded per-item checks to the spec-verifier agent — a fresh context per item keeps the check independent of the implementer's assumptions. Re-check its DEVIATES / MISSING verdicts yourself before recording them.

## STEP5_IMPL_VERIFICATION.md — the report

    # Verification report
    Status: in progress

    ## Verdicts
    - CONFORMS — <spec item>: <evidence>
    - DEVIATES — <spec item>: spec says X, implementation does Y; <evidence>
    - MISSING — <spec item>: specified, not implemented; <evidence>
    - AMBIGUOUS — <spec item>: <the question blocking a verdict>

    ## Conclusion
    (written by /close-step only)

AMBIGUOUS items are questions for the user — only the user resolves them.

## Ending the step

Use /close-step once every item has a verdict:

- Full conformance → the user explicitly confirms → the cycle is complete.
- Gaps → the gap list is agreed with the user and handed back to step 4; verification runs again after the fixes.

Either conclusion is recorded in STEP5_IMPL_VERIFICATION.md and happens only on the user's explicit confirmation. Never conclude on your own judgment.
