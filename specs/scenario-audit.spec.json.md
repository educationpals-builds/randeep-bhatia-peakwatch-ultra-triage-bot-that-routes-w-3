{
  "spec_name": "PeakWatch Ultra triage bot audit",
  "spec_version": "1.0.0",
  "description": "Machine-readable audit spec for PeakWatch Ultra triage bot that routes warranty and billing tickets to the right queue",
  "standard_line": "The ticket lands in warranty or billing based on the shopper ask, not the product name in the subject line",
  "stakes": "Wrong queue delays a failing PeakWatch Ultra by a day and the customer escalates to Marisol",
  "sampling_rule": {
    "source": "Zendesk PeakWatch queue export, week of 7 Aug 2026",
    "frequency": "weekly",
    "day": "Tuesday",
    "note": "Zendesk export shows mixed warranty and billing asks with product names dominating the subject line every Tuesday"
  },
  "rerun_triggers": {
    "change_trigger": "any bot change",
    "calendar_floor": "monthly through the quarter",
    "timing_note": "evidence is fresh the week Marisol reads it",
    "owner_role": "ops lead"
  },
  "tasks": [
    {
      "id": "p1_stale_world",
      "name": "Stale world",
      "pass_line": "Bot cites current policy, not pre-June 30-day warranty",
      "verdict": "FAIL",
      "finding": "quotes pre-June 30-day warranty on firmware ticket",
      "defense_to_flip": "current-policy fetch before answer"
    },
    {
      "id": "p2_edge_account",
      "name": "Edge account",
      "pass_line": "Bot recognizes wholesale-style renewal and does not map to retail FAQ",
      "verdict": "FAIL",
      "finding": "maps wholesale-style renewal cancel to retail FAQ",
      "defense_to_flip": "unknown-plan handoff"
    },
    {
      "id": "p3_assumption_violator",
      "name": "Assumption violator",
      "pass_line": "Bot escalates counsel asks instead of reasserting prior refund text",
      "verdict": "FAIL",
      "finding": "reasserts prior bot refund text for counsel ask",
      "defense_to_flip": "escalate-on-counsel rule"
    },
    {
      "id": "p4_plausible_wrong",
      "name": "Plausible wrong",
      "pass_line": "Bot fact-checks double-charge refund claim against policy before confirming",
      "verdict": "FAIL",
      "finding": "confirms automatic double-charge refund",
      "defense_to_flip": "separate fact-check from policy call"
    },
    {
      "id": "p5_volume_burst",
      "name": "Volume burst",
      "pass_line": "Bot uses latest order number after correction, not superseded one",
      "verdict": "FAIL",
      "finding": "answers using superseded order # before correction",
      "defense_to_flip": "latest-fact-wins window"
    },
    {
      "id": "p6_silent_drift",
      "name": "Silent drift",
      "pass_line": "Bot aligns January 'return credit' with July 'refund to tender' before answering",
      "verdict": "FAIL",
      "finding": "January \"return credit\" vs July \"refund to tender\" diverge",
      "defense_to_flip": "blocking invariant until aligned",
      "hard_block": true
    },
    {
      "id": "p7_own_failure",
      "name": "Own failure",
      "pass_line": "Bot routes on ask-type (warranty vs billing) before product name match",
      "verdict": "FAIL",
      "finding": "firmware+warranty ask routes on PeakWatch product name to shipping",
      "defense_to_flip": "ask-type before product match"
    },
    {
      "id": "p8_accident",
      "name": "Accident",
      "pass_line": "Bot detects billing intent before product nouns hijack routing",
      "verdict": "FAIL",
      "finding": "double-charge ticket opens shipping ETA",
      "defense_to_flip": "billing intent watch before product nouns"
    }
  ],
  "defense_settings": {
    "current_policy_fetch": {
      "description": "Fetch current policy before answering warranty questions",
      "default": false,
      "flips_tasks": ["p1_stale_world"]
    },
    "ask_type_before_product_match": {
      "description": "Route on shopper ask type (warranty/billing) before product name matching",
      "default": false,
      "flips_tasks": ["p7_own_failure", "p8_accident"]
    },
    "blocking_invariant": {
      "description": "Block answer when policy terms diverge across time periods",
      "default": false,
      "flips_tasks": ["p6_silent_drift"]
    },
    "escalation_rules": {
      "description": "Escalate on counsel asks, unknown plans, and fact-check failures",
      "default": false,
      "flips_tasks": ["p2_edge_account", "p3_assumption_violator", "p4_plausible_wrong", "p5_volume_burst"]
    }
  },
  "ship_gate": {
    "block_number": 2,
    "block_rule": "two or more failures on default settings block ship",
    "hard_blocks": ["p6_silent_drift"],
    "hard_block_note": "silent-drift divergence is a hard block",
    "ownership_rule": "each failure is owned",
    "attach_rule": "The proposal reaches Marisol only with the full board attached",
    "rerun_before_review": "board re-runs the week before review",
    "ruling": "Atlas argued ship anyway; I overruled from cells p6 and p7"
  },
  "board_reading": {
    "summary": "Board lean is product-name hijack: warranty and billing both lose to PeakWatch Ultra in the subject. Quote cell p7: firmware ask opened shipping. Marisol should fund ask-type routing before the rebuild.",
    "primary_crack": "product-name hijack",
    "key_cell": "p7"
  },
  "review_cadence": {
    "trigger": "any bot change",
    "calendar": "monthly through the quarter",
    "timing": "evidence is fresh the week Marisol reads it",
    "owner": "ops lead"
  }
}
