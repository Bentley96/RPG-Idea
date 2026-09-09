# The Regressor's Path — Design Documentation

A linear, narrative, regression-driven murim RPG for **Unreal Engine 5**.

> The player dies, returns to a fixed point in the same timeline, and keeps
> everything they learned. Cultivation resets. Technique and knowledge do not.

---

## Read in this order

**New to the project? These four, in order:**

1. [`00-canon/decision-log.md`](00-canon/decision-log.md) — every locked
   decision. **Canon. If a document contradicts this, the log wins**
2. [`02-loop/loop-architecture.md`](02-loop/loop-architecture.md) — the spine.
   Everything hangs off it
3. [`02-loop/persistence-matrix.md`](02-loop/persistence-matrix.md) — what
   survives death, for every system. Binding
4. [`01-narrative/story-design.md`](01-narrative/story-design.md) — the story

---

## Full index

### 00 — Canon
| Doc | Contents |
|---|---|
| [`decision-log.md`](00-canon/decision-log.md) | Locked decisions D-001 … D-007 |
| [`glossary.md`](00-canon/glossary.md) | Canonical terms, realm ladder, **banned terms** |

### 01 — Narrative
| Doc | Contents |
|---|---|
| [`story-design.md`](01-narrative/story-design.md) | Premise, Act 1, themes, content-generation rules |
| [`character-axes.md`](01-narrative/character-axes.md) | Jadedness & Confidence, tone delivery |

### 02 — The Loop *(core)*
| Doc | Contents |
|---|---|
| [`loop-architecture.md`](02-loop/loop-architecture.md) | Loop states, death model, anchors, act structure |
| [`persistence-matrix.md`](02-loop/persistence-matrix.md) | What persists vs. resets, per system |
| [`knowledge-as-key.md`](02-loop/knowledge-as-key.md) | The primary progression axis |
| [`replay-tax.md`](02-loop/replay-tax.md) | Loop acceleration and the time budget |

### 03 — Systems
| Doc | Contents |
|---|---|
| [`combat.md`](03-systems/combat.md) | Core actions, posture, readability, cut systems |
| [`cultivation-and-realms.md`](03-systems/cultivation-and-realms.md) | Realms, breakthroughs, pills, universal arts |
| [`paths-and-drift.md`](03-systems/paths-and-drift.md) | Orthodox ↔ Demonic drift, endings |
| [`progression-economy.md`](03-systems/progression-economy.md) | **Deferred stub** — constraints only |

### 04 — Technical
| Doc | Contents |
|---|---|
| [`technical-design.md`](04-technical/technical-design.md) | UE5 architecture, GAS, saves, DataTables |
| [`migration-from-unity.md`](04-technical/migration-from-unity.md) | Salvaged, cut, and reframed |

### 05 — World
| Doc | Contents |
|---|---|
| [`murim-factions.md`](05-world/murim-factions.md) | Clans, sects, weapons, abilities |

### 06 — Production
| Doc | Contents |
|---|---|
| [`vertical-slice.md`](06-production/vertical-slice.md) | **Build this first** |
| [`open-questions.md`](06-production/open-questions.md) | Every unresolved decision, prioritised |

### Legacy
| Doc | Contents |
|---|---|
| [`unity-prototype-notes.md`](legacy/unity-prototype-notes.md) | **Superseded.** Archived Unity prototype doc. Do not build from it |

---

## The rules, in brief

1. **Regression, never reincarnation.** Same world, rewound (D-005)
2. **The first death is authored; every death after is failure** (D-003)
3. **Technique carries the fight; cultivation carries the ceiling** (D-007)
4. **Two ladders.** Martial rank persists; cultivation tier resets (D-008).
   Never say "level" — it hides which ladder is meant
5. **Three paths.** Orthodox, Unorthodox, Demonic — cumulative drift plus
   conviction, never a choice (D-006, D-009)
6. **The player chooses when to advance the anchor, and it is irreversible** (D-010)
7. **Path sets language, not just tone** (D-011)
8. **Every loop yields durable progress** — no wasted loops
9. **Every act is completable at Tier I** with the expected technique set
10. **One base moveset per weapon.** A new style costs ≤5 clips (D-012)
11. **Any new system declares its persistence row before implementation**

---

## Status

| Area | State |
|---|---|
| Core loop design | **Specified** |
| Anchor advance | **Specified** (D-010); placement open (Q-18) |
| Progression ladders | **Specified** — martial rank + cultivation tier (D-008) |
| Paths & drift | **Specified** — three paths + conviction (D-009) |
| Persistence rules | **Specified** |
| Knowledge system | **Specified** |
| Act 1 | Authored |
| Acts 2–5 | **To author** (Q-04) |
| Combat | Specified; posture unconfirmed (Q-03) |
| Character voice | Model locked (D-011); coverage budget open (Q-02) |
| Animation model | **Locked** (D-012); weapon count for v1 open (Q-16) |
| Progression & economy | **Deferred** (D-004) |
| UE5 project | **Not started** — repo still holds the abandoned Unity project (Q-17) |
