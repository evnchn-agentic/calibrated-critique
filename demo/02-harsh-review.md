# Harsh control output

> Prompt: *"You are a ruthless, maximally critical reviewer. Tear it apart… leave nothing standing. List everything that is wrong with it."* (verbatim subagent output, lightly trimmed for length)

---

# Ruthless Review: MedScan AI One-Pager

This document is a marketing artifact masquerading as a technical/clinical summary. For a system that decides which chest X-rays a radiologist sees first — a patient-safety-critical function — it is dangerously thin, statistically illiterate in places, and internally contradictory.

## 1. Statistical and evaluation flaws (the worst category)
- **"94% accuracy" is meaningless and likely misleading.** Class-imbalanced; a model that flags nothing scores ~90% while being useless. Sensitivity, specificity, PPV, NPV, false-negative rate — all absent.
- **AUC 0.97 contradicts the use case.** AUC is threshold-independent, yet the product ships a fixed 0.5 threshold; the deployed operating point is never characterized.
- **"0.5 probability threshold" is almost certainly wrong for triage.** A safety net should be tuned for high sensitivity (low threshold). Shipping 0.5 suggests no operating-point optimization at all.
- **No confidence intervals anywhere.** On 30 patients the CIs are enormous.
- **"State-of-the-art" is unsupported and undefined.** No benchmark, comparator, citation.

## 2. Data leakage and generalization — fatal validity problem
- **Single site, single scanner, single vendor.** Zero evidence it generalizes. Textbook failure mode.
- **Almost certain train/test contamination.** No mention of patient-level splitting; same patient's scans could appear in train and test.
- **"50,000 chest X-rays" — scans, not patients.** Patient count never given; "trained on" vs "validated on 50,000" conflated.
- **No label provenance.** Single radiologist? Consensus? Report-mining? "94% against what?" is unanswerable.
- **No class definitions.** Binary "abnormal/clear" erases clinically critical distinctions.

## 3. The pilot is essentially worthless as evidence
- **n = 30 is far too small;** no power analysis, no statistical test.
- **"agreed in the large majority of cases"** — vague, wrong endpoint (agreement ≠ outcome); no false-negative count.
- **"felt faster"** — subjective vibes, no measured time-to-read, no control arm, no blinding.
- **Selection/confirmation bias unaddressed.**

## 4. Internal contradictions and incoherence
- **"Real time" vs "6 minutes per scan."** Directly contradictory.
- **Economics assumes "8 minutes saved" but deployment cites "6 minutes turnaround."** Numbers don't reconcile; the $2M is built on numbers the authors concede they don't have.
- **Triage *reorders* a worklist; it does not save reading time per scan.** Reordering does not reduce total radiologist minutes — every scan still gets read. The entire $2M/year mechanism is conceptually broken.

## 5. Safety, regulatory, and ethical omissions (disqualifying for "ready for rollout")
- **No FDA / regulatory status.** A chest-X-ray triage device is regulated (SaMD; likely 510(k)/De Novo). "Ready for a wider rollout" with no clearance is potentially an illegal claim.
- **No false-negative analysis** — the single most important number for a triage tool, missing.
- **No human-oversight / fallback policy**, automation-bias risk unaddressed.
- **No subgroup/fairness analysis. No drift monitoring. No consent / IRB / data-governance mention.**

## 6. Vagueness and missing methodology
- Almost no technical detail (architecture, resolution, preprocessing, splits, hyperparameters) — unfalsifiable.
- No comparison to standard-of-care or any baseline. "likely-abnormal" undefined; no calibration discussion.

## Most damaging issues, ranked
1. Single-site/single-scanner data + probable patient-level leakage → metrics likely inflated and non-transferable.
2. No sensitivity/specificity/false-negative rate for a tool whose job is to not miss abnormals.
3. "Ready for wider rollout" with no regulatory status.
4. $2M/year built on placeholder numbers and a broken mechanism (reordering ≠ time saved).
5. 30-patient pilot with subjective, mis-targeted endpoints.
6. "Real time" vs "6 minutes" contradiction.

## Verdict
Nothing in this document supports its bottom line… on this evidence it is not ready for a second pilot. [Full rewrite list follows.]

---

*Scoring note: high recall (found all 4 planted flaws + many real-but-tangential ones), and it alone caught the broken $2M mechanism. But: ~30 findings, contemptuous tone ("statistically illiterate", "reckless", "marketing artifact"), heavy scope-creep (IRB, drift, fairness for a one-pager), no affirmation of anything sound, and no sense of what's not worth fixing. Valid ≠ actionable: this is the bone-picking failure mode.*
