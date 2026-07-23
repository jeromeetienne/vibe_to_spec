# How Vibe to Spec Works: A Separate Claude Code Session for Every Phase

*Part 2 of a series on the Vibe to Spec methodology*

Part 1 laid out the argument: separate the disposable exploration from the permanent specification, and vibe coding's maintainability problem stops being a problem. That argument is easy to agree with and easy to fail at in practice, because the failure mode is always the same one — the discipline you meant to keep slips half a session in, and you're back to editing the prototype directly, because it's right there and it would only take a minute. Vibe to Spec's answer to that isn't a rule you have to remember to follow. It's a boundary that isn't there to cross in the first place.

## One folder per phase, one Claude Code session per phase

Every phase of Vibe to Spec — Exploration, Dirty Specification Extraction, Specification Cleaning, Implementation, Verification — is its own folder under `steps/`, and each folder carries its own Claude Code configuration: its own `CLAUDE.md`, its own slash commands, and in one case its own subagent. You work through the methodology by changing directory and starting a fresh Claude Code session inside whichever phase you're on:

```
cd steps/step_01_exploration && claude
```

A session started this way only ever sees the rules of the folder it was started in. The repository root deliberately carries no `CLAUDE.md` of its own, so nothing leaks in from outside any single phase. The exploration session never sees the implementation phase's engineering discipline. The implementation session has never read the prototype. This is the actual mechanism behind "separate exploration from engineering" — not a request made of one long-running conversation, but separate conversations that structurally cannot see each other.

## The same agent, configured to behave in opposite ways

You can see what that buys you by putting two phases' actual rules side by side. Step 1's `CLAUDE.md` spends a whole section suspending the instincts a coding agent normally has:

> Duplication is fine. Do not extract a shared abstraction until asked to. Rough edges are fine. Do not add error handling, validation, or edge-case handling for situations that have not actually come up yet. Do not write tests, and do not run linters, type-checkers, or formatters as a gate before showing results.

Step 4's `CLAUDE.md`, three folders later, states the opposite just as plainly:

> Step 1 rules do not apply here. This is production engineering: the user's global coding rules apply in full. Every part gets tests, written alongside the code. Clean naming, clear structure, no dead code, no debug leftovers.

It's the same underlying coding agent in both cases. What changed is which folder it was started in. That's the whole trick — the discipline lives in the configuration, not in a request you make each time and hope holds up under pressure.

## Isolation is a rule, not just a location

The isolation goes further than "the files happen to sit in a different folder." Step 4's `CLAUDE.md` names its forbidden inputs outright: it must never read the step 1 prototype, and never read `STEP2_DIRTY_SPEC.md`, the raw specification from step 2. The prototype is sitting right there, a couple of folders up, one `cd ..` away, easily readable. The rule isn't "you won't see it" — it's "even if you can see it, you don't get to use it." If the specification turns out to be missing something the prototype had, that's treated as a real finding, not a reason to go quietly peek at the old code and copy the answer: the implementation session stops, runs `/spec-gap`, and asks.

## The code lives somewhere else entirely

None of the code being explored, specified, implemented, or verified ever lives in this repository. The prototype lives at whatever location step 1's own records file notes under a `Prototype:` line — a GitHub link, a git URL, or an absolute path on disk. The production implementation gets the same treatment from step 4 onward, recorded as an `Implementation:` line. Every later phase reads that pointer to know where to look.

This wasn't the first way this was set up. The earliest version of this scaffolding kept the prototype's code inside the step 1 folder directly. It moved fully external once it became clear that mixing methodology records with project code inside the same repository blurred exactly the line the whole methodology exists to draw. What stays in this repository now is only what Vibe to Spec calls permanent: the specifications and the reasoning behind every phase's decisions. The code — prototype and production alike — is exactly the disposable part, so it lives somewhere else.

Even the records' own filenames trace this shift. Step 1 and step 2 name theirs `STEP1_VIBE_DECISIONS.md` and `STEP2_DIRTY_SPEC.md` — still carrying the "vibe" label, because they're still close to raw exploration. By step 3 the file becomes `STEP3_CLEAN_SPEC.md`: the exact point where the specification stops being a description of a prototype and starts being the source of truth. Steps 4 and 5 carry `IMPL` in their names instead. The naming was not planned as a demonstration of the thesis, but it ended up being one anyway.

## A chain that checks itself

The five phases hand off one artifact at a time: `STEP1_VIBE_DECISIONS.md` to `STEP2_DIRTY_SPEC.md` to `STEP3_CLEAN_SPEC.md` — the source of truth — to `STEP4_IMPL_SPEC_GAPS.md` to `STEP5_IMPL_VERIFICATION.md`. What makes this more than a diagram is that each phase's own instructions check the previous phase's closing signal before doing any work at all. Step 2 won't start until it has confirmed step 1 actually closed:

> Precondition: DECISIONS.md must contain the CLOSED entry — the user's explicit agreement that ended step 1. If it does not, stop and tell the user step 1 is not closed yet.

Step 4 checks step 3's specification the same way; step 5 checks both step 3's specification and step 4's closing entry before it verifies anything. None of this depends on anyone remembering where they left off. The check is written into the phase about to start, not left to memory.

## The loop inside every phase

Every phase repeats the same shape internally: do the phase's one action, review it, iterate, until the phase's own exit condition is met — and the gate at the end of that loop is always `/close-step`, which either confirms the phase is done or sends you back into it. Step 1's version of that command is fully scripted: walk through the running prototype end to end, resolve every open gap by fixing it or by having the user explicitly accept it, then ask one exact question, verbatim: "Is this running prototype exactly what you want?" Only an explicit yes appends the entry that lets step 2 begin. Anything else, including silence, logs a new gap and keeps the phase open. The agent is never the one who decides a phase is finished — the instructions are written so that it structurally can't be.

## Verification: adversarial by design

Step 5 pushes all of this furthest. Its `CLAUDE.md` states its stance outright: it is not there to confirm success, it is there to find non-conformance. Each specification item gets checked against the implementation across five dimensions — behavior, architecture, API contracts, invariants, completeness — and the bounded, one-item checks are delegated to a dedicated subagent, `spec-verifier`, that starts with no memory of how the implementation was built. It receives only the specification item, a location to check, and instructions to return one of four verdicts — CONFORMS, DEVIATES, MISSING, AMBIGUOUS — backed by concrete evidence: a file and line actually read, a test actually run, a behavior actually observed. The main session then re-checks every DEVIATES or MISSING verdict itself before it goes into the report. A finding here has to survive being independently doubted twice before it counts.

None of this is exotic. A folder per phase, a configuration file per folder, a handful of slash commands, one subagent. What it buys is the thing Part 1's argument depends on: five different jobs, each done by a session that only knows how to do that one job, handed off through checks that don't rely on anyone remembering the rules. Part 3 is about what happened when I pointed that same kind of scrutiny at the methodology's own documentation — including one place it didn't hold up, and one design question I still haven't settled.
