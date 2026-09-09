# Progression & Economy

> **STATUS: DEFERRED (D-004).** Scaffolding only. Numeric work is intentionally
> postponed until the core gameplay loop is proven playable.
>
> Do not tune these numbers yet. Do not let AI-assisted generation invent them
> and treat them as canon. This document exists to hold the *shape* of the
> problem so that decisions made elsewhere do not accidentally foreclose it.

---

## Why deferred

The core loop must be proven first (`docs/06-production/vertical-slice.md`).
Economy tuning against an unproven loop is wasted work — if the loop changes,
every curve is rebuilt.

---

## What will need numbers

### Per-loop cultivation curve
- XP required per level, 1 → 100
- XP awarded per encounter tier
- Naegong meditation rate, and its cap or diminishing-return shape

### Loop acceleration
- The re-climb multiplier for previously reached realms
- How that multiplier scales with loop count
- The `TimeToFrontier` targets in `docs/02-loop/replay-tax.md`

### Qi
- Income per encounter, per meditation tick
- What it is spent on now that permanent stat buffs are cut

### Pills
- Drop and placement rates
- Backlash and implosion damage values (prototype: 85% / 65% max HP)

### Combat
- Damage and HP curves per tier
- Posture values, if posture is confirmed

---

## Constraints already locked

These bind whatever numbers eventually land. They are **not** deferred.

**1. No permanent compounding stat buffs.**
The prototype's +15% HP / +15% damage / +8% speed per upgrade level, persisting
across lives without bound, is cut. Twenty upgrades of multiplicative +15% is
roughly 16× HP, and unbounded growth across infinite loops breaks any authored
difficulty. It also directly contradicts **D-007** — persistent power is
technique and knowledge, never multipliers.

**2. Progression must not be grindable past authored pacing.**
Content is authored and placed (D-002). Any system allowing unbounded power
accrual within a loop — infinite meditation, farmable respawning enemies —
lets players trivialise authored encounters and destroys the pacing the linear
structure depends on. See the meditation caution in
`docs/03-systems/cultivation-and-realms.md`.

**3. Every act must be completable at Tier I** with the expected technique set
(`docs/02-loop/loop-architecture.md`). This is a hard constraint on every
difficulty curve in the game.

**4. Curves live in DataTables, not code.**
`DT_CultivationCurve`, `DT_EncounterRewards`, `DT_PillEffects`, `DT_DriftSources`.
CSV-backed so they are tunable and diffable
(`docs/04-technical/technical-design.md`).

---

## Open before this document can be written

- Is the vertical slice loop fun? (blocking)
- Is posture confirmed as a core system?
- What is Qi actually spent on, now that permanent buffs are cut?
- How many loops is a full playthrough expected to be?
