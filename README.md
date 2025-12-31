# Game Design Document: STRAIN (Ver 1.3)

![Concept Art](https://raw.githubusercontent.com/poleaxe0224/STRAIN-Game-Design-Document/main/concept_art.jpg)
> *Concept Art: The tutorial stage at "J's Bar". Rin hides from an FBI Negotiator while managing critical Strain levels.*

> **License & Usage**
> This document is released under the **[CC BY 4.0 Attribution]** license.
> You are free to use these concepts to develop a game, practice game engine skills, or create prototypes.
> **Requirement**: If you release a project based on this GDD, please credit "Original Concept: Poleaxe".

---

## I. Overview

* **Title**: STRAIN
* **Genre**: Survival Action / Reverse Horror / Tactical Stealth
* **Tagline**: "We are not monsters. We are merely the infected witnesses."
* **Unique Selling Points (USP)**:
    1.  **Reverse Horror**: You play as the "monster" with devastating powers, hunted by elite human special forces.
    2.  **The Pacifist’s Burden**: You possess the power to destroy the world, but to prove your humanity, you **must not kill**.
    3.  **The Saviour Paradox**: To survive, you must incapacitate your hunters. But if you hurt them too badly (or if they hurt each other), you must risk your life to save them.

---

## II. Setting & Premise

### 1. The World
* **Location**: The Quarantine Zone of "New Eden City".
* **Incident**: A government bioweapon leak (Strain-ZV).
* **The Threat**: "The Cleaners"—a black-ops unit scorching the earth. They use flamethrowers and gas to "sanitize" the zone. **Standing still is death.**

### 2. The Protagonists
* Players control mutated survivors.
* **The Conflict**:
    * **Internal**: The virus is multiplying.
    * **External**: Humans are hunting you, but killing them confirms you are a monster (Game Over).

### 3. The Goal
1.  Secure **"The Patient Zero Sample"** to suppress the virus.
2.  Escape the Quarantine Zone before the virus consumes your mind, and expose the truth to the world.

---

## III. Core Gameplay & Controls

Before managing the infection, players must master movement and non-lethal engagement.

### A. Movement Logic
* **Sneak (Crouch)**: Essential state. Reduces visibility by 40% and noise radius to 0.5m.
* **Sprint**: High speed but high noise. Attracts enemies from adjacent rooms.
* **Dash (Universal Action)**: A sudden burst of directional movement to dodge attacks. **Consumes Strain.**
* **Traversal**: Characters can vault over low cover and drag unconscious bodies to hide them.

### B. Basic Actions (No Strain Cost)
* **CQC Strike (Melee)**: A quick physical attack. Damages Enemy **CP (Consciousness)**, not HP.
* **Takedown**: Context-sensitive action. Available when an enemy is unaware (Stealth) or stunned (0 CP). Instantly neutralizes the target without killing.
* **Interact / Stabilize**: Hold button to open doors, hack terminals, or **Save Dying Enemies**.

### C. Abilities (Strain Cost)
* Each character has 2 Unique Active Abilities (e.g., Shield, Phase Shift) linked to the **Strain Gauge**.
* Using these is powerful but accelerates your infection rate.

---

## IV. Core Mechanic: The STRAIN Gauge

This is your lifeline, replacing the traditional HP bar.
**UI Design**: A dynamic EKG-style monitor stretching across the top of the screen.

**Visual Reference: UI & Strain Dynamics**

[▶ Watch Video Reference: UI Focus] https://raw.githubusercontent.com/poleaxe0224/STRAIN-Game-Design-Document/main/UI%20Focus.mp4
> *Video Reference: The UI visually represents the infection level. Note the audio cues (heartbeat) accelerating as the Strain reaches Critical levels.*

### A. Rules of Strain
* **Passive Accumulation**: Increases slowly over time.
* **Active Cost**: Using abilities consumes Strain.
* **Damage Spike**: Pain accelerates viral replication.
* **Reduction**: Injecting rare **"Stabilizers"** (Scarcity Resource).

### B. The "Non-Lethal" Combat Loop
* **Design Challenge**: How to fight without killing?
* **Mechanic**: Enemies have **HP (Health)** and **CP (Consciousness Points)**.
    * **Takedowns**: Attacks deplete CP. When CP = 0, enemy is "Unconscious" (Safe).
    * **The Risk (Accidents)**: Using abilities too aggressively, environmental explosions, or **Enemy Friendly Fire** can deplete their **HP**.
    * **Reflected Danger**: Abilities like Arthur's Shield reflect bullets. If a SWAT member shoots the shield, the ricochet might critically wound his teammate. **This forces the player to save the enemy they didn't mean to hurt.**

### C. Mechanic: Bleeding Out
* **Trigger**: When a human enemy's HP hits 0 (due to Friendly Fire, Environmental Hazards, or Accidental Excessive Force).
* **State**: They enter a **"Critical"** state (Death Timer: 30s).
    * Timer hits 0 = **Moral Game Over**.

### D. Action: Stabilize

You must rush to the dying enemy and perform emergency care before time runs out.

**Visual Reference: The Stabilize Action**

[▶ Watch Video Reference: Gameplay Action] https://raw.githubusercontent.com/poleaxe0224/STRAIN-Game-Design-Document/main/Gameplay%20Action.mp4
> *Video Reference: Rin performs emergency stabilization on a downed SWAT member at J's Bar. Notice the tension: saving the enemy uses her own bio-energy, increasing her infection risk.*

* **Action**: Hold the interaction key to stabilize.
* **Choice**:
    1.  **Use Trauma Kit**: Safe, but strictly limited inventory (Max 2 slots).
    2.  **Cauterize (Strain Ability)**: Infinite use, but **Increases Strain by 15%**.
* **The Dilemma**: You are running out of **Trauma Kits**. You *must* use your own Strain (Health) to keep your hunters alive.

### E. The Strain Algorithm (Prototype Values)
*Note: Values are illustrative for prototyping and subject to balancing.*

* **Base Variables**: `Current_Strain` (0-100).
* **Action Costs**:
    * `Dash` (Universal): +3.0%
    * `Unique Ability`: +12.0%
    * `Cauterize`: +15.0% (The Moral Tax)
* **Thresholds**:
    * `> 80%`: Hallucinations / Accuracy penalty.
    * `100%`: **Viral Overload (Game Over)**.

---

## V. Game Over Conditions

1.  **Viral Overload**: STRAIN hits 100%. Subject loses control.
2.  **Proven Guilty**: A human enemy dies (Bleed out timer hits 0).
3.  **Sanitized**: Player is killed by "The Cleaners" (Failure to escape/move).

---

## VI. Characters & Progression

### A. Arthur - "The Breaker" (Tank)
* **Ability**: **Gravity Shield** - Absorbs and **reflects** kinetic energy.
    * *Risk*: Reflected bullets can accidentally kill enemies, forcing a rescue.

### B. Rin - "The Phantom" (Scout)
* **Ability**: **Phase Shift** - Moves through objects.
* **Ability**: **Disarm** - Steal magazines/grenades from enemies to prevent them from hurting themselves/others.

---

## VII. Antagonists & AI Logic

### AI Behavior: The "Friendly Fire" Risk
Enemies are aggressive and undisciplined.
* **Suppression**: Enemies will spray bullets blindly. If the player hides behind cover (or another enemy), the AI **might shoot their own squad mates**.
* **Panic**: When terrified (High Player Strain), enemies throw grenades erratically, causing environmental collapse.
* **The Player's Role**: You are a babysitter in a warzone. You must disable them before they kill themselves or you.

---

## VIII. Sample Level Design: "J's Bar"

**Objective**: Escape the incoming firebombing run (Urgency Timer).

**Dynamic Event**:
* **The Setup**: FBI Negotiator is hiding behind a pool table.
* **The Accident**: A stray bullet from a Riot Officer hits a gas pipe near the Negotiator.
* **The Crisis**: The explosion puts the Negotiator in "Critical State".
* **The Choice**:
    * The Riot Officer is still shooting at you.
    * Do you dash *through* the bullets (taking Strain) to Cauterize the Negotiator (taking more Strain)?

---

## IX. The Gameplay Loop

**Critique Response**: *Why not just stealth and do nothing?*
**Answer**: **Entropy & Scarcity.**

1.  **Entropy**: The "Strain" virus grows faster if you remain static (Stagnation).
2.  **The Hunter**: "Cleaner" units with flamethrowers sweep the map. **They can be saved**, but their heavy armor and aggressive tactics make incapacitating them extremely risky. Usually, they force the player forward into the path of regular police.
3.  **Resource Drain**: You have limited **Stabilizers**. Every second spent waiting is a second closer to Overload. You must push aggressively to find the exit.

---

## X. Credits & Acknowledgments

* **Original Concept**: Poleaxe
* **Design Consultant**: Gemini (AI)
* **Community Contributors**:
    * *kroltan (GDN)* - For identifying the "Passive Playstyle" flaw and the need for a "Non-Lethal Combat" definition.
    * *Bloodbane (GDN)* - For pointing out the lack of basic gameplay definitions (Movement & CQC).
    * *Joshthedruid2* (Reddit) - For genre clarification.
    * *WittyConsideration57* (Reddit) - For feedback on algorithm definition.

---

## XI. Technical Specifications

### A. Non-Lethal Hitbox Logic
* **Limbs**: Shooting legs = Slow (Movement Penalty).
* **Torso**: Shooting body = Stamina/CP Damage (Winded).
* **Head**: High CP Damage (Concussion/KO).
* **Lethal Threshold**: If CP Damage > 120% (Overkill), it converts to **Lethal Damage**, triggering the Bleed Out timer. *Players must be precise.*

> **Contact**
> Email: spellpluspoleaxe@gmail.com
