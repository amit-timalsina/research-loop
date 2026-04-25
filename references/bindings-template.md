# Project bindings (template)

Copy this file, rename to match your project (e.g. `acme-bindings.md`), and fill in your project's specifics. Then update `SKILL.md` to point at your bindings file instead of `uedc-bindings.md`.

The point of bindings: the generic skill is portable across projects. The bindings file makes it specific. It defines what "one experiment unit" is in your stack, what surfaces are locked, what surfaces are editable, and where things live.

---

## Your compute unit

One experiment unit = <fill in: e.g. "5,000 steps × 5 seeds × held-out evaluation on the canonical pipeline">.

This is your "Karpathy 5 minutes." Pick one and freeze it. Comparable experiments require comparable cost.

When the unit is wrong (e.g. one benchmark needs more steps to converge): a 1-seed pilot estimates the right size. The unit adjusts; the *fact* that there is a fixed unit per experiment does not.

## Your locked surfaces

These don't change without a separate, named project to change them. Changing them silently invalidates comparability with every prior result.

- The data preprocessing pipeline (`<your path here>`)
- The evaluation harness (`<your path here>`)
- The metric computations (`<list>`)
- The benchmark holdout split (`<your split spec>`)
- Your canonical multi-lens panel (`<list of lenses>`)

## Your editable surfaces (per question type)

For each common research question type in your stack, name the editable surface. Lock everything else.

- **<Question type 1>**: `<file/field>`
- **<Question type 2>**: `<file/field>`
- **<Algorithm questions>**: `<trainer choices>` — but argue for them explicitly because they multiply seed-count budget

## Where things live

- Specs / configs: `<path>`
- Training: `<path>`
- Benchmark results: `<table or path>`
- Live policies / production endpoints: `<endpoint>`

## Your two-tier split, made concrete

- **Exploratory:** `<dir>` — one-off runs, single seed sometimes OK, no card required, but every run gets a one-paragraph entry in `EXPLORATORY_LOG.md`.
- **Confirmatory:** `<dir>` — pre-registered card, n≥5 seeds, full multi-lens panel, ledger entry on completion.

## Your reading queue

- Paper notes: `<path>` — one file per paper, named `<year>-<first-author>-<short-slug>.md`
- Reading queue index: `<path>/READING_QUEUE.md` — prioritized list tied to open questions

## Your open-questions tracker

`<path>/QUESTIONS.md`. Each entry is one paragraph: the empirical fact, the open hypotheses, and the experiment cards referencing it. When a question is answered, archive to `QUESTIONS_ANSWERED.md` with a pointer to the ledger entry that closed it.

## What's NOT in this binding

Anything specific to one open question. Open-question files live in your question tracker, not in the skill's bindings.
