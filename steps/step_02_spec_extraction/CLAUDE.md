# Phase 2 — Specification Extraction (Vibe to Spec, phase 2 of 5)

Goal: recover the architecture that was actually built, by turning the
validated prototype of phase 1 into SPEC.md — the raw specification.

The deliverable of this phase is SPEC.md, and nothing else. No product
code is written in this phase.

## Inputs — strictly read-only

- The prototype — at the external location recorded by the `Prototype:`
  line of ../step_01_exploration/DECISIONS.md (a repository link or an
  absolute path; the code is completely outside this repository). You
  may read it, and run it to observe behavior. You must NEVER modify,
  fix, or improve it. If the location is a repository link, work from a
  local clone.
- ../step_01_exploration/DECISIONS.md — what the user validated, the gaps
  the user explicitly accepted, the directions tried or abandoned, and why.

Precondition: DECISIONS.md must contain the CLOSED entry — the user's
explicit agreement that ended phase 1. If it does not, stop and tell the
user phase 1 is not closed yet.

## The extraction loop

1. Read the prototype (and run it if observing real behavior helps) and
   read DECISIONS.md.
2. Draft or extend ONE section of SPEC.md at a time (structure below).
3. Describe what IS, not what should be. Do not redesign, do not improve,
   do not add anything the prototype does not do.
4. Ignore implementation details unless they are architecturally
   significant.
5. Every time you meet a behavior you cannot classify, ask the user
   explicitly (see "Intentional or accidental" below). Do not decide alone.
6. Show the drafted section to the user and ask: does this correctly
   describe what was built? Anything missing or over-stated?
7. Log the user's corrections into the spec, then move to the next section.

Never fill several sections without a user review in between. The user
steers the spec exactly like they steered the prototype.

## SPEC.md — required structure

    # <product name> — Specification
    Status: draft
    Prototype: <the recorded location, copied from DECISIONS.md>

    ## Purpose            — what the product is and for whom
    ## Concepts           — the nouns: each concept, its meaning, its role
    ## Responsibilities   — what each part is in charge of
    ## Workflows          — what happens, step by step, for each main flow
    ## APIs               — the surfaces: commands, endpoints, contracts
    ## Data structures    — the shapes of the data, and where they live
    ## Invariants         — what must always hold true
    ## Constraints        — limits, environment, dependencies
    ## Assumptions        — what is taken for granted
    ## Known gaps         — the gaps the user explicitly accepted in
                            phase 1, carried over from DECISIONS.md
    ## Excluded behaviors — behaviors the prototype shows but the user
                            confirmed are accidental and NOT wanted

Empty sections stay present with "None." — an absent section reads as
"not extracted yet", an explicit "None." reads as "checked, nothing".

## Intentional or accidental — only the user decides

The prototype contains both designed behavior and accidents of vibe
coding. When you find a behavior whose status is unclear:

1. Show it to the user: "the prototype does X in situation Y".
2. Ask: is this intentional (goes into the spec) or accidental?
3. If accidental, ask: should it be excluded (→ Excluded behaviors) or
   is it actually a gap the user wants noted (→ Known gaps)?

Never classify silently. A wrong guess here corrupts the source of truth
for every later phase.

## What this phase never does

- Never modifies the prototype, nor anything under
  ../step_01_exploration/.
- Never invents behavior that was not observed in the prototype.
- Never "cleans up" the design on paper — simplification is phase 3's
  job, done later on the complete raw spec.
- Never sets the spec status to agreed — only the user does, through
  /close-step.

## Ending the phase

Phase 2 ends ONLY when the user explicitly agrees that SPEC.md fully
accounts for the prototype. Use /close-step to run the closing
walkthrough. The agreement is recorded as the Status line of SPEC.md:

    Status: agreed by the user on <date>

Step 3 checks this line as its precondition.
