# Run-Local Guide

Three rungs for running the token-fit checker probes anywhere.

---

## Rung 1: Manual — Paste Protocol

For each probe in `tests/probes.jsonl`, follow this protocol:

1. **Copy the input bytes exactly** — do not re-type, do not let your editor "fix" quotes or dashes.
2. **Paste into the checker** (or any chat model with the system prompt from `blueprints/token-fit-checker.md`).
3. **Compare the output** against the expected line for that probe.

### Byte-Preservation Warning

Multilingual samples contain characters that look similar but encode differently:
- German ü vs Turkish ü (different Unicode points in some fonts)
- Em-dashes (—) vs hyphens (-)
- Curly quotes vs straight quotes

**Always copy-paste from `probes.jsonl`. Never re-type.** A single byte difference can change token counts and invalidate the probe.

### Manual Checklist

| Probe ID | Input (paste from probes.jsonl) | Target Dial | Expected Behavior | Your Result |
|----------|--------------------------------|-------------|-------------------|-------------|
| (see probes.jsonl for each row) | | | | ☐ Pass / ☐ Fail |

---

## Rung 2: Script — Embedded Runner

A ~20-line runner that reads `tests/probes.jsonl` and prints the graded grid.

```python
#!/usr/bin/env python3
"""Token-fit probe runner. Reads probes.jsonl, calls the model, prints graded grid."""
import os, json, sys

API_KEY = os.environ.get("OPENAI_API_KEY") or os.environ.get("API_KEY")
if not API_KEY:
    sys.exit("Set API_KEY or OPENAI_API_KEY in environment")

import openai
client = openai.OpenAI(api_key=API_KEY)

SYSTEM_PROMPT = open("blueprints/token-fit-checker.md").read()

def run_probe(probe):
    resp = client.chat.completions.create(
        model="gpt-4o",
        messages=[{"role":"system","content":SYSTEM_PROMPT},{"role":"user","content":probe["input"]}],
        temperature=0
    )
    return resp.choices[0].message.content

probes = [json.loads(line) for line in open("tests/probes.jsonl")]
results = []
for p in probes:
    out = run_probe(p)
    passed = p["expected"].lower() in out.lower()
    results.append({"id":p["id"],"name":p["name"],"target":p["targets"],"passed":passed})
    print(f"{'✓' if passed else '✗'} {p['id']}: {p['name']} → {p['targets']}")

passed_count = sum(1 for r in results if r["passed"])
total = len(results)
print(f"\n--- GATE VERDICT ---")
print(f"Passed: {passed_count}/{total}")
```

### Usage

```bash
export API_KEY="your-key-here"
python3 tests/run-local.py
```

The script prints:
- One line per probe with ✓ or ✗
- The graded grid summary
- The gate verdict (pass count / total)

---

## Rung 3: Eval Tool / CI Integration

Load `tests/probes.jsonl` into any eval runner so the board re-runs automatically on prompt or stance changes.

### Format

Each line in `probes.jsonl` is:
```json
{"id":"...", "name":"...", "input":"...", "targets":"...", "expected":"...", "invariant":"..."}
```

### Integration Options

**Generic eval runner:**
```bash
eval-runner --probes tests/probes.jsonl --system blueprints/token-fit-checker.md
```

**CI pipeline (GitHub Actions example):**
```yaml
- name: Run token-fit probes
  env:
    API_KEY: ${{ secrets.API_KEY }}
  run: python3 tests/run-local.py
```

**On prompt changes:** Re-run the full board whenever you edit:
- `blueprints/token-fit-checker.md`
- `skills/token-fit-advisor.skill.md`
- Any stance or refusal rules

---

## Diffing Against the EP-Certified Board

To compare your local run against the EducationPals-certified board on the listing:

1. **Run locally** and save output:
   ```bash
   python3 tests/run-local.py > local-board.txt
   ```

2. **Fetch the certified board** from the EP listing (the `probe-board.md` snapshot at certification time).

3. **Diff the results:**
   ```bash
   diff local-board.txt certified-board.txt
   ```

Any drift indicates:
- Your prompt changed behavior
- The model version shifted
- A stance rule was added or removed

Re-certify if drift exceeds the gate threshold defined in `tests/pass-gate.md`.
