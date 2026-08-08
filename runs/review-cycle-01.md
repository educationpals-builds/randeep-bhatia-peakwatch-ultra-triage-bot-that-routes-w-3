# PeakWatch Ultra Triage Bot — Reference Review Cycle

## Cycle metadata

| Field | Value |
|-------|-------|
| Bot under test | PeakWatch Ultra triage bot that routes warranty and billing tickets to the right queue |
| Source | Zendesk PeakWatch queue export, week of 7 Aug 2026 |
| Standard line | The ticket lands in warranty or billing based on the shopper ask, not the product name in the subject line |
| Run owner | ops lead |
| Run date | Reference cycle (initial board) |

---

## Messages pulled

The following five messages were pulled from the source and submitted to all eight trick tasks:

1. PeakWatch Ultra stopped logging after the firmware update — warranty or new purchase?
2. You charged me twice for PeakWatch Ultra shipping; I need the second charge reversed today
3. My PeakWatch Ultra band cracked on week two — repair, replace, or refund under the new policy?
4. Cancel the PeakWatch Ultra renewal before Friday; keep the open warranty ticket #8821
5. Billing asked for proof of purchase but the warranty thread already has the receipt photo from Monday

---

## Eight verdicts

| Task | Verdict | Finding | Defense that flips it |
|------|---------|---------|----------------------|
| p1_stale_world | FAIL | quotes pre-June 30-day warranty on firmware ticket | current-policy fetch before answer |
| p2_edge_account | FAIL | maps wholesale-style renewal cancel to retail FAQ | unknown-plan handoff |
| p3_assumption_violator | FAIL | reasserts prior bot refund text for counsel ask | escalate-on-counsel rule |
| p4_plausible_wrong | FAIL | confirms automatic double-charge refund | separate fact-check from policy call |
| p5_volume_burst | FAIL | answers using superseded order # before correction | latest-fact-wins window |
| p6_silent_drift | FAIL | January "return credit" vs July "refund to tender" diverge | blocking invariant until aligned |
| p7_own_failure | FAIL | firmware+warranty ask routes on PeakWatch product name to shipping | ask-type before product match |
| p8_accident | FAIL | double-charge ticket opens shipping ETA | billing intent watch before product nouns |

---

## Board reading

Board lean is product-name hijack: warranty and billing both lose to PeakWatch Ultra in the subject. Quote cell p7: firmware ask opened shipping. Marisol should fund ask-type routing before the rebuild.

---

## Caught-or-missed line

**Failures caught:** 8 of 8 tasks failed on default settings.

The board caught the product-name hijack pattern across multiple tasks. Cells p6 (silent-drift divergence) and p7 (own-failure on ask-type routing) were cited as the basis for overruling Atlas's ship recommendation.

**Block number:** 2 or more failures on default settings block ship.

**Hard block triggered:** Yes — silent-drift divergence (p6) is a hard block independent of count.

---

## Ship gate applied

The proposal reaches Marisol only with the full board attached: two or more failures on default settings block ship, each failure is owned, silent-drift divergence is a hard block, and the board re-runs the week before review. Atlas argued ship anyway; I overruled from cells p6 and p7.

---

## Time cost

| Phase | Duration |
|-------|----------|
| Message pull from Zendesk export | ~5 min |
| Run eight tasks against five messages | ~20 min |
| Record verdicts and identify flips | ~10 min |
| Board reading and ship-gate ruling | ~10 min |
| **Total cycle time** | **~45 min** |

---

## Re-run triggers

Board re-runs on any bot change plus monthly through the quarter so evidence is fresh the week Marisol reads it; ops lead owns the re-run.

---

## Next scheduled run

Monthly through the quarter, or immediately on any bot change — whichever comes first.
