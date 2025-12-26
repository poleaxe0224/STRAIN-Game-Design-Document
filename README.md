# Game Design Document: STRAIN

![Concept Art](concept_art.jpg)
> *Concept Art: The tutorial stage at "J's Bar". Rin hides from an FBI Negotiator while managing critical Strain levels.*

> **License & Usage**
> This document is released under the **[CC BY 4.0 Attribution]** license.
> You are free to use these concepts to develop a game, practice game engine skills, or create prototypes.
> **Requirement**: If you release a project based on this GDD, please credit "Original Concept: Poleaxe". I am also available as a consultant for lore and mechanics if needed.

---

## I. Overview

* **Title**: STRAIN
* **Genre**: Survival Action / Reverse Horror / Stealth Puzzle
* **Tagline**: "We are not monsters. We are merely the infected witnesses."
* **Implementation Options**:
    *While the core "Strain System" is genre-agnostic, the following formats are highly recommended:*
    * **2D Tactical / Top-Down**: ideal for visualizing "Line of Sight" and coordinating team-based rescues (similar to *Hotline Miami* or *Darkwood*).
    * **Third-Person Shooter (TPS)**: Enhances the visceral connection to the character's mutation and the visual stress of the "Strain Gauge" (similar to *Control* or *Dead Space*).
    * **Action RPG**: Focuses on the management of "Strain" as a resource cost for abilities and character progression.
* **Unique Selling Points (USP)**:
    1.  **Reverse Horror**: You play as the "monster" with devastating powers, hunted by elite human special forces.
    2.  **The Pacifist’s Burden**: You possess the power to destroy the world, but to prove your humanity, you **must not kill**.
    3.  **Double Death**: You must survive without being killed, without losing control (Viral Overload), and without losing your soul (killing humans).

---

## II. Setting & Premise

### 1. The World
* **Location**: The Quarantine Zone of "New Eden City" (Near Future).
* **Incident**: A covert government bioweapon experiment leaked the **"Strain-ZV"** gas.
* **Cover-up**: The government declared it a terrorist chemical attack and deployed "The Cleaners"—a black-ops unit with one mission: **Destroy all evidence**, including the survivors.

### 2. The Protagonists
* Players control a group of survivors who inhaled the gas.
* Instead of turning into mindless zombies, they mutated, gaining superhuman abilities.
* **The Conflict**:
    * **Internal**: The virus is multiplying. Without "Antivirals," you will eventually turn into a mindless abomination.
    * **External**: Heavily armed human forces are hunting you down.

### 3. The Goal
1.  Secure the Antiviral to suppress the virus.
2.  Escape the Quarantine Zone before the virus consumes your mind, and expose the truth to the world.

---

## III. Core Mechanic: The STRAIN Gauge

This is your lifeline, replacing the traditional HP bar.
**UI Design**: A dynamic EKG-style monitor stretching across the top of the screen.

### A. Rules of Strain
* **Passive Accumulation**: Increases slowly over time (viral corruption).
* **Active Cost**: Using abilities (Dash, Shield, Jamming) consumes stamina and significantly increases Strain.
* **Damage Spike**: Taking gunfire doesn't kill you instantly, but the pain accelerates viral replication, spiking your Strain.
* **Reduction**:
    * Injecting rare "Antivirals."
    * (Performing non-lethal takedowns and absorbing bio-electricity from enemy gear.)

### B. Stages of Infection
* **Stage I: Latent (0% - 20%)**: Look human. Abilities locked. Enemy suspicion is low.
* **Stage II: Awakened (20% - 80%)**: Veins glow. Abilities unlocked. Enemies shoot on sight.
* **Stage III: Critical (80% - 99%)**: Visual/Auditory hallucinations (glitch effects). Damage doubled, but accuracy plummets.
* **Stage IV: Overload (100%)**: **GAME OVER**.

### C. Mechanic: Bleeding Out
* **Trigger**: When a human enemy takes "Lethal Damage" (e.g., stray bullets, explosions), they don't die immediately.
* **State**: They enter a **"Critical"** state, downed with a red countdown timer (e.g., 30s) above their head.
    * If timer hits 0 -> Enemy dies -> **GAME OVER (Moral Failure)**.
    * If hit again -> Enemy dies -> **GAME OVER**.

### D. Action: Stabilize
You must rush to the dying enemy and perform emergency care before time runs out.
* **Action**: Hold the interaction key to stabilize.
* **Choice**:
    1.  **Use Medkit**: Consumes a rare resource. Safe.
    2.  **Cauterize (Strain Ability)**: Use your powers (e.g., thermal sealing) to save them.
* **Cost**: Cauterizing **increases Strain by 15%**.
* **The Irony**: To save the man trying to kill you, you must push yourself closer to becoming a monster.

### E. The Strain Algorithm (System Logic)
To transition from concept to code, the Strain Gauge is defined by the following logic rules:

**1. Base Variables**
* `Current_Strain` (Float: 0.00 to 100.00)
* `Panic_Multiplier` (Float: 1.0x to 2.5x based on proximity to enemies)

**2. Passive Growth Formula**
* `Delta_Strain = (Base_Rate * Time) * Panic_Multiplier`
* *Design Note*: Base_Rate should be 0.5% per second. If an enemy is within 5 meters, Panic_Multiplier increases to 1.5x.

**3. Action Costs (Hard Data)**
| Action | Strain Cost | Cooldown | Note |
| :--- | :--- | :--- | :--- |
| **Dash / Jump** | +3.0% | 1.5s | Basic movement cost. |
| **Active Ability** | +12.0% | 8.0s | E.g., Rin's Phase, Arthur's Wall Smash. |
| **Taking Damage** | +Damage_Value * 0.5 | 0s | Pain converts directly to infection. |
| **Cauterize (Save Enemy)** | +15.0% | N/A | **The Moral Tax.** |

**4. Threshold Triggers**
* `IF Current_Strain > 80%`: Apply screen shader (Glitch_Effect); Audio pitch shift +10%; Accuracy -40%.
* `IF Current_Strain >= 100%`: Trigger Function `GameOver_ViralOverload()`.

---

## IV. Game Over Conditions

Failure comes in two forms: Physical collapse or Moral collapse.

### Condition A: Viral Overload
* **Trigger**: STRAIN Gauge hits **100%**.
* **Outcome**: The protagonist loses all sanity, transforms into a monstrosity, and slaughters their team.
* **Screen Text**: "SUBJECT LOST".

### Condition B: Moral Event Horizon
* **Trigger**: **Any human enemy dies** (HP reaches 0).
* **Outcome**: In fighting to prove you aren't a monster, you proved the government right.
* **Sequence**: Time Freeze -> Zoom in on corpse -> Protagonist screams -> STRAIN instantly fills to 100% -> Overload.
* **Screen Text**: "PROVEN GUILTY".

## VI-A. Sample Level Design: "J's Bar" (Tutorial Stage)

**Objective**: Escape through the back alley.
**Constraint**: The back door is locked. Keycard is on the "FBI Negotiator".

**Zone Layout & Item Placement**:
1.  **The Front Area (Entry)**:
    * *Enemy*: 2x Riot Police (Patrolling).
    * *Item*: **Stabilizer (Small)** - Located behind the counter. Reduces Strain by 10%.
2.  **The Corridor (Choke Point)**:
    * *Hazard*: Security Camera. If spotted -> Spawns 1x PMC Merc.
    * *Tactic*: Use Dr. Aris to hack the camera OR Rin to phase through the blind spot.
3.  **The VIP Room (Boss Arena)**:
    * *Boss*: **FBI Negotiator**. He takes cover behind the pool table.
    * *Dynamic Event*: A ceiling fan is wobbling above him.
    * *Solution (Non-Lethal)*:
        1.  Wait for him to reload.
        2.  Shoot the fan support -> Fan falls -> Traps him (Pinned).
        3.  Steal Keycard -> Run.
    * *Failure State*: If you shoot him -> He enters "Bleeding Out" -> You must stabilize him within 20s or Game Over.

---

## V. Characters (The Infected)

**Visuals**: Pale skin. Glowing **Cyan/Neon Blue** liquid leaks from wounds instead of blood.

### A. Arthur - "The Breaker"
* **Background**: Construction Worker.
* **Visuals**: Right arm heavily mutated with exoskeleton-like plating. Cracks glow red.
* **Abilities**:
    * **Gravity Shield**: Deploys a one-way shield to block bullets and push forward.
    * **Breach**: Smash through walls to create shortcuts.
* **Tactics**: Hold the shield to protect teammates who are stabilizing a dying enemy.
* **Weakness**: Slow movement, large hitbox.

### B. Rin - "The Phantom"
* **Background**: Track & Field Athlete.
* **Visuals**: **Fiber Optic Veins**. Her legs pulse with bioluminescent blue light.
    * *Feedback*: Dim when calm; Blindingly bright when dashing; Glitching when critical.
* **Abilities**:
    * **Phase Dash**: High-speed dash that leaves a decoy trail.
    * **Ghosting**: Can phase through thin walls or enemies during dash.
* **Weakness**: Low health, easily suppressed.

### C. Dr. Aris - "The Silencer"
* **Background**: Betrayed Scientist.
* **Visuals**: Mutated eye, wears a digital monocle to suppress sensory overload.
* **Abilities**:
    * **Neuro Pulse**: EMP blast that jams enemy guns and blinds night-vision goggles.
    * **Hacker Vision**: See traps and circuits through walls.
* **Passive**: **Combat Medic**—Stabilizes enemies 50% faster and sees exact bleed-out timers.
* **Weakness**: Weak melee, Strain increases faster.

---

## VI. Antagonists

Enemies are tactical human squads. They prioritize cover, flanking, and suppression.

### Units
* **Riot Police**: Hesitant. Use tasers/pistols.
* **PMC Mercs**: Standard threat. Use suppression fire.
* **The Cleaners**: Elite units in HAZMAT suits. Immune to standard melee.

### Boss Fights (Survival Puzzles)
1.  **FBI Negotiator** (J's Bar Entrance)
    * *Mechanic*: Deadeye aim.
    * *Strategy*: Push forward using cover. **Escape** while he reloads.
2.  **CIA Spook** (Dark Hospital)
    * *Mechanic*: Cuts power, uses flashbangs and C4.
    * *Strategy*: Use thermal vision to find traps. Shoot sprinklers to reveal laser tripwires. **Activate elevator to escape**.
3.  **KGB Brute** (Underground Lab)
    * *Mechanic*: Heavy armor, flamethrower.
    * *Strategy*: Lure under a suspended cargo container. Shoot the chain. **Trap him (Non-lethal)**.

---

## VII. The Gameplay Loop

> **"The fight is just the beginning. Redemption is the end."**

1.  **Conflict**: Engage squad. Use powers to knock them out.
2.  **Accident**: A stray bullet hits a gas tank. Explosion injures a Cleaner.
3.  **Crisis**: Cleaner enters "Bleeding Out" state. Alarm blares.
4.  **Dilemma**:
    * Do you rush into crossfire to save him?
    * Or finish the objective (and risk running out of time)?
5.  **Cost**: Rin dashes in, using Strain to cauterize the wound. The enemy lives, but Rin is now at 95% Strain—on the brink of madness.
6.  **Restriction**: **Conscience Lock**—Exit doors will NOT open if there is a dying enemy in the area.

---

## VIII. Atmosphere

* **Visual Style**: Cold tones (Blue/Grey/Black) + High Contrast Neon Red (Enemy Lasers/UI).
* **UI**: Minimalist, Medical Monitor aesthetic with digital glitch effects.
* **Audio**:
    * *Ambience*: Low-frequency heartbeat (speeds up with Strain).
    * *Enemy*: Tactical radio chatter, static.

---
## IX. Credits & Acknowledgments

* **Original Concept**: Poleaxe
* **Design Consultant**: Gemini (AI)
* **Community Contributors**:
    * *Joshthedruid2* (Reddit) - For genre clarification and tactical feedback.
    * *WittyConsideration57* (Reddit) - For critical feedback on algorithm definition and implementation feasibility, pushing this project from abstract idea to concrete specification.

> **Contact**
> If you are interested in developing this project, feel free to reach out via Email (spellpluspoleaxe@gmail.com). I am happy to provide further consultation on mechanics and lore.
