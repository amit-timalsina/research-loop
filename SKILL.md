---
name: research-loop
description: Use this skill whenever the user is planning, running, analyzing, or arguing about ANY empirical AI/ML experiment — picking the next experiment, reading a paper, evaluating whether a result is real or seed noise, deciding whether something should ship, or asking whether a design proposal (theirs, a cofounder's, anyone's) should be tested before adopting. Enforces literature-grounded hypotheses, pre-registration before training, multi-lens evaluation with IQM + bootstrap CIs, and a two-tier exploratory-vs-confirmatory split that preserves overnight velocity without sacrificing rigor. ALWAYS use when the user mentions experiments, hypothesis, evaluation, seeds, FQE, benchmark, paper, literature, ablation, falsified, "is this real", or proposes any architectural change before running it. SKIP for pure code edits with no experimental claim or simple bug fixes.
---

# Research loop

A research methodology for AI/ML experiments. Adapts Karpathy's autoresearch philosophy with the missing primitives the field has spent a decade learning the hard way: literature in the loop, pre-registration, multi-lens evaluation, statistical rigor on seed variance.

## The thesis

An experiment is one swing of a loop:

1. **Question** — a fact our data surfaced, not a claim from a design doc
2. **Read** — a paper or two that makes a falsifiable claim about it
3. **Pre-register** — hypothesis + decision rule + locked surfaces, written down
4. **Run** — fixed compute budget, fixed eval panel, n≥5 seeds
5. **Decide** — multi-lens accept/reject; never single metric, never single seed
6. **Log** — the result, including which prior was wrong if rejected

Speed and rigor are not in tension when you separate exploration from confirmation. Karpathy's "100 experiments overnight" works for exploration. It does not work as the only mode. See `references/two-tier-loop.md` for the split.

## The five gates that always apply

These are non-negotiable because they are exactly the things people rationalize away when a deadline is close. Each maps to a documented failure mode in the ML literature, not just to taste.

### Gate 1. Hypothesis grounded in a paper note OR a logged exploratory result.

Don't run a confirmatory experiment because "it would be cool to try." Find the paper that makes a relevant prediction, write a one-page note (`assets/paper-note.md`), state what your setup should show if the paper is right. Then test it.

Why: agents and humans re-derive their own blind spots when they don't read. Action-leak, reward monotonicity, and predictor honesty all sat in our code for weeks because no one had read the paper that flagged them. For pure exploration (Karpathy mode), skip this gate. Log the result anyway.

### Gate 2. Pre-register hypothesis, metric, and decision rule before any compute.

Use `assets/experiment-card.md`. Hypothesis must be falsifiable: state direction, magnitude, and threshold. Decision rule must be explicit: "Accept if FQE improvement at 95% bootstrap CI lower bound > 2%."

Why: post-hoc reasoning will find a story for any result. Pre-registration is the commitment device. Deviations during the run get logged as exploratory, not silently incorporated into the conclusion. The pre-registration tradition (NeurIPS workshops, Lipton & Steinhardt 2018) exists because every other discipline has paid this price already.

### Gate 3. n≥5 seeds, IQM with stratified bootstrap 95% CIs.

5 is the floor. For "X beats Y" claims, run a pilot of 20 seeds first, then derive proper N from a power analysis (`references/eval-rigor.md`). Bootstrap requires n≥20 to be valid; for smaller N use Welch's t-test.

Why: SAC at seed=42 on IB scored −226. SAC at seed=0 on IB scored −2183. Same config. Means and medians lie under heavy-tailed seed distributions; IQM (drop top + bottom 25%) is robust. Source: Agarwal et al., NeurIPS 2021.

### Gate 4. Multi-lens evaluation; single metric never decides.

The default panel is FQE + behavior-KL + sim-rollout + safety-violations. Adapt per archetype, but never collapse to one number. If lenses disagree, the result is "uncertain" — that is a valid output.

Why: FQE can be fooled by the same shortcut the policy learned (we observed this with the action-leak). One number is one window into the system; multiple windows triangulate.

### Gate 5. Falsified hypotheses are first-class outputs.

When a hypothesis fails, write the ledger entry (`assets/ledger-entry.md`). Don't just delete the branch. Name the prior that was wrong. The next person — often you in two weeks — needs to know.

Why: the ICBINB workshop and the "negative results" tradition exist because the absence of a result is itself signal. Without a falsification log, the same hypothesis gets re-tested.

## The five judgment calls (flexible)

These are not gated because they require taste. Get them wrong and the next experiment will tell you.

- **Which paper to read.** Recency, public code, and adversarial review count more than authority. A 2018 paper with reproducible code beats a 2024 paper without. Search Semantic Scholar before adopting any "this is the consensus" claim.

- **How to phrase the hypothesis.** Sharper is better. "Adding feature X increases FQE" is weaker than "Adding X moves IQM-FQE by ≥2% on benchmark B with 95% bootstrap CI lower bound > 0%, and behavior-KL stays below 0.3."

- **Which lenses to add beyond the default panel.** If the archetype has a natural extra signal (constraint violation rate for resource allocation, regret bound for recommendation), add it. Don't replace the defaults — extend.

- **When the data is screaming for a pivot.** Three regressing benchmarks with the same enrichment is signal, not noise. Pivot the question, not the metrics.

- **When a domain prior beats a Bitter-Lesson alternative.** Sutton was right in general; he was not right always. Industrial RL has hard physical constraints compute cannot wash away. Document the prior; treat it as a refutable claim, not a permanent belief.

## The aesthetic

These are Karpathy's principles, kept because they are correct.

- **Delete > add.** A composite feature that doesn't move FQE > 0.5σ across the multi-lens panel — delete it. Removing a feature without losing performance is always a win.
- **Fixed compute budget per run.** Comparable experiments require comparable cost. Pick one (e.g. 5k IQL × 5 seeds × FQE) and freeze it. See your project's bindings file (worked example: `references/example-bindings.md`).
- **One editable surface per experiment.** Lock everything else. Reviewable diffs.
- **"FQE: n/a" is not a promotable state.** A policy with unresolved evaluation is a candidate, not a policy.

## When to load which reference

- **`references/two-tier-loop.md`** — when deciding whether this experiment is exploratory or confirmatory. Most of the day's work is exploratory; a small number of runs per week are confirmatory.
- **`references/literature-protocol.md`** — when starting a paper note, evaluating whether a paper applies to our setup, or arguing whether a claim is well-supported.
- **`references/pre-registration.md`** — when filling out an experiment card or deciding whether a deviation counts as a deviation.
- **`references/eval-rigor.md`** — when running statistics: IQM, bootstrap CIs, power analysis, multiple-comparisons correction.
- **`references/example-bindings.md`** (or your project's bindings file) — defines the compute unit, locked surfaces, editable surfaces, and canonical paths to the eval harness, the spec, and the results ledger. **Copy `references/bindings-template.md` and fill in your own; this is the one file to replace when porting the skill to another project.**

## Templates

When the user starts a new piece of research, scaffold from the right template:

- `assets/paper-note.md` — for capturing what a paper claims and what it predicts in our setup.
- `assets/experiment-card.md` — for pre-registering an experiment.
- `assets/ledger-entry.md` — for logging a finished experiment, including which prior was wrong.

These are short by design. A one-page filled document beats an unfilled five-page document every time.

## A worked example: the SAIL CCR2 question

The empirical fact: identical PHY_TI predictors that lift CCR1 by +50% tank CCR2 by −7%. Naive next move: "ablate one composite at a time." The skill in motion:

1. **Question** — phrased honestly: "Why does the same enrichment hurt CCR2?"
2. **Read** — Park et al. NeurIPS 2024 (policy extraction is the bottleneck); Tarasov et al. ReBRAC (auxiliary signals on narrow data); Liu et al. NeoRL-2. Three paper notes, one page each.
3. **Pre-register** — three competing hypotheses, each with a falsification rule:
   - H1 (state): a single composite is the culprit; ablation finds it.
   - H2 (reward): R10's reward predictor is misaligned for CCR2; swapping CCR1's reward into CCR2's spec would lift it.
   - H3 (saturation): CCR2's behavior policy is already near-optimal; the policy-improvement headroom is just smaller than CCR1's.
4. **Run** — pre-registered, 5 seeds each, fixed compute budget.
5. **Decide** — multi-lens panel; if lenses disagree, "uncertain" is the answer and we need a different question.
6. **Log** — whichever hypotheses fail, the ledger gets the entry plus the prior that was wrong (e.g., "I assumed enrichment-helps-X implied enrichment-helps-X-similar; ablation showed otherwise").

This is the loop, instantiated.

## A note on authority

Designs, papers, and prior beliefs (yours, the cofounder's, mine, anyone's) are hypotheses. Experiments are evidence. When they conflict, experiments win. This skill exists to make that the structural default, not just the stated value.