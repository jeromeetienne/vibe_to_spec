---
description: Run the validation ritual — show what the prototype currently does, explicitly ask the user "is this what you want?", wait for answers, and log every verdict (VALIDATED / GAP / DECISION) to DECISIONS.md. Trigger after every meaningful behavior change, or whenever the user wants to take stock.
argument-hint: [optional: which behavior to validate]
allowed-tools: Read, Edit, Write
---

Run one round of the explicit validation ritual:

1. State, in one or two sentences, what the running prototype currently
   does — focused on $ARGUMENTS if given, otherwise on what changed since
   the last checkpoint (read DECISIONS.md to see where that was).
2. Tell the user exactly how to run it and what to look at.
3. Ask the user, as explicit direct questions:
   - Is this what you want?
   - What feels wrong or off?
   - What is missing?
4. WAIT for the user's answers. Do not proceed on assumption or silence.
5. Log every verdict to DECISIONS.md, following the format defined in
   CLAUDE.md:
   - VALIDATED — each behavior the user confirmed as wanted
   - GAP — each thing the user explicitly said is not what they want,
     kept as close to the user's own words as possible
   - DECISION — any direction change that follows, and why

Create DECISIONS.md with a single `# Decisions` heading if it does not
exist yet. Do not touch any other file.
