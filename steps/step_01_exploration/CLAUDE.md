# Step 1 — Exploration (Vibe to Spec, step 1 of 5)

Goal: explore the product by vibe coding, and end with a running prototype that the user has explicitly agreed is exactly what they want.

The deliverable of this step is NOT clean code and NOT a specification. It is two things, and only these two things:

1. A running prototype, explicitly validated by the user.
2. STEP1_VIBE_DECISIONS.md — the log of what was decided, what the user validated, and where the prototype still differs from what the user wants.

## Where the prototype lives

The prototype's code is completely OUTSIDE this repository: in its own repository or folder, pointed at via a GitHub link (an https or git URL) or an absolute path on the local disk. This folder keeps only the records.

At the start of a session, if STEP1_VIBE_DECISIONS.md has no `Prototype:` line yet, ask the user where the prototype lives (an existing location, or one to create), and record it right under the `# Decisions` heading:

    Prototype: <link or absolute path>

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

    Prototype: /Users/example/prototypes/my_product

    ## 2026-07-22

    - DECISION — storage: tried SQLite, switched to a plain JSON file because setup friction was slowing iteration.
    - VALIDATED — list view: user confirmed the list layout is what they want.
    - GAP — colors: user said the current colors are not what they want; they want a warmer palette. Open.

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
