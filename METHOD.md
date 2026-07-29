# METHOD.md

## The TFLEC Framework

This checker is built on the **TFLEC** framework — a five-dial evaluation system for assessing token cost and vocabulary fit across multilingual text streams.

### The Five Dials

| Letter | Dial | What It Measures |
|--------|------|------------------|
| **T** | special_**T**oken_handling | How the tokenizer handles special tokens, control characters, and boundary markers |
| **F** | vocabulary_**F**it | How well the text's vocabulary maps to the tokenizer's trained vocabulary |
| **L** | merge_economy (token **L**ength) | How efficiently the tokenizer merges subwords — fewer tokens for the same content means better economy |
| **E** | how_it_splits (**E**xpansion) | The expansion ratio when text is tokenized — compound words, rare terms, and cross-script content |
| **C** | edge_**C**ase_survival | How the tokenizer handles edge cases: emoji, code-switching, mixed scripts, malformed input |

### Scoring

Each dial is rated 0–4:

- **0** — Fails outright; unusable for production
- **1** — Severe issues; requires significant work
- **2** — Marginal; works but with notable cost or accuracy penalties
- **3** — Acceptable; minor issues that can be monitored
- **4** — Strong fit; no concerns for this dial

### Using the Framework

1. **Pin a sample** — real bytes from your actual traffic
2. **Score all five dials** — rate each 0–4 with evidence
3. **Identify the weakest dial** — this is your decision point
4. **Call the verdict** — position + deciding dial + cost of being wrong
5. **Name the flip condition** — what measurement would reverse your call

The weakest dial determines the verdict. A text stream with four 4s and one 1 is a 1-quality fit until that dial improves.

---

*This is the only file in this repository where the TFLEC acronym appears.*
