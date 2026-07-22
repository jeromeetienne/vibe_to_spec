---
description: Run the phase-closing ritual — end-to-end walkthrough of the running prototype, resolve every open GAP (fix it or the user explicitly accepts it), then ask the final question "Is this running prototype exactly what you want?". Close phase 1 only on the user's explicit yes, recorded as the CLOSED entry in DECISIONS.md.
allowed-tools: Read, Edit, Write
---

Run the phase-closing ritual:

1. Read DECISIONS.md and list every GAP entry that is still open.
2. Walk the user through what the running prototype does, end to end, and how to run it.
3. For each open gap, ask the user: fix it now, or explicitly accept it as-is? Log the outcome (a fix becomes a DECISION entry; an acceptance is noted on the gap).
4. Then ask the final question, verbatim: "Is this running prototype exactly what you want?"
5. Only if the user explicitly answers yes:
   - append the CLOSED entry to DECISIONS.md — the user's agreement plus the list of explicitly accepted gaps (or "none");
   - tell the user phase 1 is complete and phase 2 can start in ../step_02_spec_extraction.
6. On any answer other than an explicit yes: log the new GAP and DECISION entries and continue exploring. Do not close. Never write the CLOSED entry yourself without the user's explicit agreement in this session.
