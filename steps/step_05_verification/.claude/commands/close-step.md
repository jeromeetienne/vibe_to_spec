---
description: Run the step-closing ritual — every specification item must hold a verified verdict and no AMBIGUOUS may remain; present the summary; on full conformance the user confirms the cycle is complete, otherwise the gap list is agreed and handed back to step 4. Either conclusion happens only on the user's explicit confirmation, recorded in VERIFICATION.md.
allowed-tools: Read, Edit, Write, Grep, Glob
---

Run the step-closing ritual:

1. Read VERIFICATION.md: every specification item must have a verdict, and no AMBIGUOUS may remain — resolve those with the user first. If items are still unverified, run /verify; never conclude on partial coverage.
2. Present the summary: counts per verdict, and the full list of DEVIATES and MISSING items with their evidence.
3. If everything CONFORMS, ask the user, verbatim: "Do you confirm this implementation faithfully realizes the specification?" Only on the explicit yes: set `Status: concluded on <date> — full conformance` and tell the user the Vibe to Spec cycle is complete.
4. Otherwise: agree with the user the gap list to hand back to step 4; record it under `## Handed back on <date>` in VERIFICATION.md; set `Status: concluded on <date> — gaps handed back`. Step 4 reopens, and verification runs again after the fixes.
5. Either conclusion happens only on the user's explicit confirmation. Never conclude on your own judgment.
