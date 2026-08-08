# PeakWatch Ultra triage bot — Trick-task board

**Specimen under test:** PeakWatch Ultra triage bot that routes warranty and billing tickets to the right queue

**Standard line:** The ticket lands in warranty or billing based on the shopper ask, not the product name in the subject line

**Source:** Zendesk PeakWatch queue export, week of 7 Aug 2026

---

## The eight tasks

Each task runs one of the learner's own messages through the bot, records what the bot did, and names the defense setting that flips the failure.

---

### p1 · Stale world

**Message:**
> PeakWatch Ultra stopped logging after the firmware update — warranty or new purchase?

**What the bot did:** Quotes pre-June 30-day warranty on firmware ticket

**Verdict:** FAIL

**Defense that flips it:** current-policy fetch before answer

---

### p2 · Edge account

**Message:**
> Cancel the PeakWatch Ultra renewal before Friday; keep the open warranty ticket #8821

**What the bot did:** Maps wholesale-style renewal cancel to retail FAQ

**Verdict:** FAIL

**Defense that flips it:** unknown-plan handoff

---

### p3 · Assumption violator

**Message:**
> Billing asked for proof of purchase but the warranty thread already has the receipt photo from Monday

**What the bot did:** Reasserts prior bot refund text for counsel ask

**Verdict:** FAIL

**Defense that flips it:** escalate-on-counsel rule

---

### p4 · Plausible wrong

**Message:**
> You charged me twice for PeakWatch Ultra shipping; I need the second charge reversed today

**What the bot did:** Confirms automatic double-charge refund

**Verdict:** FAIL

**Defense that flips it:** separate fact-check from policy call

---

### p5 · Volume burst

**Message:**
> My PeakWatch Ultra band cracked on week two — repair, replace, or refund under the new policy?

**What the bot did:** Answers using superseded order # before correction

**Verdict:** FAIL

**Defense that flips it:** latest-fact-wins window

---

### p6 · Silent drift

**Message:**
> My PeakWatch Ultra band cracked on week two — repair, replace, or refund under the new policy?

**What the bot did:** January "return credit" vs July "refund to tender" diverge

**Verdict:** FAIL — blocking invariant until aligned

**Defense that flips it:** blocking invariant until aligned

---

### p7 · Own failure

**Message:**
> PeakWatch Ultra stopped logging after the firmware update — warranty or new purchase?

**What the bot did:** Firmware+warranty ask routes on PeakWatch product name to shipping

**Verdict:** FAIL

**Defense that flips it:** ask-type before product match

---

### p8 · Accident

**Message:**
> You charged me twice for PeakWatch Ultra shipping; I need the second charge reversed today

**What the bot did:** Double-charge ticket opens shipping ETA

**Verdict:** FAIL

**Defense that flips it:** billing intent watch before product nouns

---

## Learner probes (p7 and p8 targets)

**Probe 1:**
> Ticket: 'PeakWatch Ultra stopped logging after the firmware update — warranty or new purchase?' Bot opens billing FAQ about renewal. Fail unless warranty queue wins.

**Probe 2:**
> Ticket: 'You charged me twice for PeakWatch Ultra shipping; reverse the second charge today.' Bot answers shipping ETA. Fail unless billing/refund queue wins.

---

## Board reading

Board lean is product-name hijack: warranty and billing both lose to PeakWatch Ultra in the subject. Quote cell p7: firmware ask opened shipping. Marisol should fund ask-type routing before the rebuild.

---

## Task-by-task summary

| Task | ID | Verdict | Defense |
|------|----|---------|---------|
| Stale world | p1 | FAIL | current-policy fetch before answer |
| Edge account | p2 | FAIL | unknown-plan handoff |
| Assumption violator | p3 | FAIL | escalate-on-counsel rule |
| Plausible wrong | p4 | FAIL | separate fact-check from policy call |
| Volume burst | p5 | FAIL | latest-fact-wins window |
| Silent drift | p6 | FAIL | blocking invariant until aligned |
| Own failure | p7 | FAIL | ask-type before product match |
| Accident | p8 | FAIL | billing intent watch before product nouns |

**Total:** 8 tasks, 8 failures, 0 passes

---

## What this board means

Every task failed on default settings. The dominant failure mode is product-name hijack — the bot routes on "PeakWatch Ultra" in the subject line instead of the actual shopper ask (warranty vs billing). Cells p6 and p7 are the blocking failures that drove the ship-hold ruling.
