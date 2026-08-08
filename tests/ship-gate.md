# Go-Live Rule for PeakWatch Ultra Triage Bot

## Block Number

**Two or more failures on default settings block ship.**

---

## Owned Conditions

| Condition | Owner | Status |
|-----------|-------|--------|
| Each failure is owned | Assigned per task | Required |
| Silent-drift divergence (p6) | Hard block | Must be resolved before ship |
| Full board attached to proposal | Marisol | Required for review |

---

## Re-Run Trigger

Board re-runs on any bot change plus monthly through the quarter so evidence is fresh the week Marisol reads it; ops lead owns the re-run.

---

## Failure That Remains

### p6_silent_drift
**Status:** FAIL — January "return credit" vs July "refund to tender" diverge; blocking invariant until aligned.

**Owner:** Ops lead  
**Resolution date:** Before next monthly board run

### p7_own_failure
**Status:** FAIL — firmware+warranty ask routes on PeakWatch product name to shipping; flip with ask-type before product match.

**Owner:** Ops lead  
**Resolution date:** Before next monthly board run

---

## Opposing Case and Recorded Ruling

### Atlas Position (Opposing Case)
Atlas argued ship anyway.

### Recorded Ruling
I overruled from cells p6 and p7.

**Rationale:** The proposal reaches Marisol only with the full board attached: two or more failures on default settings block ship, each failure is owned, silent-drift divergence is a hard block, and the board re-runs the week before review. Atlas argued ship anyway; I overruled from cells p6 and p7.

---

## Board Reading Summary

Board lean is product-name hijack: warranty and billing both lose to PeakWatch Ultra in the subject. Quote cell p7: firmware ask opened shipping. Marisol should fund ask-type routing before the rebuild.

---

## Standard Under Test

The ticket lands in warranty or billing based on the shopper ask, not the product name in the subject line.

---

## Source Evidence

Zendesk PeakWatch queue export, week of 7 Aug 2026
