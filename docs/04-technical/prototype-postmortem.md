# Prototype Postmortem

What was salvaged from the abandoned prototype, what was cut, and why.

**Source:** `docs/legacy/unity-prototype-notes.md` (the original
`ProjectSetup.md`, archived verbatim).

> **Read this first.** The project is built in **Unity 6 / URP** (**D-015**),
> which is the same engine the prototype used. **That does not make the
> prototype's design usable.** This document is a record of *design* decisions,
> not engine ones. Every cut listed below still stands, and
> `docs/legacy/unity-prototype-notes.md` is still superseded.
>
> The engine history, for anyone confused by it: prototype in Unity → design
> reset and a move to UE5 (**D-001**) → back to Unity for hardware reasons
> (**D-015**). The design reset happened at the first step and has never been
> undone.

---

## Context

The original prototype was built in Unity 6 / URP. The design
document for it described a **sandbox brawler test playground**: a scene with
the player, a camera, and respawning target dummies, plus dev cheats for
adjusting levels and Qi on the fly.

For what it was — a combat feel prototype — that was reasonable. It is not a
design document for a linear narrative regression RPG, and much of it was
written by AI assistance during respawn testing rather than as deliberate
design. Two artefacts of that process leaked into the design and became
load-bearing before anyone noticed:

1. The **"reincarnation"** framing (D-005)
2. The **locked constitution choice** at respawn (D-006)

Both are now corrected.

Note also that the repository contains **no gameplay code**. Every script the
archived document references — `CultivationSystem.cs`, `PlayerController.cs`,
`EnemyAI.cs`, `HealthSystem.cs` and the rest — was never committed. There is no
code to port, only design.

---

## Salvaged

Genuinely good work that carries into the current design.

| Element | Where it now lives | Notes |
|---|---|---|
| **Realm tier table** (7 tiers, dual naming) | `docs/00-canon/glossary.md`, `docs/03-systems/cultivation-and-realms.md` | Solid. Adopted almost unchanged |
| **Life-or-death breakthrough** | `docs/03-systems/cultivation-and-realms.md` | Excellent. Turns a progression gate into a dramatic set-piece. Now the *celebrated* breakthrough route |
| **Pill outcome matrix** | `docs/03-systems/cultivation-and-realms.md` | Backlash / implosion / perfect absorption / dissolution. Kept, but alignment now checks **drift**, not a locked constitution |
| **Three universal arts with proficiency locks** | `docs/03-systems/cultivation-and-realms.md` | The clearest expression of "technique persists, cultivation gates". Kept intact |
| **Hiding unearned abilities entirely** | `docs/02-loop/knowledge-as-key.md` | One of the best instincts in the document. Extended to knowledge flags |
| **Core defensive actions** — guard, parry, dodge, lock-on | `docs/03-systems/combat.md` | Kept. 0.25s parry window carried as a starting value |
| **Weapon / style pairing** | `docs/03-systems/combat.md` | Structure kept; content out of scope for the vertical slice |
| **Faction world design** | `docs/05-world/murim-factions.md` | Unchanged and canon |
| **Dual-path realm naming** | `docs/03-systems/paths-and-drift.md` | Now driven by drift bands |

---

## Cut

| Element | Why |
|---|---|
| **Passive evasion (RNG attack negation)** | Hidden dice rolls corrupt the player's ability to build a true model of combat — and that model is the game's primary progression currency. Also contradicts "earned power, not gifted power". Replaced with deterministic evasion: i-frames, mobility, cheaper defensive actions |
| **Suppression & Terror auras** | Level-difference speed scaling (1%–220%) was a sandbox answer to "the dummy might be any level". Authored content places encounters at the intended difficulty. Would make nearly every fight trivial or unreadable |
| **Permanent compounding stat buffs** (+15% HP / +15% damage / +8% speed per upgrade, unbounded across lives) | Directly contradicts **D-007** — persistent power must be technique and knowledge, not multipliers. Twenty upgrades of multiplicative +15% is ~16× HP, and unbounded growth across infinite loops breaks all authored difficulty |
| **MMO grind XP from respawning enemies** | Undermines authored pacing (**D-002**). Content is placed, not farmed |
| **Respawning target dummies** | Prototype scaffolding. May return as an internal training map, never as shipped content |
| **Dev cheats** — level adjust hotkeys, +Qi buttons, instant-defeat button | Prototype tooling. Reintroduce as proper debug console commands, gated out of shipping builds |
| **"Reincarnation Sanctum" shop UI** | Replaced by the Regression Interlude (D-005). The between-lives space shows what was lost, kept and learned — it is not a stat shop |
| **Constitution selection & path lock** | Replaced by cumulative drift (D-006) |
| **Qi → XP refinement button** | Prototype convenience with no in-fiction mechanism. Revisit only if one exists |

---

## Reframed

Kept in spirit, changed in meaning.

| Prototype | Now |
|---|---|
| "Reincarnation" — reborn as the next generation | **Regression** — the same world, rewound (D-005) |
| "Generation cycle" | **Loop** |
| "Reincarnation Sanctum" — spend Qi on permanent buffs | **Regression Interlude** — see what persisted, what was lost, what was learned |
| Level resets to 1, buy permanent multipliers to compensate | Level resets to 1; **technique and knowledge** are what compensate (D-007) |
| Constitution chosen and locked | **Drift**, accumulated and never locked (D-006) |
| Enemy difficulty from level-difference auras | Difficulty from **placed encounters** tuned to technique tier |

---

## Repository history

The repository has been converted in place twice rather than restarted, which
resolved Q-17 and then Q-21.

**First conversion — off the prototype, onto UE5 (D-001), commit `165931d`:**

| Step | Status |
|---|---|
| Delete prototype scaffolding — `Assets/`, `ProjectSettings/`, `Packages/`, `.vscode/`, `RPG Idea.slnx` | **Done** |
| Replace `.gitignore` / `.gitattributes` with UE versions | **Done** |
| Keep `docs/` unchanged | **Done** |
| Create the UE5 project skeleton | **Never completed** — the Editor would not run in 16GB |

**Second conversion — back to Unity (D-015):**

| Step | Status |
|---|---|
| Supersede D-001 with D-015 in the decision log | **Done** |
| Rewrite `technical-design.md` and `bootstrap.md` for Unity | **Done** |
| Replace `.gitignore` / `.gitattributes` with Unity versions, LFS for real binaries only | **Done** |
| Update `CLAUDE.md` engine rules and naming conventions | **Done** |
| Create the Unity project skeleton | **Next** — see `docs/04-technical/bootstrap.md` |
| Record verified build/test commands in `CLAUDE.md` §4 | Part of bootstrap |

Nothing of value was lost in either conversion. The prototype contained no
gameplay code, its design document is archived at
`docs/legacy/unity-prototype-notes.md`, and the full original tree remains
recoverable from git history at commit `cd9ecf6`. The UE5 period produced
documentation only — no engine project was ever created — so the move back to
Unity discards no code either.
