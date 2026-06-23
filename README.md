# calibrated-critique

**Adversarial review that actually lands** — the high-signal middle between the rubber-stamp and the teardown.

> Ask an AI to *"review this"* and it rubber-stamps. Ask it to *"tear this apart"* and it bone-picks: thirty findings, a contemptuous tone, scope-creep into things you never asked about, and no idea what's actually *fine*. Neither is useful. This is a Claude Code skill that produces the calibrated middle — **critical where it counts, constructive, and honest about what to skip.**

It's one prompt. The interesting part is *which* prompt — and we didn't guess, we ran a scored bake-off.

---

## The bake-off (why this prompt, and not just "be more critical")

We built a one-pager with **4 planted load-bearing flaws** (a contradicted "real-time" claim, a single-site validation gap, a missing safety metric, an over-reaching conclusion) plus deliberate **bait** for trivial nitpicking (a style word, an already-caveated dollar figure, a conventional threshold). Then we ran **three reviewer prompts on the same target, same model** — only the prompt varied — and scored them against the answer key. (Everything is in [`demo/`](demo/); the harness is in [`references/`](references/).)

| | Harsh *("tear it apart")* | Neutral *("give feedback")* | **Calibrated (this skill)** |
|---|---|---|---|
| real flaws found | 4 / 4 | 4 / 4 | **4 / 4** |
| total findings | ~30 | ~11 | **~5** |
| majored on bait? | partly (hammered the $ figure) | no | no — *folded* the threshold into a real point |
| affirms what's sound / names what to **skip** | ✗ / ✗ | a little | **✓ / ✓** |
| names the **one** thing to fix first | ranked list | "the 3 that matter" | **✓ explicit** |
| tone | contemptuous ("statistically illiterate", "reckless") | professional | **peer ("the system may well be good")** |
| net | **bone-picking** (valid but demoralizing) | solid but verbose | **high-signal, actionable** |

**The surprise — and the whole point:** on a strong model, *recall is not the lever.* Even bare "give feedback" found all four real flaws. "Be more critical" does **not** find more — it just finds it *worse*. The harsh framing reliably degrades into bone-picking; the calibrated prompt converts the same raw critical capacity into a tight, tiered, peer-toned review that knows what to **skip**, affirms what's **sound**, and names the **one** thing to fix first.

So this skill doesn't make a reviewer *meaner*. It makes a reviewer *land*.

---

## Use it

**In Claude Code:** the skill auto-surfaces on review / gatekeeping / red-team tasks. Or invoke it, then dispatch a review subagent with its prompt and a specific target (`"review src/foo.py:30-80 for correctness"`).

**Anywhere else:** copy the prompt out of [`SKILL.md`](SKILL.md) and paste it ahead of whatever you want reviewed. It's model-agnostic.

The prompt's load-bearing clauses (and *which failure mode each one defeats*) are tabled in [`SKILL.md`](SKILL.md) — so you can trim it without losing the calibration. The short version:

- *"WANTS this to succeed… a peer, not a gatekeeper"* → kills the contemptuous tone.
- *"'most of this is sound' is a valid outcome"* + a `COSMETIC-SKIP` tier → lets it say what's **fine**.
- *"do NOT manufacture concerns or pad"* + *"quote the exact text"* → kills padding and hand-waving.
- *rank by severity* + *"the ONE thing to fix first"* → kills the flat 30-item wall.

---

## Re-tune it (the harness is reusable)

Vetting a new reviewer model, or tweaking the prompt? Reuse the [`references/`](references/) test-bed: a short target with N planted load-bearing flaws of known severity + bait, then score **recall of the real flaws** vs **padding/bone-pick rate** vs **calibration markers** (ranking / affirmation / constructiveness / tone). Inject known faults, measure detection — the honest way to tune a critic instead of vibing it.

---

## Honest caveats (this skill would want you to keep them)

- **It calibrates; it doesn't conjure recall.** On a weak/sycophantic model it can't manufacture rigor that isn't there — pick a low-hallucination reviewer model *and* this prompt.
- **A harsh/divergent pass still has a place.** In the bake-off, the harsh reviewer surfaced one genuinely sharp thing the others missed ("reordering a worklist ≠ saving reading time → the savings model is broken"). For max-divergence red-teaming, run a harsh pass *too* — and filter it. Ship from the calibrated one.
- **Verify what any reviewer emits.** A flagged "bug at line 42" is a *lead*, not a fact. Go read line 42.

---

## Status

🧪 **Working beta.** The prompt is empirically backed (it beat two controls on a scored harness), but it's one experiment on one target with one model family — a [brick thrown to attract jade](https://github.com/evnchn-agentic/chengyu-skills/tree/main/chengyu-throw-brick-attract-jade) (拋磚引玉): a deliberately-early public pass put out *to elicit* the refinement that will show what actually needs hardening. Issues and counter-examples welcome; if you find a target where it bone-picks or rubber-stamps, that's a test-bed contribution — exactly the jade this brick is fishing for.

*Distilled by an AI agent from a live working session (where this exact discipline caught the agent's own over-claims), then scored against controls and curated by a human. The bake-off in [`demo/`](demo/) is the real run, not a reconstruction.*

MIT licensed.
