# The Five Steps

This folder implements the [Vibe to Spec](https://github.com/jeromeetienne/vibe_to_spec/issues/1) methodology as five working folders, one per phase. Each step folder carries its own Claude Code configuration (`CLAUDE.md`, `.claude/commands/`, `.claude/agents/`), so a Claude Code session started inside one step folder only sees the tooling meant for that phase.

The artifact flow through the five steps:

```
idea
  → step_01: DECISIONS.md        + prototype ……………… external, pointed at
    → step_02: SPEC.md (raw) ……………… reads the external prototype
      → step_03: SPEC.md (simplified — the source of truth)
        → step_04: SPECGAPS.md   + production code …… external, pointed at
          → step_05: VERIFICATION.md … checks the external implementation
```

## The methodology template

Every phase is described by the same eight fields:

| Field | Meaning |
|---|---|
| **Folder** | `steps/step_NN_<name>` — the phase's workspace and the working directory for its Claude Code sessions |
| **Goal** | The one thing the phase achieves (one line) |
| **Question** | The single question the phase answers |
| **Inputs** | The artifacts the phase consumes, with concrete relative paths to the earlier phase that produced them |
| **Outputs** | The artifacts the phase produces, living in its own folder, and which later phase consumes them |
| **Way of working** | The behavioral rules: what is encouraged, tolerated, and forbidden during the phase |
| **Claude Code configuration** | What lives in the phase's `CLAUDE.md`, `.claude/commands/`, `.claude/agents/` — each entry justified by the phase's job |
| **Done when** | The observable exit condition that allows hand-off to the next phase |

Cross-cutting rules that apply to every phase:

- The methodology's records (`DECISIONS.md`, `SPEC.md`, `REDUCTIONS.md`, `SPECGAPS.md`, `VERIFICATION.md`) stay in the folder of the phase that produced them; the next phase reads them through relative sibling paths (`../step_NN_.../`). Nothing is copied forward.
- The project's code is **never** in this repository: the phase 1 prototype and the phase 4 production implementation each live in their own repository or folder, pointed at via a GitHub link (an https or git URL) or an absolute path on the local disk. The pointer is recorded in the phase's log — the `Prototype:` line of `DECISIONS.md`, the `Implementation:` line of `SPECGAPS.md` — so the later phases know where to look.
- The repository root deliberately has **no** root `CLAUDE.md`, so no rules leak between phases. The developer's global `~/.claude/CLAUDE.md` still applies everywhere; a phase's own `CLAUDE.md` overrides it where the phase requires (for example, phase 1 suspends cleanup discipline).
- Configurations stay lean: a slash command for a repeatable phase action; a custom agent only where there is genuinely bounded, delegable work. Continuous creative work stays in the main session.

## The five phases

### `steps/step_01_exploration`

- **Goal**: discover the product.
- **Question**: What do I actually want?
- **Inputs**: the initial idea; nothing formal.
- **Outputs**: a running prototype explicitly validated by the user, living in its own external repository or folder (pointed at by the `Prototype:` line of `DECISIONS.md`) + `DECISIONS.md`, the log of decisions, user validations, gaps, and the final agreement → consumed by step 02.
- **Way of working**: fast and messy; duplication and technical debt are fine; changing direction is encouraged; no tests, no cleanup, no documentation. Continuous explicit user validation: after every increment, ask "is this what you want?" — silence is never agreement.
- **Claude Code configuration**: `CLAUDE.md` that suspends engineering-discipline instincts and defines the validation loop and the `DECISIONS.md` format; three commands — `/checkpoint` (one validation round: show, ask, log), `/log-decision` (log one DECISION / VALIDATED / GAP entry as it happens), `/close-step` (the closing walkthrough and the final agreement). No custom agents.
- **Done when**: the user has explicitly agreed the running prototype is exactly what they want — recorded as the `CLOSED` entry in `DECISIONS.md`, with any accepted gaps listed.
- **Workflow detail**: [step_01_exploration/README.md](step_01_exploration/README.md).

### `steps/step_02_spec_extraction`

- **Goal**: recover the architecture that was actually built.
- **Question**: What did I actually build?
- **Inputs**: the prototype, at the external location recorded in `../step_01_exploration/DECISIONS.md`, plus that `DECISIONS.md` — all treated strictly read-only.
- **Outputs**: `SPEC.md` — the raw specification: concepts, responsibilities, workflows, APIs, data structures, invariants, constraints, assumptions → consumed by step 03.
- **Way of working**: analytical, descriptive; write down what **is**, not what should be; ignore implementation details unless architecturally significant; no fixing, no improving of the prototype.
- **Claude Code configuration**: `CLAUDE.md` with the extraction rules and input paths; one command `/extract-spec` that drives a full extraction pass.
- **Done when**: `SPEC.md` fully accounts for the prototype's observed behavior.
- **Workflow detail**: [step_02_spec_extraction/README.md](step_02_spec_extraction/README.md).

### `steps/step_03_spec_simplification`

- **Goal**: reduce the design's complexity without changing behavior.
- **Question**: What is the simplest design that preserves the same behavior?
- **Inputs**: `../step_02_spec_extraction/SPEC.md`.
- **Outputs**: `SPEC.md` (simplified) — **the source of truth** for everything after → consumed by steps 04 and 05.
- **Way of working**: reductive only — merge duplicated concepts, remove needless options, unify terminology, clarify responsibilities; nothing new is invented; every removal must preserve behavior.
- **Claude Code configuration**: `CLAUDE.md` with the simplification rules; one command `/simplify-spec`.
- **Done when**: nothing further can be removed without changing behavior.
- **Workflow detail**: [step_03_spec_simplification/README.md](step_03_spec_simplification/README.md).

### `steps/step_04_implementation`

- **Goal**: build production software from the specification.
- **Question**: How should this specification be implemented?
- **Inputs**: `../step_03_spec_simplification/SPEC.md` **only** — explicitly not the prototype.
- **Outputs**: the production implementation with its tests, living in its own external repository or folder (pointed at by the `Implementation:` line of `SPECGAPS.md`) → consumed by step 05.
- **Way of working**: full engineering discipline restored (the developer's global style rules apply in full); the specification is authoritative; when it is ambiguous or looks wrong, record the gap and ask — never silently improvise around it.
- **Claude Code configuration**: `CLAUDE.md` stating "the specification is law" plus restored discipline; one command `/spec-gap` that logs specification ambiguities back for resolution (the mirror image of phase 1's `/log-decision`).
- **Done when**: the implementation is complete per the specification and its tests pass.
- **Workflow detail**: [step_04_implementation/README.md](step_04_implementation/README.md).

### `steps/step_05_verification`

- **Goal**: verify the implementation matches the specification.
- **Question**: Does this implementation faithfully realize the specification?
- **Inputs**: `../step_03_spec_simplification/SPEC.md` + the implementation at the external location recorded in `../step_04_implementation/SPECGAPS.md` — never the prototype.
- **Outputs**: `VERIFICATION.md` — a per-item verdict covering behavior, architecture, API contracts, invariants, completeness; gaps are filed back to step 04.
- **Way of working**: adversarial review; the specification is the yardstick; findings are verified before being reported.
- **Claude Code configuration**: `CLAUDE.md` with the verification rules; one command `/verify`; this phase is the natural home for custom reviewer subagents (bounded, delegable checks).
- **Done when**: `VERIFICATION.md` shows full conformance — or its gap list goes back to step 04 and the loop repeats.
- **Workflow detail**: [step_05_verification/README.md](step_05_verification/README.md).
