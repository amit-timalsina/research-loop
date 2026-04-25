# Literature protocol

Why we read papers in the loop, not before the loop.

## Three questions per paper

For every paper that bears on an open question, write a paper note answering exactly three things:

1. **The core claim.** One sentence. The thing the paper says is true.
2. **The falsifier.** What experimental result would disprove it. If you can't write this, you don't understand the paper.
3. **The prediction for our setup.** Specifically: what does this paper say should happen on benchmark X with our pipeline. If the paper is silent on our setup, write that — it's a useful signal.

The paper note template (`assets/paper-note.md`) enforces this structure.

## What "literature in the loop" means

Every confirmatory experiment cites at least one paper note. Every falsified hypothesis logs which paper's prediction it tested. Over time the paper-note corpus becomes a personal map of the field's claims and how they fared on our data.

## When to skip

Pure exploration doesn't need a paper note. The agent's prior is fine to form a hypothesis from. The cost is that exploratory results don't promote to confirmatory without the paper-note step.

## When a paper claim and our data disagree

The paper is not necessarily wrong. Our setup may differ in a way that matters. Document the difference in the ledger entry, and write a follow-up question for the next research cycle. If the difference doesn't explain the disagreement, the paper claim is on probation in our setup specifically — flag it in the paper note.

## Source quality signals

- Public code that runs and reproduces the headline number
- A held-out benchmark beyond the one the paper introduces
- A statement of the paper's failure mode (most papers omit this; the ones that include it are gold)
- A non-aspirational discussion section (a paper that admits its limits is more useful than one that doesn't)

The absence of any of these is not disqualifying. It is a calibration on how much weight to put on the claim.

## How many papers per question

Two to four. One paper makes a claim; you need at least one with an adjacent or contradicting view to triangulate. More than four and the exercise becomes literature review, which is a different mode and a different time-scale.

## Parallel reading via subagents (velocity unlock)

Reading 5 papers sequentially is ~10 hours. Reading 5 papers via 5 parallel subagents is ~45 minutes wall-clock. The wall-clock difference is the entire reason a 3-day research sprint is feasible at all.

Each subagent prompt should include:

- The paper's URL (arXiv preferred — the agent fetches and reads the actual paper, not a summary)
- The relevant context from your bindings (so the agent can write the "prediction for our setup" field with specifics, not generics)
- The paper-note template structure (so output is uniform and committable)

The agent returns a one-page note. Review for accuracy — subagents over-summarize and under-engage with method/limits more than humans do; push back on superficial reads. Then commit. Treat the subagent's note as a first draft; if it doesn't help you make a card decision, dispatch a follow-up subagent to read the paper's discussion section and limitations specifically.

This is the gap-discipline mechanism in operation: while one set of experiments runs, the next set's literature is being prepared in parallel. No wall-clock idleness.

## Where to find papers

- Semantic Scholar (semanticscholar.org) — citations + influential-citation count
- arXiv — pre-prints, often with public code
- Papers With Code — links code to claims; filters out paper-only claims
- OpenReview — peer review threads expose how reviewers attacked the claims
- Twitter/X researcher threads — fastest signal, lowest rigor; use to find papers, not to cite them

## When the paper doesn't exist

Sometimes the question is novel and no paper has addressed it. That is rare and worth writing down. Document it in the question tracker as "no relevant prior." This is itself a contribution if your eventual experiment becomes the first published result.