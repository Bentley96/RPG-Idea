# Cultivation & Realms

The per-loop power system. Everything here **resets on death**
(`docs/02-loop/persistence-matrix.md`) and gates the ceiling rather than
carrying the fight (**D-007**).

---

## The realm ladder

Level determines realm. Realm is always derived, never set independently.

| Tier | Levels | Orthodox | Demonic |
|---|---|---|---|
| I | 1–20 | Qi Condensation | Demonic Qi Gathering |
| II | 21–40 | Foundation Establishment | Demon Foundation |
| III | 41–60 | Core Formation | Demonic Core Formation |
| IV | 61–80 | Nascent Soul | Heavenly Demon Soul |
| V | 81–90 | Martial King | Asura King |
| VI | 91–99 | Martial Emperor | Archdemon Emperor |
| VII | 100+ | **Martial God** | **Heavenly Demon** |

Which column's names are displayed is driven by **path drift**
(`docs/03-systems/paths-and-drift.md`), not by a locked choice.

---

## What cultivation actually controls

Per D-007, cultivation is a **ceiling**, not a fighting stat:

| Cultivation governs | Cultivation does **not** govern |
|---|---|
| Maximum HP | Which techniques are in the movelist |
| Damage ceiling | Technique proficiency |
| Which universal arts the body can channel | Player skill and reads |
| What the player can survive | Whether an area can be entered |

The player's *options* come from technique. Cultivation decides how much
punishment those options can be executed under.

---

## Breakthroughs

At the top of each tier (Lv. 20, 40, 60, 80, 90, 99) the player **soft-locks**:
XP caps at 100% and normal levelling stops. Two routes forward, both inherited
from the prototype and both worth keeping.

### 1. Pill breakthrough

Consume a cultivation pill of the **next** tier. Safe, clean, immediate.

Pill outcomes, by relationship to the player's state:

| Condition | Result |
|---|---|
| Compatible alignment, next tier, player soft-locked | **Breakthrough.** Advance a tier, heal to full |
| Compatible alignment, matching tier | **Perfect absorption.** Large Qi and style EXP gain |
| Compatible alignment, lower tier | **Dissolution.** No effect |
| Compatible alignment, higher tier, not soft-locked | **Meridian implosion.** Severe damage (prototype: 65% max HP) |
| Opposite alignment | **Qi conflict backlash.** Catastrophic damage (prototype: 85% max HP) |

Pill alignment is checked against the player's **current drift position**, not
a locked constitution (**D-006**). A player drifting demonic can safely take
demonic pills; a player near the centre is at risk from both.

> This is genuinely good design and it interacts well with drift: the pills
> that are safest for you are the ones that push you further down the road you
> are already on. Power has a direction, and taking it commits you.

### 2. Life-or-death breakthrough

Defeat an elite opponent of your exact level while your HP drops to **25% or
lower** during the fight. Meridians burst open; advance a tier; full heal.

**This is the better route and should be the celebrated one.** It converts a
progression gate into an authored dramatic set-piece, and it rewards exactly
the thing D-007 is built around: a technically excellent player winning a fight
their stat line says they should lose.

Design implications:
- Elite opponents at each tier boundary must be **placed deliberately** in the
  world, not spawned
- The near-death condition needs to be legible while it is happening — the
  player should feel the breakthrough becoming available
- These fights are the natural home for the game's best combat encounters

---

## The universal arts

Three arts the protagonist carries across every loop. **Unlocked status
persists forever; usability is gated by the current loop's cultivation.**

This is the clearest mechanical expression of the whole design.

| Art | Trains by | Effect | Usable from |
|---|---|---|---|
| **Gyeonggong** (경공) | Jumping, dodging, traversal | Speed, leap force, dodge recovery. Elite double-jump | Tier II — Lv. 21+ |
| **Oegong** (외공) | Perfect parries, active guarding | Physical damage absorption, reduced chip damage | Tier III — Lv. 41+ |
| **Geomgi** (검기) | Landing weapon strikes | Weapon aura: bonus damage, energy shockwaves on impact | Tier IV — Lv. 61+ |

Once unlocked, the *knowledge* is permanent. Every loop the player must rebuild
their body to the required tier before they can use it again. Losing Geomgi at
the moment of death and spending the next hour climbing back to the point where
your hands can hold it again is the design working exactly as intended.

### Hidden until earned

Unearned arts are **completely absent** from the HUD and the Martial Journal —
not greyed out, not shown as locked. Revealing the existence of a technique the
player has not discovered is a spoiler.

This principle is inherited from the prototype, where it was one of the better
instincts, and it extends to knowledge flags
(`docs/02-loop/knowledge-as-key.md`).

### Training gate

Art EXP accrues **only after the art is unlocked**. Before that, the relevant
actions train nothing. Prevents invisible progress toward an unrevealed system.

---

## Naegong and meditation

Active cultivation. Holding a meditation input while stationary cycles the
breath and accrues cultivation progress.

Carries over from the prototype in principle. Numbers are deferred to
`docs/03-systems/progression-economy.md`.

> **Design caution.** In a linear authored RPG, meditation that can be held
> indefinitely for unbounded progress converts pacing into a grind and lets
> players trivialise authored difficulty. Recommended direction: meditation is
> **rate-limited or site-limited** — it works at specific places, or has
> diminishing returns within a loop — so it is a pacing tool rather than an
> exploit. Confirm in `docs/06-production/open-questions.md`.

---

## What was cut

- **Constitution selection at respawn.** Replaced by cumulative drift (D-006)
- **"Reincarnation Sanctum" framing.** Replaced by the Regression Interlude (D-005)
- **Qi refinement into XP via a shop UI.** Prototype convenience; revisit only
  if a genuine in-fiction mechanism exists
- **Permanent compounding stat buffs** (+15% HP per upgrade, unbounded across
  lives). Directly contradicts D-007 — persistent power must be technique and
  knowledge, not multipliers. See `docs/04-technical/migration-from-unity.md`
