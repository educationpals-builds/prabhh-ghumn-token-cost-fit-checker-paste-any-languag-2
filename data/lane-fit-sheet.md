# Lane Fit Sheet — Calibration Record

This data sheet documents the seeded samples, per-language lane counts, advisor dial strips, drift ruling, and stance adjustments that calibrate the token cost + fit checker.

---

## Seeded Samples

### Sample 1: German Support Ticket
```
Ihr Krankenversicherungsbeitrag wurde angepasst — bitte prüfen Sie die Beitragsbemessungsgrenze.
```

### Sample 2: Turkish Support Ticket
```
Sigortalılığınızın başlangıç tarihini öğrenebilir miyim?
```

**Source:** "Ihr Krankenversicherungsbeitrag wurde angepasst — bitte prüfen Sie die Beitragsbemessungsgrenze." / "Sigortalılığınızın başlangıç tarihini öğrenebilir miyim?" (two verbatim tickets from the queue)

---

## Per-Language Lane Counts

| Lane | Traffic Share | Notes |
|------|---------------|-------|
| German | 38% | Primary lane |
| Turkish | 22% | Secondary lane |
| English | 19% | Claimed but not represented in sample |
| Thai | Remainder | Minority lane |
| Arabic | Remainder | Minority lane |
| Mandarin | Remainder | Minority lane |

**Traffic source:** 14-day support queue export: 38% German, 22% Turkish, 19% English, remainder Thai / Arabic / Mandarin

**Builder's split observation:** I dont see any english reference even though it claims 19% english

---

## Advisor Dial Strips

The advisor evaluates incoming text against five dials (0–4 scale):

| Dial | Advisor Score | Builder Score |
|------|---------------|---------------|
| special_token_handling | — | 2 |
| vocabulary_fit | — | 2 |
| merge_economy | — | 2 |
| how_it_splits | — | 2 |
| edge_case_survival | — | 2 |

**Weakest dial identified:** vocabulary_fit

---

## Builder's Drift Ruling

I dont have this information on me right now so I cant provide.

---

## Stance Line Added

Based on the drift ruling, the following stance governs the advisor:

It can listen to events from CRM for new enteries and it reads the text files for language to translate, and it uploads the translated data to text files on CRM. But it refuses emojis and blacklisted words.

**Explicit refusal:** Emojis and blacklisted words

---

## Calibration Anchor

This sheet serves as the calibration record for the token cost + fit checker. The German+Turkish sample pair is the reference point for verification — any stranger can paste these samples and confirm per-lane counts are reported.

**Decision deadline:** Thursday's architecture review

**Stakes:** Picks the vocabulary for the on-device assistant — the embedding table is capped and inference is billed per token
