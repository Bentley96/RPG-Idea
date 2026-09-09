# Canonical Glossary

The single source of truth for terminology. Use these terms exactly, in
documents, in code identifiers, and in generated content. Where a term has a
banned synonym, the banned form is listed — do not use it anywhere.

---

## Loop terminology

| Canonical | Meaning | Banned synonyms |
|---|---|---|
| **Regression** | The protagonist's return to a fixed earlier point in the same timeline, on death | Reincarnation, rebirth, respawn, resurrection |
| **Loop** | One life, from anchor to death. Numbered from 1 | Generation, cycle, incarnation, run |
| **Anchor** | The fixed point in time and space a regression returns the player to | Spawn point, checkpoint, sanctum |
| **Anchor advance** | The story event that moves the anchor forward to a later point | — |
| **Regression Interlude** | The between-lives sequence where losses, retentions and gains are shown | Reincarnation Sanctum, death screen |
| **Loop counter** | Monotonic count of lives lived. Never resets | Generation number |
| **Knowledge flag** | A persistent record that the protagonist knows a specific fact | — |
| **Frontier** | The portion of a loop that is new content, past everything the player has already seen | — |
| **Recovery** | The portion of a loop that replays known content under acceleration | — |

> In-fiction name for the Regression Interlude is still open — see
> `docs/06-production/open-questions.md`.

---

## Cultivation terminology

| Canonical | Korean | Meaning |
|---|---|---|
| **Naegong** | 내공 | Internal energy. Core qi cultivation; fuels every other technique |
| **Gyeonggong** | 경공 | Lightness skill. Leaping, gliding, wall-running |
| **Geomgi** | 검기 | Sword aura. Energy extended along a blade |
| **Geomgang** | 검강 | Sword force. The advanced, solidified form of Geomgi |
| **Oegong** | 외공 | Iron body. Hardening the body against damage |
| **Gigam** | 기감 | Energy sense. Detecting hostile intent and hidden enemies |
| **Jeomhyeol** | 점혈 | Acupoint strikes. Sealing an opponent's energy points |
| **Magi** | 마기 | Demonic energy. Corrupting, aggressive counterpart to orthodox qi |
| **Dantian** | 단전 | The energy centre where the core forms |
| **Meridians** | 경맥 | The channels qi flows through |

---

## Realms

The canonical realm ladder. **Level determines realm**, always — realm is a
derived value, never set independently.

| Tier | Levels | Orthodox | Demonic |
|---|---|---|---|
| I | 1–20 | Qi Condensation | Demonic Qi Gathering |
| II | 21–40 | Foundation Establishment | Demon Foundation |
| III | 41–60 | Core Formation | Demonic Core Formation |
| IV | 61–80 | Nascent Soul | Heavenly Demon Soul |
| V | 81–90 | Martial King | Asura King |
| VI | 91–99 | Martial Emperor | Archdemon Emperor |
| VII | 100+ | **Martial God** | **Heavenly Demon** |

**"Core Refinement" is not a realm.** It appeared in the original story
document as the mentor's rank. The mentor's canonical rank is **Core Formation
(Tier III)** — genuinely capable, middling in the wider murim, and nowhere near
the top. This keeps him honest: skilled enough to teach fundamentals, far too
weak to be a hidden grandmaster.

> **Confirm:** mentor at Core Formation (Tier III). Flagged in
> `docs/06-production/open-questions.md` in case a lower rank is preferred.

---

## Factions

Full detail in `docs/05-world/murim-factions.md`. Short forms for use in code
and content:

| Canonical | Korean | Alignment |
|---|---|---|
| Murim Alliance | 무림맹 | Orthodox (political body) |
| Namgung Clan | 남궁세가 | Orthodox |
| Mount Hua Sect | 화산파 | Orthodox |
| Sorim Temple | 소림사 | Orthodox |
| Mudang Sect | 무당파 | Orthodox |
| Tang Clan | 사천당가 | Orthodox |
| Peng Clan | 하북팽가 | Orthodox |
| Hao Clan | 하오문 | Unorthodox |
| Sapaeryeon | 사패련 | Unorthodox |
| Nokrim (Green Forest) | 녹림 | Unorthodox |
| Heavenly Demon Cult | 천마신교 | Demonic |
| Blood Cult | 혈교 | Demonic |

Faction alignment labels: **Jeongpa** (정파, orthodox), **Sapa** (사파,
unorthodox), **Magyo** (마교, demonic).

---

## Progression terminology

| Canonical | Meaning | Persists across loops? |
|---|---|---|
| **Level** | Cultivation level, 1–100+ | No — resets to 1 |
| **Realm / Tier** | Derived from level | No — derived, so resets |
| **Technique** | A learned martial art, form, or move | **Yes** |
| **Technique proficiency** | Mastery within a single technique | **Yes** |
| **Knowledge flag** | A known fact about the world | **Yes** |
| **Path drift** | Cumulative Orthodox ↔ Demonic position | **Yes** |
| **Jadedness** | Psychological axis, rises with loops lived | **Yes** |
| **Confidence** | Psychological axis, rises with realm reached | Partly — see `docs/01-narrative/character-axes.md` |
| **Qi** | Spendable energy resource within a loop | No |

Full authority on what survives death: `docs/02-loop/persistence-matrix.md`.

---

## Banned terms

Never use these. They are Unity-prototype artefacts or genre mismatches.

- Reincarnation, Reincarnation Sanctum, generation cycle, next generation
- Respawn (for the player — enemies may still be said to respawn in tooling)
- Core Refinement (as a realm name)
- Constitution *choice* / path *lock* — drift is cumulative, never chosen
- Passive evasion, suppression aura, terror aura — cut systems, see
  `docs/04-technical/migration-from-unity.md`
