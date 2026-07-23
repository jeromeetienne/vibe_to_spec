# Vibe to Spec

> Turning exploratory prototypes into maintainable software.

Modern AI has changed how software is created. Instead of designing everything upfront, developers **vibe code**: they converse with an AI coding agent, iterate rapidly, experiment freely, and arrive at a prototype that behaves exactly as desired.

This is one of the fastest ways ever invented to *explore* software. It is also remarkably bad at producing software that can be *maintained*. Exploratory code accumulates duplicated concepts, inconsistent naming, obsolete options, historical artifacts, and architecture that emerged by accident rather than by intention.

**Vibe to Spec** is a methodology that separates **exploration** from **engineering**. The prototype is treated as a disposable exploration artifact. The lasting asset is a clean specification extracted from it. Production code is then regenerated from the specification — never evolved from the exploratory implementation.

> **The specification is permanent. The implementation is disposable.**

The full design essay behind this repository is in [issue #1](https://github.com/jeromeetienne/vibe_to_spec/issues/1).

## Blog series

A companion series of blog posts walks through this project in more depth:

1. [Vibe to Spec: Turning Exploratory Prototypes Into Maintainable Software](docs/blog_posts/01_introduction.md)
2. [How Vibe to Spec Works: A Separate Claude Code Session for Every Phase](docs/blog_posts/02_how_it_works.md)
3. [Vibe to Spec, Applied to Itself: What Self-Review Found, and What I Haven't Solved Yet](docs/blog_posts/03_design_decisions_and_open_questions.md)

## The five phases

```
Exploration → Specification Extraction → Specification Simplification → Implementation → Verification
```

Each phase answers exactly one question and reduces one kind of uncertainty:

| # | Phase | Question it answers | Output |
|---|-------|---------------------|--------|
| 1 | Exploration | What do I actually want? | A running prototype, explicitly validated by the user |
| 2 | Specification Extraction | What did I actually build? | `STEP2_VIBE_SPEC.md` — the raw specification |
| 3 | Specification Simplification | What is the simplest design? | `STEP3_PRODUCTION_SPEC.md` — simplified: **the source of truth** |
| 4 | Implementation | How should it be built? | Production code, with tests |
| 5 | Verification | Does it match the specification? | `STEP5_IMPL_VERIFICATION.md` — evidence-backed verdicts |

## The workflow: Claude changes folder at each step

Each phase is a folder under [steps/](steps/), and each folder carries its **own Claude Code configuration** — its rulebook (`CLAUDE.md`), its slash commands, and (in step 5) its verification agent.

The AI coding agent is launched *inside* the folder of the current phase. As the project advances from one phase to the next, you change folder and launch a fresh session there:

```bash
cd steps/step_01_exploration && claude        # phase 1: explore
```

```bash
cd steps/step_02_spec_extraction && claude    # phase 2: extract the spec
```

```bash
cd steps/step_03_spec_simplification && claude # phase 3: simplify the spec
```

```bash
cd steps/step_04_implementation && claude     # phase 4: implement
```

```bash
cd steps/step_05_verification && claude       # phase 5: verify
```

Each session sees only its phase's tooling — the rules, the commands, and the records of that one step. Nothing leaks between phases: the exploration session is free of engineering discipline, the implementation session has never seen the prototype, the verification session judges only against the specification.

## The project's code lives completely outside this repository

The step folders contain the methodology's configuration and its records — **never the project's code**. The code being explored, specified, implemented, and verified is entirely out of this repository:

- the phase 1 **prototype** lives in its own repository or folder;
- the phase 4 **production implementation** lives in its own repository or folder;
- each is pointed at from here via a **GitHub link** (an https or git URL) or an **absolute path** on the local disk.

The link or path to the external code is given to the agent at the start of the phase and recorded in the phase's log, so the later phases know where to look.

What stays in this repository is exactly what the methodology calls permanent: the specification and the "why" records of every phase. The implementations — prototype and production code alike — are the disposable part, and they live elsewhere.

```
idea
  → step_01: STEP1_VIBE_DECISIONS.md        + prototype ……………… external, pointed at
    → step_02: STEP2_VIBE_SPEC.md ……………… reads the external prototype
      → step_03: STEP3_PRODUCTION_SPEC.md (simplified — the source of truth)
        → step_04: STEP4_IMPL_SPEC_GAPS.md   + production code …… external, pointed at
          → step_05: STEP5_IMPL_VERIFICATION.md … checks the external implementation
                                                    against the specification
```

## The rules that hold it together

- **Only the user closes a phase.** Every step ends with a `/close-step` ritual and an explicit agreement from the user, recorded in that phase's artifact. The agent never declares a phase done on its own.
- **Each phase starts by checking the previous phase's recorded agreement.** The chain is mechanically checkable; a step refuses to start when the one before it is not closed.
- **Every phase keeps a "why" log** — the reasoning that later phases cannot reconstruct from the artifacts alone: `STEP1_VIBE_DECISIONS.md`, `STEP3_SPEC_OPTIMISATION.md`, `STEP4_IMPL_SPEC_GAPS.md`, `STEP5_IMPL_VERIFICATION.md`.
- **From step 4 on, the prototype is forbidden input.** Implementation is built from the specification alone; if the specification is not enough, that is a specification gap to resolve with the user — never something to improvise around.

## Going further

- [steps/README.md](steps/README.md) — the methodology template and the five phases at a glance
- Each step's own `README.md` — how that step starts, iterates, and ends
- [Issue #1](https://github.com/jeromeetienne/vibe_to_spec/issues/1) — the full design essay; [issue #2](https://github.com/jeromeetienne/vibe_to_spec/issues/2) — the implementation record of this scaffolding
