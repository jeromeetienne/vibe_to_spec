# Step 4 — Implementation

Build production software from the specification — and only from the
specification.

## How it starts

- **Precondition**: step 3 is done —
  `../step_03_spec_simplification/SPEC.md` is the agreed source of truth.
- **Where**: start the AI coding agent inside this folder:

  ```bash
  cd steps/step_04_implementation && claude
  ```

- **Input**: `../step_03_spec_simplification/SPEC.md` — and nothing else.
  The step 1 prototype is explicitly NOT an input: nobody reads it here,
  neither the user nor the agent.

## How it iterates

1. **Full engineering discipline is back**: the developer's global coding
   rules apply, tests are written alongside the code, the code is clean
   and maintainable.
2. **Implement the specification part by part.** The specification is
   authoritative — the implementation follows it, never reinterprets it.
3. **When the specification is ambiguous, incomplete, or looks wrong**:
   stop on that point, log it with `/spec-gap`, and ask the user. Never
   silently improvise around the specification.
4. **Agreed specification fixes are applied to the step 3 `SPEC.md`** —
   the source of truth stays true — and then implementation continues
   from the corrected specification.

## How it ends

- The implementation covers the specification completely, and its tests
  pass.
- **Hand-off**: the implementation and its tests stay in this folder;
  step 5 (`../step_05_verification`) checks them against the
  specification.
- If the implementation later becomes hard to maintain, this folder can
  be discarded and rebuilt from the specification — the implementation is
  disposable, the specification is permanent.
