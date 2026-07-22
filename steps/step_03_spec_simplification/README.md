# Step 3 — Specification Simplification

Reduce the design's complexity without changing behavior. The output is the simplified `SPEC.md` — the source of truth for everything that follows.

## How it starts

- **Precondition**: step 2 is done — `../step_02_spec_extraction/SPEC.md` fully describes the prototype, and the user agreed it does.
- **Where**: start the AI coding agent inside this folder:

  ```bash
  cd steps/step_03_spec_simplification && claude
  ```

- **Input, read-only**: `../step_02_spec_extraction/SPEC.md` (the raw specification).

## How it iterates

1. **Copy** the raw specification into this folder as the working `SPEC.md`.
2. **Propose reductions**, pass by pass: merge duplicated concepts, remove needless configuration options, unify terminology, clarify responsibilities, simplify APIs, drop historical artifacts left over from exploration.
3. **Justify every removal to the user**: what is removed, and why behavior is preserved without it. The user approves or rejects each reduction.
4. **Invent nothing.** Simplification only removes, merges, and clarifies — it never adds features, concepts, or new design.
5. **Repeat** until a full pass finds nothing left to remove.

## How it ends

- A full pass over the specification finds nothing further to remove without changing behavior, and the user explicitly agrees the specification is minimal and coherent.
- **Hand-off**: this folder's `SPEC.md` becomes **the source of truth**. Steps 4 and 5 read it. From this point on, the step 1 prototype no longer matters — implementations are built and judged from this specification alone.
