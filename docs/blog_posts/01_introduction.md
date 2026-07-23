# Vibe to Spec: Turning Exploratory Prototypes Into Maintainable Software

*Part 1 of a series on the Vibe to Spec methodology*

If you've spent real time coding alongside an AI agent recently, you already know the shape of this story. You open a conversation with no plan beyond a rough idea. You describe what you want, look at what comes back, adjust, and describe again. Within an hour you have something running that does exactly what you pictured — a feature, a tool, sometimes a whole small application — built faster than you could have designed it on paper. Then you try to build the next feature on top of it, and something has quietly gone wrong.

This way of working has a name now: **vibe coding**. 

You converse with an AI coding agent, iterate rapidly, experiment freely, and arrive at a prototype that behaves exactly as you wanted. It's one of the fastest ways ever invented to explore what software should do. It's also remarkably bad at producing software anyone can maintain afterward.

## What goes wrong

The symptoms are familiar if you've lived with one of these prototypes past its first good week. Two settings that do almost the same thing under slightly different names. A configuration flag nobody quite remembers the reason for. Three different places that each validate the same rule, almost but not quite the same way. None of this happened because anyone was careless. It happened because nobody was trying to design an architecture — they were trying to find out what the software should do, as fast as possible, and that's a completely different job.

Trying to refactor a prototype like that into production code usually fails for a specific reason, not a vague one: the real design decisions are buried inside implementation details that were never deliberate to begin with. There's nothing clean to extract, because nothing clean was ever kept separate from everything that was just exploration.

## The specification is permanent, the implementation is disposable

I've started calling the alternative Vibe to Spec, and it comes down to one sentence:

> **The specification is permanent. The implementation is disposable.**

Instead of gradually cleaning a messy prototype up into production code, Vibe to Spec treats the prototype as exactly what it is — a disposable exploration artifact, valuable for what it taught you and nothing more. The lasting asset isn't the prototype's code. It's a specification extracted from watching the prototype work: the concepts, the responsibilities, the workflows, the invariants that made it behave the way you wanted. Production code is then built from that specification. It is never evolved from the exploratory implementation that came before it.

A prototype answers one question: what do I actually want? A specification answers a different one: what's the simplest design that gets me there? Those aren't the same question, and I think trying to answer both of them with the same piece of code is exactly where vibe coding's maintainability problem comes from.

## Five phases, one job each

In practice, Vibe to Spec breaks this into five phases. Each one answers exactly one question and produces exactly one artifact, and none of them try to do more than that single job:

- **Exploration** asks *what do I actually want?* and produces a prototype, validated by you.
- **Specification Extraction** asks *what did I actually build?* and produces a raw specification.
- **Specification Simplification** asks *what's the simplest design that preserves the same behavior?* and produces the specification that becomes the source of truth for everything after it.
- **Implementation** asks *how should this be built?* and produces production code, with tests.
- **Verification** asks *does the implementation actually match the specification?* and produces an evidence-backed verdict.

Keeping each phase to exactly one job turns out to matter more than it sounds like it should. Part 2 of this series is entirely about why — and about the specific, slightly unusual thing that makes it hold up in practice rather than just on paper.

## Where this stands today

One thing worth saying plainly before going further: what I've described above is a methodology I've designed and am actively building the scaffolding for, not a playbook I've already run on a real project from end to end. I think the reasoning holds up, and I'll walk through why in detail as this series continues — including, later on, what happened when I turned the same scrutiny back onto the methodology's own documentation. But I'd rather say that upfront than let the pitch imply more certainty than I actually have.

This series is for anyone who already works this way — who has felt a prototype behave exactly as wanted, and then dreaded the idea of building the next feature on top of it. If that's familiar, the rest of this series is for you.

Part 2 goes into how this actually works in practice: five folders, five separate AI coding sessions, and one rule that turns out to matter more than any other — nothing a session learns in one phase is allowed to leak into the next one.
