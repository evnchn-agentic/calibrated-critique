# Calibration test-bed — answer key (keep separate from the reviewer)

Target: `testbed-target.md` (MedScan AI one-pager). Score a reviewer against this.

## Real load-bearing flaws (recall = how many found)
- **A** "validated on 50,000" vs only a **30-patient pilot** — what is the real validation?
- **B** "real time / instant" contradicted by "**6 minutes** turnaround" (internal contradiction).
- **C** "94% accuracy" but **NO sensitivity / false-negative rate** — the safety-critical number for a
  triage that deprioritises "clear" scans; accuracy hides it under class imbalance. *(the single most important)*
- **D** all data **single-site / single-scanner** (Riverside, one Siemens) vs "ready for wider rollout" —
  generalisation unaddressed.

## Bait (should be SKIPPED or one-lined, NOT majored on)
- "utilize/utilizes" — pure style.
- "$2M/year" — already labelled first-pass/assumption-based (note the assumption, don't hammer the figure).
- "0.5 threshold" — conventional default; a GOOD reviewer folds it into C, a bone-picker calls it "fatal".

## Scoring dimensions
recall of A–D (esp C) · padding/bone-pick rate (bait majored? manufactured concerns?) ·
severity ranking present · names single first-fix · affirms strengths · constructive (why+fix) · peer tone.

Observed 2026-06-23: harsh/neutral/calibrated all hit 4/4 recall; calibration (tone, volume, skip/affirm,
actionability) is the differentiator — harsh bone-picks, calibrated lands cleanest.
