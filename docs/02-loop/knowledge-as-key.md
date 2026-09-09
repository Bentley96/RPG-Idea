# Knowledge as Key

The primary progression axis. In a regression story the protagonist wins
because they **know things** — not because a number went up.

This system does not exist in the Unity prototype at all. It is new, and it is
the most important thing to get right.

---

## Principle

> Cultivation is what the player's body can survive. Technique is how well they
> fight. **Knowledge is what they are allowed to attempt.**

Knowledge is the only progression that death cannot touch (see
`docs/02-loop/persistence-matrix.md`). It costs nothing to carry, can never be
lost, and it is the mechanism by which a Tier I character on loop 9 does
things a Tier VII character on loop 1 could not.

---

## Two kinds of knowledge

There is a real distinction here and the game needs both.

**Player knowledge** lives in the player's head. They remember the boss opens
with a feint. They remember the door code. Nothing in the save file records it.
This is Outer Wilds' entire model, and it produces the strongest moments — the
player *personally* got better.

**Character knowledge** is a flag in the save. It gates dialogue, routes and
options. Without it the protagonist cannot act on something the player
personally knows, which is frustrating if overused and essential if used well.

**Design stance:** use character flags to gate *dialogue and routes*, and let
player knowledge carry *combat and problem-solving*. The protagonist should
never refuse to walk through a door the player knows about — but they may need
the flag before they can *tell someone else* about it.

---

## Flag taxonomy

| Type | Prefix | Gates | Example |
|---|---|---|---|
| **Location** | `K_LOC_` | Fast travel, direct routes, dialogue options | The hidden spirit spring behind the falls |
| **Secret** | `K_SEC_` | Dialogue, accusations, blackmail | The elder is a Demon Cult plant |
| **Foreknowledge** | `K_FOR_` | Pre-emption, warnings, ambush reversal | The convoy is attacked on the third night |
| **Method** | `K_MET_` | Crafting, cultivation shortcuts, counters | How to refine a Foundation pill |
| **Social** | `K_SOC_` | Compressed relationship arcs | What the innkeeper's daughter died of |
| **Combat** | `K_CBT_` | Readable tells, counter prompts | The saber master always feints left first |

Each flag is a DataTable row:

| Field | Meaning |
|---|---|
| `FlagID` | `K_LOC_SPIRIT_SPRING` |
| `Type` | Taxonomy above |
| `DisplayName` | Journal entry title |
| `AcquiredIn` | Act / loop where it becomes obtainable |
| `AcquiredBy` | Event, death, dialogue, observation |
| `Gates` | What it opens |
| `AccelerationEffect` | What it compresses on replay |
| `UsableFromLoop` | Some knowledge is useless until later — see below |

---

## Design rules

**1. Knowledge is never lost.** No exceptions, no amnesia mechanics, no
"corrupted memory". The one thing the player can always trust.

**2. Knowledge must visibly change the world's options.** A flag that only
increments a counter is not knowledge, it is XP wearing a costume. Every flag
should open a door, a line of dialogue, a route, or a counter.

**3. Deliberately grant knowledge the player cannot yet use.** Learning on loop
3 that the sect elder is a traitor, and having no standing to act on it until
loop 6, is *good*. It converts foreknowledge into anticipation and gives later
loops a reason to exist that the player already cares about. Use
`UsableFromLoop` for this.

**4. Death is a knowledge source.** Per the no-wasted-loop rule
(`docs/02-loop/loop-architecture.md`), losing a fight grants combat knowledge
about the thing that killed you. This turns failure into progress without
softening the loss.

**5. Prefer knowledge that trivialises rather than knowledge that unlocks.** A
flag that makes a hard thing *easy* feels like mastery. A flag that makes an
impossible thing *possible* feels like a key item. Both are needed, but the
first is the better feeling and should dominate.

**6. Knowledge should be legible.** The player must be able to see what they
know. A journal or record is not optional — it is the interface to the primary
progression system.

---

## The Martial Journal

The knowledge UI. Requirements:

- Lists all held knowledge flags, grouped by type
- Shows **where** each was learned (which loop, which act) — this is the
  player's record of their own history and it is quietly the emotional core of
  the interface
- Marks knowledge that is **held but not yet usable**
- Does **not** show unearned knowledge, even as blanked-out entries. Spoiling
  the existence of a secret is spoiling the secret

> The prototype's principle of hiding unearned abilities entirely — rather than
> greying them out — is correct and carries over to knowledge. It is one of the
> better instincts in the archived document.

---

## Worked example

`K_CBT_QI_STRIKE` — *"Skill does not stop qi."*

- **Acquired:** Act 1, loop 1, by dying to the cultivator (authored death)
- **Type:** Combat
- **Gates:** Unlocks the entire "seek real cultivation" thread in Act 2. Before
  this flag, NPCs who could teach cultivation give brush-off dialogue; after
  it, the protagonist knows what to ask for
- **Acceleration:** None — this is the first loop
- **Narrative:** The protagonist's second revelation, delivered as a mechanic
  rather than a cutscene

The player learns this by *being killed by it*. That is the model for the whole
system.

---

## Open work

- The full flag catalogue for Acts 2 and 3, pending those acts being authored
- Whether knowledge can be *shared* with NPCs, and whether that changes the
  world in ways the player must then manage
- Whether any antagonist eventually notices the loop
