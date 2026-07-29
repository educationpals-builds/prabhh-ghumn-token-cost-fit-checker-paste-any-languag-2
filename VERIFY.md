# Verification Protocol

This document explains how a stranger can verify that the Token Cost + Fit Checker works as calibrated.

## The Seeded Sample

Paste this exact text into the checker (via `/verify` or the main input):

```
"Ihr Krankenversicherungsbeitrag wurde angepasst — bitte prüfen Sie die Beitragsbemessungsgrenze." / "Sigortalılığınızın başlangıç tarihini öğrenebilir miyim?"
```

This is the builder's pinned sample: two verbatim tickets from the support queue — one German, one Turkish.

## What to Confirm

### 1. Per-Lane Counts Reported

The checker must report token counts **per language lane**, not just a single total. You should see separate counts for:

- German lane
- Turkish lane

If the checker returns only a combined total without breaking down by language, verification fails.

### 2. Uncounted Lane Named

The checker must explicitly name any lane it **cannot** count or where counting is unreliable. 

Per the builder's traffic source, the queue contains: 38% German, 22% Turkish, 19% English, remainder Thai / Arabic / Mandarin.

The seeded sample contains only German and Turkish. The checker should note that English (and other languages in the traffic mix) are not represented in this sample.

### 3. Five Dials Scored

The checker should score all five dials:

| Dial | Description |
|------|-------------|
| special_token_handling | How special tokens are processed |
| vocabulary_fit | Coverage of the vocabulary for this text |
| merge_economy | Efficiency of subword merges |
| how_it_splits | Tokenization behavior on compound words |
| edge_case_survival | Handling of edge cases |

The weakest dial for this checker is **vocabulary_fit**.

## Pass Criteria

Verification passes when:

1. ✅ Per-lane counts appear (German count, Turkish count — separate)
2. ✅ The uncounted lane is named (English and other languages not in sample)
3. ✅ All five dials receive a score

## If Verification Fails

If the checker does not report per-lane counts or fails to name the uncounted lane:

1. Check that the sample was pasted exactly as shown (byte-for-byte, including the em-dash and Turkish special characters)
2. Re-run with the sample isolated (no additional text)
3. If it still fails, the checker needs recalibration against the charter

---

*This verification protocol matches the builder's calibration from Thursday's architecture review deadline.*
