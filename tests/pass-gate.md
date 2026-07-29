# Pass Gate

The acceptance gate this checker must hold before shipping.

---

## Gate Definition

**Learner-specified gate:**

> I dont have it. I dont have it. I dont have it

---

## Metric

*Not specified by builder.*

## Threshold

*Not specified by builder.*

## Re-run Cadence

*Not specified by builder.*

---

## Contested-Call Rulings

When the checker and a human reviewer disagree on a dial score, the ruling is logged here with Atlas's opposing case preserved.

| Probe | Dial | Checker Score | Human Score | Ruling | Atlas's Opposing Case |
|-------|------|---------------|-------------|--------|----------------------|
| *(No contested calls recorded — gate definition incomplete)* | — | — | — | — | — |

---

## Gate Status

**Current status:** INCOMPLETE

The gate cannot be evaluated because the builder did not provide:
- A concrete metric (e.g., "vocabulary_fit accuracy across probes")
- A numeric threshold (e.g., "≥ 90%")
- A re-run trigger (e.g., "weekly, or on any prompt change")

---

## How to Complete This Gate

1. Define a measurable metric tied to one or more of the five dials
2. Set a numeric threshold that constitutes pass/fail
3. Specify when the gate must be re-run (cadence or trigger events)
4. Run the probe board and record any contested calls with Atlas's opposing reasoning

---

*This file references the probe board at `tests/probe-board.md` and the machine-readable probes at `tests/probes.jsonl`.*
