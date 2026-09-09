# Combat Design

**Scope note:** this is an action RPG combat system, not a fighting game and
not a Sifu-style brawler (**D-002**). It needs to be readable, weighty and
satisfying. It does **not** need per-move frame data tables, cancel-window
matrices, or crowd attack-token systems. If the design later drifts toward
dense one-on-one duelling, revisit that decision explicitly.

---

## The governing rule

Per **D-007**:

> **Technique carries the fight. Cultivation carries the ceiling.**

| | Controlled by | Resets on death? |
|---|---|---|
| How well you fight — options, reads, movelist | **Technique** (persistent) | No |
| What you can survive — HP, damage ceiling, channelling capacity | **Cultivation** (per-loop) | Yes |

A Level 1 player on loop 9 has a deep movelist, knows every opponent's tell,
and fights beautifully. They also die in three hits. That gap *is* the game.

### What this means for tuning

- **Encounters are tuned against technique tier, not character level.** An
  encounter's difficulty budget assumes the technique set the player is
  expected to hold when they first reach it.
- **Every act must be survivable at Level 1** (see
  `docs/02-loop/loop-architecture.md` §2). Cultivation widens the margin; it is
  never the price of entry.
- **Enemies do not scale to the player.** Content is authored and placed
  (D-002). The prototype's level-difference auras are cut.

---

## Core actions

Carried forward from the prototype where they were sound.

| Action | Behaviour |
|---|---|
| **Light attack** | Fast combo string. Primary damage over time |
| **Heavy attack** | Slow, committed, high damage. Openings and finishers |
| **Guard** | Hold to block. Reduces damage, costs stamina/posture, chip damage remains |
| **Parry** | Guard input inside a tight window before an incoming hit. Zero damage, attacker staggered, heavy posture damage to them |
| **Dodge** | Directional evade with invulnerability frames |
| **Lock-on** | Toggles camera focus onto a target. Right-shoulder framing |

**Parry window:** 0.25s is inherited from the prototype as a starting value.
Tune against readability, not difficulty.

---

## Posture

The prototype referenced "posture damage" once and never defined it. It needs
defining, because it is what makes defence active rather than passive.

**Proposal — to confirm:**

- Both player and enemies carry a **posture** gauge alongside health
- Posture depletes from: blocking, being parried, absorbing heavy hits
- Posture regenerates when not under pressure; regeneration is slower while
  guarding
- At zero posture: **stagger** — a window where the victim is open to a
  heavy punish or a finisher
- Successful parries deal heavy posture damage; this is the primary route to
  breaking a stronger opponent

**Why it matters here specifically:** posture is a *skill* stat, not a
cultivation stat. A Level 1 player with excellent parry timing can break a
far stronger opponent's posture and kill them from a position their health bar
says is hopeless. That is D-007 expressed as a moment-to-moment mechanic, and
it is probably the most important thing this combat system can do.

> **OPEN:** confirm posture as a core system, and whether enemies telegraph
> posture state visually. Tracked in `docs/06-production/open-questions.md`.

---

## Readability

Because knowledge is the primary progression axis
(`docs/02-loop/knowledge-as-key.md`), combat **must be learnable**. A player
who dies and returns needs to have learned something true.

Requirements:

1. **Every attack has a distinct, readable tell.** Wind-up pose, timing, or
   audio cue. The player should be able to name what killed them.
2. **A consistent telegraph language across the whole game.** If a colour or
   flash means "unblockable", it means that everywhere, forever. Establish it
   once, never violate it.
3. **No hidden randomness in outcomes.** Discussed below.
4. **Deaths grant combat knowledge flags** (`K_CBT_`), so that a lesson the
   player noticed is also a lesson the game recorded.

---

## Cut from the prototype

Three systems from the archived Unity document are **removed**. Rationale
recorded here and in `docs/04-technical/migration-from-unity.md`.

### Passive evasion (RNG attack negation) — **CUT**

The prototype rolled a hidden percentage chance for incoming attacks to simply
not land. This is removed for three reasons:

1. It corrupts learning. If attacks sometimes miss for invisible reasons, the
   player cannot build a true model of the combat system — and a true model is
   the game's primary progression currency.
2. It contradicts the story's thesis of earned power, not gifted power.
3. It feels broken in both directions: unearned survival, then unexplained
   death.

**Replacement:** the *fantasy* of effortless evasion is preserved through
deterministic means — extended i-frames on dodge, cheaper defensive actions,
and Gyeonggong-driven mobility that lets a skilled player simply not be there.
Evasion becomes something the player does, not something that happens to them.

### Suppression and Terror auras — **CUT**

Level-difference auras scaled enemy speed from 1% to 220%. These were a
sandbox answer to "the dummy might be any level." In authored content the
encounter is placed at the intended difficulty, so the mechanism is
unnecessary — and it would make almost every fight either trivial or
unreadable.

> If a *spiritual pressure* effect is wanted for a specific dramatic
> encounter, author it as a scripted property of that encounter. It should not
> be a global rule derived from a level subtraction.

### Respawning target dummies and dev cheats — **CUT**

Prototype scaffolding. May return as an internal training map, never as
shipped content.

---

## Weapons and styles

The prototype defined 5 weapon types and 6 sect styles. The structure is sound
and carries over, but the **content is out of scope for the vertical slice**
(`docs/06-production/vertical-slice.md`).

| Weapon | Styles it enables |
|---|---|
| Unarmed | Sorim Vajra Fist |
| Sword | Mount Hua Plum Blossom, Mudang Tai Chi |
| Saber | Peng Clan Tiger Saber |
| Staff | Sorim Vajra Staff |
| Dagger | Hao Clan Shadow Dagger |

Styles are **techniques**, so they persist across loops
(`docs/02-loop/persistence-matrix.md`). Weapons are **items**, so they do not —
the player must re-acquire the blade each loop, but they never forget how to
use it. Knowing exactly where to find a sword on day one is a knowledge flag,
and a good one.

> **Scope warning.** 5 weapons × 6 styles × full movelists is an enormous
> animation budget and is the most likely thing to sink this project. See
> `docs/06-production/open-questions.md`.

---

## Open work

- Confirm posture as a core system
- Telegraph language specification (colours, poses, audio)
- i-frame counts and defensive timing values
- Enemy archetype catalogue with attack sets and tells
- Hit feedback: hitstop, camera shake, impact audio layering
- Multi-enemy encounter rules — how many can commit at once
