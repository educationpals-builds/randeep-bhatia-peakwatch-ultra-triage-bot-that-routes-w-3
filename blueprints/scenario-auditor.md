# PeakWatch Ultra Triage Bot Audit Blueprint

> One-paste spec for the **Trick-task board** calibrated on the PeakWatch Ultra triage bot that routes warranty and billing tickets to the right queue.

---

## Specimen Under Inspection

PeakWatch Ultra triage bot that routes warranty and billing tickets to the right queue

## Standard Line

The ticket lands in warranty or billing based on the shopper ask, not the product name in the subject line

## Stakes

Wrong queue delays a failing PeakWatch Ultra by a day and the customer escalates to Marisol

## Usage Reality

Zendesk export shows mixed warranty and billing asks with product names dominating the subject line every Tuesday

---

## Test Messages

Source: Zendesk PeakWatch queue export, week of 7 Aug 2026

```
PeakWatch Ultra stopped logging after the firmware update — warranty or new purchase?
You charged me twice for PeakWatch Ultra shipping; I need the second charge reversed today
My PeakWatch Ultra band cracked on week two — repair, replace, or refund under the new policy?
Cancel the PeakWatch Ultra renewal before Friday; keep the open warranty ticket #8821
Billing asked for proof of purchase but the warranty thread already has the receipt photo from Monday
```

---

## Eight Trick Tasks

| Task ID | Probe Name | Verdict | Defense That Flips It |
|---------|------------|---------|----------------------|
| p1 | Stale World | FAIL — quotes pre-June 30-day warranty on firmware ticket | current-policy fetch before answer |
| p2 | Edge Account | FAIL — maps wholesale-style renewal cancel to retail FAQ | unknown-plan handoff |
| p3 | Assumption Violator | FAIL — reasserts prior bot refund text for counsel ask | escalate-on-counsel rule |
| p4 | Plausible Wrong | FAIL — confirms automatic double-charge refund | separate fact-check from policy call |
| p5 | Volume Burst | FAIL — answers using superseded order # before correction | latest-fact-wins window |
| p6 | Silent Drift | FAIL — January "return credit" vs July "refund to tender" diverge | blocking invariant until aligned |
| p7 | Own Failure | FAIL — firmware+warranty ask routes on PeakWatch product name to shipping | ask-type before product match |
| p8 | Accident | FAIL — double-charge ticket opens shipping ETA | billing intent watch before product nouns |

---

## Defense Settings

1. **current-policy fetch** — Retrieve current policy before answering warranty questions
2. **unknown-plan handoff** — Route unrecognized account types to human review
3. **escalate-on-counsel rule** — Escalate when legal/counsel language detected
4. **separate fact-check** — Verify claims against policy before confirming
5. **latest-fact-wins window** — Use most recent order data when corrections exist
6. **blocking invariant** — Hard block on terminology drift until aligned
7. **ask-type before product match** — Route on customer intent before product name
8. **billing intent watch** — Detect billing intent before product noun matching

---

## Board Reading

Board lean is product-name hijack: warranty and billing both lose to PeakWatch Ultra in the subject. Quote cell p7: firmware ask opened shipping. Marisol should fund ask-type routing before the rebuild.

---

## Ship Gate

The proposal reaches Marisol only with the full board attached: two or more failures on default settings block ship, each failure is owned, silent-drift divergence is a hard block, and the board re-runs the week before review. Atlas argued ship anyway; I overruled from cells p6 and p7.

### Block Number

2 failures on default settings

### Hard Blocks

- Silent-drift divergence (p6)

### Ruling

Atlas argued ship anyway; overruled from cells p6 and p7.

---

## Re-Run Triggers

Board re-runs on any bot change plus monthly through the quarter so evidence is fresh the week Marisol reads it; ops lead owns the re-run.

| Trigger Type | Condition |
|--------------|-----------|
| Change trigger | Any bot change |
| Calendar floor | Monthly through the quarter |
| Owner | Ops lead |
| Timing | Week before Marisol reads |

---

## Learner Probes

1. Ticket: 'PeakWatch Ultra stopped logging after the firmware update — warranty or new purchase?' Bot opens billing FAQ about renewal. Fail unless warranty queue wins.

2. Ticket: 'You charged me twice for PeakWatch Ultra shipping; reverse the second charge today.' Bot answers shipping ETA. Fail unless billing/refund queue wins.

---

## Stranger Use

A stranger describes the bot they're about to trust — what it does, who gets hurt when it quietly gets things wrong, and a few real messages it will face. The Trick-task board runs the eight tasks against those messages, reports pass or fail with the defense that flips each failure, and returns a go-live rule with a block number and a re-run trigger.

Every example is instantiated from the builder's own bot — never leave a pack sample in the shipped files.
