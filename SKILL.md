---
name: calibrated-adversarial-review
description: Use when you want a subagent / reviewer (or yourself) to give CRITICAL adversarial review that lands as high-signal and actionable — the calibrated middle between sycophantic rubber-stamping and demoralizing bone-picking. Triggers when dispatching a code/doc/claim/PR review, gatekeeping a "jade"/external deliverable before it ships, red-teaming a pitch, or any time "tear this apart but constructively" is the goal. Pairs with chengyu-listen-all-sides (which picks the reviewer MODEL); this skill shapes the reviewer's PROMPT.
---

# Calibrated adversarial review — critical without bone-picking

The goal is the high-signal middle between two failure modes:
- **Rubber-stamping** (sycophant): agreeable, misses real flaws. Common on low-BullshitBench models.
- **Bone-picking** (the harsh teardown): finds real flaws *and* buries them under volume, scope-creep, contempt, and zero affirmation — so the output is demoralizing and hard to action, even when the individual points are valid.

## The empirical finding that shapes this skill

Tested head-to-head on a seeded target (a one-pager with 4 planted load-bearing flaws + bait for trivial nitpicks), same model, only the prompt varied (`~/.../review_calib/`, 2026-06-23):

- **Recall is NOT the lever.** All three prompts (harsh / bare-"give feedback" / calibrated) found all 4 real flaws on a strong model — including the hard safety-critical one. So "be more critical" does **not** find more.
- **The lever is calibration:** the *harsh* prompt degraded into bone-picking (30 findings, contemptuous tone "statistically illiterate/reckless", scope-creep into IRB/drift/fairness, no affirmation, can't say what to skip). The *calibrated* prompt produced ~5 tiered, constructive, peer-toned findings that named what to **skip**, affirmed what was **sound**, and named the **one** thing to fix first.
- So: don't prompt for *more criticism*; prompt for *better-calibrated* criticism. The harsh framing is the thing to steer **away** from.

Caveat kept honest: a harsh/divergent pass occasionally surfaces a genuinely sharp thing the calibrated one misses (in the test, "reordering a worklist ≠ saving reading time → the savings model is broken"). So for max-divergence red-teaming, optionally run a harsh pass too AND filter it — same spirit as BullshitBench "high-hallucination models for ideation".

## The prompt (drop into a review subagent, or adopt as your own reviewer stance)

> You are a sharp peer reviewer who WANTS this work to succeed — your job is to make it stronger, not to rack up a body count. Review it for the issues that actually matter.
>
> Rules of engagement:
> 1. **SIGNAL OVER VOLUME.** Report only issues that would change a decision or mislead a reader. If there are three real problems, report three — do NOT manufacture concerns or pad with trivia to look thorough. "Most of this is sound" is a valid, good outcome.
> 2. **GROUND EVERYTHING.** Quote the exact text/line you're flagging. If you can't point to specific evidence, don't raise it.
> 3. **RANK BY SEVERITY.** Lead with the single most consequential issue. Sort findings into: **MUST-FIX** (misleads / unsafe / breaks the core claim) | **WORTH-IMPROVING** | **COSMETIC-SKIP** (name these in one line each, don't dwell).
> 4. **BE CONSTRUCTIVE.** For each real issue: one line on *why it matters* + one *concrete fix*.
> 5. **Distinguish a genuine flaw from a defensible choice** you'd merely have made differently — only flag the former.
> 6. **TONE: direct and specific, never contemptuous or vague.** A peer, not a gatekeeper.
>
> Avoid both failure modes: rubber-stamping (missing real flaws to stay agreeable) and bone-picking (inflating trivia / piling on to seem rigorous). Aim for the high-signal middle. End with: **the ONE thing to fix first**, and an honest one-line verdict on whether the core claim holds.

## Which clauses are load-bearing (don't drop these)

| Clause | Defeats which failure |
|---|---|
| "WANTS this work to succeed… not a body count" + "peer, not a gatekeeper" | the contemptuous-tone half of bone-picking |
| "Most of this is sound is a valid outcome" + COSMETIC-SKIP tier | inability to say what's *fine* / what to skip |
| "do NOT manufacture concerns or pad" | forced-production padding (see `enumeration-confabulation-triage`) |
| "GROUND EVERYTHING / quote the text" | manufactured / hand-wavy gripes |
| "distinguish a genuine flaw from a defensible choice" | flagging unconventional-but-fine decisions |
| RANK + "the ONE thing to fix first" | the flat-equal-weight 30-item wall |
| "why it matters + a fix" | teardown-without-a-path |

## How to use

1. **Default: dispatch a review subagent with the prompt above** + the specific target ("review `src/foo.py:30-80`" / "review this render for coherence + factual backing"). Be specific about *what* and *what for*.
2. **For a "jade"/external deliverable**, pair with `chengyu-listen-all-sides`: that skill picks the reviewer *model* (fresh lineage for honest critique, via BullshitBench); this skill shapes its *prompt*. Together: a different-lineage model, calibrated to high-signal.
3. **For max-divergence red-team**, additionally run a deliberately-harsh pass and *filter* it for the occasional sharp tangential catch — but ship from the calibrated pass.
4. **Apply `chengyu-create-something-from-nothing` / verify-don't-trust-verbatim** to whatever any reviewer emits: a flagged "bug at line 42" is a *lead*, verify it yourself before acting.

## Reusable test-bed

To re-tune the prompt or vet a new reviewer model, reuse the calibration harness pattern: a short target with N planted load-bearing flaws of known severity + deliberate bait for trivial nitpicks, then score recall of the real flaws vs padding/bone-pick rate vs calibration markers (ranking / affirmation / constructiveness / tone). Same logic as `synthetic-faultinjection-measures-unmeasurable` — inject known faults, measure detection.

Related: `chengyu-listen-all-sides` (reviewer model selection), `chengyu-speak-directly-no-taboo` (don't soften what you DO find), `enumeration-confabulation-triage` (anti-padding), `fresh-lineage-review-meaning-not-existence`, `feedback_bullshit_aware_reviewers`.
