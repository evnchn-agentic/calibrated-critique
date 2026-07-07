---
name: calibrated-adversarial-review
description: Use when you want a subagent / reviewer (or yourself) to give CRITICAL adversarial review that lands as high-signal and actionable — the calibrated middle between sycophantic rubber-stamping and demoralizing bone-picking. Triggers when dispatching a code/doc/claim/PR review, gatekeeping a "jade"/external deliverable before it ships, red-teaming a pitch, or any time "tear this apart but constructively" is the goal. Pairs with choosing the reviewer MODEL (a different, low-hallucination lineage); this skill shapes the reviewer's PROMPT.
---

# Calibrated adversarial review — critical without bone-picking

The goal is the high-signal middle between two failure modes:
- **Rubber-stamping** (sycophant): agreeable, misses real flaws. Common on low-BullshitBench models.
- **Bone-picking** (the harsh teardown): finds real flaws *and* buries them under volume, scope-creep, contempt, and zero affirmation — so the output is demoralizing and hard to action, even when the individual points are valid.

## The empirical finding that shapes this skill

Tested head-to-head on a seeded target (a one-pager with 4 planted load-bearing flaws + bait for trivial nitpicks), same model, only the prompt varied (the full run is in [`demo/`](demo/)):

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
| "do NOT manufacture concerns or pad" | forced-production padding (manufacturing concerns to seem thorough) |
| "GROUND EVERYTHING / quote the text" | manufactured / hand-wavy gripes |
| "distinguish a genuine flaw from a defensible choice" | flagging unconventional-but-fine decisions |
| RANK + "the ONE thing to fix first" | the flat-equal-weight 30-item wall |
| "why it matters + a fix" | teardown-without-a-path |

## A lightweight alternative: the 0–10 delta (Adam Grant)

When you don't want to hand over the full prompt — a quick gut-check, or eliciting critique from a *person* rather than a subagent — Adam Grant's framing does a lot with one question. Instead of "find problems" (which invites rubber-stamping or nitpicking), ask: **"On a scale of 0 to 10, how was this? And how do I get it closer to a 10?"** Almost nobody answers 10, and the "how do I get closer" clause forces a concrete, actionable *delta* instead of an open-ended teardown — the same high-signal middle this skill aims for, compressed into one sentence.

## How to use

1. **Default: dispatch a review subagent with the prompt above** + the specific target ("review `src/foo.py:30-80`" / "review this render for coherence + factual backing"). Be specific about *what* and *what for*.
2. **For a high-stakes / external deliverable**, pick the reviewer *model* first — a different, low-hallucination lineage gives more honest critique (see the [BullshitBench](https://petergpt.github.io/bullshit-benchmark/) hallucination-rate index); *then* shape its *prompt* with this skill. Together: a different-lineage model, calibrated to high-signal. A different lineage matters because same-context self-review tends to confirm a number *exists*, not that it *means* what you claimed.
3. **For max-divergence red-team**, additionally run a deliberately-harsh pass and *filter* it for the occasional sharp tangential catch — but ship from the calibrated pass.
4. **Verify, don't trust verbatim:** whatever any reviewer emits, a flagged "bug at line 42" is a *lead* — go read line 42 yourself before acting.

## Reusable test-bed

To re-tune the prompt or vet a new reviewer model, reuse the calibration harness pattern: a short target with N planted load-bearing flaws of known severity + deliberate bait for trivial nitpicks, then score recall of the real flaws vs padding/bone-pick rate vs calibration markers (ranking / affirmation / constructiveness / tone). It's a fault-injection harness — inject known faults, measure detection — the honest way to tune a critic instead of vibing it.

See also: the [chengyu-skills](https://github.com/evnchn-agentic/chengyu-skills) collection (this skill grew out of *listen-to-all-sides* / *speak-directly* discipline) and the [BullshitBench](https://petergpt.github.io/bullshit-benchmark/) index for picking a low-hallucination reviewer model.
