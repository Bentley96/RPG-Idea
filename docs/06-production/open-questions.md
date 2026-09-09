# Open Questions

Every unresolved decision, in one place. Ordered by how much downstream work
each one blocks.

When one is settled, add an entry to `docs/00-canon/decision-log.md` and remove
it from here.

**Recently resolved:** Q-01 anchor advance → **D-010** · Q-11 unaligned ending
→ **D-009** (conviction axis, four endings) · Q-12 mentor rank → **D-008**
(Core Refinement, Tier III) · Q-02 delivery model → **D-011** (budget still
open below).

---

## Blocking — resolve before significant production

### Q-02 — Voice coverage budget
**Blocks:** Acts 2–5 writing at scale
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

### Q-18 — Anchor point placement
**Blocks:** act authoring, softlock validation
**Doc:** `docs/02-loop/loop-architecture.md` §3

The anchor advance mechanic is locked (**D-010**) but the placement is not.
One advance point per act boundary is the baseline. Optional mid-act anchors
would give a finer risk/reward dial, at the cost of more softlock-safety
validation — every anchor must leave the rest of the game completable.

Also open: how the game communicates what is being given up, clearly enough
that an irreversible choice is fair.

### Q-03 — Posture as a core system
**Blocks:** vertical slice combat
**Doc:** `docs/03-systems/combat.md`

Recommended and specified, not yet confirmed. Posture is what lets a Tier I
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

### Q-16 — Animation budget
**Doc:** `docs/03-systems/combat.md`

**The single most likely thing to sink this project.** Not because it is hard,
but because the cost is invisible at design time and enormous at production
time: writing "six sect styles" takes ten seconds and builds in years of work.

**The arithmetic.** Every distinct action a character can perform needs an
animation clip — hand-keyed or motion-captured movement data, not code. One
weapon with one style needs roughly:

| Category | Clips |
|---|---|
| Light combo string | 4–5 |
| Heavy attacks | 2–3 |
| Sprint / dash attacks | 2 |
| Parry, parry success, block idle / impact / break | 5 |
| Dodges, directional | 4 |
| Hit reactions, light and heavy × 4 directions | 8 |
| Stagger, knockdown, get-up | 3–4 |
| Death | 1–2 |
| Locomotion — idle, walk, run, turn, jump, land | 8–10 |
| Signature style techniques | 3–5 |
| **Total, one weapon-style pairing** | **~40–60** |

The design lists **six weapon-style pairings**, so the player alone is roughly
**250–350 clips**. Every enemy archetype needs its own moveset — call it 30
clips each, and fifteen archetypes is another **450**. The full vision is
**700–1000+ clips**.

At an indie rate of one to three good clips a day, that is multiple years of
animation work alone.

**Why it is worse here than in most genres.** A martial arts game *is* its
animation. The entire fantasy is beautiful movement, and stiff or mismatched
animation reads as cheap instantly — you cannot hide it behind systems or
level design the way a shooter can. Six martial arts styles that do not each
look distinctly and correctly themselves are worse than two that do.

**Mitigations, roughly in order of value:**

1. **Shared base moveset per weapon + 3–5 signature techniques per style.**
   Turns six full movesets into one base plus six small sets. This is the
   practical answer and it should be the default assumption
2. **Differentiate by timing, VFX and follow-through** rather than wholly
   unique clips — same underlying swing, different aura, tempo and recovery
   gets "feels different" for perhaps 30% of the cost
3. **Cut to one or two weapons for v1.** The vertical slice already does this
4. **Buy a coherent mocap library from a single vendor** so styles at least
   match each other stylistically
5. **Share skeletons and retarget** across all humanoid characters

**The decision needed:** commit to the shared-base model now, or accept the
full per-style cost and cut the style count to what that budget actually buys.
Deferring this decision does not make it cheaper.

### Q-17 — Repository migration
**Doc:** `docs/04-technical/migration-from-unity.md`
Convert this repository to UE5, or start clean and bring `docs/` across? The
Unity scaffolding should not survive alongside the UE5 project either way.
