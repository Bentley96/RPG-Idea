# CLAUDE.md — AI Working Agreement

Read this before any task in this repository.

**Project:** *The Regressor's Path* — a linear, narrative, regression-driven
murim RPG for **Unreal Engine 5**.

---

## 1. Canon and precedence

Design documentation lives in `docs/`. Start at
[`docs/README.md`](docs/README.md).

**Precedence, highest first:**

1. `docs/00-canon/decision-log.md` — locked decisions. **If any document
   contradicts the log, the log wins and the document is wrong**
2. `docs/00-canon/glossary.md` — canonical terminology
3. `docs/02-loop/persistence-matrix.md` — binding for anything touching state
4. Everything else in `docs/`
5. `docs/legacy/` — **superseded. Never build from it**

---

## 2. Hard rules

These are not preferences.

**Terminology**
- Use **"regression"**, never "reincarnation", "rebirth", or "respawn" for the
  player. See the banned-terms list in `docs/00-canon/glossary.md`
- "Loop", never "generation" or "cycle"
- **Never say "level".** There are two ladders (D-008): **martial rank**
  (삼류 → 자연경, persists) and **cultivation tier** (I–VIII, resets). "Level"
  hides which one is meant
- **Unorthodox (사파) is a distinct path**, never a weaker Demonic and never
  mere neutrality (D-009)

**Design**
- **Technique carries the fight; cultivation carries the ceiling** (D-007).
  Never propose persistent stat multipliers as progression
- **The path is cumulative drift, never a choice** (D-006, D-009). Never write
  a menu, prompt, or dialogue wheel that asks the player to pick a path.
  Three paths, tracked by **drift** (which road) and **conviction** (how hard
  they committed)
- **Anchor advance is player-chosen and irreversible** (D-010). It must never
  make the game unwinnable — every anchor leaves the rest completable
- **A testament is taken at every act close** (D-013): automatic, permanent,
  never a player choice. It **records** path standing; it does not constrain
  future drift. The ending reads the whole trajectory. Do not conflate the
  testament (automatic) with the anchor advance (chosen) — they fire at the
  same moment and are separate systems
- **Path sets diction and register, not just sentiment** (D-011). Voice is
  three orthogonal layers: register (path), affect (jadedness), stance
  (confidence). Never author a line per combination
- **No hidden randomness in combat outcomes** (D-014, and the passive-evasion
  cut). Systems may be hidden from the UI; their **rules** must stay learnable.
  Hidden is not the same as random
- **Every act must be completable at Tier I** with the expected technique set
- **Every loop must yield durable progress** — a knowledge flag, a technique,
  or a drift shift
- **Any new system must declare its row in the persistence matrix before it is
  implemented.** A system with undefined persistence behaviour must not be built
- **One base moveset per weapon; styles layer on top at ≤5 clips each**
  (D-012). A concept needing a full moveset is a weapon, not a style. Enemy
  archetypes reuse the weapon bases
- Do not invent progression numbers. Economy work is deferred (D-004)

**Engine**
- This is a **UE5** project. The archived design document describes Unity — do
  not follow it, and do not generate Unity C#
- **C++ first.** Gameplay logic goes in C++, not Blueprint graphs. Blueprint is
  for thin glue and designer tuning only
- **Never hand-edit `.uasset` or `.umap`.** They are binary and unmergeable
- **All tuning lives in CSV-backed DataTables**, never hardcoded. If a designer
  would want to change a number, it belongs in a DataTable

---

## 3. Repository state

> **The Unity project has been removed.** The repo now contains only `docs/`,
> `CLAUDE.md`, and UE-appropriate `.gitignore` / `.gitattributes` with Git LFS
> configured for `.uasset`, `.umap` and binary art.
>
> **The UE5 project does not exist yet.** Creating it is the next task, and it
> must be done on a machine with the Editor installed — a `.uproject` and its
> module scaffolding cannot be generated from a terminal alone.
>
> **Follow `docs/04-technical/bootstrap.md`.** It is a step-by-step runbook
> with a definition of done and an explicit out-of-scope list.

The archived Unity design document remains at `docs/legacy/` for reference
only. What was salvaged and what was cut is recorded in
`docs/04-technical/migration-from-unity.md`.

---

## 4. Build & test

> **Not yet available.** No UE5 project exists.
>
> Fill this section in as soon as the project builds. It must contain: the
> build command, the automation-test command, and how to run the editor. AI
> assistance without a build-and-test signal degrades quickly — this is a
> priority, not paperwork.

Planned, per `docs/04-technical/technical-design.md` §7:
- Persistence automation tests are the highest-value tests in the project: a
  simulated death must provably keep everything marked ● and clear everything
  marked ○ in the persistence matrix
- DataTable validation runs in `RegressorEditor`
- Build + test runnable from a single command

---

## 5. Naming conventions

**C++ / UE:** standard Unreal prefixes — `A` actors, `U` UObjects, `F` structs,
`E` enums, `I` interfaces.

**Modules:** `Regressor<Area>` — see `docs/04-technical/technical-design.md` §4.

**Assets:**

| Prefix | Type |
|---|---|
| `BP_` | Blueprint |
| `IA_` / `IMC_` | Enhanced Input action / mapping context |
| `GA_` / `GE_` / `GC_` | Gameplay ability / effect / cue |
| `DT_` | DataTable |
| `DA_` | Data asset |
| `AM_` / `ABP_` | Anim montage / Anim Blueprint |

**Knowledge flags:** `K_<TYPE>_<NAME>` — e.g. `K_LOC_SPIRIT_SPRING`. Types are
in `docs/02-loop/knowledge-as-key.md`.

---

## 6. Working style

- **Ask before inventing canon.** If a design answer is missing, check
  `docs/06-production/open-questions.md` first. If it is an open question, do
  not silently pick an answer — flag it
- **Mark uncertainty explicitly.** Use `> **OPEN:**` callouts in documents and
  add an entry to `open-questions.md`
- **Prefer editing existing documents** over creating new ones
- **When a decision gets made, record it** in `docs/00-canon/decision-log.md`
  and remove it from `open-questions.md`
- Keep scope tight to the vertical slice
  (`docs/06-production/vertical-slice.md`). It is Act 1, unarmed, three enemy
  archetypes. Do not build Acts 2 and 3 content, weapons, styles, pills or
  breakthroughs into it

---

## 7. Current priorities

1. Create the UE5 project skeleton — **follow `docs/04-technical/bootstrap.md`**
   — and fill in §4 above with verified commands
2. Save architecture + persistence tests — **before any content**
3. Loop state machine: anchor → death → interlude → anchor
4. Vertical slice Act 1 content

Blocking design questions are Q-02 and Q-04 in
`docs/06-production/open-questions.md`.
