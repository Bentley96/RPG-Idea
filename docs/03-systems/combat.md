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

A Tier I player on loop 9 has a deep movelist, knows every opponent's tell,
and fights beautifully. They also die in three hits. That gap *is* the game.

### What this means for tuning

- **Encounters are tuned against technique tier, not character level.** An
  encounter's difficulty budget assumes the technique set the player is
  expected to hold when they first reach it.
- **Every act must be survivable at Tier I** (see
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

**Locked: D-014.** Posture is a real internal value, hidden from the UI, that
degrades under sustained defence and breaks deterministically.

### Rules

| | |
|---|---|
| **Who has it** | Player and every enemy |
| **Drains from** | Blocking, being parried, absorbing heavy hits |
| **Escalates** | Per-hit cost **rises the longer guard is held continuously** |
| **Recovers** | When not under pressure; slower while still guarding |
| **At zero** | **Guard break** — stagger window, open to a heavy punish or finisher |
| **Shown as** | **Nothing.** No bar, no meter, no number |

### Why it is hidden

A visible bar turns defence into meter management — the player stops watching
the fight and starts reading the HUD, and posture becomes a resource to
optimise rather than a state to feel.

Hidden, it is communicated entirely through the **character**:

| Channel | Signal |
|---|---|
| Guard height | Drops as posture falls; the stance opens up |
| Limb tremor | Arms and blade shake under sustained pressure |
| Footing | Steps become uneven, stance slips backward |
| Breathing | Audio strains |
| Block impact | VFX colour and audio weight shift as posture falls |

The player learns their guard is about to go by *watching someone struggle*,
which is both better-feeling and more appropriate to the fantasy than a
depleting rectangle.

### Why it is deterministic, not a chance roll

Posture breaks at a threshold. It does **not** roll a hidden per-hit chance.

1. **Guard break is high-consequence.** Random high-consequence outcomes read
   as unfair rather than difficult — the same objection that cut passive
   evasion (below)
2. **Knowledge is this game's primary progression axis.** A combat system that
   cannot be learned works against the core design
3. **Posture exists to let a Tier I player deliberately break a stronger
   opponent** (D-007). Random breaks mean the player cannot set up the punish,
   and posture stops expressing skill
4. **Randomness punishes blocking, not turtling.** Unpredictable breaks teach
   "blocking is unreliable, stop blocking." A learnable threshold teaches
   "I get about six blocks, then I must create space or take initiative" —
   which is the intended behaviour

> **Hidden is not random.** Concealing the *number* preserves mystery.
> Concealing the *rule* would destroy the lesson. Posture is learned by feel
> and by reading the character, exactly like an attack tell.

### Why posture matters to this game specifically

Posture is a **skill** stat, not a power stat. It is the clearest
moment-to-moment expression of **D-007**: a Tier I player with excellent parry
timing can break the posture of a far stronger opponent and kill them from a
position their health bar says is hopeless.

Parries deal heavy posture damage, making them the primary route to breaking a
stronger opponent — the skill route to victory when the cultivation route is
closed.

### Persistence

Combat-transient. Not saved in any form. Reset on encounter start.
See `docs/02-loop/persistence-matrix.md`.

### Deferred

Thresholds, drain rates, escalation curve and recovery timings are progression
numbers and wait on **D-004**.

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

Five weapon types, six sect styles. Structure carries over from the prototype;
**content is out of scope for the vertical slice**
(`docs/06-production/vertical-slice.md`), which is unarmed only.

| Weapon | Styles it enables |
|---|---|
| Unarmed | Sorim Vajra Fist |
| Sword | Mount Hua Plum Blossom, Mudang Tai Chi |
| Saber | Peng Clan Tiger Saber |
| Staff | Sorim Vajra Staff |
| Dagger | Hao Clan Shadow Dagger |

Styles are **techniques**, so they persist across loops
(`docs/02-loop/persistence-matrix.md`). Weapons are **items**, so they do not —
the player re-acquires the blade each loop but never forgets how to use it.
Knowing exactly where to find a sword on day one is a knowledge flag, and a
good one.

---

## Animation model

Per **D-012**: **one base moveset per weapon, styles layered thinly on top.**

### Weapon base set — ~40–60 clips

Authored once per weapon, including unarmed. Shared by the player and by every
enemy archetype using that weapon.

| Category | Clips |
|---|---|
| Locomotion — idle, walk, run, sprint, turn, jump, land | 8–10 |
| Light combo string | 4–5 |
| Heavy attacks | 2–3 |
| Sprint / dash attack | 2 |
| Dodges, directional | 4 |
| Guard — idle, impact, break | 3 |
| Parry — attempt, success | 2 |
| Hit reactions — light and heavy × 4 directions | 8 |
| Stagger, knockdown, get-up | 3–4 |
| Death | 1–2 |

### Style signature set — 3–5 clips

| Clip | Purpose |
|---|---|
| Signature finisher | Replaces the base combo ender |
| Signature heavy | Replaces or adds to the base heavy |
| One or two specials | The move the style is *known* for |
| Optional stance idle | Silhouette recognition |

> **Hard constraint:** a new style costs **five new clips or fewer**. A concept
> needing a full moveset is not a style — it is a weapon, and it is budgeted as
> one.

### Non-animation differentiation

This is where most of the felt difference comes from, and it is close to free.
Budget attention here before adding clips.

| Lever | Example |
|---|---|
| Timing curves | Mudang slow and circular; Peng heavy and committed — same clip, different play rate and recovery |
| VFX and aura | Plum blossoms for Mount Hua; tiger imagery for Peng; shadow smear for Hao |
| Hit-stop weight | Saber lands heavy; dagger lands light and fast |
| Audio | Distinct impact and whoosh layers per style |
| Trails and afterimages | Shape and colour of the weapon trail |
| Finisher camera | Framing and shake on signature moves |

### Rules

- Styles may **replace** base clips; they never require the base to be
  re-authored
- All humanoids share **one skeleton**; animation retargets across characters
- **Enemy archetypes draw from the weapon base sets.** An archetype costs only
  its unique moves — this is where the larger half of the total budget lives
- Signature clips are authored against the base set's timing, so they slot in
  without re-tuning the combo

### Where this does and does not help

At the current scope the saving is modest: five bases plus six signature sets
(~274 clips) against six full movesets (~300). The model is adopted for two
other reasons — each *additional* style costs ~4 clips instead of ~50, and
enemy archetypes reuse the bases.

**The remaining cost driver is weapon count, not style count.** See Q-16 in
`docs/06-production/open-questions.md`.

---

## Open work

- Telegraph language specification (colours, poses, audio)
- Posture's diegetic feedback set — the exact stance, tremor, footing and audio
  states that signal a guard about to break
- i-frame counts and defensive timing values
- Enemy archetype catalogue with attack sets and tells
- Hit feedback: hitstop, camera shake, impact audio layering
- Multi-enemy encounter rules — how many can commit at once
