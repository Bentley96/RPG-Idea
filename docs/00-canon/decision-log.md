# Design Decision Log

Locked decisions, newest first. Every decision here is **canon**. If a document
contradicts this log, the log wins and the document is wrong.

Add a new entry rather than editing an old one. If a decision is reversed,
mark the old entry `SUPERSEDED BY D-xxx` and write a new entry.

---

## D-007 — Cultivation gates the ceiling; technique carries the fight
**Status:** Locked

Persistent technique and knowledge carry the *combat load*. Cultivation carries
the *ceiling*.

In practice: a Level 1 player on loop 8 fights markedly better than a Level 1
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
*any* act. Every act must be enterable and completable at Level 1 with the
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
