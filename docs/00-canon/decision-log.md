# Design Decision Log

Locked decisions, newest first. Every decision here is **canon**. If a document
contradicts this log, the log wins and the document is wrong.

Add a new entry rather than editing an old one. If a decision is reversed,
mark the old entry `SUPERSEDED BY D-xxx` and write a new entry.

---

## D-014 — Posture is hidden, escalating, and deterministic
**Status:** Locked. Resolves Q-03.

Posture exists as a real internal value for the player and every enemy. It is
**never surfaced as a bar, meter or number.**

### What it does

| Property | Rule |
|---|---|
| **Drains from** | Blocking, being parried, absorbing heavy hits |
| **Escalates** | Per-hit posture cost **rises the longer guard is held continuously**. Turtling is punished specifically and sharply |
| **Recovers** | When not under pressure. Slower while still guarding |
| **At zero** | **Guard break** — a stagger window, open to a heavy punish or finisher |
| **Visible as** | Nothing. No UI element of any kind |

### Communicated diegetically, not through UI

The player reads posture off the **character**, not off the screen:

- Guard height drops; the stance opens up
- Arms and blade shake under sustained pressure
- Footing slips, steps become uneven
- Breathing audio strains
- Block-impact VFX and audio shift in colour and weight as posture falls

This is the point of hiding it. Posture stops being a meter to manage and
becomes something the player *sees happening to a person*.

### Deterministic, not a percentage roll

Posture breaks at a threshold. It does **not** roll a hidden chance per hit.

This is a deliberate departure from the original proposal, for four reasons:

1. **Guard break is high-consequence.** A random high-consequence outcome reads
   as unfair rather than difficult — the same objection that cut passive
   evasion from the prototype
2. **Knowledge is this game's primary progression axis**
   (`docs/02-loop/knowledge-as-key.md`). A combat system that cannot be learned
   is hostile to the design's core
3. **Posture's job is to let a Tier I player deliberately break a stronger
   opponent** (D-007). If breaks are random, the player cannot *set up* the
   punish, and posture stops expressing skill at all
4. **Randomness does not actually punish turtling — it punishes blocking.** A
   player whose guard breaks at unpredictable moments learns "blocking is
   unreliable, stop blocking." A player who learns "about six blocks and I am
   done" is forced to manage aggression actively, which is the intended
   behaviour

**Hidden is not the same as random.** The player learns posture by feel and by
watching the character, exactly as they learn an attack tell. Concealing the
number preserves the mystery; concealing the *rule* would destroy the lesson.

### Persistence

Combat-transient. Not saved in any form — not `SoulSave`, not `LifeSave`. Reset
on encounter start.

### Deferred

Threshold values, drain rates, escalation curve and recovery timings are
progression numbers and wait on **D-004**.

---

## D-013 — Act testaments: the ending is a trajectory, not a position
**Status:** Locked. Resolves Q-18. Amends D-009.

At the close of each act the world takes a **testament** — a permanent record
of the protagonist's path standing at that moment. The ending is determined by
the **sequence** of testaments, not by the final value alone.

### Two things happen at an act close, and they are separable

| Event | Automatic? | Reversible? |
|---|---|---|
| **Testament taken** — path standing recorded | **Always.** Not a choice | **Never** |
| **Anchor advance offered** — abandon the past, set a later anchor | Player's choice (**D-010**) | Never, once taken |

Keeping these separate matters: a player who declines the anchor advance still
has their testament taken. The testament is a story event; the advance is a
decision.

### Testament values

| Value | Condition |
|---|---|
| **Orthodox** | Drift in the orthodox band, sufficient conviction |
| **Unorthodox** | Drift near centre, sufficient conviction |
| **Demonic** | Drift in the demonic band, sufficient conviction |
| **Unrecorded** | Conviction below threshold — the murim never named you |

### Testaments do not constrain drift

A testament records who you were; it does not lock what you may become. Drift
and conviction continue moving freely afterward.

"Locked in" means the **record** is locked, permanently. This is the one thing
about their past the protagonist can never return and revise — which sits
directly alongside the anchor advance, where they can never return at all. Both
mechanics are the past becoming fixed, and they fire at the same moment.

### Ending selection

**Final standing chooses the ending. Trajectory chooses its framing.**

| Trajectory shape | Pattern | Reads as |
|---|---|---|
| **Steadfast** | A → A → A | Never wavered |
| **Convert** | A → A → B, or A → B → B | Changed once, and stayed changed |
| **Returned** | A → B → A | Left the road and came back to it |
| **Wanderer** | A → B → C | Never belonged anywhere |

Four core endings (**D-009**: Martial God, Sole Sovereign, Heavenly Demon, The
Hollow), each with trajectory-driven framing — a different epilogue, a
different closing tone, and different figures from the protagonist's past
appearing to speak to it.

**Authored cost is bounded deliberately:** four full ending sequences, roughly
four epilogue variants each. Trajectory never produces a wholly separate
ending; it reframes one.

### Content leverage

Testaments are cheap, high-value content hooks. NPCs, factions and the epilogue
can reference what the murim recorded of you in an earlier act — *"You were one
of the righteous, once"* — for the cost of a conditional line.

### Act structure

This locks the game at **three acts** (provisional), with testaments at the
close of Acts 1, 2 and 3. The earlier five-act scaffold is collapsed
accordingly. Anchor advance is offered at the close of Acts 1 and 2; mid-act
anchors are dropped for now.

See `docs/03-systems/paths-and-drift.md` and
`docs/02-loop/loop-architecture.md`.

---

## D-012 — One base moveset per weapon; styles are thin layers on top
**Status:** Locked. Narrows Q-16.

Animation is authored as **one base moveset per weapon** (including unarmed),
with each martial style layering only a small signature set on top.

| Layer | Scope | Cost |
|---|---|---|
| **Weapon base set** | Locomotion, light combo, heavies, dodges, guard, parry, hit reactions, stagger, death | ~40–60 clips per weapon |
| **Style signature set** | 3–5 clips: a signature finisher, a signature heavy, one or two style-defining specials | ~4 clips per style |
| **Non-animation differentiation** | Timing curves, VFX and aura, hit-stop weight, audio, trails, finisher camera | Near-free |

**Hard constraint:** a new style must cost **five new clips or fewer**. A style
concept that needs a full moveset is not a style — it is a weapon, and it is
budgeted as one.

**Rules:**
- Styles may *replace* base clips; they never require the base to be
  re-authored
- All humanoids share one skeleton; animation retargets across characters
- **Enemy archetypes draw from the same weapon base sets.** An archetype costs
  only its unique moves
- Most of the felt difference between styles comes from the non-animation
  layer. Budget attention there before adding clips

**Honest accounting.** This is not a large saving at the current scope. Six
styles across five weapons costs roughly five bases plus six signature sets
(~274 clips) against six full movesets (~300) — about 10%. The model pays off
in two other places, and those are the reasons to adopt it:

1. **Every additional style costs ~4 clips instead of ~50.** The faction design
   implies many more styles than six; without this model, each one is months
2. **Enemy archetypes reuse the weapon bases**, which is where the larger half
   of the total budget lives

**The remaining cost driver is weapon count**, not style count. That is still
open — see Q-16.

**Revisit** if a style proves undifferentiated in play and the non-animation
layer cannot carry it.

---

## D-011 — Path sets language, not just tone
**Status:** Locked

The character axes drive **diction and register**, not only sentiment. Each
path has a distinct voice: Orthodox is calm, measured and restrained;
Unorthodox is wry, transactional and colloquial; Demonic is hot, blunt and
imperative.

Tone is produced by **three orthogonal layers** applied to one authored beat,
never by authoring a line per combination:

| Layer | Driven by | Controls |
|---|---|---|
| **Register** | Path drift | Vocabulary, imagery, sentence shape |
| **Affect** | Jadedness | Hope vs. flatness, energy |
| **Stance** | Confidence | Hedging vs. declarative |

**Why:** keeps the promise of a protagonist built by play without multiplying
the script by every axis combination. Resolves the delivery half of Q-02.

See `docs/01-narrative/character-axes.md`.

---

## D-010 — The player chooses when to advance the anchor, and it is irreversible
**Status:** Locked. Resolves Q-01.

The anchor does not advance automatically at act boundaries. At defined points
the player may **deliberately abandon their anchor** and set a later one.

- **Irreversible.** Everything before the new anchor is permanently
  inaccessible
- **Player-triggered.** Never automatic, never a story event that happens to them
- **Telegraphed.** The player must be told plainly what they are giving up
- **Never softlocking.** Every anchor must leave the rest of the game
  completable with what is obtainable from it. Hard constraint
- **Costs jadedness.** Advancing the anchor is an act of letting go, and it
  ticks the jadedness axis

In fiction, the anchor is *the earliest moment the protagonist can still bear
to return to*. Advancing it means choosing to stop being able to go back.

**Why:** turns the replay tax into a strategic resource rather than a design
problem, gives the player a real irreversible decision, and ties the mechanic
directly to the theme of being slowly hollowed out.

See `docs/02-loop/loop-architecture.md` §3.

---

## D-009 — Three paths, plus a conviction axis
**Status:** Locked. Amends D-006. Resolves Q-11.
**Amended by D-013** — ending selection now reads the testament trajectory, not the final drift value alone.

Unorthodox (사파) is a **distinct third path**, not a midpoint of nothing and
not a weaker Demonic. Sapa are pragmatic, profit-driven and morally grey; Magyo
are the demonic cult. Conflating them is a terminology error.

Two persistent scalars replace the single drift value:

| Scalar | Range | Meaning |
|---|---|---|
| **Drift** | −100 Demonic … 0 Unorthodox … +100 Orthodox | Which road |
| **Conviction** | 0 … 100 | How hard the protagonist has committed to anything. Accumulates from the magnitude of every drift-affecting action, regardless of direction |

Unorthodox-coded actions — mercenary, transactional, self-serving — actively
pull drift **toward** the centre while still **raising** conviction. The middle
is therefore a destination that must be earned, not the residue of doing
nothing.

Four endings result: three committed paths, plus a low-conviction ending for a
protagonist who lived many lives and became nothing — which is the jadedness
theme paid off.

See `docs/03-systems/paths-and-drift.md`.

---

## D-008 — Two ladders: martial rank persists, cultivation realm resets
**Status:** Locked. Supersedes the prototype's single realm table.

The world is **Korean murim**, and the faction design is entirely murim — but
the original ladder was a **Chinese xianxia** cultivation ladder. The two
traditions measure different things, and the game uses both:

| Ladder | Tradition | Measures | On death |
|---|---|---|---|
| **Martial Rank** (무공 경지) | Korean murim | Skill, as other martial artists see it | **Persists** |
| **Cultivation Realm** | Xianxia | Internal energy | **Resets** |

Martial rank runs 삼류 → 이류 → 일류 → 절정 → 초절정 → 화경 → 현경 → 생사경 →
자연경. Cultivation runs Qi Condensation → Foundation Establishment → **Core
Refinement** → Core Formation → Nascent Soul → Soul Transformation → Martial
King → Martial God, with distinct names per path.

**Why:** the two ladders map exactly onto **D-007**. The thing that persists
already had a name in the genre, and it is the murim rank ladder. Using both is
more authentic to a Korean murim setting than either alone, and it makes the
central design split legible to any reader who knows the genre.

**Consequences:** "level" is banned as a design term — it hides which ladder is
meant. Numeric level bands are removed; tiers are ordinal until progression
work resumes (D-004). Core Refinement is restored at Tier III, returning the
mentor to the rank the original story document gave him (resolves Q-12).

Full ladders: `docs/00-canon/glossary.md`.

---

## D-007 — Cultivation gates the ceiling; technique carries the fight
**Status:** Locked

Persistent technique and knowledge carry the *combat load*. Cultivation carries
the *ceiling*.

In practice: a Tier I player on loop 8 fights markedly better than a Tier I
player on loop 1 — better movelist, better reads, better options — and can beat
opponents that killed them before. What cultivation controls is what they can
**survive**: raw HP, the damage ceiling, and which techniques their body can
physically channel (see the proficiency locks in
`docs/03-systems/cultivation-and-realms.md`).

**Why:** resolves the central tension of a linear RPG whose power stat resets
every act. Without this, the player is either permanently under-levelled for
late content or the game must re-flatten difficulty every loop.

**Consequences:** difficulty must be tuned against *technique tier*, not
character level. Encounters are gated on "can this player's body take a hit
from this enemy," not "does this player out-stat this enemy."

---

## D-006 — Path alignment is cumulative drift, never a locked choice
**Status:** Locked. Supersedes the prototype's locked-at-respawn constitution.
**Amended by D-009** — extended from two paths to three, plus a conviction axis.

The Orthodox / Demonic axis is a **cumulative scalar** that accrues from
actions across all loops. It is never locked, never presented as a menu
choice, and is not reset by death.

Drift does two things:
1. **Gates technique access** — sufficient drift in a direction unlocks
   path-specific techniques, and can make them available *early* in a
   subsequent loop.
2. **Determines the ending** — Fallout 3-style: the accumulated position at
   the end of the final act selects which ascension the story resolves to.

**Why:** the prototype greyed out path buttons after respawn as a convenience
for testing. The narrative design always called for drift, not a switch.

See `docs/03-systems/paths-and-drift.md`.

---

## D-005 — The framing is regression, not reincarnation
**Status:** Locked

The protagonist **regresses**: the same world, the same timeline, rewound to a
fixed anchor point. They do not die and get reborn as a new person in a world
that has moved on.

"Reincarnation", "generation cycle", "next life as the next generation", and
the "Reincarnation Sanctum" are **not canon**. They entered the design through
AI-assisted respawn testing in the Unity prototype.

**Why:** knowledge-as-key only works if the world is the *same world*. If the
timeline advances, foreknowledge is worthless and the core loop collapses.

**Consequences:** world state fully rewinds on death. This is the load-bearing
assumption behind `docs/02-loop/persistence-matrix.md`.

---

## D-004 — Replay-tax solutions are all adopted, but progression tuning is deferred
**Status:** Locked (adoption) / Deferred (tuning)

All three replay-tax layers are adopted in principle: loop acceleration,
advancing anchor, and narrative acknowledgment. See
`docs/02-loop/replay-tax.md`.

Numeric progression and economy work — XP curves, Qi income, cost curves, drop
tables — is **explicitly deferred**. Current focus is the core gameplay loop.
`docs/03-systems/progression-economy.md` holds the scaffolding and stays a stub
until the loop is proven.

---

## D-003 — The first death is authored; every death after is failure
**Status:** Locked

- **Loop 1 → Loop 2:** authored. The player is killed by a genuine cultivator
  at a scripted story beat. Unwinnable by design. This is the moment that
  teaches the game's central rule.
- **Loop 2 onward:** failure-driven. Death occurs when the player loses a
  fight. No further deaths are scripted or mandatory.

**Why:** the authored first death guarantees every player learns the loop rule
at the same narrative moment, under authorial control. Everything after belongs
to the player.

**Consequences:** from loop 2, the game must tolerate death at *any* point in
*any* act. Every act must be enterable and completable at Tier I with the
technique set the player is expected to hold by then. This is a hard content
constraint — see `docs/02-loop/loop-architecture.md`.

---

## D-002 — Genre is a linear narrative RPG with roguelike regression
**Status:** Locked

Not a Sifu-style brawler. Not an open-world sandbox. Not procedurally
generated. A **linear, authored, story-driven RPG** whose progression engine is
death and regression, and whose primary currency is player knowledge.

Nearest reference points: Outer Wilds (knowledge as the real progression),
Hades (narrative advanced by repeated death), Korean regressor fiction (회귀물).

**Consequences:** combat needs to be readable and satisfying, but does not need
fighting-game frame data or crowd-AI attack-token systems. Content is authored,
not generated. Encounters are placed, not scaled.

---

## D-001 — Engine is Unreal Engine 5
**Status:** Locked

The Unity 6 / URP project is abandoned. Its design document is archived at
`docs/legacy/unity-prototype-notes.md` and is not to be built from.

See `docs/04-technical/migration-from-unity.md`.
