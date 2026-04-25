# Ledger entry: <one-line title>

**Date:** <date>
**Card:** <link to experiment card>
**Tier:** [exploratory | confirmatory]

## Hypothesis (verbatim from card)

<copy from experiment card>

## Result

| Lens | Value | 95% CI | Direction |
|---|---|---|---|
| FQE | ... | ... | ... |
| Behavior-KL | ... | ... | ... |
| Sim-rollout | ... | ... | ... |
| Safety violations | ... | ... | ... |

## Decision

[Accept | Reject | Uncertain]

## If reject: which prior was wrong?

The specific belief the experiment falsified. Be honest. Not "we needed to tune more"; specific: "I assumed enrichment-helps-X implied enrichment-helps-X-similar; ablation showed otherwise."

This field is what makes the ledger valuable two months later. Without it, the same hypothesis gets re-tested.

## Predicted vs actual

Predicted: <from card>
Actual: <result>
Calibration delta: <how off was the prediction>

## Follow-up

Choose one:

- A new question logged in `QUESTIONS.md`, OR
- A new experiment card scheduled (link), OR
- "None — question closed."