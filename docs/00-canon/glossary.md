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
| **Anchor** | The point in time and space a regression returns the player to | Spawn point, checkpoint, sanctum |
| **Anchor advance** | The player's deliberate, irreversible act of moving the anchor forward, abandoning everything before it (**D-010**) | — |
| **Regression Interlude** | The between-lives sequence where losses, retentions and gains are shown | Reincarnation Sanctum, death screen |
| **Loop counter** | Monotonic count of lives lived. Never resets | Generation number |
| **Knowledge flag** | A persistent record that the protagonist knows a specific fact | — |
| **Testament** | The permanent record of the protagonist's path standing, taken at each act close (**D-013**) | Checkpoint, alignment lock |
| **Trajectory** | The sequence of testaments across all acts. Frames the ending | — |
| **Frontier** | The portion of a loop that is new content | — |
| **Recovery** | The portion of a loop that replays known content under acceleration | — |

> In-fiction names for the Regression Interlude and the Testament are still
> open — see `docs/06-production/open-questions.md`.

**Act structure:** three acts (provisional, **D-013**). Testaments at the close
of each; anchor advance offered at the close of Acts 1 and 2.

---

## The two ladders

The game has **two separate progression ladders** and they must never be
conflated (**D-008**). One measures skill and persists; the other measures
internal energy and resets. This is **D-007** made structural.

| | **Martial Rank** (무공 경지) | **Cultivation Realm** (내공) |
|---|---|---|
| Measures | Martial skill and mastery | Internal energy accumulation |
| Tradition | Korean murim (무협) | Xianxia cultivation |
| On death | **Persists** | **Resets** |
| Who can see it | Other martial artists, on sight | Largely hidden |
| Drives | How well you fight | What you can survive |

### Ladder A — Martial Rank (persists)

The Korean murim rank ladder. This is what other martial artists recognise in
you, and it is the ladder the protagonist genuinely climbs across loops.

| # | Rank | Korean | Meaning |
|---|---|---|---|
| 0 | Unranked | 무명 (Mumyeong) | Not a martial artist |
| 1 | Third-Rate | 삼류 (Samryu) | Trained. Basic forms |
| 2 | Second-Rate | 이류 (Iryu) | Competent |
| 3 | First-Rate | 일류 (Ilryu) | A master (고수) begins here |
| 4 | Peak | 절정 (Jeoljeong) | Elite. Command of intent |
| 5 | Transcendent | 초절정 (Chojeoljeong) | Above the peak |
| 6 | Hwagyeong | 화경 (化境) | Transformation Realm |
| 7 | Hyeongyeong | 현경 (玄境) | Profound Realm. Pinnacle of human martial arts |
| 8 | Saengsagyeong | 생사경 (生死境) | Life-and-Death Realm |
| 9 | Jayeongyeong | 자연경 (自然境) | Nature Realm. The summit |

### Ladder B — Cultivation Realm (resets)

The xianxia realm ladder. Rebuilt from Tier I every loop. Names differ by the
protagonist's **path drift** (`docs/03-systems/paths-and-drift.md`) — the same
tier reads differently depending on what they have become.

| Tier | Orthodox (정파) | Unorthodox (사파) | Demonic (마교) |
|---|---|---|---|
| **I** | Qi Condensation | Qi Scavenging | Demonic Qi Gathering |
| **II** | Foundation Establishment | Crude Foundation | Demon Foundation |
| **III** | **Core Refinement** | Tempered Core | Corrupt Core Refinement |
| **IV** | Core Formation | Blackened Core | Demonic Core Formation |
| **V** | Nascent Soul | Severed Soul | Heavenly Demon Soul |
| **VI** | Soul Transformation | Soul Devourer | Asura Transformation |
| **VII** | Martial King | Sapa Overlord | Archdemon |
| **VIII** | **Martial God** (무신) | **Sole Sovereign** (독존) | **Heavenly Demon** (천마) |

**Level bands are deliberately not assigned.** Tiers are ordinal. Numeric level
mapping is progression work and is deferred (**D-004**). Reference tiers, never
levels, in design and code.

> **Core Refinement** sits at Tier III, between Foundation Establishment and
> Core Formation: the core is condensed here and completed at Tier IV. It is
> not a standard realm in published xianxia ladders — it is this project's
> insertion, and it restores the mentor to the rank the original story document
> gave him.

---

## Cultivation terminology

| Canonical | Korean | Meaning |
|---|---|---|
| **Naegong** | 내공 | Internal energy. Fuels every other technique |
| **Gyeonggong** | 경공 | Lightness skill. Leaping, gliding, wall-running |
| **Geomgi** | 검기 | Sword aura. Energy extended along a blade |
| **Geomgang** | 검강 | Sword force. The solidified form of Geomgi |
| **Oegong** | 외공 | Iron body. Hardening the body against damage |
| **Gigam** | 기감 | Energy sense. Detecting hostile intent |
| **Jeomhyeol** | 점혈 | Acupoint strikes. Sealing an opponent's energy points |
| **Magi** | 마기 | Demonic energy |
| **Gosu** | 고수 | A master. First-Rate and above |
| **Dantian** | 단전 | The energy centre where the core forms |
| **Meridians** | 경맥 | The channels qi flows through |

---

## Paths

Three paths, not two (**D-009**). Unorthodox is **not** a lesser demonic — it
is a distinct road with its own ethic.

| Path | Korean | Ethic | Endgame title |
|---|---|---|---|
| **Orthodox** | 정파 (Jeongpa) | Duty, restraint, the slow honest climb | Martial God (무신) |
| **Unorthodox** | 사파 (Sapa) | Pragmatism, profit, survival. Morally grey, not evil | Sole Sovereign (독존) |
| **Demonic** | 마교 (Magyo) | Appetite, domination, forbidden arts | Heavenly Demon (천마) |

---

## Factions

Full detail in `docs/05-world/murim-factions.md`.

| Canonical | Korean | Path |
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

---

## Progression terminology

| Canonical | Meaning | Persists? |
|---|---|---|
| **Martial rank** | Ladder A. Skill as other martial artists see it | **Yes** |
| **Cultivation realm / tier** | Ladder B. Internal energy | No |
| **Technique** | A learned martial art, form, or move | **Yes** |
| **Technique proficiency** | Mastery within a single technique | **Yes** |
| **Knowledge flag** | A known fact about the world | **Yes** |
| **Path drift** | Position on the Orthodox ↔ Demonic axis | **Yes** |
| **Conviction** | How strongly the protagonist has committed to anything | **Yes** |
| **Testament** | Path standing recorded at an act close. Four values: Orthodox, Unorthodox, Demonic, Unrecorded | **Yes** |
| **Jadedness** | Rises with loops lived and anchors abandoned | **Yes** |
| **Confidence** | Rises with realm reached | Partly |
| **Qi** | Spendable energy within a loop | No |

Full authority: `docs/02-loop/persistence-matrix.md`.

---

## Banned terms

- Reincarnation, Reincarnation Sanctum, generation cycle, next generation
- Respawn (for the player)
- **"Level" as a design term** — use **martial rank** or **cultivation tier**.
  Saying "level" hides which of the two ladders is meant
- Constitution *choice* / path *lock* — drift is cumulative, never chosen
- Treating Unorthodox as a weaker Demonic, or as mere neutrality
- Passive evasion, suppression aura, terror aura — cut systems
