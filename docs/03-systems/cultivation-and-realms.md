# Martial Rank & Cultivation Realms

The two progression ladders (**D-008**). They measure different things, they
behave differently on death, and conflating them is a design error.

| | **Martial Rank** (무공 경지) | **Cultivation Realm** (내공) |
|---|---|---|
| Tradition | Korean murim (무협) | Xianxia |
| Measures | Skill and mastery | Internal energy |
| On death | **Persists** | **Resets to Tier I** |
| Visible to others | Yes — martial artists read it on sight | Largely hidden |
| Governs | How well you fight | What you can survive |

This is **D-007** made structural: technique carries the fight, cultivation
carries the ceiling. The genre already had two ladders for exactly these two
things, and the game uses both.

> **Why both.** The world is Korean murim and the factions are entirely murim,
> but the original ladder was Chinese xianxia. Murim ranks describe *how good a
> fighter you are*; xianxia realms describe *how much energy you have
> accumulated*. Using one for the persistent half and one for the resetting
> half is more authentic to the setting than either alone, and it makes the
> central split immediately legible to anyone who knows the genre.

---

## Ladder A — Martial Rank *(persists)*

| # | Rank | Korean | What it means |
|---|---|---|---|
| 0 | Unranked | 무명 | Not a martial artist. Where the protagonist begins |
| 1 | Third-Rate | 삼류 | Trained. Knows real forms |
| 2 | Second-Rate | 이류 | Competent. Handles ordinary opponents |
| 3 | First-Rate | 일류 | A **gosu** (고수) — a master, by murim reckoning |
| 4 | Peak | 절정 | Elite. Command of intent |
| 5 | Transcendent | 초절정 | Above the peak. Rare |
| 6 | Hwagyeong | 화경 | Transformation Realm |
| 7 | Hyeongyeong | 현경 | Profound Realm. Pinnacle of human martial arts |
| 8 | Saengsagyeong | 생사경 | Life-and-Death Realm |
| 9 | Jayeongyeong | 자연경 | Nature Realm. The summit |

**Rank rises with technique and proficiency, never with qi.** It is derived
from what the protagonist has learned and how deeply, and it survives every
death.

Rank is the ladder the game is actually *about*. A protagonist on loop 12 at
Tier I cultivation and First-Rate martial rank is the design's core image: a
master's hands in a novice's body.

### Rank is social

Martial rank is legible to other martial artists on sight. This drives content:

- NPCs address the protagonist by their standing — a Second-Rate is spoken to
  very differently from a Peak
- Sects gate teaching by rank, not by cultivation
- A high-rank, Tier-I protagonist reads as an **anomaly**, and the world should
  find that unsettling. Someone whose skill and energy do not match is either a
  fraud, a cripple, or something the murim has no word for. That reaction is
  free characterisation and free foreshadowing

> **Open:** does anyone ever work out what that mismatch means? Tracked as Q-15
> in `docs/06-production/open-questions.md`.

---

## Ladder B — Cultivation Realm *(resets)*

Rebuilt from Tier I every loop. Names vary by **path drift**
(`docs/03-systems/paths-and-drift.md`).

| Tier | Orthodox (정파) | Unorthodox (사파) | Demonic (마교) |
|---|---|---|---|
| **I** | Qi Condensation | Qi Scavenging | Demonic Qi Gathering |
| **II** | Foundation Establishment | Crude Foundation | Demon Foundation |
| **III** | **Core Refinement** | Tempered Core | Corrupt Core Refinement |
| **IV** | Core Formation | Blackened Core | Demonic Core Formation |
| **V** | Nascent Soul | Severed Soul | Heavenly Demon Soul |
| **VI** | Soul Transformation | Soul Devourer | Asura Transformation |
| **VII** | Martial King | Sapa Overlord | Archdemon |
| **VIII** | **Martial God** (무신) | **Sole Sovereign** (독존) | **Heavenly Demon** (천마) |

**Core Refinement** sits at Tier III: the core is condensed here and completed
at Tier IV. It is this project's insertion rather than a standard xianxia
realm, and it restores the mentor to the rank the original story document gave
him.

**No level bands.** Tiers are ordinal. Numeric mapping is deferred (**D-004**).
Design and code reference tiers, never levels — "level" is a banned design term
precisely because it hides which ladder is meant.

### What cultivation controls

| Governs | Does **not** govern |
|---|---|
| Maximum HP | Which techniques are in the movelist |
| Damage ceiling | Technique proficiency |
| Which universal arts the body can channel | Martial rank |
| What the player can survive | Whether an area can be entered |

---

## Breakthroughs

At the top of each cultivation tier the player **soft-locks**: normal progress
stops. Two routes forward.

### 1. Pill breakthrough

Consume a pill of the **next** tier. Safe, clean, immediate.

| Condition | Result |
|---|---|
| Compatible path, next tier, soft-locked | **Breakthrough.** Advance a tier, heal to full |
| Compatible path, matching tier | **Perfect absorption.** Large qi and proficiency gain |
| Compatible path, lower tier | **Dissolution.** No effect |
| Compatible path, higher tier, not soft-locked | **Meridian implosion.** Severe damage |
| Opposing path | **Qi conflict backlash.** Catastrophic damage |

Compatibility checks **current drift**, not a locked constitution (**D-006**),
and there are now three pill alignments rather than two. A centre-drift
protagonist takes Sapa pills safely and is at risk from both edges.

### 2. Life-or-death breakthrough

Defeat an elite opponent while your health falls to a near-fatal threshold
during the fight. Meridians burst open; advance a tier; full heal.

**This is the celebrated route.** It converts a progression gate into an
authored dramatic set-piece and rewards exactly what D-007 is built around: a
technically excellent player winning a fight their energy says they should lose.

- Elite opponents at each tier boundary are **placed deliberately**, never spawned
- The near-death condition must be legible while it is happening
- These fights are the natural home for the game's best encounters

---

## The universal arts

Three arts the protagonist carries across every loop. **Unlocked status
persists forever; usability is gated by the current loop's cultivation tier.**

The clearest single expression of the whole design.

| Art | Trains by | Effect | Usable from |
|---|---|---|---|
| **Gyeonggong** (경공) | Jumping, dodging, traversal | Speed, leap force, dodge recovery | **Tier II** |
| **Oegong** (외공) | Perfect parries, active guarding | Damage absorption, reduced chip damage | **Tier IV** |
| **Geomgi** (검기) | Landing weapon strikes | Weapon aura: bonus damage, shockwaves | **Tier V** |

Once unlocked, the *knowledge* is permanent. Every loop the body must be rebuilt
to the required tier before the art can be used again. Losing Geomgi at the
moment of death and climbing back to the point where your hands can hold it is
the design working exactly as intended.

### Hidden until earned

Unearned arts are **completely absent** from the HUD and journal — not greyed
out, not shown as locked. Revealing that a technique exists is a spoiler.

Inherited from the prototype, where it was one of the better instincts, and
extended to knowledge flags (`docs/02-loop/knowledge-as-key.md`).

### Training gate

Art proficiency accrues **only after the art is unlocked**. Prevents invisible
progress toward an unrevealed system.

---

## Naegong and meditation

Active cultivation: holding a meditation input while stationary cycles the
breath and accrues cultivation progress. Numbers deferred to
`docs/03-systems/progression-economy.md`.

> **Design caution.** Meditation that can be held indefinitely for unbounded
> progress converts authored pacing into a grind. Recommended: **rate-limited
> or site-limited**, or diminishing returns within a loop, so it is a pacing
> tool rather than an exploit. Tracked as Q-08.

---

## What was cut

- **Single-ladder realm table.** Split into two ladders (D-008)
- **Numeric level bands.** Deferred with the rest of progression (D-004)
- **Constitution selection at respawn.** Replaced by drift (D-006, D-009)
- **"Reincarnation Sanctum" framing.** Replaced by the Regression Interlude (D-005)
- **Permanent compounding stat buffs.** Contradicts D-007 — persistent power is
  technique, rank and knowledge, never multipliers
