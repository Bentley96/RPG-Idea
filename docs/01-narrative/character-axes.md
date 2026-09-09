# Character Axes: The Living Personality

The protagonist has no fixed personality. They are *built* by how the player
lives, across two independent psychological axes.

---

## Axis 1 — Jadedness

**Driven by:** loops lived. **Persists:** always. **Decreases:** never.

With every death and return, the weight of repeated life grinds against the
character's spirit. The more loops lived, the more **jaded, weary and
emotionally flattened** they become. Early lives carry hope and novelty; later
lives carry the monotony of someone who has seen the same beginning too many
times.

Jadedness is monotonic. It is the one thing in the game that only ever gets
worse, and that is the point — it is the price of the gift.

Fed directly by the loop counter (`docs/02-loop/persistence-matrix.md`) and
expressed most strongly through the acknowledgment layer of loop acceleration
(`docs/02-loop/replay-tax.md`).

---

## Axis 2 — Confidence

**Driven by:** realm breakthroughs. **Persists:** partially — see below.

With every realm breakthrough the character grows more **confident, assured and
commanding.** Power breeds self-belief. Someone who climbs high in a single
life walks and speaks like someone who knows their own strength.

### The reset problem, and its resolution

Cultivation resets to nothing every loop
(`docs/02-loop/persistence-matrix.md`). If Confidence were purely a function of
current realm, it would collapse to zero on every death — and the protagonist
would spend the entire game as a nervous novice, arriving at the finale with no
accumulated bearing at all. That is wrong for the story and wrong for the
character.

**Resolution — Confidence has two components:**

```
Confidence = Floor(lifetime peak realm) + Current(realm reached this loop)
```

| Component | Behaviour |
|---|---|
| **Floor** | Set by the highest realm ever reached across all loops. Persists. Never decreases |
| **Current** | Driven by the realm reached in the current loop. Resets on death, climbs back |

A protagonist who once stood at Nascent Soul does not become genuinely timid
again just because their core is gone. They carry the bearing of someone who
knows what they are capable of, sitting uncomfortably in a body that cannot yet
do it. That gap is rich characterisation and it costs nothing to implement.

The **Current** component still matters: it gives each loop an internal arc, so
that climbing back is felt rather than merely endured.

> **Recommended, not yet locked.** If a simpler model is preferred, the
> alternative is a pure lifetime-peak Floor with no current component — cheaper,
> but each loop loses its internal emotional arc. Tracked in
> `docs/06-production/open-questions.md`.

---

## The resulting personality

The axes move independently, so the protagonist's character is a **blend**
shaped by the whole campaign:

| | **Low Confidence** | **High Confidence** |
|---|---|---|
| **Optimistic** *(low jadedness)* | Hopeful but unsure — a hesitant idealist | Hopeful and bold — a radiant, driven hero |
| **Jaded** *(high jadedness)* | Worn down and withdrawn — a quiet, hollow survivor | Cold and certain — a ruthless, unshakable power |

No two campaigns produce the same person.

- A player who dies often but climbs slowly becomes **jaded and unsure**
- A player who breaks through quickly in few loops becomes **optimistic and
  confident**
- The demonic path tends to pull hardest toward **jaded and certain**

> The story's emotional ending is authored by the player's habits, not scripted
> in advance. Tone of voice, choices and the final ascension should all reflect
> where the protagonist lands on both axes.

Note that this is **orthogonal to path drift**
(`docs/03-systems/paths-and-drift.md`). Drift is what the protagonist has
*become*; the axes are how they *feel about it*. A jaded, confident Martial God
and a jaded, confident Heavenly Demon are recognisably the same person who made
different decisions — which is a far better outcome than a good/evil split.

---

## Delivery: the open question

Both axes are stated to drive **dialogue and narration tone**. That is the
boldest promise in the design and the most expensive to keep. Three approaches:

| Approach | Cost | Quality | Voice acting |
|---|---|---|---|
| **Authored variants** — write each significant line in multiple tonal versions | Very high writing budget: quadrants × lines | Highest, fully controlled | Possible |
| **Tonal selection** — author lines once per *beat*, select from a small pool by axis position | Moderate | Good, needs discipline | Possible |
| **Runtime generation** — an LLM re-voices lines against axis state | Low writing, high engineering | Variable, needs guardrails | No |

**Recommendation: tonal selection.** Author each story beat once, with two to
four tonal variants selected by axis quadrant, and reserve full per-line variation
for the handful of beats that carry the theme. It keeps the promise where the
player will actually feel it without multiplying the entire script.

Runtime generation is worth prototyping but should be treated as a whole
technical subsystem — prompting, caching, latency, cost, offline fallback,
tone drift, and the loss of voice acting — not a shortcut around the writing
budget.

> **OPEN — blocking for narrative production.** Tracked in
> `docs/06-production/open-questions.md`. Nothing in Acts 2–5 should be written
> at scale until this is decided, because it determines the script's shape.

---

## Implementation notes

- Both axes live on `SoulSave`
- Expose as a **quadrant enum** to content systems, not raw floats — writers
  and DataTables should key off `Jaded_Confident`, not `jadedness > 0.62`
- Quadrant thresholds are data, not constants
- Axis state must be queryable by the dialogue system, narration, and any
  content-generation tooling
