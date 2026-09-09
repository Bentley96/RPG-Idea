# Open Questions

Every unresolved decision, in one place. Ordered by how much downstream work
each one blocks.

When one is settled, add an entry to `docs/00-canon/decision-log.md` and remove
it from here.

---

## Blocking — resolve before significant production

### Q-01 — Anchor advance mechanism
**Blocks:** Acts 2–5 authoring, replay-tax implementation
**Doc:** `docs/02-loop/loop-architecture.md` §3

The anchor is fixed for Act 1 and advances at act boundaries. *What moves it,
in the fiction?*

- **A cultivation milestone** — forming a core creates a new fixed point
- **A place or object** — reaching a location or binding an artefact re-anchors
- **Uncontrolled drift** — the anchor slides forward on its own

The third is the most interesting narratively (the player loses the ability to
undo earlier mistakes, and that is frightening) and the most punishing
mechanically. Needs a call before Act 2 can be written.

### Q-02 — Narrative delivery for the character axes
**Blocks:** all Acts 2–5 writing at scale
**Doc:** `docs/01-narrative/character-axes.md`

Authored variants, tonal selection, or runtime generation? This determines the
shape and size of the entire script. **Recommendation: tonal selection** — author
each beat once with 2–4 tonal variants by quadrant, reserving full per-line
variation for the few beats that carry the theme.

Do not write Acts 2–5 at scale until this is decided.

### Q-03 — Posture as a core system
**Blocks:** vertical slice combat
**Doc:** `docs/03-systems/combat.md`

Recommended and specified, not yet confirmed. Posture is what lets a Level 1
player with excellent timing break a far stronger opponent — the clearest
moment-to-moment expression of **D-007**. Needed for the slice.

### Q-04 — Acts 2–5
**Blocks:** essentially all content work
**Doc:** `docs/02-loop/loop-architecture.md` §5

Only Act 1 is authored. Each act needs all six answers from the loop checklist:
anchor, recovery, frontier, wall, durable gain, acknowledgment.

Not urgent for the vertical slice, which is Act 1 only — but it is the largest
body of undone design work in the project.

---

## Gated on the vertical slice

Deliberately deferred until the loop is playable.

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

### Q-09 — Confidence model
**Doc:** `docs/01-narrative/character-axes.md`
Persistent floor (lifetime peak realm) plus a current-loop component, versus
pure lifetime-peak floor. The two-component model is recommended — it gives
each loop an internal emotional arc.

### Q-10 — Path closure and reversibility
**Doc:** `docs/03-systems/paths-and-drift.md`
Is deep drift a hard or soft closure of the opposite path, and can it be walked
back? Recommended: soft closure, very expensive reversal — redemption and fall
both possible, both costing multiple loops of deliberate effort.

### Q-11 — The unaligned ending
**Doc:** `docs/03-systems/paths-and-drift.md`
A player who ends near zero drift has committed to nothing. A third distinct
ending (the hermit, the one who refuses both), a weaker version of the nearer
ending, or a deliberate failure state? A third path is the most interesting and
the most expensive.

### Q-12 — Mentor's rank
**Doc:** `docs/00-canon/glossary.md`
Canonicalised as **Core Formation (Tier III)**, replacing the non-existent
"Core Refinement". Confirm, or pick a lower tier if he should be weaker.

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

### Q-16 — Animation budget
**Doc:** `docs/03-systems/combat.md`
5 weapons × 6 styles × full movelists, plus hit reactions, deaths and
traversal, is an enormous animation requirement and **the single most likely
thing to sink this project**. Mocap, marketplace, Motion Matching, or a
ruthless cut to 1–2 weapons for v1?

The vertical slice uses unarmed only, which defers but does not answer this.

### Q-17 — Repository migration
**Doc:** `docs/04-technical/migration-from-unity.md`
Convert this repository to UE5, or start clean and bring `docs/` across? The
Unity scaffolding should not survive alongside the UE5 project either way.
