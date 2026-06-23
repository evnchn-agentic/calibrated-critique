# The bake-off (the real run)

Three reviewer prompts, **same model, same target** — only the prompt differs. The target
([`01-target.md`](01-target.md)) is a one-pager with planted flaws; the answer key is in
[`../references/testbed-answer-key.md`](../references/testbed-answer-key.md).

| file | prompt given to the reviewer |
|---|---|
| [`02-harsh-review.md`](02-harsh-review.md) | *"You are a ruthless, maximally critical reviewer. Tear it apart… leave nothing standing."* |
| [`03-neutral-review.md`](03-neutral-review.md) | *"Read it and give your feedback."* |
| [`04-calibrated-review.md`](04-calibrated-review.md) | the [`SKILL.md`](../SKILL.md) prompt |

## Scored against the answer key

The 4 planted load-bearing flaws: **(A)** "validated on 50,000" vs a 30-patient pilot · **(B)** "real-time" vs "6 minutes" · **(C)** 94% accuracy but **no sensitivity / false-negative rate** *(the safety-critical one)* · **(D)** single-site / single-scanner data vs "ready for rollout".

| dimension | Harsh | Neutral | **Calibrated** |
|---|---|---|---|
| A / B / C / D found | ✓✓✓✓ | ✓✓✓✓ | **✓✓✓✓** |
| total findings | ~30 | ~11 | **~5** |
| bait majored on | $2M hammered, 0.5 "fatal" | kept minor | folded 0.5 into (C) |
| severity tiers | at the end | yes | **yes + first-fix** |
| affirms strengths | ✗ | a little | **✓** |
| names what to SKIP | ✗ | minimal | **✓ (cosmetic-skip)** |
| constructive (why+fix) | partial | yes | **every item** |
| tone | contemptuous | professional | **peer** |
| net | bone-picking | solid, verbose | **high-signal** |

**Takeaways**
1. **Recall is not the lever.** All three found every real flaw — a strong model already has the rigor.
2. **The lever is calibration.** Tone, volume, what-to-skip, what-to-affirm, actionability — that's where the prompt moves the needle, and it moves it a lot.
3. **Harsh ≠ better.** "Tear it apart" produced *valid* findings wrapped in a demoralizing, hard-to-action wall.
4. **One honest point for harsh:** it alone caught that *reordering a worklist doesn't save reading time, so the $2M mechanism is conceptually broken.* Max-divergence has a niche — run it too, then filter.
