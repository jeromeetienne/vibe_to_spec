---
description: Append one entry to STEP1_VIBE_DECISIONS.md — DECISION (direction chosen/changed/abandoned and why), VALIDATED (user confirmed a behavior), or GAP (user explicitly said something is not what they want). Use the moment it happens in conversation, without waiting for a full checkpoint.
argument-hint: [what happened and why]
allowed-tools: Read, Edit, Write
---

Append one entry to `STEP1_VIBE_DECISIONS.md`, following the format defined in CLAUDE.md (dated section, labeled bullet: DECISION, VALIDATED, or GAP).

- Create the file if it does not exist yet: a single `# Decisions` heading. If the project's `.vibe_to_spec.yaml` has no `dirty_impl_resources` entry yet, ask the user for the prototype's location and description and add it there.
- Add a `## <today's date>` heading if today does not already have one.
- Base the entry on $ARGUMENTS if given. If $ARGUMENTS is empty, infer it from the last few turns of conversation and show it to the user before writing, so it can be corrected.
- For GAP entries, keep the user's own words as closely as possible.

Keep each entry to 1-3 sentences. This is a log of decisions and validations, not a summary of the code. Do not touch any other file.
