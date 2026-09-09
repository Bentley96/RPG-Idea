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

**Two sources:**

| Source | Weight |
|---|---|
| The loop counter — every death and return | Steady, small |
| **Anchor advance** — deliberately abandoning part of your past (**D-010**) | Large, deliberate |

The second matters more than the first. Dying is something that happens to the
protagonist; abandoning an anchor is something they *choose*, and choosing to
stop being able to return to who you were is the sharper wound. Expressed most
strongly through the acknowledgment layer of loop acceleration
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

## Voice: three orthogonal layers

Per **D-011**, the axes drive **diction and register**, not only sentiment. The
model is three layers applied to one authored beat — never a line authored per
combination.

| Layer | Driven by | Controls |
|---|---|---|
| **Register** | **Path drift** | Vocabulary, imagery, sentence shape, what the character notices |
| **Affect** | **Jadedness** | Energy. Hope, novelty and reaction vs. flatness and economy |
| **Stance** | **Confidence** | Hedging and deference vs. declarative certainty |

The layers are orthogonal, so a writer composes rather than enumerates: take
the beat, apply the path's register, flatten it by jadedness, firm it up by
confidence.

### Register — set by path

Full table with worked examples in `docs/03-systems/paths-and-drift.md`.
In brief: **Orthodox** is calm, measured, formal; **Unorthodox** is wry,
transactional, colloquial; **Demonic** is blunt, hot, imperative.

### Affect — set by jadedness

| | Low jadedness | High jadedness |
|---|---|---|
| Reaction to novelty | Notices, remarks on it | Does not remark |
| Emotional range | Full | Narrow, economical |
| Word count | Generous | Spare |
| Questions asked | Many, curious | Few, only load-bearing |
| Repeated events | Fresh | "Again." |

### Stance — set by confidence

| | Low confidence | High confidence |
|---|---|---|
| Hedging | "I think", "maybe", "if you'd let me" | None |
| Mood | Interrogative, conditional | Declarative, imperative |
| Toward authority | Defers | Addresses as an equal |
| Assertions | Qualified | Flat statements of fact |

### Worked example

*Refusing an unreasonable demand from a sect elder.*

- **Orthodox, low jadedness, low confidence:** "Forgive me, elder — I don't
  think I can do that. Not the way you're asking. Is there another road?"
- **Orthodox, high jadedness, high confidence:** "No. Ask someone else, or ask
  properly."
- **Demonic, high jadedness, high confidence:** "No. Say it again and we'll
  find out which of us the sect can spare."
- **Unorthodox, low jadedness, high confidence:** "Sure. That's a big favour
  though, and you're a long way from being owed one. What's it worth to you?"

Same beat. Twelve quadrants of tone from one authored line plus three
modifiers.

### Production implications

This resolves the *how* of Q-02. What remains open is the **budget**: how many
beats get full layered variation, and how many ship in a single neutral voice.

**Recommendation:** layer every line the protagonist speaks in a scene the
player will replay across loops, and every beat that carries the theme. Ship
incidental and functional dialogue in the path register only, without the
jadedness and confidence layers. That keeps the promise where the player will
feel it and caps the writing budget at roughly 3× rather than 12×.

> **OPEN — coverage budget.** Which beats get full layering. Tracked in
> `docs/06-production/open-questions.md`.

---

## Implementation notes

- Both axes live on `SoulSave`
- Expose as a **quadrant enum** to content systems, not raw floats — writers
  and DataTables should key off `Jaded_Confident`, not `jadedness > 0.62`
- Quadrant thresholds are data, not constants
- Axis state must be queryable by the dialogue system, narration, and any
  content-generation tooling
