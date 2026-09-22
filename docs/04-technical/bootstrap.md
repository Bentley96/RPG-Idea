# Bootstrap Runbook — Creating the Unity Project

**Audience:** whoever creates the Unity project, human or AI-assisted, on a
machine with Unity installed.

**Read first:** `CLAUDE.md` at the repo root, then
`docs/04-technical/technical-design.md`. This runbook assumes both.

**Goal:** a project that opens, runs, has one greybox level you can walk
around in, and has its build and test commands recorded. Nothing more.

> **This replaces the UE5 bootstrap runbook**, which was never completed — the
> UE5 Editor is not workable in this machine's 16GB of RAM (**D-015**). Unity
> is, and a much larger share of what follows can be done from a terminal
> rather than by hand in the Editor.

---

## Prerequisites

| | |
|---|---|
| Unity | **Unity 6 LTS** (6000.x). Install via Unity Hub. Record the exact version in `CLAUDE.md` §4 |
| Render pipeline | **URP.** Not HDRP — it is heavier than this machine or this art direction wants |
| IDE | Visual Studio, Rider, or VS Code with the C# extension |
| Git LFS | `git lfs install` — run once per machine |
| This repo | Cloned, on `main` |

> **Keep the install lean.** On 16GB, the Editor's footprint is the budget that
> matters. Install one Editor version, skip build-support modules you are not
> targeting, and do not import large sample or demo content.

---

## Step 1 — Confirm the repo is clean

The UE5-era `.gitignore` / `.gitattributes` have been replaced with Unity
versions. The repo should contain only:

```
.gitignore   .gitattributes   CLAUDE.md   docs/
```

If `Source/`, `Content/` or a `.uproject` are present, the pull is stale.

Run `git lfs install` before adding any binary assets.

---

## Step 2 — Create the project

**Template: Universal 3D.** Create it **at the repo root**, so `Assets/` sits
beside `docs/`.

Via Unity Hub: New Project → Universal 3D → set the location to the repo root
and the name so that it resolves there. From the command line:

```
Unity.exe -createProject "<REPO>" -batchmode -quit
```

Then add **Starter Assets: ThirdPerson** from the Package Manager
(Unity Registry, free, published by Unity).

### Why Starter Assets and not an empty scene

It supplies a working third-person `CharacterController`, a **Cinemachine**
camera rig, and the **Input System** already wired with an `.inputactions`
asset and a generated input class. That is several days of scaffolding this
game needs anyway, and its capsule/armature is a serviceable placeholder for
the unarmed base moveset (**D-012**) later.

This is the same reasoning the UE5 runbook used for the Third Person template.
Building it from scratch would mean rebuilding all of it for no benefit.

> The starter character class will be renamed and moved during later work. Do
> not restructure it in this step — get it opening and running first.

---

## Step 3 — Project settings that must be set before the first asset

These three are painful to retrofit. Set them now.

| Setting | Value | Where |
|---|---|---|
| **Asset Serialization** | **Force Text** | Project Settings → Editor |
| **Visible Meta Files** | On | Project Settings → Editor |
| **Company / Product Name** | Your choice / `Regressor` | Project Settings → Player |

Then configure **UnityYAMLMerge** as the git merge driver for `.unity` and
`.prefab`. Unity ships the tool with the Editor; wiring it up now turns scene
conflicts from unresolvable into routine.

Add the **Newtonsoft JSON** package (`com.unity.nuget.newtonsoft-json`) — the
save layer needs it (`technical-design.md` §3).

---

## Step 4 — Assemblies and folder layout

Everything under a single project root folder, so package and asset-store
content can never collide with ours:

```
Assets/Regressor/
├── Scripts/
│   ├── Core/        <- Regressor.Core.asmdef
│   └── Editor/      <- Regressor.Editor.asmdef
├── Prefabs/
├── Scenes/
├── Data/            <- imported ScriptableObjects
├── Input/
├── UI/
├── VFX/
└── Art/
```

Create **two** assembly definitions now — `Regressor.Core` and
`Regressor.Editor` — not all seven from the TDD. `Regressor.Core` comes first
because the save architecture is the next priority and it owns the persistence
contract every other assembly depends on. Add the rest when there is code for
them, not before.

Move the Starter Assets content into `Assets/Regressor/` where it makes sense,
or leave it in place for now and move it when the character work starts —
either is fine, but decide and be consistent.

### CSV source of truth

Tuning tables are **imported from CSVs that live outside `Assets/`**, as text:

```
Data/CSV/          <- repo root. The source of truth. Diffable, AI-editable
   KnowledgeFlags.csv
   Techniques.csv
   ...
```

Import these into `Assets/Regressor/Data/` as ScriptableObjects. When a value
changes, the **CSV** is edited and re-imported — never the `.asset` directly.
This is what makes tuning reviewable and AI-assistable (`CLAUDE.md`, Engine
rules).

Create `Data/CSV/` now, even if empty, with a `.gitkeep`.

---

## Step 5 — Record build and test commands

**This is the most important step in the runbook.** `CLAUDE.md` §4 is
currently a placeholder. Fill it in with the real, verified commands for this
machine. AI assistance without a build-and-test signal degrades quickly.

Templates — substitute the real Editor path and **verify each one actually
runs** before recording it:

**Run EditMode tests headless:**
```
"<UNITY>\Unity.exe" -batchmode -projectPath "<REPO>" -runTests -testPlatform EditMode -testResults "<REPO>\TestResults.xml" -logFile -
```

**Run PlayMode tests headless:**
```
"<UNITY>\Unity.exe" -batchmode -projectPath "<REPO>" -runTests -testPlatform PlayMode -testResults "<REPO>\TestResults.xml" -logFile -
```

**Build a player** (requires a small `BuildScript` with a static method):
```
"<UNITY>\Unity.exe" -batchmode -quit -projectPath "<REPO>" -executeMethod Regressor.Editor.BuildScript.Build -logFile -
```

**Open the editor:**
```
"<UNITY>\Unity.exe" -projectPath "<REPO>"
```

Where `<UNITY>` is typically
`C:\Program Files\Unity\Hub\Editor\<version>\Editor`.

`-runTests` exits on its own; do not combine it with `-quit`. Note that
batchmode holds the project lock, so the Editor cannot be open at the same
time.

Replace the whole placeholder block in `CLAUDE.md` §4 with the verified
versions, and update §3 to say the Unity project now exists.

---

## Step 6 — First level: the alley

**`Assets/Regressor/Scenes/Alley_Anchor.unity`**

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
| Spawn point | At the open end |
| Lighting | One Directional Light plus the URP default sky. Keep it simple |

### Low-poly look, cheaply

Make one **flat-colour URP material** with a `BaseColor` property, then create
material variants per surface — walls, ground, crates. Flat shading over simple
geometry reads as a deliberate low-poly style rather than as unfinished
greybox, and it costs almost nothing.

### Build it from a script, not by hand

> **This is where Unity pays off over UE5.** `.unity` is YAML, not a binary
> blob, so a blockout is reviewable in a diff. Better still, write a small
> **editor script** — a `[MenuItem]` in `Regressor.Editor` that places the
> geometry — and check *that* in. The script becomes the source of truth, the
> scene becomes a regenerable artefact, and the blockout can be edited,
> reviewed and improved from a terminal by AI assistance without ever opening
> the Editor.
>
> Building it by hand in the Editor is perfectly acceptable for a first pass.
> But the script route is cheap here in a way it never was in Unreal, and it is
> the recommended one.

### Verification

Enter Play mode, walk around the alley with the starter character, confirm
collision works and nothing falls through the floor. That is the whole bar for
this step.

---

## Step 7 — Commit

```
git add -A
git commit -m "Add Unity project skeleton and alley blockout"
git push -u origin main
```

Check that LFS caught only what it should: `git lfs ls-files` should list
textures, models and audio — and should **not** list `.unity`, `.prefab`,
`.asset` or `.cs`. If scenes are in LFS, `.gitattributes` is wrong; fix it
before pushing, because retrofitting LFS is painful.

Confirm `.meta` files are committed. A missing `.meta` silently breaks asset
references for everyone but you.

---

## Definition of done

- [ ] Project opens in the Editor without errors
- [ ] EditMode tests run headless from the command line
- [ ] `Alley_Anchor` is walkable, collision works
- [ ] `Assets/Regressor/` layout in place
- [ ] `Regressor.Core` and `Regressor.Editor` assemblies exist and compile
- [ ] `Data/CSV/` exists
- [ ] Asset Serialization is **Force Text**; UnityYAMLMerge configured
- [ ] `CLAUDE.md` §4 has **verified** build, test and editor commands
- [ ] `CLAUDE.md` §3 updated — the project now exists
- [ ] `git lfs ls-files` lists binaries only, no scenes or scripts
- [ ] Pushed to `main`

---

## Explicitly out of scope

Do **not** build these during bootstrap. Each is gated or sequenced elsewhere.

| Not now | Why |
|---|---|
| The ability and attribute layer | Gated on the vertical slice (Q-05) |
| Combat, posture, parry | Comes after the save architecture |
| Save/persistence code | The next task, not this one |
| The other five assemblies | Add when there is code for them |
| Enemies, AI, behaviour graphs | Later in the slice |
| Weapons, styles, pills, breakthroughs | Not in the slice at all (`CLAUDE.md` §6) |
| Art, animation, audio passes | Blockout only |

---

## What comes next

Per `CLAUDE.md` §7, once this runbook is complete:

1. **Save architecture** — `SoulSave` / `LifeSave` / `WorldSave`, shaped to
   match `docs/02-loop/persistence-matrix.md`
2. **Persistence tests** — a simulated death must provably keep everything
   marked ● and clear everything marked ○. The highest-value test in the
   project, and a pure EditMode test that needs no scene
3. **Loop state machine** — anchor → death → interlude → anchor

Steps 1 and 2 come before any content. The persistence rules *are* the game;
building content on an unproven loop means rebuilding the content.

All three are pure C#, which means they can be written, tested and reviewed
without opening the Editor.
