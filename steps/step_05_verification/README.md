# Step 5 — Verification

Verify that the implementation faithfully realizes the specification.

## How it starts

- **Precondition**: step 4 is done — the implementation and its passing tests are complete, at the location recorded as `clean_impl_resources` in the project's `.vibe_to_spec.yaml`.
- **Where**: start the AI coding agent inside this folder:

  ```bash
  cd steps/step_05_verification && claude
  ```

- **Inputs, read-only**:
  - `<artifacts>/STEP3_CLEAN_SPEC.md` — the yardstick
  - the implementation — the subject under verification, at the external location(s) recorded as `clean_impl_resources` in the project's `.vibe_to_spec.yaml`

  The step 1 prototype is never consulted. Verification is against the specification, not against the exploratory prototype.

## What you say to steer it

This step is a conversation you steer, not a form you fill in. The agent checks the implementation against the specification item by item and reports verdicts backed by evidence; you set what to check, resolve the calls it cannot make, and decide the conclusion. Below are the kinds of things you would say at each moment of the loop; use your own words, these are only to show the shape.

- **To begin** — start a verification pass:
  > "Run /verify. Go through the spec item by item and show me the verdicts."

- **To steer what gets checked** — you can narrow the focus:
  > "Check the invariants first — the 'total always matches the sum' one especially."

- **To resolve an ambiguous verdict** — the call only you can make:
  > "The spec is unclear there, so treat it as conforming — that's what I meant."

- **To conclude** — either everything conforms, or the gaps go back to step 4:
  > "Every item conforms. Run /close-step and complete the cycle."
  > "Those three deviations are real. Hand them back to step 4 to fix, then we verify again."

## How it iterates

```mermaid
flowchart TD
    Start(["Step 4 done —
implementation CLOSED"]) --> Pick["Pick the next unverified
specification item(s)"]
    Pick --> Check["Check against the implementation:
behavior, architecture, API contracts,
invariants, completeness"]
    Check --> Delegate["Delegate bounded per-item
checks to spec-verifier"]
    Delegate --> Verify{"Claimed deviation
or missing item?"}
    Verify -->|"yes"| Reverify["Re-check with concrete
evidence before recording"]
    Reverify --> Record
    Verify -->|"no, conforms"| Record["Record verdict in
STEP5_IMPL_VERIFICATION.md"]
    Record --> Ambiguous{"Verdict is
AMBIGUOUS?"}
    Ambiguous -->|"yes"| AskUser["Ask the user
to resolve it"]
    AskUser --> Record
    Ambiguous -->|"no"| More{"Items still
unverified?"}
    More -->|"yes"| Pick
    More -->|"no, every item
has a verdict"| Conclude{{"/close-step"}}
    Conclude -->|"full conformance"| Done(["Cycle complete"])
    Conclude -->|"gaps found"| BackToStep4(["Gap list handed back
to step 4"])

    classDef loop fill:#5f4a1e,stroke:#d9b84a,stroke-width:2px,color:#fff;
    classDef cmd fill:#1e3a5f,stroke:#4a90d9,stroke-width:1.5px,color:#fff;
    classDef rec fill:#1e4a4a,stroke:#4ad9d9,stroke-width:1.5px,color:#fff;
    classDef done fill:#1e5f3a,stroke:#4ad98a,stroke-width:2px,color:#fff;
    classDef critic fill:#4a1e1e,stroke:#d94a4a,stroke-width:2px,color:#fff;
    class Start done;
    class Pick,Check,Verify,Reverify,Ambiguous,AskUser,More loop;
    class Delegate critic;
    class Record,BackToStep4 rec;
    class Conclude cmd;
    class Done done;
```

See the [global workflow map's legend](../../docs/global_workflow.md#legend) for what each color and symbol means.

1. **Go through the specification item by item**: behavior, architecture, API contracts, invariants, completeness.
2. **Check each item against the implementation** and record a verdict in `STEP5_IMPL_VERIFICATION.md`: conforms / deviates (and how) / missing.
3. **Verify findings before reporting them** — re-check each claimed deviation against both the specification and the code (running it where needed). No unconfirmed claims enter the report.
4. **Delegate bounded checks** to reviewer subagents where useful; their findings go through the same verification before entering the report.

## How it ends

- Every specification item has a verdict in `STEP5_IMPL_VERIFICATION.md`.
- **Full conformance** → the cycle is complete: the specification is the enduring asset, and the implementation in step 4 realizes it.
- **Gaps found** → the gap list goes back to step 4; after the fixes, verification runs again. The loop repeats until `STEP5_IMPL_VERIFICATION.md` shows full conformance.
