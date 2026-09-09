# Bootstrap Runbook — Creating the UE5 Project

**Audience:** whoever creates the Unreal project, human or AI-assisted, on a
machine with UE5 and the Editor installed.

**Read first:** `CLAUDE.md` at the repo root, then
`docs/04-technical/technical-design.md`. This runbook assumes both.

**Goal:** a project that builds, runs, has one greybox level you can walk
around in, and has its build and test commands recorded. Nothing more.

---

## Prerequisites

| | |
|---|---|
| Unreal Engine | Whatever version is installed. **5.5+** preferred (StateTree). Record the exact version in `CLAUDE.md` §4 |
| Visual Studio / Xcode | With C++ game development workload |
| Git LFS | `git lfs install` — run once per machine |
| This repo | Cloned, on `main` |

---

## Step 1 — Confirm the repo is clean

The Unity scaffolding has already been removed and UE-appropriate
`.gitignore` / `.gitattributes` are committed. The repo should contain only:

```
.gitignore   .gitattributes   CLAUDE.md   docs/
```

If `Assets/`, `ProjectSettings/` or `Packages/` are present, the pull is stale.

Run `git lfs install` before adding any binary assets.

---

## Step 2 — Create the project

**Template: Third Person, C++, no Starter Content.**

| Setting | Value |
|---|---|
| Template | **Third Person** |
| Implementation | **C++** (not Blueprint) |
| Target platform | Desktop |
| Quality | Maximum |
| Starter Content | **No** |
| Raytracing | Off |
| **Project name** | `Regressor` |
| **Location** | The repo root, so `Regressor.uproject` sits beside `docs/` |

### Why Third Person and not Blank

The template supplies a working `ACharacter` with movement, a spring-arm
camera, and **Enhanced Input already wired** (`IMC_Default`, `IA_Move`,
`IA_Look`, `IA_Jump`), plus the Manny/Quinn mannequin and its animation set.
That is several days of scaffolding that matches what this game needs anyway,
and the mannequin is a serviceable placeholder for the unarmed base moveset
(**D-012**) later.

Blank would mean rebuilding all of it for no benefit.

> The template's character class will be renamed and moved during later work.
> Do not restructure it in this step — get it building and running first.

---

## Step 3 — Modules

The TDD (`technical-design.md` §4) lists seven modules. **Do not create all
seven now.** Empty modules are overhead and obscure where code actually lives.

Create **two**:

| Module | Purpose |
|---|---|
| `Regressor` | Primary game module. Created by the template |
| `RegressorCore` | Loop state machine, save objects, persistence rules |

`RegressorCore` comes first because priority 2 is the save architecture, and
because it owns the persistence contract every other module depends on.

Add the remaining modules when there is code for them, not before.

---

## Step 4 — Content layout

Everything under a single project root folder, so marketplace and plugin
content can never collide with ours:

```
Content/
└── Regressor/
    ├── Characters/
    ├── Input/
    ├── Levels/
    ├── Data/          <- imported DataTables
    ├── UI/
    └── VFX/
```

Move the template's assets into `Content/Regressor/` and fix up redirectors.

### CSV source of truth

DataTables are **imported from CSVs that live outside `Content/`**, as text:

```
Data/CSV/          <- repo root. The source of truth. Diffable, AI-editable
   DT_KnowledgeFlags.csv
   DT_Techniques.csv
   ...
```

Import these into `Content/Regressor/Data/`. When a value changes, the **CSV**
is edited and re-imported — never the `.uasset` directly. This is what makes
tuning reviewable and AI-assistable (`CLAUDE.md`, Engine rules).

Create `Data/CSV/` now, even if empty, with a `.gitkeep`.

---

## Step 5 — Record build and test commands

**This is the most important step in the runbook.** `CLAUDE.md` §4 is
currently a placeholder. Fill it in with the real, verified commands for this
machine. AI assistance without a build-and-test signal degrades quickly.

Templates — substitute the real engine path and verify each one actually runs:

**Build (Windows):**
```
"<UE_ROOT>\Engine\Build\BatchFiles\Build.bat" RegressorEditor Win64 Development -Project="<REPO>\Regressor.uproject" -WaitMutex -FromMsBuild
```

**Regenerate project files:**
```
"<UE_ROOT>\Engine\Binaries\DotNET\UnrealBuildTool\UnrealBuildTool.exe" -projectfiles -project="<REPO>\Regressor.uproject" -game -rocket -progress
```

**Run automation tests headless:**
```
"<UE_ROOT>\Engine\Binaries\Win64\UnrealEditor-Cmd.exe" "<REPO>\Regressor.uproject" -ExecCmds="Automation RunTests Regressor;Quit" -unattended -nopause -nosplash -testexit="Automation Test Queue Empty" -log
```

**Open the editor:**
```
"<UE_ROOT>\Engine\Binaries\Win64\UnrealEditor.exe" "<REPO>\Regressor.uproject"
```

Replace the whole placeholder block in `CLAUDE.md` §4 with the verified
versions, and update §3 to say the UE5 project now exists.

---

## Step 6 — First level: the alley

**`Content/Regressor/Levels/L_Alley_Anchor`**

Not a generic test box. This is **Act 1's anchor** — the alley where the
bullies are, the place every regression returns to, and the location the whole
game is built around (`docs/02-loop/loop-architecture.md` §5). Building it
first is both practically and thematically correct.

### Greybox scope

Blockout only. Twenty minutes of dragging shapes, not an art pass.

| Element | Notes |
|---|---|
| Ground | Scaled plane or cube |
| Two long walls | Forming a narrow alley — the space should feel confined |
| Dead end | One end closed. The orphan has nowhere to run |
| Crates / clutter | A handful of cubes for cover and scale reference |
| PlayerStart | At the open end |
| Lighting | Directional Light + SkyAtmosphere + SkyLight. Keep it simple |

### Low-poly look, cheaply

Make one **flat-colour master material** (an unlit or minimally-lit constant
with a `BaseColour` vector parameter), then create material instances per
surface — walls, ground, crates. Flat shading over simple geometry reads as a
deliberate low-poly style rather than as unfinished greybox, and it costs
almost nothing.

### Verification

Launch, walk around the alley with the template character, confirm collision
works and nothing falls through the floor. That is the whole bar for this step.

> **Note for AI assistance:** `.umap` and `.uasset` are binary and cannot be
> authored from a terminal. The level must be built in the Editor by hand.
> If repeatable, diffable blockouts become useful later, enable the **Python
> Editor Script Plugin** and generate levels from a checked-in `.py` script —
> the script becomes the source of truth and the `.umap` a build artefact.
> Not needed for the first scene.

---

## Step 7 — Commit

```
git add -A
git commit -m "Add UE5 project skeleton and alley blockout"
git push -u origin main
```

Verify LFS caught the binaries: `git lfs ls-files` should list the `.umap` and
any `.uasset` files. If it is empty, `.gitattributes` was not applied before
the add — fix it before pushing, because retrofitting LFS is painful.

---

## Definition of done

- [ ] Project builds from the command line, not just the Editor
- [ ] Editor opens the project without errors
- [ ] `L_Alley_Anchor` is walkable, collision works
- [ ] `Content/Regressor/` layout in place, template assets moved
- [ ] `Data/CSV/` exists
- [ ] `RegressorCore` module exists and compiles, even if empty
- [ ] `CLAUDE.md` §4 has **verified** build, test and editor commands
- [ ] `CLAUDE.md` §3 updated — the project now exists
- [ ] `git lfs ls-files` lists binary assets
- [ ] Pushed to `main`

---

## Explicitly out of scope

Do **not** build these during bootstrap. Each is gated or sequenced elsewhere.

| Not now | Why |
|---|---|
| GAS setup | Gated on the vertical slice (Q-05) |
| Combat, posture, parry | Comes after the save architecture |
| Save/persistence code | The next task, not this one |
| The other five modules | Add when there is code for them |
| Enemies, AI, StateTree | Later in the slice |
| Weapons, styles, pills, breakthroughs | Not in the slice at all (`CLAUDE.md` §6) |
| Art, animation, audio passes | Blockout only |

---

## What comes next

Per `CLAUDE.md` §7, once this runbook is complete:

1. **Save architecture** — `SoulSave` / `LifeSave` / `WorldSave`, shaped to
   match `docs/02-loop/persistence-matrix.md`
2. **Persistence automation tests** — a simulated death must provably keep
   everything marked ● and clear everything marked ○. The highest-value test
   in the project
3. **Loop state machine** — anchor → death → interlude → anchor

Steps 1 and 2 come before any content. The persistence rules *are* the game;
building content on an unproven loop means rebuilding the content.
