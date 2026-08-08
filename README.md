# Trick-task board

A stress-test kit that runs eight trick tasks against any bot you're about to trust—then returns a go-live rule with a block number and a re-run trigger.

---

## Worked example

**Bot under inspection:** PeakWatch Ultra triage bot that routes warranty and billing tickets to the right queue

**Standard line:** The ticket lands in warranty or billing based on the shopper ask, not the product name in the subject line

**Stakes:** Wrong queue delays a failing PeakWatch Ultra by a day and the customer escalates to Marisol

**Source:** Zendesk PeakWatch queue export, week of 7 Aug 2026

---

## Board verdict

All eight probes failed on default settings:

| Probe | Verdict | Defense that flips it |
|-------|---------|----------------------|
| p1_stale_world | FAIL — quotes pre-June 30-day warranty on firmware ticket | current-policy fetch before answer |
| p2_edge_account | FAIL — maps wholesale-style renewal cancel to retail FAQ | unknown-plan handoff |
| p3_assumption_violator | FAIL — reasserts prior bot refund text for counsel ask | escalate-on-counsel rule |
| p4_plausible_wrong | FAIL — confirms automatic double-charge refund | separate fact-check from policy call |
| p5_volume_burst | FAIL — answers using superseded order # before correction | latest-fact-wins window |
| p6_silent_drift | FAIL — January "return credit" vs July "refund to tender" diverge | blocking invariant until aligned |
| p7_own_failure | FAIL — firmware+warranty ask routes on PeakWatch product name to shipping | ask-type before product match |
| p8_accident | FAIL — double-charge ticket opens shipping ETA | billing intent watch before product nouns |

**Board reading:** Board lean is product-name hijack: warranty and billing both lose to PeakWatch Ultra in the subject. Quote cell p7: firmware ask opened shipping. Marisol should fund ask-type routing before the rebuild.

---

## Go-live rule

The proposal reaches Marisol only with the full board attached: two or more failures on default settings block ship, each failure is owned, silent-drift divergence is a hard block, and the board re-runs the week before review. Atlas argued ship anyway; I overruled from cells p6 and p7.

**Re-run cadence:** Board re-runs on any bot change plus monthly through the quarter so evidence is fresh the week Marisol reads it; ops lead owns the re-run.

---

## One-paste rebuild

Paste your own bot's details to run the same eight tasks against your setup:

```
Bot under inspection:
[Describe the bot and what it routes or decides]

Standard line:
[What "doing its job" means — the bar a task can fail]

Stakes:
[What breaks if the bot quietly gets things wrong]

Sample messages (at least 5):
[Paste real messages the bot will face — include at least one with quoted chains, scanning noise, or boilerplate]

Source:
[Where those messages came from and when]
```

The kit runs all eight probes against your messages, reports pass or fail with the defense that flips each failure, and returns a go-live rule with a block number and a re-run trigger.

---

## What a stranger gets

1. Eight verdicts—one per probe—against your pasted messages
2. For every failure: the defense setting that would flip it
3. A board reading organized by crack and direction of failure
4. A go-live rule with a block number and the trigger that re-opens it
5. The failure that remains after defenses are applied

---

## Files in this repo

- **charter.md** — Full run: eight verdicts, defenses, board reading, go-live rule, and the failure that remains
- **METHOD.md** — The five principles behind the eight probes
- **VERIFY.md** — Stranger verification steps
- **STORY.md** — Builder's first-person account of what the board caught

<!-- educationpals-build-verified -->
