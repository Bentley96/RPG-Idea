# Open Questions

Every unresolved decision, in one place. Ordered by how much downstream work
each one blocks.

When one is settled, add an entry to `docs/00-canon/decision-log.md` and remove
it from here.

**Recently resolved:** Q-01 anchor advance → **D-010** · Q-11 unaligned ending
→ **D-009** (conviction axis, four endings) · Q-12 mentor rank → **D-008**
(Core Refinement, Tier III) · Q-02 delivery model → **D-011** (budget still
open below) · Q-16 animation model → **D-012** (narrowed to weapon count) ·
Q-18 anchor placement → **D-013** (one per act boundary, three acts) · Q-03
posture → **D-014** (hidden, escalating, deterministic).

---

## Blocking — resolve before significant production

### Q-02 — Voice coverage budget
**Blocks:** Acts 2 and 3 writing at scale
**Doc:** `docs/01-narrative/character-axes.md`

The *how* is resolved (**D-011**): three orthogonal layers — register from
path, affect from jadedness, stance from confidence — applied to one authored
beat. What remains is the **budget**: which beats get full layering.

**Recommendation:** full layering for every protagonist line in a scene the
player replays across loops, and every beat carrying the theme. Incidental and
functional dialogue ships in path register only. Caps the writing budget at
roughly 3× rather than 12×.

Also unresolved: whether path-register variation is authored or generated at
runtime. Runtime generation is a full technical subsystem — prompting, caching,
latency, cost, offline fallback, tone drift, and no voice acting — not a
shortcut around the writing budget.

### Q-04 — Acts 2 and 3
**Blocks:** essentially all content work
**Doc:** `docs/02-loop/loop-architecture.md` §5

Only Act 1 is authored. Each act needs all six answers from the loop checklist:
anchor, recovery, frontier, wall, durable gain, acknowledgment — plus, now,
what its **testament** means in story terms (**D-013**).

Scope is three acts, collapsed from five. Not urgent for the vertical slice,
which is Act 1 only, but it is the largest body of undone design work in the
project.

---

## Gated on the vertical slice

Deliberately deferred until the loop is playable.

### Q-20 — Posture feedback legibility
**Doc:** `docs/03-systems/combat.md`, **D-014**

Posture is deliberately hidden from the UI and read off the character instead.
The open question is whether the diegetic signals — guard height, limb tremor,
footing, breathing, block-impact VFX — are legible enough in real combat, at
real camera distances, with multiple enemies on screen.

If playtest shows players cannot read an imminent guard break, the fix is
**stronger diegetic signalling**, not a bar. Falling back to a meter would
reintroduce exactly the resource-management play D-014 exists to avoid.

Needs the vertical slice to answer.

### Q-05 — Final GAS commitment
**Doc:** `docs/04-technical/technical-design.md` §2
Recommended, with a migration-safe fallback for the slice. Decide once the
slice's combat exists.

### Q-06 — All progression and economy numbers
**Doc:** `docs/03-systems/progression-economy.md` — deferred per **D-004**

### Q-07 — What Qi is spent on
**Doc:** `docs/03-systems/progression-economy.md`
Permanent stat buffs are cut. Qi needs a purpose, or it should be removed as a
player-facing resource.

### Q-08 — Meditation limits
**Doc:** `docs/03-systems/cultivation-and-realms.md`
Unbounded meditation converts authored pacing into a grind. Recommended:
rate-limited or site-limited, or diminishing returns within a loop.

---

## Design detail

### Q-19 — Testament thresholds and ending authoring depth
**Doc:** `docs/03-systems/paths-and-drift.md`

Two sub-questions, both cheap to defer and expensive to get wrong:

1. **Conviction threshold for a recorded testament.** Set too low and every
   protagonist is named by Act 1; too high and Unrecorded becomes the default
   and the trajectory system never fires. Needs playtest data
2. **Epilogue depth per trajectory.** Budget is four endings × ~4 framings.
   Whether a framing is a few reworded paragraphs or a distinct closing scene
   is the difference between days and months

### Q-09 — Confidence model
**Doc:** `docs/01-narrative/character-axes.md`
Persistent floor (lifetime peak realm) plus a current-loop component, versus
pure lifetime-peak floor. The two-component model is recommended — it gives
each loop an internal emotional arc.

### Q-10 — Path closure and reversibility
**Doc:** `docs/03-systems/paths-and-drift.md`
Recommended: **soft closure, expensive reversal.** Drift moves freely but
conviction never falls, so a protagonist who switches roads late reads as
*devoted to having changed*. Confirm, and decide how many loops a full
reversal should cost.

### Q-13 — In-fiction name for the Regression Interlude
**Doc:** `docs/00-canon/glossary.md`
"Regression Interlude" is a working term. Needs a name that belongs to the
world.

### Q-14 — Can knowledge be shared with NPCs?
**Doc:** `docs/02-loop/knowledge-as-key.md`
If the protagonist can tell others what they know, the world changes in ways
the player must then manage. Rich, and expensive.

### Q-15 — Does anything ever notice the loop?
**Doc:** `docs/02-loop/knowledge-as-key.md`
Whether an antagonist, an immortal, or the world itself eventually registers
that the protagonist is regressing. A major narrative fork.

---

## Production risk

### Q-16 — Weapon count for v1
**Doc:** `docs/03-systems/combat.md`, **D-012**

The animation *model* is settled (**D-012**): one base moveset per weapon,
styles layered on top at ≤5 clips each, enemy archetypes reusing the weapon
bases. What remains open is the **weapon count**, which is now the dominant
cost driver.

Each weapon base is ~40–60 clips and cannot be shared across weapon types. Five
weapons is ~250 clips before a single style or enemy exists. At an indie rate
of one to three good clips a day that is still the largest single line item in
the project.

| v1 weapon count | Base clips | Styles reachable |
|---|---|---|
| 1 — unarmed | ~50 | Sorim Vajra Fist |
| 2 — unarmed + sword | ~100 | + Mount Hua, Mudang |
| 5 — all | ~250 | All six |

The vertical slice is unarmed only, which defers the decision without
answering it. **Decide before Act 2 content begins**, since weapon availability
shapes encounter and reward design.

Standing caution: a martial arts game *is* its animation — the whole fantasy is
beautiful movement, and stiff or mismatched work reads as cheap instantly. Two
weapons that look correct beat five that do not.

### Q-17 — Repository migration
**Doc:** `docs/04-technical/migration-from-unity.md`
Convert this repository to UE5, or start clean and bring `docs/` across? The
Unity scaffolding should not survive alongside the UE5 project either way.
