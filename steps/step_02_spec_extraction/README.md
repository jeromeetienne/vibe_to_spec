# Step 2 — Specification Extraction

Recover the architecture that was actually built. This step turns the validated prototype of step 1 into `STEP2_VIBE_SPEC.md` — the raw specification of what the prototype is and does.

## How it starts

- **Precondition**: step 1 is closed — the prototype in `../step_01_exploration/` runs, and its `STEP1_VIBE_DECISIONS.md` ends with the `CLOSED` entry (the user's explicit agreement).
- **Where**: start the AI coding agent inside this folder:

  ```bash
  cd steps/step_02_spec_extraction && claude
  ```

- **Inputs, strictly read-only**:
  - the prototype — at the external location recorded by the `Prototype:` line of `../step_01_exploration/STEP1_VIBE_DECISIONS.md` (its code is completely outside this repository)
  - `../step_01_exploration/STEP1_VIBE_DECISIONS.md` — the validations, gaps, and pivots, with the reasoning behind them

## How it iterates

1. **Read** the prototype and `STEP1_VIBE_DECISIONS.md`; observe what the prototype actually does.
2. **Write** `STEP2_VIBE_SPEC.md` in this folder, section by section: concepts, responsibilities, workflows, APIs, data structures, invariants, constraints, assumptions.
3. **Describe what IS, not what should be.** Implementation details are ignored unless architecturally significant. Gaps the user explicitly accepted at the end of step 1 are recorded in the spec as known gaps — never silently "fixed" on paper.
4. **Review with the user**, section by section: does the spec say what the prototype does? Is anything missing or over-stated?
5. **Repeat** until the spec accounts for all observed behavior.

The prototype is never modified in this step — it is an input, not a workspace. Nothing is improved, nothing is redesigned; that comes later.

## How it ends

- `STEP2_VIBE_SPEC.md` fully accounts for the prototype's observed behavior, and the user explicitly agrees it does.
- **Hand-off**: `STEP2_VIBE_SPEC.md` stays in this folder; step 3 (`../step_03_spec_simplification`) reads it.
