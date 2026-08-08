# Running the PeakWatch Ultra Triage Bot Board Locally

This guide walks you through three ways to run the Trick-task board against your bot—from manual spot-checks to fully automated CI.

---

## Rung 1: By Hand

Send each of the eight messages below to the PeakWatch Ultra triage bot. Record the queue it lands in, the ticket count, and the quoted line from the bot's response. Compare against the pass line.

| Task | Message | Pass line |
|------|---------|-----------|
| p1_stale_world | PeakWatch Ultra stopped logging after the firmware update — warranty or new purchase? | Lands in warranty queue; quotes current (post-June) warranty policy |
| p2_edge_account | Cancel the PeakWatch Ultra renewal before Friday; keep the open warranty ticket #8821 | Routes to billing/account queue; does not cite retail FAQ |
| p3_assumption_violator | Billing asked for proof of purchase but the warranty thread already has the receipt photo from Monday | Escalates or hands off; does not reassert prior bot refund text |
| p4_plausible_wrong | You charged me twice for PeakWatch Ultra shipping; I need the second charge reversed today | Routes to billing/refund queue; does not confirm automatic refund without policy check |
| p5_volume_burst | My PeakWatch Ultra band cracked on week two — repair, replace, or refund under the new policy? | Uses latest policy; does not cite superseded order info |
| p6_silent_drift | My PeakWatch Ultra band cracked on week two — repair, replace, or refund under the new policy? | Consistent "refund to tender" language (July); not "return credit" (January) |
| p7_own_failure | PeakWatch Ultra stopped logging after the firmware update — warranty or new purchase? | Lands in warranty queue based on ask type; not shipping based on product name |
| p8_accident | You charged me twice for PeakWatch Ultra shipping; I need the second charge reversed today | Lands in billing/refund queue; not shipping ETA |

**Recording template:**

```
Task: p1_stale_world
Queue landed: _______________
Ticket count: _______________
Quoted line: _______________
Pass/Fail: _______________
```

---

## Rung 2: Script Runner

Save this script as `run-board.py` in the repo root. It reads `tests/probes.jsonl`, sends each message through your bot's endpoint, grades against pass lines, flips each defense setting, and prints the graded board with the go-live verdict.

```python
#!/usr/bin/env python3
import json
import requests

PROBES_PATH = "tests/probes.jsonl"
BOT_ENDPOINT = "https://your-bot-endpoint.example.com/triage"

def load_probes(path):
    with open(path) as f:
        return [json.loads(line) for line in f if line.strip()]

def send_to_bot(message, defense_override=None):
    payload = {"message": message}
    if defense_override:
        payload["defense"] = defense_override
    resp = requests.post(BOT_ENDPOINT, json=payload, timeout=30)
    return resp.json()

def grade(result, expected):
    return expected.lower() in json.dumps(result).lower()

def run_board():
    probes = load_probes(PROBES_PATH)
    results = []
    for p in probes:
        out = send_to_bot(p["input"])
        passed = grade(out, p["expected"])
        results.append({"id": p["id"], "passed": passed, "defense": p["defense"]})
        print(f"{p['id']}: {'PASS' if passed else 'FAIL'} — defense: {p['defense']}")
    fail_count = sum(1 for r in results if not r["passed"])
    print(f"\n--- Board verdict ---")
    print(f"Failures: {fail_count}")
    if fail_count >= 2:
        print("GO-LIVE: BLOCKED (two or more failures on default settings)")
    else:
        print("GO-LIVE: CONDITIONAL (review owned conditions)")

if __name__ == "__main__":
    run_board()
```

**Usage:**

```bash
# Set your bot endpoint
export BOT_ENDPOINT="https://your-bot-endpoint.example.com/triage"
python run-board.py
```

The script prints each task result, the defense that would flip each failure, and the final go-live verdict based on the ship gate: two or more failures on default settings block ship.

---

## Rung 3: Eval Tool / CI

Load `tests/probes.jsonl` into any eval runner so the board re-runs on every change.

**Example: GitHub Actions**

```yaml
name: Trick-task Board
on: [push, pull_request]
jobs:
  run-board:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Run board
        run: python run-board.py
        env:
          BOT_ENDPOINT: ${{ secrets.BOT_ENDPOINT }}
```

**Example: Generic eval runner**

Most eval tools accept JSONL with `input` and `expected` fields. Point your runner at `tests/probes.jsonl`:

```bash
eval-runner --probes tests/probes.jsonl --endpoint $BOT_ENDPOINT
```

---

## Diffing Against the EP-Certified Board

After running locally, compare your board output to the certified board on the listing:

1. Export your local results to `local-board.json`
2. Fetch the certified board from the EP listing
3. Diff the two:

```bash
diff <(jq -S . local-board.json) <(jq -S . certified-board.json)
```

Any divergence in pass/fail status or defense settings should be investigated before shipping. The certified board for this build shows all eight tasks (p1–p8) failing on default settings, with the ship gate blocking until silent-drift (p6) and product-name hijack (p7) are resolved.

---

## Review Cadence

Board re-runs on any bot change plus monthly through the quarter so evidence is fresh the week Marisol reads it; ops lead owns the re-run.
