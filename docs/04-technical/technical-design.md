# Technical Design Document

**Engine: Unreal Engine 5** (**D-001**). The Unity 6 / URP project is
abandoned — see `docs/04-technical/migration-from-unity.md`.

---

## 1. Guiding constraint: this project is AI-assisted

This shapes the architecture more than any other factor, and it produces
recommendations that differ from a conventional UE5 project.

**AI can read, diff, and edit text. It cannot meaningfully edit a `.uasset`.**

| Decision | Rationale |
|---|---|
| **C++ first, Blueprint as thin glue** | Gameplay logic in Blueprint graphs is invisible to AI assistance and unmergeable in version control. Blueprint is for designer-facing tuning and simple actor composition only |
| **Everything data-driven via CSV-backed DataTables** | Frame values, enemy stats, pill tiers, drift sources, knowledge flags, dialogue — all editable as text, all diffable, all AI-accessible. This single decision is worth more than any amount of prompt engineering |
| **Text-friendly formats wherever a choice exists** | Prefer CSV/JSON over binary data assets when both work |
| **Automation tests from day one** | AI-assisted changes need a pass/fail signal. Without a build-and-test loop, AI-assisted UE5 work degrades quickly |
| **A `CLAUDE.md` at repo root** | Conventions, build commands, hard rules. Read before every task |

### Version control

- **Git with LFS.** `.uasset`, `.umap`, and all binary art through LFS
- The current Unity `.gitattributes` and `.gitignore` must be **replaced** with
  UE equivalents
- **Blueprints do not merge.** Two people (or two branches) editing the same
  Blueprint means one loses. Keep logic in C++; treat Blueprint files as
  exclusively-owned
- Consider `Content/` asset locking discipline even on a solo project — it
  builds the right habits and prevents self-inflicted merge losses

---

## 2. The GAS decision

**Recommendation: adopt the Gameplay Ability System, with a migration-safe
fallback.**

The design maps onto GAS almost one-to-one:

| Design element | GAS construct |
|---|---|
| HP, Qi, posture, max HP | `Attributes` on an `AttributeSet` |
| Pill effects, backlash, implosion, breakthrough heals | `GameplayEffect` |
| Realm tiers, drift bands, stance states, path alignment | `GameplayTag` |
| Gyeonggong, Geomgi, Oegong, parry, dodge, sect styles | `GameplayAbility` |
| Proficiency locks ("Tier IV to channel Geomgi") | Ability tag requirements — declarative, no branching code |
| Meditation, temporary boosts | Duration/periodic `GameplayEffect` |

The proficiency-lock system in particular is *exactly* what GAS tag
requirements exist for, and hand-rolling it produces a mess of conditionals
that GAS expresses as data.

**The counter-argument** is real: GAS has a steep learning curve, verbose
boilerplate, and painful debugging, and it is a heavy commitment for a first
UE5 project. Retrofitting it later, however, is a rewrite.

**Recommended path:** build the vertical slice's combat behind a thin attribute
component *shaped* like GAS — attributes as named values, effects as data rows,
gates as tags — so that if GAS is adopted the migration is mechanical, and if
it is rejected nothing is lost. Decide for real once the slice is playable.

> **OPEN:** final GAS commit, at end of vertical slice. Tracked in
> `docs/06-production/open-questions.md`.

---

## 3. Save architecture

The most design-critical technical decision, because persistence *is* the game.

Structure the save to **match the persistence matrix**
(`docs/02-loop/persistence-matrix.md`), so the rules are enforced structurally
rather than by discipline:

| Save object | Contains | On death |
|---|---|---|
| **`SoulSave`** | Techniques, proficiency, knowledge flags, drift, jadedness, confidence floor, loop counter, anchor, act progress | **Kept** |
| **`LifeSave`** | Level, XP, HP, Qi, inventory, equipment, current-loop confidence | **Discarded** |
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
- Save versioning from the first commit. Persistence is the core mechanic;
  a save-breaking patch is a project-ending bug
- Anchor snapshots must be cheap to take and cheap to restore
- `SoulSave` writes on gain, not only on death — a crash must never cost
  knowledge
- Any new system declares its save object before implementation

---

## 4. Module layout

```
Source/
├── RegressorCore/          Loop state machine, save objects, persistence rules
├── RegressorCombat/        Attributes, abilities, damage, posture
├── RegressorCultivation/   Realms, breakthroughs, pills, universal arts
├── RegressorKnowledge/     Knowledge flags, journal, gating queries
├── RegressorNarrative/     Dialogue, axis-driven tone selection, drift
├── RegressorAI/            Enemy behaviour (StateTree)
└── RegressorEditor/        Tooling, validation, DataTable checks
```

`RegressorCore` owns the loop and the persistence contract. Every other module
depends on it, and none of them may write persistent state without going
through it.

---

## 5. Unity → UE5 mapping

For anyone reading the archived prototype document:

| Unity | UE5 |
|---|---|
| `Assets/Scripts/*.cs`, MonoBehaviour | `Source/`, `AActor` / `UActorComponent` |
| `PlayerControls.inputactions` | Enhanced Input: `IA_` actions + `IMC_` mapping contexts |
| CharacterController | `ACharacter` + `UCharacterMovementComponent` |
| C# events / delegates | `DECLARE_DYNAMIC_MULTICAST_DELEGATE` |
| Coroutines | Timers, latent actions, Gameplay Tasks |
| ScriptableObject | `UPrimaryDataAsset` / `UDataTable` |
| `CompareTag("Player")` | Gameplay Tags, interfaces, collision channels |
| Animator controller | Animation Blueprint state machine / Motion Matching |
| Cinemachine | `USpringArmComponent` + `UCameraComponent`, camera modifiers |
| NavMesh + custom AI | `NavMeshBoundsVolume` + **StateTree** |
| `Update()` | `Tick` — but prefer event-driven |
| Prefab | Blueprint class / actor template |

---

## 6. DataTables

All tuning lives here. CSV-backed, source-controlled as text.

| Table | Contents |
|---|---|
| `DT_Techniques` | Movelist entries, proficiency curves, path requirements |
| `DT_KnowledgeFlags` | Flag catalogue — see `docs/02-loop/knowledge-as-key.md` |
| `DT_Realms` | Tier boundaries, names per path, proficiency gates |
| `DT_PillEffects` | Tier, alignment, outcome matrix |
| `DT_DriftSources` | Actions and their drift deltas |
| `DT_Encounters` | Placed enemies, technique-tier budget |
| `DT_CultivationCurve` | XP curves *(deferred, D-004)* |
| `DT_DialogueVariants` | Beat text keyed by axis quadrant |

**Rule:** if a designer or an AI assistant would want to change a number, it
belongs in a DataTable, not in C++.

---

## 7. Testing

- **Automation tests** for persistence above all: a simulated death must
  provably keep everything marked ● and clear everything marked ○ in the
  persistence matrix. This is the single most valuable test in the project
- Loop state machine transition tests
- DataTable validation in `RegressorEditor` — every referenced flag exists,
  every technique has a valid path requirement, no orphan rows
- Build + test must be runnable from one command, documented in `CLAUDE.md`

---

## 8. Repository state

> The repository currently contains the **abandoned Unity project**
> (`Assets/`, `ProjectSettings/`, `Packages/`, Unity `.gitignore` and
> `.gitattributes`). It has no gameplay code — the scripts described in the
> archived design document were never committed.
>
> **Next step:** replace it with a UE5 project skeleton, or start a clean
> repository and bring `docs/` across. Either way the Unity scaffolding should
> not survive alongside the UE5 project. See
> `docs/04-technical/migration-from-unity.md`.
