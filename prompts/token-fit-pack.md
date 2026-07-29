# Token Fit Prompt Pack

Five standalone prompts for evaluating token cost and vocabulary fit. Each prompt targets one dial and can be used independently in any chat model.

---

## 1. Special Token Handling

```
You are a token-cost analyst. Given the following text sample, evaluate how well a tokenizer handles special tokens.

Check for:
- Proper handling of language-specific punctuation
- Treatment of quotation marks, dashes, and currency symbols
- Whether special characters inflate token count unnecessarily

Text to analyze:
[PASTE YOUR SAMPLE HERE]

Report:
1. List each special token/character found
2. Note whether it tokenizes efficiently or creates overhead
3. Rate special token handling: 0 (severe issues) to 4 (excellent)
```

---

## 2. Vocabulary Fit

```
You are a token-cost analyst. Given the following text sample, evaluate vocabulary fit for the target tokenizer.

Check for:
- Whether common words in the text exist in the vocabulary
- Frequency of out-of-vocabulary fallbacks
- Language coverage gaps

Text to analyze:
[PASTE YOUR SAMPLE HERE]

Report:
1. Identify words likely missing from a standard vocabulary
2. Note which language lanes have coverage gaps
3. Rate vocabulary fit: 0 (poor coverage) to 4 (excellent coverage)
```

---

## 3. Merge Economy

```
You are a token-cost analyst. Given the following text sample, evaluate merge economy — how efficiently the tokenizer combines subwords.

Check for:
- Whether common word pairs merge efficiently
- Compound word handling (especially in German)
- Subword fragmentation patterns

Text to analyze:
[PASTE YOUR SAMPLE HERE]

Report:
1. List compound words and their likely token splits
2. Identify inefficient merges that inflate cost
3. Rate merge economy: 0 (wasteful) to 4 (efficient)
```

---

## 4. How It Splits

```
You are a token-cost analyst. Given the following text sample, analyze the splitting behavior.

Check for:
- Character-to-token ratio per language lane
- Whether splits preserve semantic units
- Unexpected fragmentation patterns

Text to analyze:
[PASTE YOUR SAMPLE HERE]

Report:
1. Estimate token count per language segment
2. Note any splits that break meaning
3. Rate splitting behavior: 0 (destructive) to 4 (clean)
```

---

## 5. Edge Case Survival

```
You are a token-cost analyst. Given the following text sample, evaluate edge case survival.

Check for:
- Mixed-script handling (Latin + non-Latin in same message)
- Numeric and date format handling
- Code-switching within sentences
- Unusual punctuation or formatting

Text to analyze:
[PASTE YOUR SAMPLE HERE]

Report:
1. List edge cases present in the sample
2. Note which edge cases would cause tokenizer stress
3. Rate edge case survival: 0 (fails on edges) to 4 (robust)
```

---

## Usage Notes

- Paste your sample text where indicated
- Each prompt returns a 0–4 rating for its dial
- For multi-language samples, request per-lane breakdowns
- The weakest dial across all five determines overall fit

## Calibration Reference

Builder's pinned sample for calibration:

> "Ihr Krankenversicherungsbeitrag wurde angepasst — bitte prüfen Sie die Beitragsbemessungsgrenze." / "Sigortalılığınızın başlangıç tarihini öğrenebilir miyim?" (two verbatim tickets from the queue)

Traffic mix: 14-day support queue export: 38% German, 22% Turkish, 19% English, remainder Thai / Arabic / Mandarin

Builder's dial ratings: special_token_handling: 2, vocabulary_fit: 2, merge_economy: 2, how_it_splits: 2, edge_case_survival: 2

Weakest filter identified: vocabulary_fit
