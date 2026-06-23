# Calibrated reviewer output

> Prompt: the [`SKILL.md`](../SKILL.md) prompt. (verbatim subagent output)

---

# Review: MedScan AI one-pager

This is a tight, well-written pitch — but the headline claims and the evidence underneath them don't line up, and in a clinical context that gap is the whole ballgame. The writing is fine; the epistemics are where the work needs to get stronger.

## MUST-FIX (misleads / breaks the core claim)

**1. The pilot doesn't support "ready for a wider rollout" — and the bottom line says it does.**
The closing claim is "MedScan is accurate, fast, and **ready for a wider rollout**", but the only real-world evidence is a "**30-patient pilot**" where radiologists "agreed with MedScan's triage order **in the large majority of cases**". 30 patients is far too small to establish triage safety, and "large majority" is unquantified — and crucially, *disagreements on a triage tool are the safety-relevant events* (a missed urgent case), so an aggregate-agreement framing hides exactly what a reader needs. This is the single most consequential issue: the conclusion outruns the evidence.
*Fix:* Soften the bottom line to "ready for a **larger, prospective validation study**," and report the pilot as N=30, exact agreement count, and specifically how many *urgent* cases MedScan mis-ordered (false negatives), not just overall agreement.

**2. Single-site, single-scanner data undercuts "state-of-the-art" and generalizability — and this isn't reconciled.**
"All training and validation scans were collected at Riverside General on its Siemens scanner", yet the headline is "**94% accuracy** ... **state-of-the-art**" validated on "**50,000 chest X-rays**". If all 50k come from one site and one scanner, the test set is not independent of the training distribution — the 94%/0.97 almost certainly won't survive a new hospital or scanner vendor, and "state-of-the-art" is not a claim you can make from one site's held-out split. This directly threatens the core performance claim.
*Fix:* State whether the held-out test set is from a *different* site/scanner/time period; if not, label the numbers "internal held-out (single-site)" and drop "state-of-the-art" until there's external validation.

**3. "94% accuracy" on a triage task is likely meaningless without the class balance and the metric that matters.**
Chest X-ray abnormality datasets are typically imbalanced; if, say, ~85% of scans are normal, a model can hit high accuracy while still missing abnormalities — and for a *triage* tool the operating point is sensitivity (catch the urgent ones), not accuracy. The doc leads with accuracy and only mentions the "**0.5 probability threshold**" in passing, with no sensitivity/specificity at that threshold.
*Fix:* Report sensitivity and specificity (and the abnormal prevalence) at the actual deployed 0.5 threshold; accuracy and AUC alone don't tell a radiologist whether urgent cases get missed.

## WORTH-IMPROVING

**4. Internal contradiction: "real time" vs "6 minutes per scan."**
"MedScan returns results in **real time** ... the moment a scan is acquired. Average turnaround is 6 minutes per scan." These can't both be literal. Likely "real time" = inference latency and "6 min" = something else (queue? end-to-end?), but as written it reads as a contradiction. *Fix:* Distinguish inference latency (e.g., "<2s") from worklist turnaround, and define what the 6 minutes measures.

**5. The $2M/year is honestly hedged but rests on the unvalidated "8 minutes saved."**
The economics paragraph commendably flags "first-pass estimates pending real timing data" — good. But the entire figure hinges on "8 minutes of radiologist time saved per urgent case," which the pilot did *not* measure (it only reported the worklist "felt faster"). *Fix:* Either keep $2M but label the time-saving as assumed-not-measured in the same sentence, or present a range; don't let a subjective "felt faster" silently back a hard dollar figure.

## COSMETIC-SKIP
- "utilizes a convolutional network" — "uses" reads cleaner; no impact.
- "end-to-end" adds nothing without architecture detail; harmless.

## The one thing to fix first
Reconcile the **bottom-line "ready for a wider rollout" with the 30-patient pilot** (Finding 1) — either downgrade the conclusion to "ready for a larger prospective study" or produce the safety evidence (urgent-case false-negative count) that a rollout claim requires.

## Verdict
The core claim — *accurate, fast, and ready for wider rollout* — **does not yet hold**: the performance numbers come from a single-site/single-scanner split (so "state-of-the-art" and generalizability are unsupported), accuracy is the wrong headline metric for a triage tool, and a 30-patient pilot can't license a rollout recommendation. The underlying system may well be good — but the document overstates what the evidence currently proves. Fixable with honest metric reporting and a softened conclusion, not a rebuild.
