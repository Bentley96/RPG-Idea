# Paths & Drift

The Orthodox ↔ Demonic morality system. Per **D-006** this is a **cumulative
scalar**, never a locked choice.

---

## What changed from the prototype

The archived Unity document had the player *select* a "core constitution" at
respawn, after which the choice was permanently locked and the UI buttons
greyed out. That was a prototype convenience for testing two damage tables.

The narrative design always called for drift: *"the player's drift between them
is cumulative across reincarnations rather than a single locked choice."* The
story document was right; the prototype was expedient. Drift wins.

---

## The drift scalar

A single persistent value:

```
   -100 ─────────────── 0 ─────────────── +100
  Heavenly Demon    unaligned      Martial God
     (Magyo)                        (Jeongpa)
```

| Property | Value |
|---|---|
| Range | −100 (fully demonic) to +100 (fully orthodox) |
| Starting value | 0 |
| Persists across loops? | **Yes** — never reset by death |
| Directly chosen by the player? | **No** — never presented as a menu |
| Visible to the player? | Indirectly — see below |

### What moves it

Actions, not dialogue-wheel selections. Candidate sources:

| Source | Direction |
|---|---|
| Techniques used habitually — absorption and blood arts vs. disciplined forms | Toward the technique's alignment |
| Cultivation methods adopted | Strong pull |
| Pills consumed | Moderate pull |
| Faction alliances and betrayals | Moderate |
| Resolution of moral situations — mercy, restraint, cruelty, expedience | Small but frequent |
| Killing the defeated, or sparing them | Small, cumulative |

**Design intent:** drift should be something the player notices *having
happened*, not something they steer. The player who takes every shortcut
because shortcuts are efficient should arrive at the Heavenly Demon and
recognise how they got there.

### Visibility

The raw number is never shown. The player perceives drift through:

- **Realm names.** The displayed realm title shifts column as drift crosses
  thresholds — the same Level 45 reads as *Core Formation* or *Demonic Core
  Formation*
- **Technique availability** (below)
- **NPC reaction.** Orthodox sects grow warier; unorthodox contacts grow warmer
- **Tone.** Narration and the protagonist's voice colour with drift, alongside
  the psychological axes (`docs/01-narrative/character-axes.md`)
- **Visual language.** Qi effect colour, and eventually the protagonist's
  appearance

---

## Effect 1 — Technique gating and early unlocks

Sufficient drift unlocks path-specific techniques. Crucially, drift persists
while cultivation does not, which produces the effect described in the brief:

> Techniques can become available **early in a subsequent loop**.

A player deep into demonic drift begins a new loop at Level 1 — but demonic
techniques that were previously gated behind drift thresholds are already
available to them, because their *nature* carried across even though their
*body* did not.

This is a genuinely elegant interaction. It gives drift a mechanical reward
that scales with commitment, it makes late loops feel materially different from
early ones, and it reinforces the persistence rules without adding a new
currency.

| Drift band | Effect |
|---|---|
| ±0–25 | Unaligned. Only universal arts and neutral techniques |
| ±26–60 | Committed. Path techniques unlock; some cross-path options close |
| ±61–100 | Devoted. Deep path techniques; strong early-loop availability; the opposing path is largely shut |

> **OPEN:** whether closing off the opposite path is hard or soft, and whether
> extreme drift can be walked back. Recommendation: **soft closure, very
> expensive reversal** — redemption and fall should both be possible but should
> cost multiple loops of deliberate effort. Tracked in
> `docs/06-production/open-questions.md`.

---

## Effect 2 — Ending determination

Fallout 3 model, per your decision. Drift is measured at the end of the final
act and selects which ascension resolves.

| Final drift | Ending |
|---|---|
| Strongly positive | **Martial God.** Power matched by control |
| Strongly negative | **Heavenly Demon.** Overwhelming, corrupting tyranny |
| Near zero | **Open** — see below |

> **OPEN — the unaligned ending.** A player who reaches the end near zero has
> committed to nothing. Options: a third distinct ending (the hermit / the one
> who refuses both), a weaker version of the nearer ending, or a deliberate
> failure state. A third path is the most interesting and the most expensive.
> Tracked in `docs/06-production/open-questions.md`.

The ending is **not** a final-scene choice prompt. It is the sum of how the
player lived every loop — which is the story document's stated thesis:
*"the story's emotional ending is authored by the player's habits, not scripted
in advance."*

---

## Interaction with pills

Pill alignment checks against **current drift**, not a locked constitution
(`docs/03-systems/cultivation-and-realms.md`).

Consequences worth noting:
- A near-zero player is at risk from pills of **both** alignments. Neutrality
  is genuinely inconvenient, which is thematically correct
- Committing to a path makes that path's pills safe, accelerating it further
- Deliberately taking an opposite-alignment pill to slow one's drift is a
  catastrophic-damage decision, which is a nice bit of desperation gameplay

---

## Implementation notes

- Single float on `SoulSave` (`docs/02-loop/persistence-matrix.md`)
- Drift sources are DataTable-driven (`DT_DriftSources`) so tuning needs no code
- Band thresholds are data, not constants
- If GAS is adopted, bands are Gameplay Tags (`Path.Drift.Demonic.Devoted`) so
  ability requirements are declarative
