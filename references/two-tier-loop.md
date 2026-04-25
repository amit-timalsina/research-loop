# Two-tier loop

Pre-registration kills exploration speed. Karpathy's "100 experiments overnight" works only without it. Real research separates the modes.

## Exploratory tier

Karpathy-style. Agent picks a hypothesis from its prior or from current data, makes a code change, runs the fixed-budget experiment, looks at the result, keeps or discards.

**What's locked:**

- The compute budget (one experiment unit)
- The locked surfaces (data, eval harness)

**What's free:**

- The hypothesis
- The code change
- Whether to follow the result anywhere

**What's required:**

- Log every run with hypothesis + result + decision into the exploratory ledger

**What's forbidden:**

- Calling an exploratory result a "finding." It is a candidate.
- Promoting an exploratory result to a paper, a tweet, or a CEO summary without first running the confirmatory tier.

## Confirmatory tier

Pre-registered, full rigor. Hypothesis comes from either an exploratory candidate or a paper. Experiment card written before any compute. n≥5 seeds with IQM + 95% CI. Multi-lens panel. Decision rule explicit.

**What's locked:**

- All of the exploratory locks
- Plus: the experiment card
- Plus: the decision rule (cannot be revised after seeing results)
- Plus: which lenses count

**What's free:**

- The hypothesis (chosen from exploration or literature)
- The choice of which exploration to confirm

**What's required:**

- The experiment card lives in the workspace before any seed is launched
- Deviations during the run are logged as exploratory adjuncts, not incorporated into the confirmatory conclusion

## When to promote an exploratory result to confirmatory

- It would change a production decision if real
- The effect size is worth the seed count to verify (use a pilot to estimate the seed budget; trust the budget)
- Adjacent literature predicts the effect is real (so it's not a statistical fluke surfaced by running many experiments)

A finding that doesn't meet these gates does not need confirmation. It can stay in the exploratory ledger as "noted, not pursued."

## The honest "we don't know yet"

A confirmatory run with disagreeing lenses is a "we don't know yet." This is a valid output. Tweeting "we don't know yet" is more honest and more useful than tweeting an over-claim. Most over-claims in ML come from refusing this option.

## Why this two-tier split exists

Pure exploration (Karpathy autoresearch as written) loses to false positives at scale. With ~100 experiments overnight and no decision rule, you find spurious wins by sheer multiple-comparisons inflation. Pure confirmation kills velocity — every experiment costs a paper-read + a card + a power analysis, and most ideas die before reaching that bar anyway.

The split lets exploration be cheap and prolific while confirmation stays disciplined. Exploratory results filter into confirmatory candidates. Confirmatory results filter into product. The pipeline is what makes overnight throughput compatible with rigor.