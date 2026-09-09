# Persistence Matrix

**The authoritative answer to "what survives death?"** for every system in the
game. If you are implementing or generating anything, this table is binding.

---

## The invariant

> **Regression rewinds the world. Only the protagonist's mind and soul carry
> forward.**

Because the framing is regression and not reincarnation (**D-005**), the world
returns to exactly the state it held at the anchor. NPCs are alive again.
Quests are un-completed. Items are back where they were. Nothing the player
physically did persists.

What persists is **what the protagonist is** — technique burned into muscle
memory, knowledge held in the mind, and the accumulated weight of having lived
this before.

Any proposed exception to this invariant requires an entry in
`docs/00-canon/decision-log.md`. Do not add exceptions casually — each one
weakens the player's ability to reason about the rules, and the entire design
depends on those rules being learnable.

---

## The matrix

**Legend:** ● PERSISTS · ○ RESETS · ◐ PARTIAL (see note)

### Cultivation — the body

| System | On death | Notes |
|---|---|---|
| Cultivation tier | ○ | Returns to Tier I. Always |
| Cultivation progress | ○ | Returns to 0 |
| Realm name | ○ | Derived from tier and drift |
| Max HP | ○ | Returns to base |
| Current HP | ○ | Full at anchor |
| **Posture** | ○ | Combat-transient. Not saved at all — reset on encounter start (**D-014**) |
| Qi (current) | ○ | Empty at anchor |
| Naegong cultivation progress | ○ | The core is gone |
| Breakthrough state | ○ | Soft-locks re-apply from Tier I |

### Technique — the soul

| System | On death | Notes |
|---|---|---|
| **Martial rank** (무공 경지) | ● | Ladder A. Derived from technique and proficiency, never from qi (**D-008**) |
| Technique unlocks (movelist) | ● | The heart of D-007 |
| Technique proficiency | ● | Mastery within each technique |
| Sect style masteries | ● | |
| Gyeonggong — unlocked | ● | Usability still gated by current cultivation tier |
| Geomgi — unlocked | ● | Usability still gated by current cultivation tier |
| Oegong — unlocked | ● | Usability still gated by current cultivation tier |
| Combat instinct / reads | ● | Held by the *player*, not the save file |

> **Unlocked ≠ usable.** The three universal arts persist as knowledge forever,
> but the body must be rebuilt each loop before it can channel them. See the
> proficiency locks in `docs/03-systems/cultivation-and-realms.md`. This is the
> single clearest expression of "technique persists, cultivation gates."

### Knowledge — the mind

| System | On death | Notes |
|---|---|---|
| Knowledge flags | ● | Never lost, under any circumstance |
| Map / location discovery | ● | Knowing where a place is *is* knowledge |
| Pill and technique formulae | ● | |
| Enemy pattern knowledge | ● | Granted as flags, see no-wasted-loop rule |
| Event foreknowledge | ● | Ambushes, betrayals, disasters |
| Dialogue already heard | ● | Drives skip/compress in loop acceleration |

### Psychology

| System | On death | Notes |
|---|---|---|
| Loop counter | ● | Monotonic. Never resets |
| Jadedness | ● | Increments each loop, and sharply on anchor advance (**D-010**). Never decreases |
| Confidence | ◐ | Persistent floor from lifetime peak realm, plus a current-loop component. See `docs/01-narrative/character-axes.md` |
| Path drift | ● | Cumulative across all loops (D-006, **D-009**) |
| Conviction | ● | Monotonic. Never decreases (**D-009**) |
| Act testaments | ● | Permanent path record per act close. Never revised (**D-013**) |

### World

| System | On death | Notes |
|---|---|---|
| NPC alive/dead state | ○ | Everyone the player killed is alive again |
| NPC relationship / trust | ○ | Mechanically reset — but the player *knows* how to rebuild it fast |
| Quest / objective state | ○ | |
| World flags, doors, switches | ○ | |
| Faction standing | ○ | Reset. Drift (●) is the protagonist's nature, not their reputation |
| Time of day / calendar | ○ | Rewinds to anchor time |

### Inventory

| System | On death | Notes |
|---|---|---|
| Items | ○ | Physical objects rewind with the world |
| Equipment / weapons | ○ | Back where they were found |
| Consumed pills | ○ | Un-consumed, back in the world |
| Currency | ○ | |

> Knowing where an item is, and that it is worth having, is a **knowledge
> flag** (●). Losing the sword but knowing exactly which floorboard it is
> under is the regressor fantasy working as intended.

### Meta

| System | On death | Notes |
|---|---|---|
| Anchor position | ● | Advances only when the player chooses, irreversibly (**D-010**) |
| Abandoned content | ● | Content behind an advanced anchor stays permanently inaccessible |
| Act / story progress | ● | Story does not rewind with the world |
| Current act | ● | Testaments are taken at act close, not loop end |
| Settings, accessibility | ● | |

---

## Relationships: the important subtlety

Relationship state resets; relationship *knowledge* persists. Mechanically the
NPC does not know the player. But the player knows:

- what that NPC wants
- what to say to earn trust in one conversation instead of ten
- that they will betray you on the seventh day
- that their brother is alive, which is the thing that breaks them open

This is how regressor fiction actually works, and it is far more interesting
than a persistent affinity number. Social content should be authored so that
**knowledge collapses a long relationship arc into a short one**, rather than
skipping it.

---

## Rule for new systems

> Any new system must declare its row in this matrix **before** implementation.
> A system whose persistence behaviour is undefined must not be built.

This applies equally to AI-generated systems. If a prompt produces a mechanic
without a persistence answer, the mechanic is incomplete.

---

## Implementation note

Split save data along this line at the storage layer, not just conceptually:

- **`SoulSave`** — everything marked ● or ◐. Survives death. Written on gain.
- **`LifeSave`** — everything marked ○. Discarded wholesale on death.
- **`WorldSave`** — world state for the current loop. Regenerated from the
  anchor snapshot.

A death is then: discard `LifeSave`, restore `WorldSave` from the anchor
snapshot, keep `SoulSave`, increment the loop counter. Making the code shape
match the design shape means the rules are enforced structurally rather than by
discipline.

See `docs/04-technical/technical-design.md`.
