# UEDC bindings

This file binds the generic research-loop methodology to our specific stack. Replace it for other projects; the rest of the skill is portable.

## Our compute unit

One experiment unit = 5,000 IQL steps × 5 seeds × FQE evaluation on temporal 80/20 holdout, on the canonical eval pipeline.

This is our "5 minutes." Pick one and freeze it. Comparable experiments require comparable cost.

When this unit is wrong (e.g., 5k is below convergence; IB needed 10k): the pilot is a 1-seed run that estimates step-to-convergence. The unit adjusts; the *fact* that there is a fixed unit per experiment does not.

## Our locked surfaces

Across all experiments, these don't change without a separate, named project to change them:

- The data preprocessing pipeline (e.g. `policy_features.py`)
- The FQE evaluation harness
- The behavior-KL computation
- The sim-rollout harness (with action-lag refresh)
- The benchmark holdout split (temporal 80/20)
- The canonical multi-lens panel: FQE + behavior-KL + sim-rollout + safety-violation rate

If you change one of these, you have invalidated comparability with every prior result. Don't do it as a side effect of an experiment.

## Our editable surfaces (per question type)

- **MDP-design questions**: `spec.json` reward_components, state declarations, action spec
- **State-enrichment questions**: composite-feature R8/R9/R10 generators in the discovery pipeline
- **Algorithm questions**: trainer choice (IQL / ReBRAC / CQL-IQL); but these multiply the seed-count budget — argue for them explicitly
- **Predictor questions**: which predictors are admitted as state or as reward components, the honesty gates that gate them

For any one experiment: name exactly one editable surface and lock everything else.

## Where things live

- Specs: `cloud/cloud-services/internal/discovery/...` (Go) → spec.json rendered into the policy manifest
- Training: `ml-service/...` (Python)
- Benchmark results: `rl_policy_benchmarks` Postgres table
- Live policies: `/api/v2/rl/infer`
- Discovery hypotheses + evidence: `discovery_hypotheses` Postgres table

## The results ledger

Every confirmatory experiment writes to `benchmarking/experimental/LEDGER.md` using the `assets/ledger-entry.md` schema. Every falsified hypothesis gets an entry. The ledger is the institutional memory.

## The reading queue

Paper notes live at `benchmarking/papers/`. One file per paper, named `<year>-<first-author>-<short-slug>.md`. The reading queue itself lives at `benchmarking/papers/READING_QUEUE.md` — a prioritized list tied to the open questions each paper bears on.

## Open questions tracker

`benchmarking/QUESTIONS.md`. Each entry is one paragraph: the empirical fact, the open hypotheses, and the experiment cards referencing it. When a question is answered, archive to `benchmarking/QUESTIONS_ANSWERED.md` with a pointer to the ledger entry that closed it.

## Our two-tier split, made concrete

- **Exploratory:** `benchmarking/experimental/EXPLORATORY/` — one-off runs, one seed sometimes OK, no card required, but every run gets a one-paragraph entry in `EXPLORATORY_LOG.md`.
- **Confirmatory:** `benchmarking/experimental/CONFIRMATORY/` — pre-registered card, n≥5 seeds, full multi-lens panel, ledger entry on completion. Promotion from exploratory requires the gates listed in `references/two-tier-loop.md`.

## What's NOT in this binding

Anything specific to one open question. Open-question files (e.g. CCR2 ablation tracking) live in `benchmarking/QUESTIONS.md` and the per-question ledger. They are not part of the skill.