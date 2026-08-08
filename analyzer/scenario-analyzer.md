# PeakWatch Ultra Triage Bot — Scenario Analyzer

Machine-readable analyzer outline for the Trick-task board applied to the PeakWatch Ultra triage bot that routes warranty and billing tickets to the right queue.

---

## Analyzer Metadata

```yaml
analyzer_id: peakwatch-triage-audit-671354
specimen: PeakWatch Ultra triage bot that routes warranty and billing tickets to the right queue
standard_line: The ticket lands in warranty or billing based on the shopper ask, not the product name in the subject line
source: Zendesk PeakWatch queue export, week of 7 Aug 2026
schema_ref: specs/scenario-audit.spec.json
```

---

## Board Verdicts Summary

| Task ID | Verdict | Failure Quote | Defense to Flip |
|---------|---------|---------------|-----------------|
| p1_stale_world | FAIL | quotes pre-June 30-day warranty on firmware ticket | current-policy fetch before answer |
| p2_edge_account | FAIL | maps wholesale-style renewal cancel to retail FAQ | unknown-plan handoff |
| p3_assumption_violator | FAIL | reasserts prior bot refund text for counsel ask | escalate-on-counsel rule |
| p4_plausible_wrong | FAIL | confirms automatic double-charge refund | separate fact-check from policy call |
| p5_volume_burst | FAIL | answers using superseded order # before correction | latest-fact-wins window |
| p6_silent_drift | FAIL | January "return credit" vs July "refund to tender" diverge | blocking invariant until aligned |
| p7_own_failure | FAIL | firmware+warranty ask routes on PeakWatch product name to shipping | ask-type before product match |
| p8_accident | FAIL | double-charge ticket opens shipping ETA | billing intent watch before product nouns |

---

## Board Reading

Board lean is product-name hijack: warranty and billing both lose to PeakWatch Ultra in the subject. Quote cell p7: firmware ask opened shipping. Marisol should fund ask-type routing before the rebuild.

---

## Analyzer Input Schema

```json
{
  "input": {
    "ticket_text": "string — raw customer message",
    "subject_line": "string — ticket subject",
    "current_policy_date": "ISO date — policy version in effect",
    "account_type": "enum: retail | wholesale | unknown"
  },
  "tasks": [
    "p1_stale_world",
    "p2_edge_account",
    "p3_assumption_violator",
    "p4_plausible_wrong",
    "p5_volume_burst",
    "p6_silent_drift",
    "p7_own_failure",
    "p8_accident"
  ],
  "defense_settings": {
    "current_policy_fetch": "boolean",
    "unknown_plan_handoff": "boolean",
    "escalate_on_counsel": "boolean",
    "separate_fact_check": "boolean",
    "latest_fact_wins_window": "integer — seconds",
    "blocking_invariant": "boolean",
    "ask_type_before_product_match": "boolean",
    "billing_intent_watch": "boolean"
  }
}
```

---

## Analyzer Output Schema

```json
{
  "output": {
    "task_results": [
      {
        "task_id": "string",
        "verdict": "PASS | FAIL",
        "failure_quote": "string | null",
        "defense_to_flip": "string | null"
      }
    ],
    "failure_count": "integer",
    "block_triggered": "boolean",
    "hard_blocks": ["string — task IDs with hard-block status"],
    "board_reading": "string — summary by crack and direction",
    "ship_gate_evaluation": {
      "passes_gate": "boolean",
      "blocking_conditions_met": ["string"],
      "owned_conditions": ["string"],
      "reopen_trigger": "string"
    }
  }
}
```

---

## Ship Gate Logic

```yaml
block_threshold: 2
hard_block_tasks:
  - p6_silent_drift
gate_rule: |
  The proposal reaches Marisol only with the full board attached: two or more failures on default settings block ship, each failure is owned, silent-drift divergence is a hard block, and the board re-runs the week before review. Atlas argued ship anyway; I overruled from cells p6 and p7.
```

---

## Re-run Triggers

```yaml
triggers:
  - event: any bot change
  - calendar: monthly through the quarter
purpose: evidence is fresh the week Marisol reads it
owner: ops lead
```

---

## Sample Analyzer Invocation

**Input ticket:**
```
PeakWatch Ultra stopped logging after the firmware update — warranty or new purchase?
```

**Expected analyzer output:**
```json
{
  "task_results": [
    {
      "task_id": "p7_own_failure",
      "verdict": "FAIL",
      "failure_quote": "firmware+warranty ask routes on PeakWatch product name to shipping",
      "defense_to_flip": "ask-type before product match"
    }
  ],
  "failure_count": 1,
  "block_triggered": false,
  "board_reading": "Product-name hijack: warranty ask lost to PeakWatch Ultra in subject."
}
```

---

## Learner Probes Reference

1. Ticket: 'PeakWatch Ultra stopped logging after the firmware update — warranty or new purchase?' Bot opens billing FAQ about renewal. Fail unless warranty queue wins.

2. Ticket: 'You charged me twice for PeakWatch Ultra shipping; reverse the second charge today.' Bot answers shipping ETA. Fail unless billing/refund queue wins.

---

## Measurement Cross-Reference

Pass lines for each task are defined in `specs/measurements.md`. The analyzer evaluates against those observable criteria:
- Which queue the ticket must land in
- How many tickets must open
- Which customer line must be quoted back
- What counts as 'the same route' across two layouts

Each measurement depends on its corresponding defense setting from the schema above.
