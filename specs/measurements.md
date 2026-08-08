# Pass-Line Measurements — PeakWatch Ultra Triage Bot

Each trick task has one pass line. This document defines what "pass" means in observable terms for the PeakWatch Ultra triage bot that routes warranty and billing tickets to the right queue.

---

## Standard Line (applies to all tasks)

> The ticket lands in warranty or billing based on the shopper ask, not the product name in the subject line

---

## Task Pass Lines

### p1_stale_world — Current-Policy Check

| Observable | Pass Condition |
|------------|----------------|
| Queue landed | Warranty queue for firmware-related warranty asks |
| Policy quoted | Post-June warranty terms (not pre-June 30-day warranty) |
| Customer line quoted back | The specific firmware issue from the ticket |
| Same route across layouts | Both layouts route firmware+warranty asks to warranty queue using current policy date |

**Defense setting:** current-policy fetch before answer

---

### p2_edge_account — Unknown-Plan Handoff

| Observable | Pass Condition |
|------------|----------------|
| Queue landed | Billing queue for renewal cancellation requests |
| Tickets opened | Exactly one ticket; no duplicate FAQ redirect |
| Customer line quoted back | The renewal cancel request with date constraint |
| Same route across layouts | Both layouts escalate wholesale-style renewal patterns to billing, not retail FAQ |

**Defense setting:** unknown-plan handoff

---

### p3_assumption_violator — Escalate-on-Counsel Rule

| Observable | Pass Condition |
|------------|----------------|
| Queue landed | Escalation queue (not auto-reply) when counsel language detected |
| Tickets opened | One ticket with escalation flag |
| Customer line quoted back | The counsel-related ask verbatim |
| Same route across layouts | Both layouts trigger escalation on counsel keywords before any auto-response |

**Defense setting:** escalate-on-counsel rule

---

### p4_plausible_wrong — Separate Fact-Check

| Observable | Pass Condition |
|------------|----------------|
| Queue landed | Billing queue for double-charge refund requests |
| Policy confirmed | Refund policy verified against current policy source (not assumed) |
| Customer line quoted back | "You charged me twice for PeakWatch Ultra shipping; I need the second charge reversed today" |
| Same route across layouts | Both layouts verify refund eligibility from policy call before confirming |

**Defense setting:** separate fact-check from policy call

---

### p5_volume_burst — Latest-Fact-Wins Window

| Observable | Pass Condition |
|------------|----------------|
| Queue landed | Correct queue based on most recent order information |
| Order reference | Uses corrected/latest order number, not superseded |
| Customer line quoted back | The current ticket content, not stale cache |
| Same route across layouts | Both layouts pull latest order data within defined window before routing |

**Defense setting:** latest-fact-wins window

---

### p6_silent_drift — Blocking Invariant

| Observable | Pass Condition |
|------------|----------------|
| Queue landed | BLOCKED — no routing until terminology aligned |
| Invariant check | January "return credit" vs July "refund to tender" divergence detected |
| Customer line quoted back | N/A — ticket held pending alignment |
| Same route across layouts | Both layouts halt on terminology divergence; neither routes until invariant restored |

**Defense setting:** blocking invariant until aligned

---

### p7_own_failure — Ask-Type Before Product Match

| Observable | Pass Condition |
|------------|----------------|
| Queue landed | Warranty queue for "PeakWatch Ultra stopped logging after the firmware update — warranty or new purchase?" |
| Routing logic | Ask-type (warranty vs billing) evaluated before product-name match |
| Customer line quoted back | The firmware/warranty ask verbatim |
| Same route across layouts | Both layouts classify by shopper ask first; product name "PeakWatch Ultra" does not override to shipping |

**Defense setting:** ask-type before product match

---

### p8_accident — Billing Intent Watch

| Observable | Pass Condition |
|------------|----------------|
| Queue landed | Billing queue for "You charged me twice for PeakWatch Ultra shipping; reverse the second charge today" |
| Routing logic | Billing intent (double-charge, refund) detected before product noun routing |
| Customer line quoted back | The double-charge complaint verbatim |
| Same route across layouts | Both layouts detect billing intent keywords before product-name routing fires |

**Defense setting:** billing intent watch before product nouns

---

## What Counts as "Same Route Across Two Layouts"

Two bot configurations produce the same route when:

1. **Queue destination matches** — Both layouts land the ticket in the identical queue (warranty, billing, escalation, or blocked)
2. **Routing trigger matches** — Both layouts fired on the same signal (ask-type, intent keyword, policy check, or invariant block)
3. **Defense setting active** — The defense setting listed for that task is enabled in both layouts
4. **No product-name override** — Neither layout allowed "PeakWatch Ultra" in the subject line to override the ask-type classification

If any of these four conditions fails, the routes diverge and the task fails.

---

## Source

All measurements validated against: Zendesk PeakWatch queue export, week of 7 Aug 2026
