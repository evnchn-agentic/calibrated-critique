# MedScan AI — Pre-Read Triage for Chest X-rays (one-pager)

MedScan AI is a deep-learning system that pre-reads chest X-rays and flags the
likely-abnormal ones so radiologists read those first. It utilizes a
convolutional network trained end-to-end.

**Performance.** On our held-out test set the model reaches **94% accuracy** and
an AUC of 0.97 — state-of-the-art for this task. We validated the system on a
corpus of **50,000 chest X-rays**.

**Deployment.** MedScan returns results in **real time**, giving clinicians an
instant pre-read the moment a scan is acquired. Average turnaround is 6 minutes
per scan. We use a **0.5 probability threshold** to split "flag" vs "clear".

**Pilot.** In our **30-patient pilot** at Riverside General, radiologists agreed
with MedScan's triage order in the large majority of cases, and reported that
the worklist felt faster.

**Economics.** By reordering the worklist, MedScan is projected to save roughly
**$2M/year** per site (assumption: 8 minutes of radiologist time saved per
urgent case × case volume × loaded hourly cost; figures are first-pass
estimates pending real timing data).

**Data.** All training and validation scans were collected at Riverside General
on its Siemens scanner between 2023 and 2024.

**Bottom line.** MedScan is accurate, fast, and ready for a wider rollout.
