# Vertical Slice

**The one thing to build first.** Nothing else in this repository matters until
this works.

---

## The slice

> **Loop 1 → authored death → Loop 2, and the moment of return feels good.**

That is the whole game in miniature. If returning to the anchor with technique
intact and flattening the people who tormented you does not produce a strong
feeling, no amount of faction content, weapon variety, or realm progression
will save the project.

---

## Scope: in

| Element | Detail |
|---|---|
| **One anchor** | The alley. The bullies. Act 1's fixed anchor |
| **One mentor sequence** | Compressed. Enough to teach fundamentals and establish his ordinariness |
| **One weapon** | Unarmed. No weapon-switching, no styles. This builds the **unarmed base moveset** (D-012), which is the template every later weapon copies — author it as a reusable base, not as a one-off |
| **Three enemy archetypes** | Bully (trivial), competent fighter (real), the cultivator (unwinnable) |
| **Core combat** | Light, heavy, guard, parry, dodge, lock-on. Posture if confirmed |
| **The authored death** | The cultivator. Scripted, unwinnable, legible |
| **Regression Interlude** | Shows what was lost, what was kept, what was learned |
| **The return** | Anchor, Tier I, full technique set. The bullies are trivial |
| **2–4 knowledge flags** | Including `K_CBT_QI_STRIKE` from the authored death |
| **Persistence** | `SoulSave` / `LifeSave` / `WorldSave` split, working and tested |
| **Loop acceleration, minimal** | The mentor sequence compresses on loop 2. Proves the mechanism |

## Scope: out

Explicitly deferred. Do not build these for the slice.

- Acts 2–5, and any faction content
- The other four weapons and all six sect styles
- Cultivation levelling past what Act 1 needs — **the slice may not need
  levelling at all**, since Act 1's premise is a skilled fighter with an empty core
- Pills, breakthroughs, the universal arts
- Path drift (record it; do not act on it)
- The Martial Journal as full UI — a debug list is fine
- Economy and progression tuning (**D-004**)
- Anchor advance — Act 1's anchor is fixed
- Axis-driven dialogue variation — record axis state, vary two or three lines
  at most

> Act 1 is unusually cheap to build because the protagonist has **no
> cultivation** in it. The realm system, pills, breakthroughs and universal arts
> can all wait. Take that gift.

---

## Success criteria

The slice succeeds if a first-time player, with no explanation:

1. **Feels genuinely weak** in the opening, without quitting
2. **Feels competent** by the time they meet the cultivator
3. **Understands the authored death was not their fault** — reads it as a rule
   change, not a skill gap or a difficulty spike
4. **Recognises what they kept** at the moment of return, without a tutorial
   telling them
5. **Wants to fight the bullies immediately**, and enjoys it when they do
6. **Articulates the loop rule unprompted** — "I keep my skills but lose my
   cultivation" — when asked what just happened

Criterion 6 is the real test. If players cannot state the rule after
experiencing it once, the authored death is not doing its job.

---

## What the slice proves

| Question | Answered by |
|---|---|
| Is the core loop fun? | The whole slice |
| Does persistence read clearly? | Criteria 4 and 6 |
| Is the authored death legible, not cheap? | Criterion 3 |
| Does technique-over-cultivation feel good? | Criterion 5 |
| Does the save architecture hold? | Automation tests |
| Is combat readable enough to learn from? | Criterion 2 |

---

## What the slice does *not* prove

Be honest about this — do not over-claim from a successful slice.

- **Whether the replay tax is solved.** Two loops is not enough to feel it.
  That risk only surfaces at loops 5–10 and needs a longer build to test
- **Whether the animation budget is achievable.** One weapon hides the problem
- **Whether drift-driven endings land.** Requires the full campaign
- **Whether axis-driven tone is affordable.** Needs a real script at scale

---

## Decisions gated on the slice

These stay open until it is playable, deliberately:

- Final GAS commitment (`docs/04-technical/technical-design.md` §2)
- Posture as a core system (`docs/03-systems/combat.md`)
- All progression and economy numbers (**D-004**)
- Anchor advance mechanism (`docs/02-loop/loop-architecture.md` §3)

---

## Build order

1. UE5 project skeleton, modules, `CLAUDE.md` build commands
2. Save architecture + persistence automation tests — **before any content**
3. Loop state machine: anchor → death → interlude → anchor
4. Core combat against one enemy archetype
5. The three archetypes, and the authored death encounter
6. Act 1 content: alley, mentor, competence beats
7. Knowledge flags and the debug journal
8. Minimal loop acceleration on the mentor sequence
9. Playtest against the six success criteria

Steps 2 and 3 come before content deliberately. The persistence rules are the
game; building content on an unproven loop means rebuilding the content.
