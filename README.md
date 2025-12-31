# Game Design Document: STRAIN (Ver 1.1)

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

---

## III. Core Mechanic: The STRAIN Gauge

**UI Design**: A dynamic EKG-style monitor replacing the HP bar.

### A. Rules of Strain
* **Passive Accumulation**: Increases slowly over time.
* **Active Cost**: Using abilities consumes Strain.
* **Damage Spike**: Pain accelerates viral replication.
* **Reduction**: Injecting rare "Antivirals" (Scarcity Resource).

### B. The "Non-Lethal" Combat Loop (Updated)
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
You must rush to the dying enemy and perform emergency care.
* **Choice**:
    1.  **Use Medkit**: Safe, but strictly limited inventory (Max 2 slots).
    2.  **Cauterize (Strain Ability)**: Infinite use, but **Increases Strain by 15%**.
* **The Dilemma**: You are running out of Medkits. You *must* use your own Strain (Health) to keep your hunters alive.

### E. The Strain Algorithm (Prototype Values)
*Note: Values are illustrative for prototyping and subject to balancing.*

* **Base Variables**: `Current_Strain` (0-100).
* **Action Costs**:
    * `Dash`: +3.0%
    * `Ability`: +12.0%
    * `Cauterize`: +15.0% (The Moral Tax)
* **Thresholds**:
    * `> 80%`: Hallucinations / Accuracy penalty.
    * `100%`: **Viral Overload (Game Over)**.

---

## IV. Game Over Conditions

1.  **Viral Overload**: STRAIN hits 100%. Subject loses control.
2.  **Proven Guilty**: A human enemy dies (Bleed out timer hits 0).
3.  **Sanitized**: Player is killed by "The Cleaners" (Failure to escape/move).

---

## V. Characters & Progression

### A. Arthur - "The Breaker" (Tank)
* **Ability**: **Gravity Shield** - Absorbs and **reflects** kinetic energy.
    * *Risk*: Reflected bullets can accidentally kill enemies, forcing a rescue.

### B. Rin - "The Phantom" (Scout)
* **Ability**: **Phase Dash** - Moves through objects.
* **Ability**: **Disarm** - Steal magazines/grenades from enemies to prevent them from hurting themselves/others.

---

## VI. Antagonists & AI Logic

### AI Behavior: The "Friendly Fire" Risk
Enemies are aggressive and undisciplined.
* **Suppression**: Enemies will spray bullets blindly. If the player hides behind cover (or another enemy), the AI **might shoot their own squad mates**.
* **Panic**: When terrified (High Player Strain), enemies throw grenades erratically, causing environmental collapse.
* **The Player's Role**: You are a babysitter in a warzone. You must disable them before they kill themselves or you.

---

## VII. Sample Level Design: "J's Bar"

**Objective**: Escape the incoming firebombing run (Urgency Timer).

**Dynamic Event**:
* **The Setup**: FBI Negotiator is hiding behind a pool table.
* **The Accident**: A stray bullet from a Riot Officer hits a gas pipe near the Negotiator.
* **The Crisis**: The explosion puts the Negotiator in "Critical State".
* **The Choice**:
    * The Riot Officer is still shooting at you.
    * Do you dash *through* the bullets (taking Strain) to Cauterize the Negotiator (taking more Strain)?

---

## VIII. The Gameplay Loop (Addressed)

**Critique Response**: *Why not just stealth and do nothing?*
**Answer**: **Entropy & Scarcity.**

1.  **Entropy**: The "Strain" virus grows faster if you remain static (Stagnation). Movement flushes the system.
2.  **The Hunter**: "Cleaner" units with flamethrowers sweep the map. They cannot be saved; they must be run from. They force the player forward into the path of regular police.
3.  **Resource Drain**: You have limited Antivirals. Every second spent waiting is a second closer to Overload. You must push aggressively to find the exit.

---

## IX. Credits & Acknowledgments

* **Original Concept**: Poleaxe
* **Design Consultant**: Gemini (AI)
* **Community Contributors**:
    * *kroltan (GDN)* - For identifying the "Passive Playstyle" flaw and the need for a "Non-Lethal Combat" definition.
    * *Joshthedruid2* (Reddit) - For genre clarification.
    * *WittyConsideration57* (Reddit) - For feedback on algorithm definition.

---

## X. Technical Specifications

### A. Non-Lethal Hitbox Logic
* **Limbs**: Shooting legs = Slow (Movement Penalty).
* **Torso**: Shooting body = Stamina/CP Damage (Winded).
* **Head**: High CP Damage (Concussion/KO).
* **Lethal Threshold**: If CP Damage > 120% (Overkill), it converts to **Lethal Damage**, triggering the Bleed Out timer. *Players must be precise.*

> **Contact**
> Email: spellpluspoleaxe@gmail.com
