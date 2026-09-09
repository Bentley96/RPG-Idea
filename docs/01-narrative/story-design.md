# Story Design
## *Working Title: The Regressor's Path* (회귀)

A zero-to-hero murim RPG built on death, regression, and the slow accumulation
of skill and knowledge across many lives. Set in an ancient Korean murim world.

> **Revision note.** This supersedes the original story design document. The
> narrative is unchanged in substance; the revisions align terminology with
> **D-005** (regression, not reincarnation), formalise **D-003** (first death
> authored, the rest failure), replace the locked path choice with cumulative
> drift (**D-006**), and make explicit what the original left implicit — that
> technique carries the fight while cultivation carries the ceiling (**D-007**).

---

## Premise

The player begins as a powerless orphan in an ancient murim society —
talentless, unremarkable, routinely bullied. There is no chosen-one prophecy
and no hidden bloodline. What they have instead is something the world does not
yet understand: **when they die, they go back.**

Not reborn. Not reincarnated. **Returned** — to the same world, the same
people, the same moment, with everything they learned still in their hands.

Each death resets the character's **cultivation to nothing**. Their **martial
technique, hard-won muscle memory, and everything they know about the world
persist.** Over many lives the player transforms from a beaten orphan into a
figure capable of reshaping the murim — by the disciplined path of the
righteous, or the forbidden path of the demon.

The journey is not only about power. With each regression the character's
psychology shifts, and the person they become by the end is shaped entirely by
how the player has lived their many lives.

---

## The Opening

The game starts small and deliberately humbling.

The player is an orphan with no standing and no protection, regularly tormented
by local bullies. There is no immediate escape and no power fantasy — the early
game is meant to feel weak, frustrating, and human.

An old man, a wandering martial artist, takes pity on the orphan and offers to
teach. Crucially, this mentor is **not** a hidden grandmaster. He sits at
**Core Refinement (Tier III)** — genuinely capable, middling in the wider
murim, nowhere near the top — and he openly admits the player lacks natural talent.
What he can offer is fundamentals: stances, footwork, how to take a hit and how
to throw one. Honest, basic, unglamorous instruction.

This establishes the tone: skill here is earned, not gifted.

> The mentor must never be revealed as secretly powerful. Resist it completely.
> His ordinariness is the proof that the player's growth is their own.

---

## The First Loop (The Wall)

Under the old man's teaching, the player's **fighting ability improves through
real combat** — every brawl, every survived encounter sharpens technique. But
their **cultivation never rises**, because they were never taught to refine qi
or follow a true cultivation method. They are a skilled fighter with an empty
core.

This works, for a while. The player grows competent enough to handle ordinary
opponents. Let them feel it.

Then they hit the wall. They meet a **genuine cultivator** — someone with real
qi behind their strikes — and they are killed.

**This death is authored** (D-003). It is unwinnable by design, it is the only
scripted death in the game, and it exists to teach the rule. The player should
land clean hits and watch them fail to matter. The failure is qi, not skill.

Death is not the end. The player **regresses** to the very beginning, back to
the moment they faced the bullies. But this time they carry every technique
they learned. The bullies who once tormented them are now trivial. The player
wins.

**First revelation:** technique persists, cultivation resets, and the loop can
be exploited.

**Second revelation, immediately after:** raw skill has a ceiling. To break past
true cultivators, the player needs **real qi and authentic cultivation
methods.** The hunt for those becomes the engine of the mid-game.

---

## The Core Loop

The central mechanic and the spine of the entire narrative. Full specification
in `docs/02-loop/loop-architecture.md`.

- **On death:** cultivation returns to nothing; the world rewinds to the anchor.
- **What persists:** martial technique, technique mastery, combat instinct,
  every knowledge flag, path drift, and the memory of every prior life.
- **What resets:** the qi foundation, the body, and the world's state — forcing
  the climb again from the bottom each time.

This creates a paradoxical figure: someone whose *body* keeps starting over but
whose *fighting mind* only ever grows sharper. A player on their tenth life may
have the cultivation of a novice and the technical mastery of a veteran.

**The first death is authored. Every death after belongs to the player**
(D-003). From loop 2 onward, death is failure — it can happen anywhere, at any
time, and the story must accommodate it.

The tension of the game lives in the gap between skill and cultivation, and in
the cost of closing it.

---

## Knowledge as Power

The original document under-stated this, and it is arguably the most important
mechanic in the game.

The regressor's true advantage is not muscle memory. It is **knowing what
happens.** Where the manual is hidden. Which elder is a plant. That the convoy
is ambushed on the third night. What the innkeeper's daughter died of, and what
saying so will open.

Knowledge is the only progression death cannot touch. It costs nothing to
carry, it can never be lost, and it is how a Tier I protagonist on loop 9 does
what a Tier VII protagonist on loop 1 could not.

Full system: `docs/02-loop/knowledge-as-key.md`.

---

## The Three Paths

A branching morality system driven by **cumulative drift**, never by a choice
prompt (D-006, D-009). Three roads, not two — **Unorthodox (사파) is a distinct
path**, not a weaker Demonic and not the absence of commitment.

### The Path of Murim — *Toward the Martial God*
The orthodox, disciplined road. Mastery through righteous sects, legitimate
cultivation methods, restraint, and the slow honest climb. The endpoint is to
ascend as a **Martial God** — a paragon whose power is matched by control.

### The Unorthodox Road — *Toward the Sole Sovereign*
The pragmatic road. Profit, leverage, survival. Sapa are not villains: they
lie, deal, steal and keep their bargains because reputation is an asset. The
endpoint is the **Sole Sovereign** (독존) — someone who answers to no one and
serves no ideal, having decided that ideals are expensive and they could not
afford them.

### The Demonic Path — *Toward the Heavenly Demon*
The forbidden road. Faster, hungrier, crueller. Demonic arts, energy
absorption, techniques that consume rather than cultivate. The endpoint is to
become the **Heavenly Demon** (천마, Cheonma) — a tyrant of overwhelming,
corrupting power.

Each path offers different abilities, allies, enemies and tones. The player's
position accumulates across all loops and is **never locked**. Because drift
persists while cultivation does not, deep commitment to a path makes its
techniques available *early* in later loops — the body starts over, the nature
does not.

The ending is selected by accumulated **drift and conviction** at the end of
the final act — including a fourth outcome for a protagonist who lived many
lives and committed to nothing. Full system:
`docs/03-systems/paths-and-drift.md`.

---

## Character Evolution

The protagonist is not a fixed character — they are *built* by how the player
lives, across two independent psychological axes: **Jadedness** (rises with
loops lived) and **Confidence** (rises with realms reached).

Because the axes move independently, the final character is a blend authored by
the whole campaign rather than scripted in advance.

Full system, including how Confidence behaves under cultivation reset:
`docs/01-narrative/character-axes.md`.

---

## Tone & Themes

- **Earned power, not gifted power** — talent is denied; everything is
  practice, repetition and persistence.
- **The cost of returning** — coming back is a gift that slowly hollows you out.
- **Identity as accumulation** — you are not who you were born as; you are the
  sum of how you have lived.
- **The pull between discipline and hunger** — the Martial God and the Heavenly
  Demon as two answers to the same question of what to do with power.

---

## Notes for Content Generation

Binding rules for anyone — human or AI — writing content for this game.

- Early-game writing should feel grounded, small, and physically frustrating.
  Avoid grandeur.
- The mentor (Core Refinement, no hidden depths) stays humble and human. Never
  reveal him as secretly powerful.
- Dialogue and narration tone must be **dynamically adjusted** to the
  character's current position on the Jadedness and Confidence axes.
- **Use "regression", never "reincarnation."** See
  `docs/00-canon/glossary.md` for the full banned-terms list.
- Cultivation resets should land as a genuine loss every time, never
  trivialised, even as technique persistence softens the blow.
- The three paths must feel mechanically and tonally distinct, not a
  good/evil cosmetic swap. **Unorthodox is never written as a weaker Demonic**
  or as fence-sitting.
- Each path has its own **diction and register**, not just sentiment: Orthodox
  is calm and measured, Unorthodox wry and transactional, Demonic blunt and
  hot (**D-011**). See `docs/03-systems/paths-and-drift.md`.
- **Never present the path as a choice.** No menus, no dialogue wheel with a
  demonic option flagged as such. Drift accumulates from what the player does.
- Content must assume the player may arrive at **any act at Tier I** with high
  technique. Never write a scene that assumes a minimum cultivation level.
- Every loop must yield durable progress — a knowledge flag, a technique, or a
  drift shift. See the no-wasted-loop rule in
  `docs/02-loop/loop-architecture.md`.

---

## Act structure

Act 1 is authored above. Acts 2–5 are scaffolded in
`docs/02-loop/loop-architecture.md` §5 and remain **to author**.
