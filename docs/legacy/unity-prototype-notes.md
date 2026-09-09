# ARCHIVED — Unity Prototype Notes

> **STATUS: SUPERSEDED. DO NOT BUILD FROM THIS DOCUMENT.**
>
> This is the original `ProjectSetup.md` from the abandoned Unity 6 / URP
> prototype, preserved verbatim below for reference only.
>
> It is wrong on two axes:
>
> 1. **Wrong engine.** It describes Unity C# MonoBehaviours, the Unity Input
>    System, and a Unity asset layout. The project has moved to Unreal Engine 5.
> 2. **Wrong genre.** It describes a sandbox brawler test playground —
>    respawning target dummies, MMO grind XP, dev cheat buttons, level-adjust
>    hotkeys. The actual game is a **linear, narrative, regression-driven RPG**.
>
> It also uses **"reincarnation"** framing throughout. That framing was an
> artefact of AI-assisted respawn testing in Unity and is **not canon**. The
> canonical framing is **regression** — see `docs/00-canon/glossary.md`.
>
> For what was salvaged from this document and what was cut, see
> `docs/04-technical/migration-from-unity.md`.

---

# Sifu-Style Reincarnation & Martial Arts Cultivation 3D Brawler

Welcome to the project documentation for your 3D brawler! This document details the architectural decisions, control structures, and combat mechanics designed to recreate a responsive, physically heavy combat style similar to ***Sifu***, featuring a cyclic **Reincarnation and Martial Arts Cultivation System**.

---

## 1. Project Directory Structure
All custom assets and systems are neatly organized within the standard Unity directory structure:
```
Assets/
├── Documentation/
│   └── ProjectSetup.md          <-- [This File] Complete system documentation & reference guide
├── Input/
│   └── PlayerControls.inputactions <-- Project-wide Input Actions (New Input System)
├── Scripts/
│   ├── CharacterStats.cs        <-- Level trackers and base movement configurations
│   ├── CultivationSystem.cs     <-- Permanent progression, Qi spending, and breakthroughs
│   ├── DirectionIndicator.cs    <-- 3D projection ring showing facing direction under player feet
│   ├── EnemyAI.cs               <-- Level-scaled enemy AI, attacks, parrying & auto-respawns
│   ├── HealthSystem.cs          <-- Shared health pools, damage distribution, and death callbacks
│   ├── LevelSystemHUD.cs        <-- Persistent gameplay HUD, 3D name badges, and sanctum shop
│   ├── PlayerController.cs      <-- Player movements, combos, dodge logic, and parry checks
│   └── ThirdPersonCamera.cs     <-- Smooth orbital camera with raycast collision avoidance
└── Scenes/
    └── SampleScene.unity        <-- Brawler playground featuring Player, Camera, and Target Dummy
```

---

## 2. Input Action Layout (`PlayerControls.inputactions`)
Powered by Unity's **New Input System**, these global actions handle continuous and discrete inputs across Keyboard and Gamepads natively:

| Action Name | Action Type | Expected Control Type | Keyboard & Mouse Binding | Gamepad Binding |
| :--- | :--- | :--- | :--- | :--- |
| **`Move`** | Value | Vector2 | `W` `A` `S` `D` (Composite) | Left Stick |
| **`Look`** | Value | Vector2 | Mouse Delta | Right Stick |
| **`Sprint`** | Button | Button | Left `Shift` | Left Stick Click (`L3`) |
| **`Dodge`** | Button | Button | `Space` (Jump/Double Jump OR Dodge-Roll based on active mode) | South Button (`A` / `Cross`) |
| **`LightAttack`**| Button | Button | Left Mouse Button | West Button (`X` / `Square`) |
| **`HeavyAttack`**| Button | Button | Right Mouse Button | North Button (`Y` / `Triangle`) |
| **`Guard`** | Button | Button | `F` | Left Shoulder (`L1`) |
| **`LockOn`** | Button | Button | Middle Mouse Button | Right Stick Press (`R3`) |
| **`DodgeStance`**| Button | Button | `Q` (Hold) | Left Trigger (`L2`) (Hold) |
| **`ToggleMode`** | Button | Button | Left `Alt` | (Toggles active Spacebar movement mode) |

*Note: The asset is configured as the global default actions asset under **Edit > Project Settings > Input System Package** and is referenced directly in script via `InputSystem.actions`.*

---

## 3. Core Systems & Mechanics

### A. Health & Combat Loop (`HealthSystem.cs`)
A modular health tracker attached to both the Player and Enemies:
*   Standardized operations: `TakeDamage()`, `Heal()`, `Revive()`, and `SetMaxHealth()`.
*   Triggers delegates on change and death, which feed into HUD components and AI controllers instantly.

### B. MMORPG Leveling & Dual Breakthrough Systems (`CultivationSystem.cs`)
Upon death, the Player enters a transitional **Reincarnation Sanctum** where they invest accumulated Qi to grow their stats and breakthrough martial realms.
*   **The Reincarnation Cycle (Mortal Decay):**
    *   **Level Reset to 1:** Death is a major transitional state. In alignment with wuxia legends, when the player dies and reincarnates into their next life, **their core cultivation level resets back to Level 1**, and their active level experience (XP) resets to 0.
    *   **Spiritual Preservation:** While physical bone structure and meridians decay (returning level to 1), **the player's acquired spiritual abilities (Gyeonggong, Geomgi, Oegong) and combat school masteries are permanently retained and persist through each incarnation!**
*   **Experience & Level Progression:** You now level up using a standard progressive MMORPG experience points (XP) system.
    *   **Combat EXP:** Defeating any opponent grants Level Experience. Harder, higher-level enemies grant significantly more XP.
    *   **Naegong Meditation EXP:** Cultivating and meditating (holding **`C`** while standing still) continuously grants small amounts of Level EXP as you cycle your breathing (+1 EXP normal, +4 EXP when boosted!).
    *   **Qi Refinement (Sanctum):** In the Reincarnation Sanctum, you can refine your accumulated Qi directly into level experience (**"Refine Qi"** button), converting 40 Qi into +40 Level EXP.
*   **Soft-Locked Core Boundaries:** As you reach the maximum level of each stage (Lv. 20, 40, 60, 80, 90, 99), your experience caps at 100% and you are soft-locked. You cannot level up normally. The only way to trigger a breakthrough to the next level (unlocking the next core realm) is through two special, high-difficulty wuxia-themed methods:
    1.  **Sacred Cultivation Pills (Pill Breakthrough):** Consume a compatible cultivation pill of the *next* stage. For example, if you are locked at Level 20, consuming a *Foundation Establishment Pill* safely activates a spiritual pill breakthrough, cleanly elevating you to Level 21 and healing you to full.
    2.  **Life-or-Death Battle Breakthrough:** Defeat an elite opponent of your exact level (e.g. Level 20) during a grueling, near-death battle (where your remaining HP drops to **$25\%$ or lower** during the fight). Winning the fight under these intense, near-fatal conditions forces your meridians to burst open, triggering a spectacular combat breakthrough, instantly advancing you to the next stage, and fully restoring your health!
*   **Core Constitutions Selection (Locked by Reincarnation):** During reincarnation, players select their spiritual core constitution for their next lives:
    *   **Orthodox Dao Core (Normal Qi):** Pure, disciplined internal energy. Allows safe use of Orthodox cultivation pills.
    *   **Demonic Arts Core (Demonic Ki):** Fierce, chaotic, forbidden power. Allows safe use of Demonic cultivation pills.
    *   *Path Lock:* Once a player commits to a path and reincarnates (**`REINCARNATE / COMMENCE NEXT CYCLE`**), the choice is permanently locked for the rest of this playthrough sequence/incarnation. The buttons are grayed out on the HUD, preventing switching back and forth between lives.
    *   *The active constitution type is displayed dynamically on your persistent player HUD.*
*   **Persistent Martial Abilities & Proficiency Locks (`MartialArtsAbilitySystem.cs`):**
    The player's soul carries three ancient universal abilities that they can train across many lives. However, they must cultivate their core level back to high heights in each life to possess the physical meridian strength to utilize them (Proficiency Locks). **To keep the narrative and progression completely immersive, these abilities are completely hidden from the HUD and Martial Journal until unlocked, preventing spoilers regarding techniques the player has not yet discovered!**
    1.  **Gyeonggong (경공 - Lightness Skill / Qinggong):**
        *   *Train:* Gained by jumping, double-jumping, or executing manual dodge-rolls. **(Experience can only be gained once Gyeonggong is actively unlocked!)**
        *   *Benefit:* Permanent improvements to speed, leap force, and dodge recovery.
        *   *Lock:* To utilize the elite **Double-Jump** capability, you must have reached at least **Foundation Establishment** (Level 21+) in your current life.
    2.  **Geomgi (검기 - Aura Blade / Sword Force):**
        *   *Train:* Gained by hitting enemies with light/heavy weapon strikes. **(Experience can only be gained once Geomgi is actively unlocked!)**
        *   *Benefit:* Coats your weapon in spiritual energy, adding $+35\%$ base damage ($+5\%$ extra per level) and shooting slicing energy shockwaves on impact.
        *   *Lock:* The high-density Sword Force is locked until your body is sturdy enough, requiring at least **Nascent Soul / Heavenly Demon Soul** (Level 61+) in your current life.
    3.  **Oegong (외공 - Iron Body / Golden Bell Shield):**
        *   *Train:* Gained by executing perfect parries or active guarding/blocking. **(Experience can only be gained once Oegong is actively unlocked!)**
        *   *Benefit:* Hardens muscle fibers, adding $+12\%$ physical damage absorption and cutting block chip damage by up to $5\%$ per level.
        *   *Lock:* Locked until your dantian forms a protective shell, requiring at least **Core Formation / Demonic Core Formation** (Level 41+) in your current life.
*   **Boss Mob Cultivation Pill Drops (`CultivationPill.cs`):** Defeating boss/elite enemies (any over-leveled opponent with a level difference $> 1$ or explicitly marked as a boss) causes them to drop a physical, floating **Cultivation Pill** matching their own tier:
    *   **Interactive Pickup & Active Tagging:** Physical drops (Pills, Beast Cores, etc.) utilize Unity trigger colliders to detect player proximity. **The Player GameObject is explicitly tagged with the `"Player"` tag in the scene to ensure reliable proximity checks, interactive [E] pickup prompts, item collection, and successful addition to the Spiritual Inventory.**
    *   **Qi Conflict Backlash (Risk of Death):** If a player consumes a pill of the opposite alignment (e.g., Orthodox player eating a Demonic Pill), they suffer a catastrophic **Qi Conflict Backlash** dealing **$85\%$ of their Max HP** as damage.
    *   **Meridian Implosion (Risk of Death):** If a player consumes a compatible pill that is of a **higher tier** than their current core realm (and they are not locked at the boundary ready for a breakthrough), they suffer a **Meridian Implosion** dealing **$65\%$ of their Max HP** as damage.
    *   **Perfect Absorption:** If both the player's core constitution and their current core realm tier match the pill perfectly, they assimilate its energy completely, receiving a massive **$+250$ Qi and $+100$ Sect Style EXP**!
    *   **Pill Dissolution:** If a player consumes a compatible pill of a **lower tier** than their current core realm, the pill dissolves harmlessly with zero effect.
*   **Permanent Upgrades:**
    *   **Max HP Buff:** Permanent $+15\%$ maximum health multiplier per upgrade level.
    *   **Martial Damage Buff:** Permanent $+15\%$ combat damage and strike-thrust velocity per level.
    *   **Agility/Speed Buff:** Permanent $+8\%$ movement and evasion speed per level.
*   **Tightly Bound Leveling & Dual Cultivation Tiers:**
    The character's level directly determines their **Martial Cultivation Realm (Tier)**. Depending on the player's active soul constitution path, their core breakthroughs map to distinct titles and tiers:
    
    | Tier | Level Range | Orthodox Dao Core Realms | Demonic Arts Core Realms |
    | :--- | :--- | :--- | :--- |
    | **Tier I** | Lv. 1-20 | Qi Condensation | Demonic Qi Gathering |
    | **Tier II** | Lv. 21-40 | Foundation Establishment | Demon Foundation |
    | **Tier III** | Lv. 41-60 | Core Formation | Demonic Core Formation |
    | **Tier IV** | Lv. 61-80 | Nascent Soul | Heavenly Demon Soul |
    | **Tier V** | Lv. 81-90 | Martial King | Asura King |
    | **Tier VI** | Lv. 91-99 | Martial Emperor | Archdemon Emperor |
    | **Tier VII**| Lv. 100+ | **Martial God** | **Heavenly Demon** |

    *All passive stats multipliers, damage scaling, parry deflections, and auto-dodge/teleport styles map perfectly to match these dual-path breakthrough tiers.*

### C. Advanced Player Controls (`PlayerController.cs`)
*   **Parry Mechanic:** When holding `F` (or L1) within **0.25 seconds** of an incoming enemy attack, the player performs a **Perfect Parry** (capsule flashes purple). The attack is deflected, the player takes zero damage, and the enemy is knocked backward, suffering severe posture damage.
*   **Divine Teleport Auto-Dodge:** Reaching the *Nascent Soul* realm upgrades standard automatic passive dodges into golden flashes that travel $+40\%$ further and faster, representing elite spatial compression teleportation techniques.
*   **Jump & Manual Dodge-Roll (Toggle via Left-Alt):** Pressing **`Left-Alt`** dynamically toggles your Spacebar manual action mode (visible on the top-left HUD):
    *   **Jump Mode (Default):** Tapping **`Space`** triggers a vertical **Jump**. Double-tapping **`Space`** while in mid-air triggers a **Double-Jump** (creates a cyan *DOUBLE JUMP* floating combat text above your head).
    *   **Dodge-Roll Mode:** Tapping **`Space`** instantly executes a high-speed **Dodge-Roll** or quickstep, letting you manually slip under enemy attacks and reposition. Reaching *Nascent Soul (Tier IV)* upgrades manual dodge-rolls into golden teleportation flashes.
*   **Passive Avoidance (Auto-Evasion):** The player possesses a spiritual **Passive Evasion Chance** based on their cultivation tier and speed upgrades. If an opponent's attack is about to land and you aren't active in a manual defensive state, a passive percentage check is rolled. On success, **the opponent's attack simply doesn't land** (it is passively phased through, dealing zero damage, with zero physical disruption to your movement or combos).
*   **Dodge Stance / Evasive Mode (Hold Q):** Holding **`Q`** (or Gamepad **L2 / Left Trigger**) enters a dedicated **Dodge Stance** (capsule and indicator glow a vibrant ghost-blue). While tensed in this stance:
    *   Your passive evasion success chance is **boosted significantly by +40%** (for example, raising Qi Condensation evasion from 10% to 50%, and Nascent Soul evasion to 85%!).
    *   Your movement transitions into slow, alert strafe-stepping, making it a high-success block alternative when facing specific fighting styles or unblockable weapons.
*   **Precision Mouse-Aimed Strikes:** To provide elite, high-precision combat control, attacks are steered independently of locomotion (WASD) movement direction. Clicking an attack instantly snaps your character to face your mouse/camera crosshair direction. While executing combos, you can continue to rotate your mouse to "steer" and curve strikes in real-time, matching standard high-skill third-person action game systems.
*   **Camera Lock-On Toggle:** Pressing the **Middle Mouse Button** (or Gamepad **R3**) locks the *camera's* orbit directly onto the nearest enemy within range. The camera dynamically sweeps to frame the target on screen as you circle-strafe. Pressing it again toggles the camera lock-on off. If the target is defeated, the lock-on automatically breaks.
    *   *Right-Shoulder Framing:* During lock-on, the camera automatically offsets slightly to the **player's right shoulder** (smoothly lerping into position). This shifts the player character to the left-third of the screen, leaving the center perfectly clear to frame the combat action and enemy movements without blocking your view!
*   **Directional Projector Ring (`DirectionIndicator.cs`):** A custom 3D line-projector system rendered flat on the floor underneath the player's feet. It renders a clean circular outline and a sharp, elegant V-arrow pointing directly in the player's forward-look direction. This is especially helpful during fast-paced attacks to visual-cue target angles.
*   **Aesthetic Stance Syncing:** To maintain cohesive brawler aesthetics, the projector ring and arrow smoothly color-shift in real-time to match the player's active state (Normal = White, Locked-On = Cyan, Guarding = Green, Perfect Parry = Magenta, Dodge Stance = Ghost Blue, Dodging = Yellow/Gold, Attacking = Red, Defeated = Deep Grey).
*   **Kinetic Color & Scale Feedback:** The player's capsule dynamically changes colors and deforms based on active states to make actions feel physically satisfying.

### D. Adaptive Enemy AI (`EnemyAI.cs`)
An active combat agent that dynamically adapts its physical presence, stats, and behavior relative to the level difference ($diff = L_{enemy} - L_{player}$) and **proximity/distance to the player**:
*   **Proximity-Based Spiritual Auras (Radius: 12.0m):**
    *   **Under-Leveled Suppression Aura ($diff < -1$):** If the player is of a higher level than the enemy, they project a crushing spiritual martial pressure. 
        *   *Gradual Slow:* As the weak enemy gets closer to the player within a **12.0m radius**, their speed smoothly decelerates.
        *   *Pinnacle Suppression:* The maximum slow is scaled based on the level gap (capping at a **99% slow** for a $20+$ level difference, representing a *Martial God*).
        *   *Attack Rate Slow:* This suppression applies directly to their combat reflexes, slowing down their **movement and attack rate by up to 99%** right next to the player!
    *   **Over-Leveled Terror Aura ($diff > 1$):** If an enemy is of a much higher level than the player (represented by a glowing **Deep Red** giant boss), they project an oppressive, terrifying aura.
        *   *Intimidating Acceleration:* As they close in on the player within a **12.0m radius**, their speed smoothly accelerates to build extreme pressure.
        *   *Pinnacle Speed & Agility:* Their speed and attack frequency increase by up to **+120% (2.2x speed)** when right next to the player, making their strikes incredibly fast and challenging!
*   **Equal-Leveled ($-1 \le diff \le 1$):** Enemy stands at standard $1.0x$ proportions, moves and attacks at standard speed, and glows warning **Orange**.
*   **Sandbox Respawn Loop:** Defeating an enemy rewards Qi instantly. The defeated enemy capsule flattens, turns grey, and auto-respawns at its original post after **3.0 seconds**, ensuring continuous combat training.

### E. Spiritual Inventory System & Beast Core Drops (`InventorySystem.cs`, `BeastCoreDrop.cs`)
To survive and excel across lifetimes, the player can acquire rare physical items to bolster their active training:
*   **Togglable Inventory Panel (Press I):** Pressing **`I`** toggles an elegant inventory grid on the screen. The inventory tracks and stores collectible materials.
*   **Automatic Magnetic Pickups:** Defeated enemies have a rare chance of dropping a highly valuable **Ancient Beast Core** (represented by a rotating, floating unlit-emitting 3D cube). 
    *   *Standard drop rate:* $12\%$.
    *   *Boss/Over-leveled drop rate:* $40\%$.
    *   *Magnetic Pull:* If the player is within **6.0 meters**, the Beast Core automatically activates its magnetic pull, drawing itself towards the player at high velocities and adding itself directly to their inventory!
*   **Spiritual Core Boost (Right-Click to Consume):** Right-clicking a Beast Core in your inventory consumes it to trigger a **temporary, heavy Naegong meditation boost for 15.0 seconds** (countdown displayed in top center):
    *   **Cultivation Speed Boost (2.5x Ticks):** When holding **`C`** to meditate with a core active, your inner breath cycles 2.5x faster.
    *   **Qi Multiplier (4x Qi):** Your Qi absorption amount is multiplied by 4x. Together, you absorb a whopping **$+20$ Qi every $0.1$ seconds**!
    *   **Ethereal Naegong Effects:** Spawns glowing purple/magenta floating feedback texts (**`+20 Qi (BOOSTED)`**) and color shifts your character capsule to a celestial magenta state during boosted meditation.

### F. Interactive NPC Dialogue System (`NPCDialogue.cs`)
To bring the town's social structure to life, players can interact directly with the local populace:
*   **Proximity Detection & Dynamic Attachment:** Pressing **`E`** near any neutral/civilian NPC (Workers, Drunks, Shoppers) triggers conversation.
    *   If the target NPC doesn't have a pre-assigned dialogue system, the game auto-attaches `NPCDialogue` dynamically on-the-fly and customizes their lines based on their role tags (Workers complain about heavy labor and high pill prices, drunks shout about wine and the Drunken Fist, shoppers talk about elixirs and rare drops!).
*   **Dialogue Interface Box:** A beautifully crafted, centered cinematic dialogue box rises from the bottom of the screen when active:
    *   *Cyan Nameplates:* Shows the speaker's name in glowing cyan.
    *   *Word-wrapped content:* Standard dialogue lines with soft wuxia details.
    *   *Control Tips:* Displays an interaction guide at the bottom right.
*   **Line Advancement & Auto-Close:** 
    *   Pressing **`E`** repeatedly cycles through the NPC's custom lines.
    *   Walking away (moving further than **$3.5$ meters**) naturally closes the dialogue interface, seamlessly sliding you back into focus.

---

## 4. Reincarnation Sanctum Shop UI (`LevelSystemHUD.cs`)
Tailored for a seamless gameplay loop, the HUD features:
*   **2D Combat Overlay:** Tracks current generation cycle, current martial realm, and persistent HP/Qi bars. It also lists your dynamic **Passive Evasion Success Chance** (flashes green with a boosted rating when Dodge Stance is held!).
*   **3D Billboard Overlays:** Floats character names and levels dynamically above NPC/enemy heads.
*   **Floating Combat Text & Damage Numbers:** Real-time world-space projected text pops up and floats upwards when combat interactions occur:
    *   **`MISS` (Cyan):** Pops up over the player when a passive auto-evasion succeeds.
    *   **`MISS` (Yellow):** Pops up over the player when they actively slip an attack using a manual dodge/quickstep.
    *   **`PARRY` (Magenta):** Flashes on successful active parry deflections.
    *   **`BLOCK` (Green):** Displays on active blocking.
    *   **`Damage Numbers (Orange)`:** Shows damage dealt to enemies on light attack strikes (e.g., `-10`).
    *   **`CRITICAL! Damage Numbers (Red)`:** Shows massive damage dealt to enemies on heavy finishing strikes (e.g., `CRITICAL!\n-35`).
    *   **`Incoming Damage (Red)`:** Shows damage suffered by the player from enemy strikes (e.g., `-15`).
*   **Reincarnation Sanctum Overlay:** When the player falls, a dark spiritual space overlays the screen. Standard gameplay freezes. Players spend accumulated Qi on stats or cultivation breakthroughs, then click **Reincarnate** to resurrect as the next generation of martial artist, spawning back in the center with updated permanent buffs.

---

## 5. Testing Playground & Controls
Press **Play** in the Unity Editor and use the following keys/buttons to test:
*   **Move & Look:** `WASD` & Mouse.
*   **Combat Lock-On Toggle:** Press **Middle Mouse Button** (or Gamepad **R3** / Right Stick Press) to lock the camera onto the dummy. Press it again to release lock-on.
*   **Combo Attack:** Left Click for Light Combo / Right Click for Heavy Finisher (requires Tier II breakthrough).
*   **Equip Weapons:** Walk up to any of the 5 floating weapon spheres on pedestals in front of your starting position:
    *   **Vajra Fists (Grey):** Equips **Unarmed** (Fist weapon art).
    *   **Orthodox Sword (Cyan):** Equips **Sword** (allows Mount Hua & Mudang sword arts).
    *   **Tiger Saber (Orange):** Equips **Saber** (allows Peng Clan saber arts).
    *   **Vajra Staff (Green):** Equips **Staff** (allows Sorim staff arts).
    *   **Shadow Daggers (Purple):** Equips **Dagger** (allows Hao Clan dagger arts).
*   **Switch Martial Style / Weapon Art (Keys 1 - 6):** Press numbers to switch styles:
    *   `1` -> Mount Hua Plum Blossom *(Requires Sword)*
    *   `2` -> Peng Clan Tiger Saber *(Requires Saber)*
    *   `3` -> Mudang Tai Chi *(Requires Sword)*
    *   `4` -> Sorim Vajra Staff *(Requires Staff)*
    *   `5` -> Hao Clan Shadow Dagger *(Requires Dagger)*
    *   `6` -> Sorim Vajra Fist *(Requires Unarmed)*
    *   *Weapon Locking:* Attempting to switch style without the corresponding weapon equipped displays a red `LOCKED! Requires [WEAPON]` floating text.
*   **Toggle Movement Mode:** Press **`Left-Alt`** to toggle between **Jump Mode** and **Dodge-Roll Mode** (current active mode is displayed in real-time in your top-left stats panel!).
*   **Manual Movement Action:**
    *   *In Jump Mode:* Tap **`Space`** to Jump. Tap **`Space`** in mid-air to **Double-Jump**!
    *   *In Dodge-Roll Mode:* Tap **`Space`** to execute a manual **Dodge-Roll** in your direction of movement.
*   **Dodge Stance / Evasive Mode:** Hold **`Q`** (or Gamepad **L2 / Left Trigger**) to enter Dodge Stance, boosting your passive evasion success rate by $+40\%$.
*   **Defend & Parry:** Hold `F` to block. Tap `F` right as the enemy dummy swings to **Deflect and Parry**!
*   **Adjust Player Level:** Press **`Page Up`** to level up, or **`Page Down`** to level down in real-time.
*   **Adjust Enemy Level:** Press **`]`** (Right Bracket) to level up, or **`[`** (Left Bracket) to level down in real-time.
*   **Dev Qi Cheats:** Use on-screen HUD buttons to instantly add `+50` or `+250` Qi to test breakthrough purchases.
*   **Instant Defeat Cheat:** Click "Trigger Defeat / Enter Cultivation Cycle" to instantly enter the Reincarnation Sanctum and spend your Qi!

---

## 6. Long-Term RPG Vision & Faction World Structure

This project is architected as a **Long-Form Martial Arts RPG**, mapping a grand narrative progression from a martial "nobody" to an immortal **Martial God** over a vast sequence of generations and cycles.

### A. Core Progression: Zero to Sovereign
The progression model emphasizes **cyclic growth** over single-life survival.
1.  **The Mortal Phase (Early Game):** The player starts with no martial talents, pathetic health pools, and zero techniques. Combat is brutal, and death is frequent.
2.  **The Sect Apprentice Phase (Mid Game):** By seeking out regional factions and learning their arts, players build specific school proficiencies (Mount Hua, Peng Clan, Mudang, etc.). Stance EXP and level increases survive the mortal decay of death.
3.  **The Sovereign Awakening (Late Game):** Through multi-generational reincarnation, the player fuses separate school masteries together, breaksthroughs global spiritual realms (Nascent Soul, Martial King), and challenges legendary sect leaders.

### B. Murim World & Territory Map Design
The final game environment will consist of modular, faction-controlled territories:
*   **The Orthodox Cities (Jeongpa):** High-walled, pristine cities housing the majestic **Namgung Clan Manor** and bustling marketplaces. Guarded by elite sword-sentries where illegal combat or theft is heavily penalized.
*   **Mountain Sect Sanctuaries:** Secluded Taoist temples (such as the misty peaks of **Mount Hua** or **Mudang**) where non-combatant monks and Taoists cultivate inner Naegong, brew medicine, and teach high-tier sword forms.
*   **The Unorthodox Outposts (Sapa):** Gritty, neon-lit alleyways of the **Hao Clan** spy networks, or lawless mountain forests controlled by the bandits of **Nokrim**. Survival here requires bribery, high-precision combat stealth, and mastery of poison.
*   **Antagonist strongholds:** The hidden volcano fortresses of the **Heavenly Demon Cult**, featuring dark sacrificial altars and aggressive, high-risk elite guards.

### C. NPC Ecosystem Layout
To bring these settlements to life, the NPC system utilizes a dual-behavior tree layout:
1.  **Combatant NPCs:** 
    *   Sect guards, martial apprentices, rogue mercenaries, and faction heads.
    *   Equipped with specific faction weapons and fighting styles (Saber, Staff, Needle, Sword) that interact with the **Matchup Matrix** in real-time.
2.  **Non-Combatant NPCs:**
    *   **Town Merchants & Blacksmiths:** Sell medicinal herbs, purchase spoils of war, and forge custom weapons.
    *   **Spiritual Elders & Hermits:** Found in secluded shrines, offering hidden side-quests or guidance on breakthrough requirements.
    *   **Civilians & Informants:** Populate city streets, reacting to combat, and feeding rumors regarding faction territory disputes.
