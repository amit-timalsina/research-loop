# Pre-registration protocol

Why and how. Adapted from the NeurIPS pre-registration workshop tradition (proceedings.mlr.press/v148) and Lipton & Steinhardt's "Troubling Trends in ML Scholarship" (arXiv:1807.03341).

## The commitment

Before any compute: write the experiment card. The card states

1. The hypothesis (falsifiable: direction + magnitude + threshold)
2. The metric panel (which lenses count)
3. The decision rule (what numerical condition triggers accept vs reject)
4. The locked surfaces (what does NOT change in this experiment)
5. The editable surface (the one thing that does change)
6. The seed count and the rationale for it
7. The compute budget
8. The predicted outcome (the agent's best guess, written down)

Use `assets/experiment-card.md`. The card is short — one page. A long unfilled card is worse than a short filled one.

## Why falsifiability matters

A non-falsifiable hypothesis can survive any result. "Adding X helps" is non-falsifiable: any positive result confirms it, any negative result is explained as "we didn't tune it well." A falsifiable hypothesis names the threshold: "Adding X increases IQM-FQE by ≥2% with stratified bootstrap 95% CI lower bound > 0%." Either the threshold is met or it is not.

## The decision rule is locked

Once the seeds are running, the decision rule cannot be revised. If the result lands at +1.8% and the rule said +2%, the result is a reject, not an "almost." The discipline is that "almost" doesn't ship.

This sounds harsh; it is the only way the gates do work. The next experiment can have a different threshold. This experiment cannot.

## Deviations

Things will deviate. A bug surfaces, a lens needs reinterpretation, a seed crashes. Log the deviation in the experiment card under "deviations." The deviated portion of the analysis is exploratory, not confirmatory. The confirmatory conclusion only covers the parts that ran as pre-registered.

## What pre-registration is not

It is not a guarantee against bias. It is a structural disincentive against post-hoc rationalization. Both can survive bad faith; neither can survive sloppy thinking left unchecked.

## Pre-registration is cheap

A typical card takes 15 minutes to fill. The 15 minutes return weeks of saved confusion when the result is ambiguous. The cost-benefit is overwhelming and the only reason most teams don't do it is that nobody told them how cheap it is.

## The predicted-outcome field

The "predicted outcome" field exists for a non-obvious reason: tracking calibration over time. If your predictions are routinely too optimistic, that itself is a research finding about your priors. After 20 experiment cards, audit the predicted vs actual; the gap tells you something the individual experiments cannot.

## What goes in the workspace

Each experiment card is a markdown file in the experiments directory, named with a stable slug (e.g. `2026-04-26-ccr2-h1-feature-ablation.md`). Pre-commit it before launching seeds. The git history is the audit trail. If the card was modified after seeds launched, the reviewer will see it.