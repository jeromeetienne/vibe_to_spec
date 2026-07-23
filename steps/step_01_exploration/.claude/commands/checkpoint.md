---
description: Run the validation ritual — show what the prototype currently does, explicitly ask the user "is this what you want?", wait for answers, and log every verdict (VALIDATED / GAP / DECISION) to STEP1_VIBE_DECISIONS.md. Trigger after every meaningful behavior change, or whenever the user wants to take stock.
argument-hint: [optional: which behavior to validate]
allowed-tools: Read, Edit, Write
---

Run one round of the explicit validation ritual:

1. State, in one or two sentences, what the running prototype currently does — focused on $ARGUMENTS if given, otherwise on what changed since the last checkpoint (read STEP1_VIBE_DECISIONS.md to see where that was).
2. Tell the user exactly how to run it and what to look at.
3. Ask the user, as explicit direct questions:
   - Is this what you want?
   - What feels wrong or off?
   - What is missing?
4. WAIT for the user's answers. Do not proceed on assumption or silence.
5. Log every verdict to STEP1_VIBE_DECISIONS.md, following the format defined in CLAUDE.md:
   - VALIDATED — each behavior the user confirmed as wanted
   - GAP — each thing the user explicitly said is not what they want, kept as close to the user's own words as possible
   - DECISION — any direction change that follows, and why

Create STEP1_VIBE_DECISIONS.md if it does not exist yet: a single `# Decisions` heading plus the `Prototype: <link or absolute path>` line (ask the user for the location if it is not known yet). Do not touch any other file in this folder.
