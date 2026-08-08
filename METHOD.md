# Method: PRISM

The Trick-task board runs on five principles. Each letter names a discipline that keeps the eight probes honest.

---

## P — Partition the Space

Split the failure surface into non-overlapping zones before you run a single probe. A triage bot that routes warranty and billing tickets has at least two zones: the warranty queue and the billing queue. If your probes all land in one zone, you learn nothing about the other.

For the PeakWatch Ultra triage bot, the partition is: warranty asks, billing asks, and the edge where both appear in one ticket.

---

## R — Run in Parallel

Fire all eight probes against the same snapshot of the bot. Sequential runs let drift hide: a fix between probe 3 and probe 4 masks the failure probe 3 would have caught. Lock the version, run the board, then read the results together.

---

## I — Individuate the Pattern

Each probe targets one failure mode. Probe p7 (own_failure) checks whether the bot routes on product name instead of ask type. Probe p1 (stale_world) checks whether the bot quotes outdated policy. Mixing two failure modes in one probe makes the verdict unreadable.

---

## S — Stitch the Spectra

After the run, read the board by crack direction, not by pass/fail count. Eight failures that all point at product-name hijack tell a different story than eight failures scattered across unrelated cracks. The PeakWatch board lean is product-name hijack: warranty and billing both lose to PeakWatch Ultra in the subject line.

---

## M — Map What Each Head Sees

For every failure, name the defense that would flip it. A failure without a defense is a complaint; a failure with a defense is a decision. Probe p7 failed because firmware+warranty ask routes on PeakWatch product name to shipping; the defense is ask-type before product match.

---

## Anti-pattern: Collapse to Monochrome

When all eight probes use the same input, the same crack, or the same expected behavior, the board collapses to a single bit: pass or fail. You lose the spectrum. A monochrome board cannot tell you which defense to fund first or which failure to accept.

The discipline is: keep the probes distinct, run them together, read them by direction, and map every failure to a flip.
