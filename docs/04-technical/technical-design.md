# Technical Design Document

**Engine: Unity 6 LTS, Universal Render Pipeline** (**D-015**). Unreal Engine 5
is abandoned — see `docs/04-technical/prototype-postmortem.md` for the full
engine history and why the archived prototype design is still not canon.

---

## 1. Guiding constraint: this project is AI-assisted

This shapes the architecture more than any other factor.

**AI can read, diff, and edit text. It cannot meaningfully edit a binary
asset.** Unity is unusually good on this axis, and the architecture should
spend that advantage rather than waste it.

| Decision | Rationale |
|---|---|
| **All gameplay logic in C#** | No visual scripting. Unity has no Blueprint-shaped trap here, so the rule is simply: logic lives in `.cs` files, which diff, merge and review cleanly |
| **Force Text serialization, always** | `.unity`, `.prefab` and `.asset` become YAML. Scenes and prefabs are then readable in a diff and mergeable — the single biggest tooling gain over UE5 for this project |
| **Everything data-driven via CSV-backed `ScriptableObject`s** | Frame values, enemy stats, pill tiers, drift sources, knowledge flags, dialogue — all editable as text, all diffable, all AI-accessible. Worth more than any amount of prompt engineering |
| **Tests from day one** | AI-assisted changes need a pass/fail signal, and Unity Test Framework runs headless from the command line. Without a build-and-test loop, AI-assisted work degrades quickly |
| **A `CLAUDE.md` at repo root** | Conventions, build commands, hard rules. Read before every task |
| **Unity MCP for Editor-shaped work** | Unity 6 ships an official MCP server (`com.unity.ai.assistant`). A local AI client can drive scenes, assets and the console directly. Setup is Step 6 of `bootstrap.md` |

> **A caution on YAML editing.** Scenes and prefabs being *readable* is not
> permission to hand-author them. They are graphs of GUIDs and local file IDs,
> and a careless edit corrupts references in ways the Editor reports badly.
> Read them freely, review them in diffs, and make structural changes in the
> Editor.

### Version control

- **Git with LFS**, but only for **genuine binaries** — textures, FBX, audio,
  video. Scenes, prefabs, ScriptableObjects, `.inputactions` and `.meta` files
  stay as diffable text and must **not** go through LFS
- Set **Asset Serialization Mode: Force Text** and **Visible Meta Files** in
  Project Settings before the first asset is committed. Retrofitting either is
  painful
- Configure **UnityYAMLMerge** as the merge driver for `.unity` and `.prefab`.
  Unity ships it; it turns scene conflicts from unresolvable into routine
- Commit `.meta` files always. A missing `.meta` silently breaks references

---

## 2. Ability and attribute architecture

Unity has no Gameplay Ability System. The UE5-era recommendation to adopt GAS
is **void** (**D-015**), and with it the whole "adopt or defer" framing.

What survives is the fallback that document already described, which now
becomes the primary plan: **a thin, data-driven attribute and ability layer,
hand-rolled and owned by this project.**

| Design element | Construct |
|---|---|
| HP, Qi, posture, max HP | Named values on an `AttributeSet` component |
| Pill effects, backlash, implosion, breakthrough heals | `EffectDefinition` ScriptableObjects, applied through one pipeline |
| Realm tiers, drift bands, stance states, path alignment | String-keyed **tags** on a tag component |
| Gyeonggong, Geomgi, Oegong, parry, dodge, sect styles | `AbilityDefinition` ScriptableObjects + a runtime instance |
| Proficiency locks ("Tier IV to channel Geomgi") | Declarative tag requirements on the ability, checked by the activation path |
| Meditation, temporary boosts | Duration and periodic effects through the same pipeline |

Hand-rolling this is a far smaller undertaking in Unity than it sounds, because
the scope is fixed by the design: one player, placed encounters, no networking,
no prediction. The parts of GAS that are genuinely hard to replace —
replication and client-side prediction — are exactly the parts this game does
not need.

**Requirements on whatever gets built:**
- Proficiency locks stay **declarative**. The moment gating becomes a pile of
  `if` statements in ability code, the system has failed its one job
- Effects go through a single application path, so posture, pills and
  breakthroughs cannot each invent their own rules
- Attributes are named and enumerable, so the debug overlay and the persistence
  layer can both walk them generically

> **OPEN:** whether to take a third-party ability-system package as a
> dependency instead of hand-rolling. Decide once the slice's combat exists and
> the real shape of the requirement is known — not before. Tracked as **Q-05**
> in `docs/06-production/open-questions.md`.

---

## 3. Save architecture

The most design-critical technical decision, because persistence *is* the game.
**Engine-neutral, and unchanged by the move to Unity.**

Structure the save to **match the persistence matrix**
(`docs/02-loop/persistence-matrix.md`), so the rules are enforced structurally
rather than by discipline:

| Save object | Contains | On death |
|---|---|---|
| **`SoulSave`** | Techniques, proficiency, knowledge flags, drift, jadedness, confidence floor, loop counter, anchor, act progress | **Kept** |
| **`LifeSave`** | Cultivation tier, cultivation progress, HP, Qi, inventory, equipment, current-loop confidence | **Discarded** |
| **`WorldSave`** | NPC state, quest flags, world switches, time | **Restored from anchor snapshot** |

A death becomes a single clean operation:

```
discard LifeSave
restore WorldSave from anchor snapshot
keep SoulSave
increment loop counter
apply jadedness tick
```

**Requirements:**
- **JSON on disk**, under `Application.persistentDataPath`. Use Newtonsoft
  JSON, not `JsonUtility` — the latter cannot serialise dictionaries,
  polymorphism or nulls, and all three appear in these objects
- Save **versioning from the first commit**, with a migration path. Persistence
  is the core mechanic; a save-breaking patch is a project-ending bug
- Anchor snapshots must be cheap to take and cheap to restore
- `SoulSave` writes **on gain**, not only on death — a crash must never cost
  knowledge
- Any new system declares its save object before implementation

---

## 4. Assembly layout

Unity's equivalent of modules is the **assembly definition** (`.asmdef`). The
layout mirrors the UE module plan, with the same dependency rule:

```
Assets/Regressor/Scripts/
├── Core/          Regressor.Core         Loop state machine, save objects, persistence
├── Combat/        Regressor.Combat       Attributes, abilities, damage, posture
├── Cultivation/   Regressor.Cultivation  Realms, breakthroughs, pills, universal arts
├── Knowledge/     Regressor.Knowledge    Knowledge flags, journal, gating queries
├── Narrative/     Regressor.Narrative    Dialogue, axis-driven tone selection, drift
├── AI/            Regressor.AI           Enemy behaviour
└── Editor/        Regressor.Editor       Tooling, validation, table checks
```

`Regressor.Core` owns the loop and the persistence contract. Every other
assembly depends on it, and **none of them may write persistent state without
going through it.** Assembly definitions enforce this at compile time, which is
stronger than the UE module boundary was.

As with the UE plan: **do not create all seven up front.** `Regressor.Core`
first, the rest when there is code for them.

---

## 5. Reading the older documents

Git history, the postmortem, and any UE-era note use Unreal vocabulary. The
mapping, for translation only:

| UE5 | Unity |
|---|---|
| `Source/`, `AActor` / `UActorComponent` | `Assets/Regressor/Scripts/`, `MonoBehaviour` |
| Module | Assembly definition (`.asmdef`) |
| Enhanced Input, `IA_` / `IMC_` | Input System package, `.inputactions` asset |
| `ACharacter` + `UCharacterMovementComponent` | `CharacterController` or Rigidbody controller |
| `UDataTable` / `UPrimaryDataAsset` | CSV-backed `ScriptableObject` |
| Gameplay Tags | String-keyed tag component (hand-rolled, §2) |
| Gameplay Ability System | No equivalent — hand-rolled ability layer (§2) |
| Animation Blueprint state machine | Animator Controller, humanoid retargeting |
| `USpringArmComponent` + `UCameraComponent` | Cinemachine |
| StateTree | Unity **Behavior** package, or a hand-rolled FSM |
| Blueprint class | Prefab |
| `.uasset` / `.umap` *(binary)* | `.asset` / `.unity` *(YAML text)* |
| Automation tests | Unity Test Framework (NUnit), EditMode + PlayMode |

---

## 6. Tuning tables

All tuning lives here. **CSV is the source of truth**, committed as text at the
repo root, imported into ScriptableObjects. When a value changes, the **CSV** is
edited and re-imported — never the `.asset` directly.

| Table | Contents |
|---|---|
| `Techniques` | Movelist entries, proficiency curves, path requirements |
| `KnowledgeFlags` | Flag catalogue — see `docs/02-loop/knowledge-as-key.md` |
| `Realms` | Tier boundaries, names per path, proficiency gates |
| `PillEffects` | Tier, alignment, outcome matrix |
| `DriftSources` | Actions and their drift deltas |
| `Encounters` | Placed enemies, technique-tier budget |
| `CultivationCurve` | Progression curves *(deferred, D-004)* |
| `DialogueVariants` | Beat text keyed by axis quadrant |

**Rule:** if a designer or an AI assistant would want to change a number, it
belongs in a table, not in C#.

---

## 7. Testing

Unity Test Framework, **runnable headless from one command** — this is the
build-and-test signal `CLAUDE.md` §4 exists to record.

- **Persistence tests above all:** a simulated death must provably keep
  everything marked ● and clear everything marked ○ in the persistence matrix.
  This is the single most valuable test in the project, and it is a pure
  **EditMode** test — no scene, no play loop, fast enough to run on every change
- Loop state machine transition tests (EditMode)
- Table validation in `Regressor.Editor` — every referenced flag exists, every
  technique has a valid path requirement, no orphan rows
- PlayMode tests only where a test genuinely needs the engine running

---

## 8. Repository state

> The repository contains `docs/`, `CLAUDE.md`, and Unity-appropriate
> `.gitignore` / `.gitattributes`. **No Unity project exists yet** — creating
> it is the next task.
>
> Follow `docs/04-technical/bootstrap.md`. Unlike the UE5 attempt, this one is
> within reach of the development machine, and much of it can be done from a
> terminal.
