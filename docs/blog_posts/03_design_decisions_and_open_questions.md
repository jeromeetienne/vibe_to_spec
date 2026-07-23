# Vibe to Spec, Applied to Itself: What Self-Review Found, and What I Haven't Solved Yet

*Part 3 of a series on the Vibe to Spec methodology*

Part 2 described the mechanism this methodology uses to keep each phase honest: a folder per phase, a Claude Code configuration per folder, nothing leaking between them. That whole scaffolding — the design essay behind it, the five phase folders, the configuration inside each one — came together over a single evening. Once it existed, I did the obvious thing a methodology built around specifications and review should do to itself: I turned the same scrutiny on its own documentation.

## Reviewing the documentation the way step 3 reviews a specification

I read all twenty files under `steps/` — the root README, every phase's own `README.md` and `CLAUDE.md`, every command, the one subagent — looking for exactly what Specification Simplification looks for in a specification: duplicated concepts, inconsistent naming, options nobody had a reason for anymore.

The most damning finding wasn't any single item on the list. It was the sentence that explained why the list existed at all: the root README's per-phase summary, each phase's own `README.md`, and each phase's `CLAUDE.md` all restate the same inputs, outputs, working rules, and ending condition — each in its own phrasing, none of them generated from the others. Three hand-maintained copies of the same set of facts, and in several places, they had already drifted apart from each other.

A methodology whose entire premise is that the specification is the one source of truth had, for its own documentation, three sources of truth that nobody had reconciled.

## What the drift actually looked like

The drift itself was small, almost funny in how small it was. Every phase's folder name, every flow diagram, and every phase's own `README.md` call it a "step." Every phase's `CLAUDE.md` calls the same thing a "phase" — and even that split wasn't clean: one `CLAUDE.md` referred to "Step 3" checking a line, directly underneath a heading that called the current one "Phase 2." A README table promised phases were described by eight fields; every phase entry in that same document actually listed nine. Three separate commands told the agent to run tests or observe real behavior, and none of the three had actually been granted permission to run anything.

None of it was dangerous. All of it was exactly the kind of thing this methodology exists to catch — just not in someone else's prototype this time. Two settings that do almost the same thing under slightly different names was the example I used in [Part 1](01_introduction.md) for what a vibe-coded prototype looks like after its first good week. Documentation drifts the same way code does, and for the same reason: nobody was maintaining one source of truth, they were maintaining three descriptions and trusting memory to keep them in sync.

## Fixing it, the same way a phase closes

I fixed all eight findings from that review, across two follow-up commits: the terminology settled on "step," the template corrected to nine fields, the missing permissions granted. The duplication itself didn't get removed — it got written down. `steps/README.md` now says explicitly that the template, each step's own `README.md`, and each step's `CLAUDE.md` describe the same facts at three levels of detail, that none is generated from the others, and that a rule change has to be applied in all three places by hand to stay in sync. Naming the tradeoff doesn't fix it. It at least means the next drift is a known risk instead of a silent one.

## What didn't get resolved

Not everything the review turned up got closed that cleanly. [Issue #4](https://github.com/jeromeetienne/vibe_to_spec/issues/4) came out of the same pass and is still open: whether each phase needs its own internal loop — build, analyze, review, critique — before a single unit of work counts as done, not just before the whole phase closes.

A few phases are already most of the way there, without ever having been designed for it. Verification's `spec-verifier` subagent already gets re-checked by the main session before its findings are recorded — a real build-review-critique loop, just scoped to one specification item at a time, not to the phase's own output. Specification Simplification already proposes and gets approval for one reduction at a time, which is close to the same shape. Specification Extraction and Implementation are the least loop-like of the five: each drafts a piece of work and reviews it with the user, but nothing in either phase's configuration asks the agent to critique its own output before showing it.

Turning that into a deliberate rule, instead of an accident of which phase happened to need it, raises questions I don't have settled answers to yet:

- Does every phase get the same loop, or does each one need its own version, the way verification's already does?
- Is it a new command, or does it fold into the loop each phase already runs?
- Who does the critiquing — the same session marking its own work, a fresh subagent with none of the context that produced it, or the user, which is really just moving the existing review earlier?
- Does it run automatically after every unit of work, or only when asked?
- Where does it sit relative to the moment the user actually sees the result — before, after, or does it replace part of that conversation?

## Why I'm leaving it open

I filed this as a design question instead of answering it on the spot, for the same reason step 4's configuration stops and asks about a specification gap instead of quietly deciding for itself: it changes the shape of every phase's configuration, and a change that size deserves its own real design pass, not a patch bolted on while I was reviewing something else.

Three posts in, the throughline hasn't moved since [Part 1](01_introduction.md): the specification is permanent, the implementation is disposable. It turns out that holds even when the thing being specified is the methodology itself. What's actually built lives at the [Vibe to Spec repository](https://github.com/jeromeetienne/vibe_to_spec), issues and all, including the one I haven't closed yet. If you have an opinion on how that loop should work, I'd like to hear it.
