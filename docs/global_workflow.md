# The Global Workflow of Vibe to Spec

A single map of the whole [Vibe to Spec](../README.md) methodology: the five phases, the external code they build and check, the records they write, the loops that keep the specification honest, and the human approval gates that only the user can pass.

## A note on reading this map (why it is layered, not one giant picture)

A single flowchart that shows every command, every subagent, every record, and every loop at once is unreadable, and no diagram tool that renders inside a markdown file on the web can help: a diagram renders at one fixed level of detail, and zooming out only makes the same crowded picture smaller — it never shows *less*. So this map is layered instead, the way a road atlas has a country map and then city maps:

- **[The overview graph](#the-overview-graph-the-zoomed-out-view)** is the zoomed-out view: only the five phases, the two external code stores, and the loops between them. This is the whole methodology on one screen.
- **[The five per-phase graphs](#phase-1--exploration-detail)** are the zoomed-in views: each one opens a single phase and shows its rulebook, its slash commands, its adversarial subagent, its internal loop, the records it writes, and its close-step gate.

Read the overview first, then open only the phase you care about. That is the "zoom in without seeing everything" behavior, done by hierarchy rather than by a zoom control.

## Metrics at a glance

| Metric | Count | What it counts |
|---|---:|---|
| **Phases** | **5** | Exploration → Dirty Specification Extraction → Specification Cleaning → Implementation → Verification |
| **Isolated Claude Code sessions** | **5** | One session per phase, each launched inside its own folder — nothing leaks between them |
| **Rulebooks** (`CLAUDE.md`) | **5** | One rulebook per phase, carrying only that phase's discipline |
| **Slash commands** | **11** | 11 command definitions across the phases — 7 distinct commands, with `/close-step` specialized once per phase |
| **Adversarial subagents** | **4** | `extraction-critic`, `cleaning-critic`, `implementation-critic`, `spec-verifier` — one per phase from 2 to 5 |
| **Durable record files** | **6** | `STEP1_VIBE_DECISIONS.md`, `STEP2_DIRTY_SPEC.md`, `STEP3_CLEAN_SPEC.md`, `STEP3_SPEC_OPTIMISATION.md`, `STEP4_IMPL_SPEC_GAPS.md`, `STEP5_IMPL_VERIFICATION.md` |
| **External code stores** | **2** | The phase 1 prototype and the phase 4 production implementation — both live entirely outside this repository |
| **Loops** | **11** | 5 within-phase working loops + 4 adversarial self-critique loops (phases 2–5) + 2 cross-phase feedback loops (phase 4 → 3, phase 5 → 4) |
| **Human approval gates** | **5** | The `/close-step` ritual of each phase — only the user's explicit "yes" closes a phase |
| **Precondition gates** | **4** | Phases 2–5 each refuse to start until the previous phase is recorded as closed |
| **Verification dimensions** | **5** | behavior, architecture, API contracts, invariants, completeness |
| **Completeness checklist items** (phase 1) | **16** | The sections the decision log must cover before phase 1 can close |
| **Specification sections** | **11** | Purpose, Concepts, Responsibilities, Workflows, APIs, Data structures, Invariants, Constraints, Assumptions, Known gaps, Excluded behaviors |
| **Distinct log entry kinds** | **14** | `DECISION`, `VALIDATED`, `GAP`, `CLOSED`, `MERGED`, `REMOVED`, `RENAMED`, `REJECTED`, `PRODUCT`, `RESOLVED`, `CONFORMS`, `DEVIATES`, `MISSING`, `AMBIGUOUS` |

A note on skills: the methodology defines **0** custom skills on purpose. Its automation is carried entirely by the 11 slash commands and the 4 adversarial subagents above — there is no separate skill layer to count.

## The overview graph (the zoomed-out view)

The whole methodology on one screen: five phases on a spine, the two external code stores off to the side, and the loops that feed the source of truth. Everything a phase does internally is hidden here — open its own graph below for that.

```mermaid
flowchart TD
    Idea(["💡 idea"]) --> P1

    P1["<b>Phase 1 · Exploration</b><br/>vibe-code loop · 3 commands<br/>writes STEP1_VIBE_DECISIONS.md"]
    P2["<b>Phase 2 · Dirty Specification Extraction</b><br/>extraction loop · 2 commands · 1 critic<br/>writes STEP2_DIRTY_SPEC.md"]
    P3["<b>Phase 3 · Specification Cleaning</b><br/>cleaning loop · 2 commands · 1 critic<br/>writes STEP3_CLEAN_SPEC.md — the source of truth"]
    P4["<b>Phase 4 · Implementation</b><br/>build loop · 2 commands · 1 critic<br/>writes STEP4_IMPL_SPEC_GAPS.md"]
    P5["<b>Phase 5 · Verification</b><br/>verify loop · 2 commands · 1 verifier<br/>writes STEP5_IMPL_VERIFICATION.md"]

    P1 --> P2 --> P3 --> P4 --> P5
    P5 -->|"full conformance"| Done(["✅ Cycle complete<br/>STEP3_CLEAN_SPEC.md is the enduring output;<br/>the code is disposable"])

    Proto[("Prototype<br/>external location")]
    Impl[("Implementation<br/>external location")]

    P1 -.->|builds| Proto
    Proto -.->|read-only| P2
    P4 -.->|builds| Impl
    Impl -.->|read-only| P5

    P4 -->|"spec gap: agreed fix<br/>amends the source of truth"| P3
    P5 -->|"gaps found: handed back"| P4

    classDef phase fill:#1e3a5f,stroke:#4a90d9,stroke-width:2px,color:#fff;
    classDef code fill:#5a3a1e,stroke:#d98a4a,stroke-width:2px,color:#fff;
    classDef done fill:#1e5f3a,stroke:#4ad98a,stroke-width:2px,color:#fff;
    class P1,P2,P3,P4,P5 phase;
    class Proto,Impl code;
    class Done,Idea done;
```

**The two loops that keep the source of truth honest:**

1. **Phase 4 → Phase 3** — every specification gap found while implementing is logged in `STEP4_IMPL_SPEC_GAPS.md`; once the user resolves it, the agreed fix is written back into `STEP3_CLEAN_SPEC.md` itself. The specification never drifts silently from the implementation.
2. **Phase 5 → Phase 4** — verification hands its list of deviations back to phase 4, which fixes the code; verification then runs again. The cycle repeats until every specification item conforms.

## Phase 1 — Exploration (detail)

**Session:** `cd steps/step_01_exploration && claude` · **Question:** what do I actually want? · **Output:** a running prototype the user has explicitly validated, plus its decision log.

```mermaid
flowchart TD
    subgraph S1["🖥️ Session · steps/step_01_exploration"]
        direction TB
        R1["📕 CLAUDE.md<br/>exploration rulebook<br/>(duplication fine · no tests · no README)"]
        Cmd1a(["/checkpoint"])
        Cmd1b(["/log-decision"])
        Cmd1c(["/close-step"])

        Loop1{{"Exploration loop<br/>build smallest real change →<br/>show it → ask 'is this what you want?' →<br/>log the verdict → repeat"}}

        R1 -.governs.-> Loop1
        Cmd1a --> Loop1
        Cmd1b --> Loop1
        Loop1 --> Log1["📄 STEP1_VIBE_DECISIONS.md<br/>DECISION · VALIDATED · GAP · CLOSED"]

        Cmd1c --> Gate1{"👤 user: 'is this running prototype<br/>exactly what you want?'"}
        Gate1 -->|"not yet"| Loop1
        Gate1 -->|"yes"| Closed1["✅ CLOSED entry written<br/>16-point checklist verified first"]
    end

    Proto1[("Prototype<br/>external location in .vibe_to_spec.yaml")]
    Loop1 -.->|builds| Proto1
    Closed1 ==>|"unlocks phase 2"| Next1(["→ steps/step_02_spec_extraction"])

    classDef rule fill:#3a1e5f,stroke:#a04ad9,stroke-width:1.5px,color:#fff;
    classDef cmd fill:#1e3a5f,stroke:#4a90d9,stroke-width:1.5px,color:#fff;
    classDef loop fill:#5f4a1e,stroke:#d9b84a,stroke-width:2px,color:#fff;
    classDef rec fill:#1e4a4a,stroke:#4ad9d9,stroke-width:1.5px,color:#fff;
    classDef gate fill:#5f1e3a,stroke:#d94a8a,stroke-width:2px,color:#fff;
    classDef code fill:#5a3a1e,stroke:#d98a4a,stroke-width:2px,color:#fff;
    class R1 rule;
    class Cmd1a,Cmd1b,Cmd1c cmd;
    class Loop1 loop;
    class Log1,Closed1 rec;
    class Gate1 gate;
    class Proto1 code;
```

## Phase 2 — Dirty Specification Extraction (detail)

**Session:** `cd steps/step_02_spec_extraction && claude` · **Question:** what did I actually build? · **Precondition:** phase 1's `CLOSED` entry exists. · **Output:** `STEP2_DIRTY_SPEC.md`, the raw specification.

```mermaid
flowchart TD
    In2a[("Prototype<br/>read-only")]
    In2b["📄 STEP1_VIBE_DECISIONS.md<br/>read-only"]

    subgraph S2["🖥️ Session · steps/step_02_spec_extraction"]
        direction TB
        R2["📕 CLAUDE.md<br/>extraction rulebook<br/>(describe what IS, never what SHOULD be)"]
        Cmd2a(["/extract-spec"])
        Cmd2b(["/close-step"])

        Loop2{{"Extraction loop<br/>draft one section → critique → revise →<br/>ask user to classify → user reviews section"}}
        Critic2["🤖 extraction-critic (subagent, fresh context)<br/>DESCRIBES-WHAT-SHOULD-BE · SILENT-CLASSIFICATION<br/>· OMISSION · UNSUPPORTED"]

        R2 -.governs.-> Loop2
        Cmd2a --> Loop2
        Loop2 -->|"each drafted section"| Critic2
        Critic2 -->|"ISSUES: revise & re-run"| Loop2
        Critic2 -->|"CLEAN"| Draft2["📄 STEP2_DIRTY_SPEC.md<br/>11 sections · Status: draft"]

        Cmd2b --> Gate2{"👤 user: 'does this spec fully<br/>account for the prototype?'"}
        Gate2 -->|"not yet"| Loop2
        Gate2 -->|"yes"| Closed2["✅ Status: agreed by the user on (date)"]
    end

    In2a -.->|read| Loop2
    In2b -.->|read| Loop2
    Closed2 ==>|"unlocks phase 3"| Next2(["→ steps/step_03_spec_cleaning"])

    classDef rule fill:#3a1e5f,stroke:#a04ad9,stroke-width:1.5px,color:#fff;
    classDef cmd fill:#1e3a5f,stroke:#4a90d9,stroke-width:1.5px,color:#fff;
    classDef loop fill:#5f4a1e,stroke:#d9b84a,stroke-width:2px,color:#fff;
    classDef rec fill:#1e4a4a,stroke:#4ad9d9,stroke-width:1.5px,color:#fff;
    classDef gate fill:#5f1e3a,stroke:#d94a8a,stroke-width:2px,color:#fff;
    classDef code fill:#5a3a1e,stroke:#d98a4a,stroke-width:2px,color:#fff;
    classDef critic fill:#4a1e1e,stroke:#d94a4a,stroke-width:2px,color:#fff;
    class R2 rule;
    class Cmd2a,Cmd2b cmd;
    class Loop2 loop;
    class Draft2,Closed2,In2b rec;
    class Gate2 gate;
    class In2a code;
    class Critic2 critic;
```

## Phase 3 — Specification Cleaning (detail)

**Session:** `cd steps/step_03_spec_cleaning && claude` · **Question:** what is the simplest design? · **Precondition:** the raw spec's status reads "agreed by the user". · **Output:** `STEP3_CLEAN_SPEC.md` — the source of truth for everything after.

```mermaid
flowchart TD
    In3["📄 STEP2_DIRTY_SPEC.md<br/>read-only — never touched"]

    subgraph S3["🖥️ Session · steps/step_03_spec_cleaning"]
        direction TB
        R3["📕 CLAUDE.md<br/>cleaning rulebook<br/>(reduce the DESIGN · never change behavior)"]
        Cmd3a(["/clean-spec"])
        Cmd3b(["/close-step"])

        Loop3{{"Cleaning loop<br/>find ONE reduction → justify behavior preserved →<br/>critique → user approves → apply & log → repeat"}}
        Critic3["🤖 cleaning-critic (subagent, fresh context)<br/>DISGUISED-PRODUCT-CHANGE ·<br/>BEHAVIOR-NOT-PRESERVED · INVENTED"]

        R3 -.governs.-> Loop3
        Cmd3a --> Loop3
        Loop3 -->|"each proposed reduction"| Critic3
        Critic3 -->|"ISSUES: fix, drop, or reclassify"| Loop3
        Critic3 -->|"CLEAN"| GateApprove{"👤 user approves this reduction?"}
        GateApprove -->|"reject"| OptLog
        GateApprove -->|"approve"| Clean3["📄 STEP3_CLEAN_SPEC.md<br/>the working copy"]
        Loop3 --> OptLog["📄 STEP3_SPEC_OPTIMISATION.md<br/>MERGED · REMOVED · RENAMED · REJECTED · PRODUCT"]

        Cmd3b --> Gate3{"👤 user: 'is this the simplest design<br/>that preserves the behavior?'"}
        Gate3 -->|"not yet"| Loop3
        Gate3 -->|"yes"| Closed3["✅ Status: agreed … source of truth"]
    end

    In3 -.->|read| Loop3
    Closed3 ==>|"unlocks phase 4 · prototype now irrelevant"| Next3(["→ steps/step_04_implementation"])

    classDef rule fill:#3a1e5f,stroke:#a04ad9,stroke-width:1.5px,color:#fff;
    classDef cmd fill:#1e3a5f,stroke:#4a90d9,stroke-width:1.5px,color:#fff;
    classDef loop fill:#5f4a1e,stroke:#d9b84a,stroke-width:2px,color:#fff;
    classDef rec fill:#1e4a4a,stroke:#4ad9d9,stroke-width:1.5px,color:#fff;
    classDef gate fill:#5f1e3a,stroke:#d94a8a,stroke-width:2px,color:#fff;
    classDef critic fill:#4a1e1e,stroke:#d94a4a,stroke-width:2px,color:#fff;
    class R3 rule;
    class Cmd3a,Cmd3b cmd;
    class Loop3 loop;
    class Clean3,OptLog,Closed3,In3 rec;
    class Gate3,GateApprove gate;
    class Critic3 critic;
```

## Phase 4 — Implementation (detail)

**Session:** `cd steps/step_04_implementation && claude` · **Question:** how should it be built? · **Precondition:** the spec's status reads "agreed … source of truth". · **Output:** production code with tests, plus the gap log. · **Forbidden input:** the prototype and the raw spec — never read.

```mermaid
flowchart TD
    In4["📄 STEP3_CLEAN_SPEC.md<br/>the ONLY input — source of truth"]
    Forbid["🚫 Prototype & STEP2_DIRTY_SPEC.md<br/>forbidden input"]

    subgraph S4["🖥️ Session · steps/step_04_implementation"]
        direction TB
        R4["📕 CLAUDE.md<br/>implementation rulebook<br/>(engineering discipline restored · tests everywhere)"]
        Cmd4a(["/spec-gap"])
        Cmd4b(["/close-step"])

        Loop4{{"Build loop<br/>pick spec part → implement with tests →<br/>critique → fix → report coverage → repeat"}}
        Critic4["🤖 implementation-critic (subagent, fresh context)<br/>IMPROVISED-PAST-A-GAP · DEVIATES ·<br/>MISSING · UNTESTED"]

        R4 -.governs.-> Loop4
        Loop4 -->|"each finished part"| Critic4
        Critic4 -->|"ISSUES: fix"| Loop4
        Critic4 -->|"spec unclear → raise, never improvise"| Cmd4a
        Cmd4a --> GapLog["📄 STEP4_IMPL_SPEC_GAPS.md<br/>GAP · RESOLVED · CLOSED"]
        Cmd4a --> Amend["✏️ amend STEP3_CLEAN_SPEC.md<br/>only after the user's decision"]

        Cmd4b --> Gate4{"👤 user: 'does this cover the spec,<br/>with passing tests?'"}
        Gate4 -->|"not yet"| Loop4
        Gate4 -->|"yes (real test run shown)"| Closed4["✅ CLOSED entry written"]
    end

    In4 -.->|read| Loop4
    Forbid -.->|"never read"| S4
    Loop4 -.->|builds| Impl4[("Implementation<br/>external location in .vibe_to_spec.yaml")]
    Amend -->|"spec gap loop back to phase 3's file"| In4
    Closed4 ==>|"unlocks phase 5"| Next4(["→ steps/step_05_verification"])

    classDef rule fill:#3a1e5f,stroke:#a04ad9,stroke-width:1.5px,color:#fff;
    classDef cmd fill:#1e3a5f,stroke:#4a90d9,stroke-width:1.5px,color:#fff;
    classDef loop fill:#5f4a1e,stroke:#d9b84a,stroke-width:2px,color:#fff;
    classDef rec fill:#1e4a4a,stroke:#4ad9d9,stroke-width:1.5px,color:#fff;
    classDef gate fill:#5f1e3a,stroke:#d94a8a,stroke-width:2px,color:#fff;
    classDef critic fill:#4a1e1e,stroke:#d94a4a,stroke-width:2px,color:#fff;
    classDef code fill:#5a3a1e,stroke:#d98a4a,stroke-width:2px,color:#fff;
    classDef forbid fill:#3a3a3a,stroke:#d94a4a,stroke-width:2px,color:#fff,stroke-dasharray:5 5;
    class R4 rule;
    class Cmd4a,Cmd4b cmd;
    class Loop4 loop;
    class GapLog,Amend,Closed4,In4 rec;
    class Gate4 gate;
    class Critic4 critic;
    class Impl4 code;
    class Forbid forbid;
```

## Phase 5 — Verification (detail)

**Session:** `cd steps/step_05_verification && claude` · **Question:** does it match the specification? · **Precondition:** the spec is the source of truth and phase 4's `CLOSED` entry exists. · **Output:** `STEP5_IMPL_VERIFICATION.md` — evidence-backed verdicts.

```mermaid
flowchart TD
    In5a["📄 STEP3_CLEAN_SPEC.md<br/>the yardstick — read-only"]
    In5b[("Implementation<br/>read-only")]

    subgraph S5["🖥️ Session · steps/step_05_verification"]
        direction TB
        R5["📕 CLAUDE.md<br/>verification rulebook<br/>(adversarial · find non-conformance)"]
        Cmd5a(["/verify"])
        Cmd5b(["/close-step"])

        Loop5{{"Verify loop<br/>pick unverified items → check 5 dimensions →<br/>verify evidence → record verdict → repeat until all judged"}}
        Verifier5["🤖 spec-verifier (subagent, fresh context per item)<br/>CONFORMS · DEVIATES · MISSING · AMBIGUOUS<br/>five dimensions: behavior · architecture ·<br/>API contracts · invariants · completeness"]

        R5 -.governs.-> Loop5
        Cmd5a --> Loop5
        Loop5 -->|"bounded per-item check"| Verifier5
        Verifier5 -->|"re-checked, then recorded"| Report5["📄 STEP5_IMPL_VERIFICATION.md<br/>verdicts + evidence"]

        Cmd5b --> Gate5{"👤 user confirms the verdict"}
        Gate5 -->|"full conformance"| Done5["✅ Status: concluded — full conformance"]
        Gate5 -->|"gaps found"| HandBack["📄 gap list handed back"]
    end

    In5a -.->|read| Loop5
    In5b -.->|read & run| Loop5
    HandBack ==>|"reopens phase 4"| Back5(["→ steps/step_04_implementation<br/>fix, then verify again"])
    Done5 ==> Complete(["🏁 Vibe to Spec cycle complete<br/>STEP3_CLEAN_SPEC.md is the enduring output"])

    classDef rule fill:#3a1e5f,stroke:#a04ad9,stroke-width:1.5px,color:#fff;
    classDef cmd fill:#1e3a5f,stroke:#4a90d9,stroke-width:1.5px,color:#fff;
    classDef loop fill:#5f4a1e,stroke:#d9b84a,stroke-width:2px,color:#fff;
    classDef rec fill:#1e4a4a,stroke:#4ad9d9,stroke-width:1.5px,color:#fff;
    classDef gate fill:#5f1e3a,stroke:#d94a8a,stroke-width:2px,color:#fff;
    classDef critic fill:#4a1e1e,stroke:#d94a4a,stroke-width:2px,color:#fff;
    classDef code fill:#5a3a1e,stroke:#d98a4a,stroke-width:2px,color:#fff;
    classDef done fill:#1e5f3a,stroke:#4ad98a,stroke-width:2px,color:#fff;
    class R5 rule;
    class Cmd5a,Cmd5b cmd;
    class Loop5 loop;
    class Report5,HandBack,In5a rec;
    class Gate5 gate;
    class Verifier5 critic;
    class In5b code;
    class Done5,Complete done;
```

## Legend

| Symbol | Meaning |
|---|---|
| 📕 rulebook | A phase's `CLAUDE.md` — the discipline that governs that one session |
| `/command` | A slash command available inside that phase's session |
| 🤖 subagent | An adversarial reviewer run in a fresh context: the three critics and the verifier |
| 📄 record | A durable `STEP*.md` file — the phase's permanent output |
| 🗄️ external store | The prototype or the production implementation, both living outside this repository |
| 👤 gate | A human approval gate — only the user's explicit answer passes it |
| dashed edge | Read-only access, or "builds" — never a modification of the source |
| solid double edge (`==>`) | A phase unlocks or hands off to the next |
