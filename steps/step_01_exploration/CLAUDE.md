# Step 1 — Exploration (Vibe to Spec, step 1 of 5)

Goal: explore the product by vibe coding, and end with a running prototype that the user has explicitly agreed is exactly what they want.

The deliverable of this step is NOT clean code and NOT a specification. It is two things, and only these two things:

1. A running prototype, explicitly validated by the user.
2. `<artifacts>/STEP1_VIBE_DECISIONS.md` — the log of what was decided, what the user validated, and where the prototype still differs from what the user wants.

## Where the prototype lives

The prototype's code is completely OUTSIDE this repository: in its own repository or folder, pointed at via a GitHub link (an https or git URL) or an absolute path on the local disk. This folder keeps only the records.

At the start of a session, if the active project's `.vibe_to_spec.yaml` has no `dirty_impl_resources` entry yet, ask the user where the prototype lives (an existing location, or one to create), and add it there as a `path` plus a short `description`.

All code work happens at that location. If it is a repository link rather than a local path, work from a local clone and ask the user where that clone should live.

## The exploration loop

Work in small, visible increments. For every increment:

1. Build the smallest change that shows real behavior.
2. Show it to the user: say exactly how to run it and what to look at.
3. Explicitly ask the user: "Is this what you want?" Do not assume. Do not treat silence as agreement. Ask pointed questions:
   - Is this behavior what you expected?
   - What feels wrong or off?
   - What is missing?
   - Should this direction continue, or change?
4. Log the user's answers in STEP1_VIBE_DECISIONS.md (format below):
   - what the user validated as wanted
   - what the user explicitly said is NOT what they want (a gap)
   - any change of direction, and why
5. Repeat.

Never chain several increments without asking. The whole point of this step is that the user steers continuously.

Use /checkpoint to run this ask-and-log ritual; also trigger it yourself after every meaningful behavior change, without waiting to be asked.

## STEP1_VIBE_DECISIONS.md — the log format

One dated section per day, one labeled bullet per entry. Four entry kinds:

- `DECISION` — a direction was chosen, changed, or abandoned, and why.
- `VALIDATED` — the user explicitly confirmed a behavior is what they want.
- `GAP` — the user explicitly said something is NOT what they want. Record it in the user's own words as closely as possible. A gap stays open until it is either fixed (log the fix) or explicitly accepted by the user at closing time.
- `CLOSED` — the step-ending entry: the user's explicit agreement that the running prototype is exactly what they want, plus the list of explicitly accepted gaps (or "none").

Example:

    # Decisions

    ## 2026-07-22

    - DECISION — storage: tried SQLite, switched to a plain JSON file because setup friction was slowing iteration.
    - VALIDATED — list view: user confirmed the list layout is what they want.
    - GAP — colors: user said the current colors are not what they want; they want a warmer palette. Open.

## The log must carry everything the specification will need

Step 2 writes the specification (STEP2_DIRTY_SPEC.md) from exactly two sources: the prototype's code, and STEP1_VIBE_DECISIONS.md. The code shows WHAT the prototype does. It cannot show what the user meant, why a direction was chosen, which behaviors the user actually wants, or which behaviors are accidents of vibe coding. All information of that second kind exists later ONLY if it was written into STEP1_VIBE_DECISIONS.md during this step. Anything left unrecorded is permanently lost, because step 2 is forbidden from inventing it.

The test to apply to every entry, and to the log as a whole at closing time: **could a person who was NOT in these conversations, holding only the prototype's code and this log, write the full specification without asking the user a single question?** Whenever the answer is no, the missing information belongs in the log, now.

This does not turn the log into a specification — it stays a dated log of DECISION / VALIDATED / GAP / CLOSED entries, as defined above. What follows is the checklist of information those entries must have captured, between them, by the time the step closes. Most items name the section of the specification that will be written from them; the last ones capture information that cuts across the whole specification, or that step 2 needs before it can even run the prototype.

### Purpose — what the product is and for whom

- The product's name — the specification's title line is `# <product name> — Specification`. If the user never named the product, ask for the name before closing.
- The problem the product solves, in the user's own words.
- Who uses the product, and in what situation.
- What "done" or "success" looks like for the user.
- What matters most, when the user ranked things ("the most important part is the search"), and what the user called secondary.
- If the user ever restated or narrowed the purpose mid-exploration, the restatement and when it happened.

### Concepts — the nouns and their names

- Every name the user uses for a thing in the product (a screen, an object, a state, a role), and what the user means by that name.
- Every renaming: when the user said "do not call it X, call it Y", log it — the specification must use the user's final vocabulary.
- Distinctions the user insisted on ("a draft is not the same thing as an unpublished post").

### Responsibilities — what each part is in charge of

- When the user said where something belongs ("the server decides that, not the page", "that check happens on save, not on load"), log the assignment.
- When the user said something must NOT be a given part's job, log that too.

### Workflows — what happens, step by step

- For every main flow the user validated: the trigger, the steps in order, and the visible end result. A bare "VALIDATED — the flow works" is useless to step 2; the entry must say what the flow IS.
- Orderings the user cares about ("ask for confirmation BEFORE deleting"), and orderings the user explicitly does not care about.
- What the user said should happen when a flow goes wrong (bad input, missing file, network failure), whenever the subject came up. If the user dismissed an error case as not worth handling, log that dismissal — it is a decision.

### APIs — the surfaces

- Every command, endpoint, keyboard interaction, or file the user actually exercised and validated: its exact name, its inputs, its outputs.
- Exact values the user fixed: a command name, a flag, a URL path, a port, a file name, a key binding. Record the exact text, never a paraphrase.
- Surfaces the user explicitly rejected ("no configuration file", "no command-line flags for this").

### Data structures — the shapes of the data, and where they live

- What data the product keeps, where it is kept (a file, a database, memory only), and in what format — and WHY that choice was made, when a choice was made ("switched to a plain JSON file because setup friction was slowing iteration").
- Fields the user asked for by name, and fields the user said to drop.
- What must survive a restart and what may be lost.
- Data the product must work with that already exists outside it (a file the user already has, an external source): where it lives and its exact format.

### Invariants — what must always hold true

- Every rule the user stated with "always", "never", "must", or "cannot": "two entries can never have the same name", "the total always matches the sum of the lines". These sentences are the invariants section of the specification, and the code alone can never prove they were intended rather than accidental — they only exist if logged.

### Constraints — limits, environment, dependencies

- The environment the user validated on (operating system, browser, terminal, screen size) whenever it mattered to a validation.
- Dependencies the user accepted, and dependencies the user refused ("no database", "nothing that needs an account").
- Limits the user stated: how much data, how many users, how fast, how big.
- Anything the user said about cost, licensing, privacy, or where data is allowed to go.

### Assumptions — what is taken for granted

- Everything the prototype silently relies on that the user agreed to rely on: "single user, no login", "input files are always well-formed", "the machine is always online". When you notice yourself building on such an assumption, say it to the user and log the answer.

### Known gaps — what the user accepted as missing

- Every GAP entry, in the user's own words, with its final state: fixed (and when), or explicitly accepted at closing time. The accepted gaps are copied into the specification verbatim.
- Decisions the user explicitly deferred ("I do not know yet, we will see") — log each as an open GAP, so closing time forces it to be resolved or explicitly accepted.

### Excluded behaviors — accidents that must not survive

- Every behavior the prototype shows that the user, when shown it, said is NOT wanted: log the behavior precisely (what the prototype does, in what situation) and the user's verdict. Without these entries, step 2 cannot tell design from accident and must interrupt the user for every doubt.

### Decisions and rejected alternatives — the why

- For every direction chosen: what the alternatives were, which one was picked, and the reason. The reason is the part the code can never show, and it is what stops later steps from "improving" the product back into an already-rejected design.
- For every direction abandoned mid-exploration: what was tried, why it was dropped, and in favor of what.
- Ideas that were proposed but never built: features the user declined or postponed ("not now, maybe later"), and why. Without these entries, later steps innocently re-propose what was already turned down.

### Appearance and wording — what the user sees

- Layout, colors, and visual arrangement choices the user validated or corrected — the example GAP above ("they want a warmer palette") is exactly this kind of information.
- The exact wording of user-facing text the user fixed or approved: labels, button text, messages, error messages. Record the exact text.
- The language the user interface is written in, if it ever came up.

### Validated examples — the real inputs and outputs

- For each validation, the concrete example it ran on: the exact input (typed text, a sample file, a sequence of clicks) and the exact visible result the user approved.
- Where any sample data used during a validation lives, so the same scenario can be re-run later.
- These examples become worked examples in the specification, and step 5 reuses them to verify the production implementation behaves the same way as the validated prototype.

### How to run the prototype

- For each location listed under `dirty_impl_resources` in the project's `.vibe_to_spec.yaml`: what to install or set up first, the exact command that starts it, and any configuration or credentials it needs.
- Log this as a DECISION entry the first time the prototype becomes runnable, and again every time the way to run it changes.
- This step's own rules forbid writing a README for the prototype, so STEP1_VIBE_DECISIONS.md is the only durable record of how to run it — and step 2 must be able to run it.

### How to record, so the information survives

- Use the user's own words for wants, refusals, and gaps — paraphrasing loses the nuance the specification needs.
- Be concrete: name the exact situation, the exact behavior, the exact verdict. "VALIDATED — search" is lost information; "VALIDATED — search: typing in the search box filters the list as you type, matching on title only; user confirmed matching on title only is what they want" is a specification sentence waiting to happen.
- Keep exact values exact: names, numbers, paths, formats. Never round, never summarize a value away.
- One fact per bullet. A bullet that bundles three facts will be half-read later.
- Log at the moment the information appears in conversation. Reconstructing it at closing time from memory produces exactly the vague entries this section forbids.

At closing time (/close-step), walk this checklist against the log before asking the user for the CLOSED agreement, and fill any hole by asking the user now — this is the last moment the answers are cheap.

## What the user validates

The user validates BEHAVIOR — what the running prototype does — never the code. Show running behavior, not diffs, when asking for validation.

## Code rules during exploration

- Duplication is fine. Do not extract a shared abstraction until asked to.
- Rough edges are fine. Do not add error handling, validation, or edge-case handling for situations that have not actually come up yet.
- Do not write tests, and do not run linters, type-checkers, or formatters as a gate before showing results. Only reach for them if they help debug something that is actually broken right now.
- Do not refactor or tidy up earlier attempts while working on something else. Dead code can be left alone or deleted outright.
- Do not write a README, architecture notes, or a specification for this prototype — recovering that is step 2's job, done later from the validated snapshot.
- Changing direction mid-stream is expected and encouraged.
- When in doubt, take the crudest path that shows real behavior soonest.

## Ending the step

Step 1 ends ONLY when the user explicitly says the running prototype is exactly what they want. Use /close-step to run the closing walkthrough.

- If the user agrees with reservations ("yes, except X"), X must be written in STEP1_VIBE_DECISIONS.md as an explicitly accepted gap before closing.
- You never declare this step done on your own. Only the user's explicit agreement, recorded as the CLOSED entry in STEP1_VIBE_DECISIONS.md, closes it.
