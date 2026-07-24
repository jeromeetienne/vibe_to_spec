# Vibe to Spec

> Turning vibe-coding into maintainable software.

Modern AI has changed how software is created. Instead of designing everything upfront, developers **vibe code**: they converse with an AI coding agent, iterate rapidly, experiment freely, and arrive at a prototype that behaves exactly as desired.

This is one of the fastest ways ever invented to *explore* software. It is also remarkably bad at producing software that can be *maintained*. Exploratory code accumulates duplicated concepts, inconsistent naming, obsolete options, historical artifacts, and architecture that emerged by accident rather than by intention.

**Vibe to Spec** is a methodology that separates **exploration** from **engineering**. The prototype is treated as a disposable exploration artifact. The lasting asset is a clean specification extracted from it. Production code is then regenerated from the specification — never evolved from the exploratory implementation.

> **The specification is permanent. The implementation is disposable.**

## Blog series

A companion series of blog posts walks through this project in more depth:

1. [Vibe to Spec: Turning Exploratory Prototypes Into Maintainable Software](docs/blog_posts/01_introduction.md)
2. [How Vibe to Spec Works: A Separate Claude Code Session for Every Phase](docs/blog_posts/02_how_it_works.md)
3. [Vibe to Spec, Applied to Itself: What Self-Review Found, and What I Haven't Solved Yet](docs/blog_posts/03_design_decisions_and_open_questions.md)

## The five phases

```
Exploration → Dirty Spec Extraction → Spec Cleaning → Implementation → Verification
```

Each phase answers exactly one question and reduces one kind of uncertainty (see the [steps overview](./steps/README.md) for a walkthrough of each):

| # | Phase | Question it answers | Output |
|---|-------|---------------------|--------|
| 1 | [Exploration](./steps/step_01_exploration/README.md) | What do I actually want? | A running prototype, explicitly validated by the user |
| 2 | [Dirty Spec Extraction](./steps/step_02_spec_extraction/README.md) | What did I actually build? | `STEP2_DIRTY_SPEC.md` — the raw specification |
| 3 | [Spec Cleaning](./steps/step_03_spec_cleaning/README.md) | What is the simplest design? | `STEP3_CLEAN_SPEC.md` — cleaned: **the source of truth** |
| 4 | [Implementation](./steps/step_04_implementation/README.md) | How should it be built? | Production code, with tests |
| 5 | [Verification](./steps/step_05_verification/README.md) | Does it match the specification? | `STEP5_IMPL_VERIFICATION.md` — evidence-backed verdicts |

## How to install

You need [Claude Code](https://claude.com/claude-code) installed and a working `git`. This repository *is* the methodology — there is nothing to build or compile. Installing it means cloning it and pointing it at your own project.

1. Clone the repository:

```bash
git clone git@github.com:jeromeetienne/vibe_to_spec.git
cd vibe_to_spec
```

2. Launch Claude Code inside the folder of the step you are starting from (step 1 for a brand-new project):

```bash
cd steps/step_01_exploration && claude
```

3. On the first session, there is no active project yet. Claude Code reads the root `CLAUDE.md`, sees that `.active_project.vibe_to_spec.yaml` is missing, and asks you which project to work on. It then creates `projects/<project-name>.vibe_to_spec.yaml` for you and records where your artifacts folder and your prototype and production code live. From then on, every session confirms the active project before doing anything else.

Both `projects/*.vibe_to_spec.yaml` and the `.active_project.vibe_to_spec.yaml` symlink are git-ignored: they hold paths specific to your machine and are never committed.

## The workflow: Claude changes folder at each step

Each phase is a folder under [steps/](steps/), and each folder carries its **own Claude Code configuration** — its rulebook (`CLAUDE.md`), its slash commands, and (in step 5) its verification agent.

The AI coding agent is launched *inside* the folder of the current phase. As the project advances from one phase to the next, you change folder and launch a fresh session there:

```bash
cd steps/step_01_exploration && claude        # phase exploration
```

```bash
cd steps/step_02_spec_extraction && claude    # phase dirty-spec-extraction
```

```bash
cd steps/step_03_spec_cleaning && claude # phase spec-cleaning
```

```bash
cd steps/step_04_implementation && claude     # phase implementation
```

```bash
cd steps/step_05_verification && claude       # phase verification
```

Each session sees only its phase's tooling — the rules, the commands, and the records of that one step. Nothing leaks between phases: the exploration session is free of engineering discipline, the implementation session has never seen the prototype, the verification session judges only against the specification. This is the [separation of concerns](https://en.wikipedia.org/wiki/Separation_of_concerns) principle applied to the workflow itself: each phase is a distinct concern, and keeping them apart is what stops one phase's habits from contaminating another.

## The project's records and code all live outside this repository

The step folders contain only the methodology's configuration — **never a project's records or code**. Everything specific to one project — the five `STEP*.md` records, the prototype, the production implementation — lives entirely outside this repository, in locations recorded in that project's `.vibe_to_spec.yaml` config file:

- **`artifacts`** points at the folder holding all five `STEP*.md` records;
- **`dirty_impl_resources`** points at the **prototype** built in phase exploration — one or more paths or links, each with a short description;
- **`clean_impl_resources`** points at the **production implementation** built in phase implementation, in the same shape.

Each of these is a **GitHub link** (an https or git URL) or an **absolute path** on the local disk. A session finds the active project's `.vibe_to_spec.yaml` through a symlink at the repository root, `.active_project.vibe_to_spec.yaml` — see the root [`CLAUDE.md`](CLAUDE.md) for the full mechanism. A template for this file is at [`projects/template.vibe_to_spec.yaml-sample`](projects/template.vibe_to_spec.yaml-sample).

What stays in this repository is exactly the methodology template itself: the step folders' configuration, never a project's own records or code. Every project's `.vibe_to_spec.yaml` is git-ignored, since it holds paths specific to one user's machine.

```
idea
  → step_01: STEP1_VIBE_DECISIONS.md        + prototype ……………… external, in .vibe_to_spec.yaml
    → step_02: STEP2_DIRTY_SPEC.md ……………… reads the external prototype
      → step_03: STEP3_CLEAN_SPEC.md (cleaned — the source of truth)
        → step_04: STEP4_IMPL_SPEC_GAPS.md   + production code …… external, in .vibe_to_spec.yaml
          → step_05: STEP5_IMPL_VERIFICATION.md … checks the external implementation
                                                    against the specification
```

## The rules that hold it together

- **Only the user closes a phase.** Every step ends with a `/close-step` ritual and an explicit agreement from the user, recorded in that phase's artifact. The agent never declares a phase done on its own.
- **Each phase starts by checking the previous phase's recorded agreement.** The chain is mechanically checkable; a step refuses to start when the one before it is not closed.
- **Every phase keeps a "why" log** — the reasoning that later phases cannot reconstruct from the artifacts alone: `STEP1_VIBE_DECISIONS.md`, `STEP3_SPEC_OPTIMISATION.md`, `STEP4_IMPL_SPEC_GAPS.md`, `STEP5_IMPL_VERIFICATION.md`.
- **From step 4 on, the prototype is forbidden input.** Implementation is built from the specification alone; if the specification is not enough, that is a specification gap to resolve with the user — never something to improvise around.
- **Specification gaps flow back into the specification.** Every gap found while implementing is logged in `STEP4_IMPL_SPEC_GAPS.md`, and once the user resolves it, the agreed fix is applied to `STEP3_CLEAN_SPEC.md` itself. The real output of the whole cycle is that specification — the code, prototype and production alike, is disposable and can be rebuilt from it.
