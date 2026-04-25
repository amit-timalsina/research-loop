# Eval rigor

Concrete recipes from Henderson 2018, Agarwal 2021, Colas 2018, and the Google Tuning Playbook. The opinions here are not original; they are the field's hard-earned consensus.

## The default reporter

For every confirmatory experiment, report:

1. **IQM** — interquartile mean: drop top + bottom 25%, mean the middle 50%
2. **Stratified bootstrap 95% confidence interval** around the IQM
3. **Performance profile** if comparing more than 2 algorithms (empirical CDF of normalized scores)

Stop reporting bare means and medians as the headline. They lie under the heavy-tailed seed distributions deep RL produces. Use `rliable` (github.com/google-research/rliable) — it does this in roughly 10 lines of code.

## Seed counts

5 seeds is a floor for sanity. For "X beats Y" claims:

- Run a 20-seed pilot to estimate σ
- Derive proper N from a power analysis with α=0.05, β=0.2
- Inflate by a margin to compensate for σ-estimation error
- For multiple-algorithm comparisons, apply Bonferroni correction (or Holm step-down or BH-FDR if you care about tightness)

When N < 20, Welch's t-test (not bootstrap) is the right inference. Bootstrap requires N ≥ 20 to give valid CIs.

## Knob classification

Before each experiment round, classify every hyperparameter as

- **Scientific** (the variable under test)
- **Nuisance** (must be re-tuned per condition for fair comparison)
- **Fixed** (held constant; this is a documented caveat on the conclusion)

Tune nuisance knobs per condition. Hold fixed knobs constant. Most "novel-method gains" in the literature are nuisance-knob gains. Isolation plots — best trial across nuisance knobs as a function of the scientific knob — are the only apples-to-apples comparison.

Reference: github.com/google-research/tuning_playbook

## What the codebase, framework, and hardware count as

Confounds, not constants. Henderson 2018 showed that identical hyperparameters produced materially different results across rllab vs. OpenAI Baselines vs. the original code. Hooker's "Hardware Lottery" (CACM 2021) makes the same point for hardware/compiler stacks.

When reporting a result, scope the conclusion. "On our pipeline, implementation X, hardware Y" is honest. "Algorithm A beats algorithm B" without scoping is over-claim.

## Multiple comparisons

If you run K algorithms and pick the winner, the headline result has K-fold inflated false-positive risk. Bonferroni correction is the simple, conservative answer. If you run K hyperparameter settings and pick the best, same problem. The discipline is to pre-register the comparison set; if you must explore K alternatives, treat the winner-pick as exploratory and run a confirmatory follow-up on a held-out test set.

## Performance profiles

For >2 algorithms: plot the empirical tail-CDF of normalized scores (fraction of runs above threshold τ, swept across τ). This shows distributional dominance, not just point estimates. A method whose performance profile dominates another's is a stronger claim than one where the means differ.

## Stop doing these

- Reporting max-of-training scores
- Fixing seeds (deep RL has GPU non-determinism that defeats this; the seed claim is unverifiable)
- Bare point-estimate tables without CIs
- Dichotomous p-value calls (effect sizes, please)
- Cherry-picked benchmarks where the method wins

## Citation pointers

- Henderson, Islam, Bachman, Pineau, Precup, Meger 2018
  "Deep Reinforcement Learning That Matters" (arXiv:1709.06560)
- Agarwal, Schwarzer, Castro, Courville, Bellemare 2021
  "Deep RL at the Edge of the Statistical Precipice" — NeurIPS Outstanding Paper (arXiv:2108.13264)
- Colas, Sigaud, Oudeyer 2018
  "How Many Random Seeds?" (arXiv:1806.08295)
- Pineau et al. 2021
  "Improving Reproducibility in ML Research" (jmlr.org/papers/v22/20-303.html)
- Google Brain Tuning Playbook (github.com/google-research/tuning_playbook)
- Hooker 2021
  "The Hardware Lottery" — CACM (arXiv:2009.06489)