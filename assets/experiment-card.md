# Experiment card: <one-line title>

**Card ID:** <unique slug, e.g. ccr2-h1-feature-ablation-2026-04-26>
**Author:** <name>
**Date written:** <date>
**Tier:** [exploratory | confirmatory]
**Paper grounding:** <link(s) to paper-note(s); empty for exploratory>

## Hypothesis

One sentence. Falsifiable. State direction + magnitude + threshold.

Example: "Removing composite feature `temp_residual` from SAIL CCR2 spec increases IQM-FQE by ≥5% with stratified bootstrap 95% CI lower bound > 0%, while behavior-KL stays below 0.3."

## Editable surface

The one thing that changes in this experiment. Be specific (file, field, line). Everything else is locked.

## Decision rule

The numerical condition that triggers accept vs reject. Locked once seeds run.

Example: "Accept if IQM-FQE 95% CI lower bound > 0% AND behavior-KL ≤ 0.3. Reject if either fails. Uncertain if FQE accepts but sim-rollout disagrees."

## Metric panel

Which lenses count for this experiment. Default is FQE + behavior-KL + sim-rollout + safety-violations. Extend, don't replace.

## Seed count + rationale

n = ___. Why this n: <pilot estimate / power analysis / floor>.

## Compute budget

One experiment unit (5k × 5 seeds × FQE) unless explicitly justified.

## Predicted outcome

The author's best guess of the result, written down before the run. Used to calibrate priors over time. Audit predicted vs actual every ~20 cards.

## Deviations

(Filled in during/after the run. Each deviation is logged here. Deviated analysis is exploratory; only the as-pre-registered analysis is confirmatory.)