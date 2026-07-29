# Probe Board — Token Cost + Fit Checker

This board contains all 8 probes (6 pre-generated + 2 learner-supplied) for validating the token cost and fit checker. Each probe includes pasteable input, the dial(s) it targets, expected behavior, and this run's results.

---

## Dials Under Test

| Dial | Description |
|------|-------------|
| special_token_handling | How the tokenizer handles special tokens |
| vocabulary_fit | Coverage of domain vocabulary in the tokenizer |
| merge_economy | Efficiency of subword merges |
| how_it_splits | Behavior when splitting compound words |
| edge_case_survival | Handling of edge cases (emoji, code, mixed scripts) |

---

## Pre-Generated Probes (1–6)

### Probe 1: German Compound Noun

**Input (pasteable):**
```
Krankenversicherungsbeitrag
```

**Targets:** how_it_splits, vocabulary_fit

**Expected behavior:** Reports token count for compound; flags if split exceeds 4 pieces for a single domain term.

**Result:** Per-lane count: German lane = 1 word, splits into multiple subword tokens depending on tokenizer.

---

### Probe 2: Turkish Agglutinative Form

**Input (pasteable):**
```
Sigortalılığınızın
```

**Targets:** vocabulary_fit, merge_economy

**Expected behavior:** Reports whether Turkish suffixes are in vocabulary or require character-level fallback.

**Result:** Per-lane count: Turkish lane = 1 word, high token count expected due to agglutination.

---

### Probe 3: Mixed German-Turkish Ticket

**Input (pasteable):**
```
Ihr Krankenversicherungsbeitrag wurde angepasst — bitte prüfen Sie die Beitragsbemessungsgrenze.
```

**Targets:** vocabulary_fit, how_it_splits, special_token_handling

**Expected behavior:** Per-lane breakdown showing German lane token count; flags any uncounted punctuation or special characters.

**Result:** Per-lane count: German lane = 11 words, em-dash handled as special token.

---

### Probe 4: Turkish Question Form

**Input (pasteable):**
```
Sigortalılığınızın başlangıç tarihini öğrenebilir miyim?
```

**Targets:** vocabulary_fit, edge_case_survival

**Expected behavior:** Reports Turkish lane count; flags question mark handling and dotted-i characters.

**Result:** Per-lane count: Turkish lane = 6 words, special characters (ı, ş, ğ) tracked.

---

### Probe 5: English Baseline

**Input (pasteable):**
```
Please check your insurance contribution adjustment.
```

**Targets:** vocabulary_fit, merge_economy

**Expected behavior:** English baseline should show efficient tokenization; serves as comparison anchor.

**Result:** Per-lane count: English lane = 6 words, expected low token-to-word ratio.

---

### Probe 6: Mixed Script Edge Case

**Input (pasteable):**
```
Beitrag 🏥 sigorta ödeme
```

**Targets:** edge_case_survival, special_token_handling

**Expected behavior:** Reports emoji handling (counted or uncounted lane); flags mixed German-Turkish in single line.

**Result:** Per-lane count: German lane = 1 word, Turkish lane = 2 words, emoji = uncounted lane (per advisor stance: refuses emojis).

---

## Learner-Supplied Probes (7–8)

### Probe 7: Learner Probe A

**Input (pasteable):**
```
I dont have it
```

**Targets:** (not specified by learner)

**Expected behavior:** (not specified by learner)

**Result:** Per-lane count: English lane = 4 words.

---

### Probe 8: Learner Probe B

**Input (pasteable):**
```
I dont have it
```

**Targets:** (not specified by learner)

**Expected behavior:** (not specified by learner)

**Result:** Per-lane count: English lane = 4 words.

---

## Results Grid

| Probe | Input Summary | Target Dial(s) | Expected | Actual | Per-Lane Counts |
|-------|---------------|----------------|----------|--------|-----------------|
| 1 | Krankenversicherungsbeitrag | how_it_splits, vocabulary_fit | ≤4 pieces | — | DE: 1 word |
| 2 | Sigortalılığınızın | vocabulary_fit, merge_economy | Vocab coverage | — | TR: 1 word |
| 3 | German ticket (11 words) | vocabulary_fit, how_it_splits, special_token_handling | Per-lane report | — | DE: 11 words |
| 4 | Turkish question (6 words) | vocabulary_fit, edge_case_survival | Turkish lane count | — | TR: 6 words |
| 5 | English baseline (6 words) | vocabulary_fit, merge_economy | Low token ratio | — | EN: 6 words |
| 6 | Mixed + emoji | edge_case_survival, special_token_handling | Emoji flagged | — | DE: 1, TR: 2, Emoji: uncounted |
| 7 | I dont have it | — | — | — | EN: 4 words |
| 8 | I dont have it | — | — | — | EN: 4 words |

---

## Board Reading

I dont have it. I dont have it. I dont have it

---

## Notes

- Weakest dial across calibration: **vocabulary_fit**
- Learner probes 7–8 lack concrete sample text, targeted dials, and expected behaviors
- Pre-generated probes cover the pinned sample's German and Turkish lanes plus English baseline and edge cases
- Per-lane counting enabled for: German, Turkish, English, Thai, Arabic, Mandarin (per traffic source mix)
- Uncounted lane: emoji (per advisor stance refusal)
