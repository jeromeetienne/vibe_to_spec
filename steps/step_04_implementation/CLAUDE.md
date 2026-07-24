# Step 4 — Implementation (Vibe to Spec, step 4 of 5)

Goal: build production software from the specification — and only from the specification.

The prototype no longer exists for this step. The specification is the only description of what to build. If the specification is not enough to build from, that is a specification gap to resolve — never something to improvise around.

## Input

- `<artifacts>/STEP3_CLEAN_SPEC.md` — the source of truth.

Precondition: its Status line must read "agreed by the user … source of truth". If it does not, stop and tell the user step 3 is not closed.

## Where the implementation lives

The production code is completely OUTSIDE this repository: in its own repository or folder, pointed at via a GitHub link (an https or git URL) or an absolute path on the local disk. This folder keeps only the records.

At the start of the first session, if the active project's `.vibe_to_spec.yaml` has no `clean_impl_resources` entry yet, ask the user where the implementation must live (an existing location, or one to create), add it there as a `path` plus a short `description`, and create STEP4_IMPL_SPEC_GAPS.md right away — before any coding — with the heading:

    # Specification gaps

All code work happens at that location. If it is a repository link rather than a local path, work from a local clone and ask the user where that clone should live.

## Forbidden input

NEVER read the step 1 prototype (wherever `dirty_impl_resources` in the project's `.vibe_to_spec.yaml` points) or `<artifacts>/STEP2_DIRTY_SPEC.md` (the raw spec). If the source of truth seems to be missing something they contained, that is exactly a specification gap: run /spec-gap and ask the user.

## Engineering discipline is fully restored

Step 1 rules do not apply here. This is production engineering:

- The user's global coding rules apply in full.
- Every part gets tests, written alongside the code.
- Clean naming, clear structure, no dead code, no debug leftovers.
- The tech stack comes from the specification's Constraints section; if it is not specified there, that is a spec gap — ask.

## The implementation loop

1. Pick the next specification part to implement (follow the spec's own structure; the user can reorder priorities).
2. Implement it with its tests, exactly as specified. The specification is authoritative — follow it, do not reinterpret it.
3. The moment the specification is ambiguous, incomplete, or looks wrong: STOP work on that point and run /spec-gap. Never silently improvise around the specification, and never silently "fix" it.
4. Before marking the part done, critique it with the implementation-critic subagent (see "Self-critique before the part is done" below). Fix every DEVIATES, MISSING, and UNTESTED finding; turn every IMPROVISED-PAST-A-GAP finding into a /spec-gap. Re-run until it returns CLEAN.
5. Report progress against the specification — which parts are covered, which are not yet — and continue.

## Self-critique before the part is done

Before a specification part counts as done, an implementation-critic subagent reviews it in a fresh context — blind to the assumptions that produced the code. Give it the spec part and the implementation's location (from `clean_impl_resources`). It hunts the one characteristic failure of implementation — improvising past a specification gap instead of raising it — plus deviation from the spec, missing elements, and spec'd behavior left untested.

Act on its verdict yourself: fix deviations, missing elements, and untested behavior; for anything it flags as improvised past a gap, STOP and run /spec-gap — never keep the improvised decision. Re-run until it returns CLEAN. The critic raises the quality of the code before the user reviews it; it never replaces the user's agreement, and it never resolves a specification gap on its own — only the user does that.

## STEP4_IMPL_SPEC_GAPS.md — the gap log

Dated sections, one labeled bullet per event:

- `GAP — <spec section>: what is ambiguous, incomplete, or wrong. Open.`
- `RESOLVED — <spec section>: the user's decision; whether and how the source-of-truth spec was amended.`
- `CLOSED — the user's explicit agreement that the implementation covers the specification with passing tests; accepted leftovers listed, or "none".`

## Amending the source of truth

When the user resolves a gap with a specification change:

1. Apply the agreed fix to `<artifacts>/STEP3_CLEAN_SPEC.md` — the ONLY allowed write outside the external implementation location, and only after the user's explicit decision in a /spec-gap exchange.
2. Add under its Status line: `Amended on <date>: <one-line summary>`.
3. Log the RESOLVED entry in STEP4_IMPL_SPEC_GAPS.md.

The specification stays the single source of truth — the implementation never drifts from it silently, in either direction.

## Ending the step

Step 4 ends ONLY when the user explicitly agrees the implementation covers the specification and the tests pass. Use /close-step. The agreement is recorded as the CLOSED entry in STEP4_IMPL_SPEC_GAPS.md. Step 5 checks that entry as its precondition.
