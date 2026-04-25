# research-loop

A Claude Code skill for doing AI/ML research the way it should be done — fast, but with the discipline that the field has spent a decade learning the hard way.

Adapts [Karpathy's autoresearch](https://github.com/karpathy/autoresearch) philosophy and adds the primitives it leaves implicit: **literature in the loop, pre-registration before training, multi-lens evaluation, statistical rigor on seed variance.**

## Why this exists

Karpathy's autoresearch loop is great: agent reads code → forms hypothesis from its prior → 5-min train → keep/discard. It runs ~100 experiments overnight.

The implicit assumption is that the agent's deep-learning literature prior is enough to generate good hypotheses. For our offline-RL stack at [Noumenal](https://noumenal.ai) it wasn't:

- The agent's prior missed an action-leak in our state vector for weeks.
- It missed a monotone-in-action reward that guaranteed policy saturation.
- It missed predictor honesty failures — overfit composite features that destroyed downstream FQE.

Each cost weeks of training runs and chased misleading metrics. The fix wasn't smarter algorithms. It was reading the papers that had already documented these exact failure modes.

So this skill adds **literature *in* the loop**: every confirmatory experiment cites at least one paper note. The agent (and the human) re-read the field before forming a hypothesis. Otherwise we re-derive our own blind spots.

## What it does

The skill installs into Claude Code (`~/.claude/skills/research-loop/`) and triggers when you talk about experiments, papers, evaluation, seeds, ablations, hypotheses, or any architectural change you want to test before adopting.

It enforces five gates:

1. Hypothesis grounded in a paper note (or a logged exploratory result)
2. Pre-register hypothesis + decision rule before any compute
3. n≥5 seeds with IQM + stratified bootstrap 95% CIs
4. Multi-lens evaluation; single metric never decides
5. Falsified hypotheses logged with the prior that was wrong

And it preserves Karpathy's velocity by separating **exploratory** runs (agent picks, no pre-reg, fixed budget, just logs) from **confirmatory** runs (full rigor). Most of the day's work is exploratory; a small number of confirmatory runs per week graduate findings into product.

## Install

```bash
git clone https://github.com/amit-timalsina/research-loop.git ~/.claude/skills/research-loop
```

In a Claude Code session, the skill auto-triggers on relevant prompts. To force-load:

```
/skill research-loop
```

## Adapt to your stack

The only project-specific file is [`references/uedc-bindings.md`](references/uedc-bindings.md) — it shows what "the locked surfaces, the editable surfaces, and the canonical paths" look like for our project (Noumenal's industrial-RL platform). Copy [`references/bindings-template.md`](references/bindings-template.md), fill in your own paths and unit definitions, and replace the UEDC binding with yours.

The other four reference files are generic:

- `references/two-tier-loop.md` — exploratory vs confirmatory split
- `references/literature-protocol.md` — the three-questions paper-reading discipline
- `references/pre-registration.md` — the experiment card protocol
- `references/eval-rigor.md` — IQM, bootstrap CIs, power-analyzed seed counts, knob classification

## The structure

```
research-loop/
├── SKILL.md                       The spine: thesis, gates, judgment calls, aesthetic
├── references/
│   ├── two-tier-loop.md           Exploratory vs confirmatory
│   ├── literature-protocol.md     Three questions per paper
│   ├── pre-registration.md        The experiment-card discipline
│   ├── eval-rigor.md              IQM + bootstrap + seeds + knob classification
│   ├── bindings-template.md       Blank template for your project
│   └── uedc-bindings.md           Worked example (Noumenal's offline-RL stack)
├── assets/
│   ├── paper-note.md              Template
│   ├── experiment-card.md         Template
│   └── ledger-entry.md            Template
└── evals/
    └── evals.json                 Three test prompts
```

## Lineage

This skill stands on the shoulders of the field's hard-earned consensus on what rigorous ML research actually requires. The references cite primary sources where the rules came from:

- **Karpathy 2025** — [autoresearch](https://github.com/karpathy/autoresearch) (the loop structure, the fixed budget, the aesthetic)
- **Karpathy 2019** — ["A Recipe for Training Neural Networks"](http://karpathy.github.io/2019/04/25/recipe/) (data first, dumb baselines, overfit one batch)
- **Henderson et al. 2018** — ["Deep RL That Matters"](https://arxiv.org/abs/1709.06560) (multi-seed reporting, codebase as confound)
- **Agarwal et al. 2021** — ["Statistical Precipice"](https://arxiv.org/abs/2108.13264) (IQM, stratified bootstrap, performance profiles)
- **Colas et al. 2018** — ["How Many Random Seeds?"](https://arxiv.org/abs/1806.08295) (power analysis from pilots)
- **Lipton & Steinhardt 2018** — ["Troubling Trends in ML Scholarship"](https://arxiv.org/abs/1807.03341) (ablations, robustness)
- **Pineau et al. 2021** — [NeurIPS Reproducibility Checklist](https://jmlr.org/papers/v22/20-303.html)
- **Google Brain** — [Tuning Playbook](https://github.com/google-research/tuning_playbook) (scientific/nuisance/fixed knob classification)
- **Hooker 2021** — ["The Hardware Lottery"](https://arxiv.org/abs/2009.06489) (scope conclusions to hardware regime)

The five gates are not novel. The contribution is making them the structural default for an AI agent collaborating on research, where the rationalization pressure is highest.

## Contribute

This is early. We're using it on our own RL stack and watching where it bends or breaks. PRs welcome — especially for:

- Bindings files for other projects (publish yours, link from here)
- Additional reference files for archetypes the skill doesn't cover well yet
- Test prompts that stress-test the skill's triggering and behavior
- Evidence that any of the five gates is wrong on your data

If you use it and have feedback, open an issue. The skill itself is hypothesis #1.

## License

MIT. See [LICENSE](LICENSE).

## Author

[Amit Timalsina](https://github.com/amit-timalsina) — building [Noumenal](https://noumenal.ai), AI that runs operations.
