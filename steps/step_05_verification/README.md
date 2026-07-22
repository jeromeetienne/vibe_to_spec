# Step 5 — Verification

Verify that the implementation faithfully realizes the specification.

## How it starts

- **Precondition**: step 4 is done — the implementation and its passing
  tests are in `../step_04_implementation/`.
- **Where**: start the AI coding agent inside this folder:

  ```bash
  cd steps/step_05_verification && claude
  ```

- **Inputs, read-only**:
  - `../step_03_spec_simplification/SPEC.md` — the yardstick
  - the implementation — the subject under verification, at the
    external location recorded by the `Implementation:` line of
    `../step_04_implementation/SPECGAPS.md`

  The step 1 prototype is never consulted. Verification is against the
  specification, not against the exploratory prototype.

## How it iterates

1. **Go through the specification item by item**: behavior, architecture,
   API contracts, invariants, completeness.
2. **Check each item against the implementation** and record a verdict in
   `VERIFICATION.md`: conforms / deviates (and how) / missing.
3. **Verify findings before reporting them** — re-check each claimed
   deviation against both the specification and the code (running it where
   needed). No unconfirmed claims enter the report.
4. **Delegate bounded checks** to reviewer subagents where useful; their
   findings go through the same verification before entering the report.

## How it ends

- Every specification item has a verdict in `VERIFICATION.md`.
- **Full conformance** → the cycle is complete: the specification is the
  enduring asset, and the implementation in step 4 realizes it.
- **Gaps found** → the gap list goes back to step 4; after the fixes,
  verification runs again. The loop repeats until `VERIFICATION.md` shows
  full conformance.
