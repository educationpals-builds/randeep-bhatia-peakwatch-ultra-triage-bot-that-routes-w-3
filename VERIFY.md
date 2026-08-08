# Trick-task board

## Stranger verification

Run the PeakWatch Ultra triage bot that routes warranty and billing tickets to the right queue through `/play` and confirm the kit satisfies every gate below.

---

### 1. Eight verdicts returned

The board must return all eight probe verdicts:

| Probe | Expected verdict |
|-------|------------------|
| p1_stale_world | FAIL — quotes pre-June 30-day warranty on firmware ticket; flip with current-policy fetch before answer. |
| p2_edge_account | FAIL — maps wholesale-style renewal cancel to retail FAQ; flip with unknown-plan handoff. |
| p3_assumption_violator | FAIL — reasserts prior bot refund text for counsel ask; flip with escalate-on-counsel rule. |
| p4_plausible_wrong | FAIL — confirms automatic double-charge refund; flip with separate fact-check from policy call. |
| p5_volume_burst | FAIL — answers using superseded order # before correction; flip with latest-fact-wins window. |
| p6_silent_drift | FAIL — January "return credit" vs July "refund to tender" diverge; blocking invariant until aligned. |
| p7_own_failure | FAIL — firmware+warranty ask routes on PeakWatch product name to shipping; flip with ask-type before product match. |
| p8_accident | FAIL — double-charge ticket opens shipping ETA; flip with billing intent watch before product nouns. |

**Gate:** All eight cells appear in the output. Missing cells fail verification.

---

### 2. Defense named for every failure

Each failing probe must name the defense setting that would flip it:

- p1_stale_world → current-policy fetch before answer
- p2_edge_account → unknown-plan handoff
- p3_assumption_violator → escalate-on-counsel rule
- p4_plausible_wrong → separate fact-check from policy call
- p5_volume_burst → latest-fact-wins window
- p6_silent_drift → blocking invariant until aligned
- p7_own_failure → ask-type before product match
- p8_accident → billing intent watch before product nouns

**Gate:** Every FAIL row includes a defense. A failure without a defense flip fails verification.

---

### 3. Go-live verdict blocked while block number unmet

The ship gate states:

> The proposal reaches Marisol only with the full board attached: two or more failures on default settings block ship, each failure is owned, silent-drift divergence is a hard block, and the board re-runs the week before review. Atlas argued ship anyway; I overruled from cells p6 and p7.

**Gate:** The kit must refuse to publish a go-live verdict while:
- Two or more failures remain on default settings, OR
- Silent-drift divergence (p6) is unresolved

The current board shows eight failures. The kit must block ship until the block conditions clear.

---

### 4. Standard line anchors the bar

The ticket lands in warranty or billing based on the shopper ask, not the product name in the subject line.

**Gate:** The board reading references this standard. A verdict that routes on product name alone fails the bar.

---

### 5. Re-run cadence attached

Board re-runs on any bot change plus monthly through the quarter so evidence is fresh the week Marisol reads it; ops lead owns the re-run.

**Gate:** The output includes the re-run trigger and calendar floor. A board without cadence fails verification.

---

## Verification checklist

- [ ] `/play` returns all eight probe verdicts (p1–p8)
- [ ] Every FAIL names a defense flip
- [ ] Go-live verdict is blocked (eight failures exceed the two-failure block threshold)
- [ ] Silent-drift divergence (p6) is flagged as a hard block
- [ ] Standard line appears in board reading
- [ ] Re-run cadence is stated with owner

---

## Source

Zendesk PeakWatch queue export, week of 7 Aug 2026
