# Token Cost + Fit Checker

A conversational checker that evaluates token cost and vocabulary fit for multilingual text streams. Built for teams making embedding and inference decisions under vocabulary constraints.

---

## How This Checker Was Built

This checker was calibrated against a real support queue with mixed-language traffic. The builder pinned a concrete sample, scored it across five dials, recorded a verdict, and defined the conditions that would flip that verdict. The result is a checker that carries the builder's counting discipline—not a generic rubric.

The five dials:
1. **special_token_handling** — how the tokenizer treats special tokens
2. **vocabulary_fit** — coverage of the target vocabulary
3. **merge_economy** — efficiency of subword merges
4. **how_it_splits** — observable tokenization behavior
5. **edge_case_survival** — handling of unusual inputs

---

## Worked Example: The Builder's Own Sample

**Pinned sample (verbatim):**

> "Ihr Krankenversicherungsbeitrag wurde angepasst — bitte prüfen Sie die Beitragsbemessungsgrenze." / "Sigortalılığınızın başlangıç tarihini öğrenebilir miyim?" (two verbatim tickets from the queue)

**Traffic source:** 14-day support queue export: 38% German, 22% Turkish, 19% English, remainder Thai / Arabic / Mandarin

**What this decides:** Picks the vocabulary for the on-device assistant — the embedding table is capped and inference is billed per token

**Decision deadline:** Thursday's architecture review

**Weakest dial:** vocabulary_fit

**Verdict:** The vocabulary accuracy metrics should be more than 90% accurate for it to be acceptable.

**Flip condition:** If the vocabulary accuracy metrics is lower than 60%, it would be unacceptable.

**Sharpest test:** Run 100 samples and it meets the bare min 90% accuracy.

See [charter.md](charter.md) for the full run with all dial scores.

---

## One-Paste Rebuild Block

To rebuild this checker from scratch:

1. Copy the system instructions from [blueprints/token-fit-checker.md](blueprints/token-fit-checker.md)
2. Paste into any chat model that supports system prompts
3. Load the calibration from [data/lane-fit-sheet.md](data/lane-fit-sheet.md)
4. Verify with the protocol in [VERIFY.md](VERIFY.md)

For standalone dial prompts (no system prompt required), see [prompts/token-fit-pack.md](prompts/token-fit-pack.md).

---

## Repository Structure

| Path | Purpose |
|------|---------|
| `charter.md` | The builder's full run: sample, dials, verdict, flip condition |
| `blueprints/token-fit-checker.md` | One-paste system instructions for the checker |
| `prompts/token-fit-pack.md` | 5 standalone prompts, one per dial |
| `skills/token-fit-advisor.skill.md` | Portable skill file for assistant runtimes |
| `data/lane-fit-sheet.md` | Calibration record with seeded samples and drift rulings |
| `tests/probe-board.md` | All 8 probes with results grid |
| `tests/probes.jsonl` | Machine-readable probe export |
| `tests/pass-gate.md` | Gate metric, threshold, and re-run cadence |
| `tests/run-local.md` | Run-anywhere guide (manual, script, CI) |
| `METHOD.md` | The framework behind the five dials |
| `VERIFY.md` | Stranger verification protocol |
| `STORY.md` | The builder's first-person account |

---

## Quick Start

1. Read the worked example above
2. Open [VERIFY.md](VERIFY.md) and run the verification protocol
3. Paste your own sample into the checker
4. Compare your dial scores against the calibration in [data/lane-fit-sheet.md](data/lane-fit-sheet.md)

---

## License

This checker was built through the EducationPals Build-Along Workshop. See provenance.json for generation details.

<!-- educationpals-build-verified -->
