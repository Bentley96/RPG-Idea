# Loop Architecture

The spine of the game. Everything else hangs off this document.

**Prerequisite reading:** `docs/00-canon/decision-log.md` (D-002, D-003, D-005,
D-007), `docs/00-canon/glossary.md`.

---

## 1. The core loop

A **loop** is one life: from the anchor, to death, to the next anchor.

```
        ┌──────────────────────────────────────────────────┐
        │                                                  │
        ▼                                                  │
   ┌─────────┐   ┌──────────┐   ┌──────────┐   ┌───────┐   │
   │ ANCHOR  │──▶│ RECOVERY │──▶│ FRONTIER │──▶│ WALL  │   │
   │ /RETURN │   │ (known)  │   │  (new)   │   │       │   │
   └─────────┘   └──────────┘   └──────────┘   └───┬───┘   │
                                      │            │       │
                                      │ pass       │ fail  │
                                      ▼            ▼       │
                              ┌───────────────┐  ┌──────┐  │
                              │ ACT ADVANCE   │  │DEATH │  │
                              │ anchor moves  │  └──┬───┘  │
                              └───────┬───────┘     │      │
                                      │             ▼      │
                                      │      ┌────────────┐│
                                      └─────▶│ REGRESSION ││
                                             │ INTERLUDE  │┘
                                             └────────────┘
```

### The five states

**1. Anchor / Return.** The player materialises at the anchor point. Level 1.
Qi empty. Body reset. Every technique, every knowledge flag, all path drift and
jadedness intact. The loop counter increments.

**2. Recovery.** Replay of content the player has already cleared, compressed
by loop acceleration (`docs/02-loop/replay-tax.md`). The player rebuilds
cultivation along a path they already know. This section must shrink every
loop — that is a hard requirement, not an aspiration.

**3. Frontier.** New content. The reason this loop exists. Everything past the
furthest point the player has previously reached.

**4. Wall.** The obstacle that ends the loop: an opponent, a sealed route, a
situation the player cannot yet survive. Walls are beaten with **knowledge and
technique**, not with levels (D-007).

**5a. Act advance.** The wall falls. The story moves forward and, at act
boundaries, the **anchor advances** — future regressions return the player to a
later point.

**5b. Death → Regression Interlude.** The wall wins. The interlude shows what
was lost, what was kept, and what was learned, then returns to the anchor.

---

## 2. Death model

Per **D-003**:

| | Loop 1 | Loop 2+ |
|---|---|---|
| **Death is** | Authored, scripted, unavoidable | Player failure |
| **When** | A fixed story beat | Any time, any place |
| **Can it be avoided?** | No — the fight is unwinnable by design | Yes, by playing well |
| **Purpose** | Teach the loop rule | Consequence |

### The authored first death

The player, trained by the mentor in fundamentals only, has real fighting skill
and **an empty core**. They meet a genuine cultivator and are killed. Skill
alone is not enough against true qi.

Design requirements for this fight:
- It must be **legible as unwinnable** without feeling cheap. The player should
  land hits and see them not matter — the failure is qi, not skill.
- It must **not** read as a difficulty spike the player should retry. No "you
  died, retry?" — the regression follows immediately.
- The player must have felt **competent** immediately before it. Give them a
  won fight against ordinary opponents first, so the loss lands as a rule
  change and not as a skill gap.

### Every death after

From loop 2, death can happen anywhere. This is a **hard content constraint**:

> Every act must be enterable and completable at Level 1, holding only the
> technique set and knowledge flags the player is expected to have by the time
> they reach that act.

If any act requires a specific cultivation level to be survivable, the design
is broken — because a player who dies at the start of that act arrives back at
it at Level 1 and must be able to climb again. Difficulty is tuned against
**technique tier**, never against character level.

---

## 3. The anchor and how it moves

A fixed anchor forever is thematically pure (the player always returns to the
alley and the bullies) but the replay tax grows without bound — by act 5 the
player replays four acts to reach new content. Unacceptable.

**Design:** the anchor is fixed for Act 1, then **advances at act boundaries**.

- **Act 1's anchor is sacred.** Returning to the alley, to the bullies who
  killed nothing but your dignity, and flattening them with technique you
  earned in a life they know nothing about — that is the game's thesis
  statement. It must be experienced at least once.
- **From Act 2 on, the anchor advances** with the story. Each advance needs an
  in-fiction reason, not a menu.

> **OPEN — anchor advance mechanism.** *What*, in the fiction, moves the
> anchor? Candidate directions:
> - **A cultivation milestone** — forming a core creates a new "fixed point"
>   the soul returns to.
> - **A place or object** — reaching a location or binding to an artefact
>   re-anchors the regression.
> - **Uncontrolled drift** — the anchor slides forward on its own, which is
>   *frightening*: the player loses the ability to undo earlier mistakes.
>
> The third is the most interesting narratively and the most punishing
> mechanically. Tracked in `docs/06-production/open-questions.md`.

---

## 4. The no-wasted-loop rule

> **Every loop must yield at least one piece of durable progress: a knowledge
> flag, a technique, or a path-drift shift.**

This is the most important rule in the document. A loop that ends with the
player holding exactly what they started with is a punishment with no lesson,
and it is where players quit regression games.

Practical consequences:
- Losing a fight should teach something *recorded*, not just something the
  player privately noticed. Dying to an opponent grants knowledge of their
  patterns, their tell, or their weakness as an actual flag.
- Dying while exploring should bank the discovery. Locations reached are known.
- If the player somehow dies having gained nothing, the interlude must grant
  something — even if it is only a fragment of insight.

---

## 5. Act structure

Only Act 1 is fully authored. Later acts are scaffolded from the story
document's stated shape and must be expanded.

### Act 1 — The Wall *(authored)*

| Beat | Content | Loop |
|---|---|---|
| Opening | Orphan. No standing, no protection. Tormented by local bullies. Deliberately powerless | 1 |
| The mentor | An old wandering martial artist, **Core Formation**, offers to teach. Openly says the player lacks talent. Fundamentals only: stances, footwork, taking and throwing a hit | 1 |
| Competence | The player improves through real combat. Handles ordinary opponents. **Cultivation never rises** — they were never taught to refine qi | 1 |
| **The wall** | A genuine cultivator kills them. **Authored death** | 1 |
| **The return** | Regression to the anchor. Every technique intact. The bullies are trivial. The player wins | 2 |
| The second revelation | Skill has a ceiling. Real qi and authentic cultivation methods are required to go further | 2 |

Act 1 delivers both revelations: *abilities persist and the cycle can be
exploited*, then *skill alone caps out*. The hunt for cultivation methods
becomes the engine of the mid-game.

### Act 2 — The Hunt for Qi *(scaffold)*
Acquiring real cultivation methods. First contact with the faction world. The
first genuine breakthrough.

> **TO AUTHOR:** which faction, which method, what the wall is, where the
> anchor advances to.

### Act 3 — The Sects *(scaffold)*
Faction entanglement. Sect techniques. Orthodox and unorthodox roads diverge in
availability, and path drift begins to bite.

> **TO AUTHOR.**

### Act 4 — Divergence *(scaffold)*
Accumulated drift crystallises. Path-specific techniques, allies and enemies.

> **TO AUTHOR.**

### Act 5 — Ascension *(scaffold)*
Martial God or Heavenly Demon, selected by accumulated drift (D-006).

> **TO AUTHOR.**

---

## 6. What each loop must define

When authoring any loop or act, answer all six. This is the checklist for both
human and AI-assisted content generation.

1. **Anchor** — where does the player return to?
2. **Recovery** — what known content is replayed, and how is it compressed?
3. **Frontier** — what is new?
4. **Wall** — what ends this loop, and what does beating it require?
5. **Durable gain** — what does the player keep, win or lose? (no-wasted-loop)
6. **Acknowledgment** — how do the protagonist and world register the loop count?

---

## 7. Systems this document constrains

| System | Constraint imposed |
|---|---|
| Combat | Tuned against technique tier, never character level (D-007) |
| Encounter design | Every act survivable at Level 1 |
| Save system | Must persist loop counter, flags, techniques, drift across deaths |
| Content authoring | Every loop yields durable progress |
| UI | Must communicate what persisted vs. what was lost, every interlude |
| Dialogue | Must vary on loop count and knowledge flags |
