# Lesson 8: Projectile Movement Component

**Project:** GolfSim  
**Prerequisite:** Lesson 7 (you understand gravity, launch angle, and flight arc concept)  
**Goal:** Replace hand-rolled Tick gravity with UE's **Projectile Movement** component for cleaner flight.

**Plain English:** Unreal has a pre-built "flying object" component. You give it starting speed; it handles movement, gravity, and optional bounce — less graph wiring than Lesson 7.

Maps to **Phase 2** in your roadmap.

---

## Part A — What You Are Building (Overview)

By the end of this lesson you will have:

1. **Projectile Movement** component added to `BP_MovingBall` (or a copy).
2. **`bInFlight`** bool to switch between **projectile flight** and **ground roll** (Lesson 9).
3. Manual Event Tick movement **disabled during flight**.
4. Launch sets **Initial Velocity** on the component instead of your manual gravity loop.

**End result:** Press launch → Projectile Movement flies the ball → on landing you turn it off and hand off to roll logic later.

---

## Part B — New Words

| Word | Simple meaning |
|------|----------------|
| **Component** | Extra piece you attach to an Actor (like adding a motor to a toy). |
| **Projectile Movement** | UE component built for bullets, balls, arrows in flight. |
| **Initial Speed / Velocity** | Starting motion when projectile activates. |
| **Gravity Scale** | Multiplier on world gravity. `1` = normal, `0` = no fall, `2` = heavy fall. |
| **Updated Component** | Which part of the Actor the projectile moves (usually root or mesh). |
| **Should Bounce** | Lets projectile bounce off surfaces using its own bounce setting. |
| **Is Active** | Whether the component is currently driving movement. |

---

## Part C — Blueprint Wires (Quick Reminder)

> - **White** = execution. **Colored** = data.
> - **bInFlight**, **bIsMoving** → **Get** from **My Blueprint** → **Branch Condition** (colored bool).
> - **Projectile Movement** nodes: drag from **Components** pin or search with component reference — not from Event Tick white pin.

## Part D — Build Section 1: Add the Component

### What we're building now
Attach Unreal's flight component to your ball Blueprint.

### What you need first
- `BP_MovingBall` with Lessons 3–7 logic (launch input, collision, optional manual arc).

### Variables to create

| Name | Type | Default | Why |
|------|------|---------|-----|
| `bInFlight` | **Boolean** | `false` | True = projectile controls ball. False = manual/roll controls ball. |

### Build steps

#### Step 1 — Add Projectile Movement

> 1. Open **`BP_MovingBall`** → **Components** panel → **Add** → search **Projectile Movement** → add it.
> **What this component does:** Each frame it moves the owner, applies gravity, handles bounce settings, and can stop on hit.
> **Why:** Less custom Tick code than Lesson 7; tuned for flying objects.

#### Step 2 — Configure component defaults

> Select **Projectile Movement** → **Details**:
> | Setting | Suggested value | Why |
> |---------|-----------------|-----|
> | **Updated Component** | Your **Static Mesh** or **Root** | Which transform gets moved. |
> | **Initial Speed** | `0` | We set velocity manually on launch. |
> | **Gravity Scale** | `1.0` | Normal fall — tune later. |
> | **Should Bounce** | `true` (optional) | Simple bounce without Reflect nodes. |
> | **Bounciness** | `0.4` | Similar idea to Lesson 5 — tune by feel. |
> | **Is Active** | `false` | Don't fly until launch. |
> **What Is Active = false does:** Component exists but doesn't move the ball until you activate it.
> **Why:** Ball should rest until player launches (Lesson 3 behavior).

#### Step 3 — Compile + Save

> Click **Compile** + **Save** in the Blueprint toolbar.

---

### Plain English summary
The ball now has a flight engine attached but sleeping until launch.

---

## Part E — Build Section 2: Gate Manual Tick Movement

### What we're building now
Stop your Lesson 7 manual gravity/movement from fighting Projectile Movement.

### Build steps

#### Step 1 — Wrap manual Tick chain in a Branch

> 1. Find **Event Tick** → **Branch (bIsMoving)** → manual movement + gravity nodes.

#### Step 2 — Add second condition

> 1. Add **Branch** after **bIsMoving True** (or combine with **AND**):
> **Option A — Two branches in sequence:**
> ```
> Event Tick
>   → Branch (bIsMoving)
>       True → Branch (bInFlight)
>           False → [your Lesson 7 manual gravity + move chain]
>           True → (empty — projectile handles it)
>       False → (nothing)
> ```
> **What the bInFlight Branch does:** Manual Tick only runs when **`bInFlight` is false**.
> **Why:** When projectile is flying, only one system should move the ball.

#### Step 2 — Set bInFlight on BeginPlay

> 1. **Event BeginPlay** → **Set bInFlight** = **false**.
> **Why:** Ball starts on ground / idle.

#### Step 3 — Compile + Save

> Click **Compile** + **Save** in the Blueprint toolbar.

---

### Plain English summary
Manual movement runs only when the ball is not in projectile flight mode.

---

## Part F — Build Section 3: Launch Using Projectile Movement

### What we're building now
On key press, activate the component and set its starting velocity from **ShotSpeed** + **LaunchAngle** (Lesson 7 variables).

### What you need first
- **`ShotSpeed`** and **`LaunchAngle`** from Lesson 7 (or create them now — same defaults: `1200`, `30`).

### Build steps

#### Step 1 — Open launch input event

> Same **Space / Launch** input from Lesson 3.

#### Step 2 — Compute launch velocity (same math as Lesson 7)

> 1. **Deg Cos(LaunchAngle) × ShotSpeed** → horizontal
> 2. **Deg Sin(LaunchAngle) × ShotSpeed** → vertical
> 3. **Make Vector** (X = horizontal, Y = 0, Z = vertical)
> **What this does:** Same launch direction as Lesson 7 — keeps learning consistent.
> **Why:** Later Mevo+ data maps to these same two numbers.

#### Step 3 — Activate projectile and set velocity

> Connect in order:
> 1. **Set bInFlight** = **true**
> **What this does:** Tells Tick graph "projectile is in control — don't run manual movement."
> **Why:** Prevents double movement.
>    ↓
> 2. **Set Velocity in Local Space** (target = **Projectile Movement** component)
> **Pin connections:**
> - **New Velocity** ← **Make Vector** from step 2
> **What Set Velocity in Local Space does:** Sets the projectile's current speed and direction.
> **Why:** This replaces **Set Velocity** on your manual variable for the flight phase.
>    ↓
> 3. **Activate** (target = **Projectile Movement**) — or **Set Active** = true
> **What Activate does:** Turns on the component — it starts applying gravity and moving the ball.
> **Why:** Sleeping component doesn't move until activated.
>    ↓
> 4. **Set bIsMoving** = **true** (if other logic still uses it)

#### Step 4 — Compile + Save

> Click **Compile** + **Save** in the Blueprint toolbar.

---

### Plain English summary
Launch → flight mode on → projectile gets speed + angle → component flies the ball with built-in gravity.

### Test

1. **Play** → press launch.
2. Ball should arc through the air without your Lesson 7 Tick gravity running.
3. Tune **Gravity Scale** and **Bounciness** on the component — see changes without new nodes.

---

## Part G — Build Section 4: Detect Landing (Setup for Lesson 9)

### What we're building now
Know when flight ended so you can switch to roll mode in Lesson 9.

### Build steps

#### Step 1 — Use On Projectile Stop event

> 1. **Components** → select **Projectile Movement**.
> 2. **Details** → scroll to **Events** → **On Projectile Stop** → click **+** to add to graph.
> **What On Projectile Stop does:** Fires when the projectile considers the ball stopped or hit ground (depends on settings).
> **Alternative:** Use your Lesson 4 **Sweep Hit** / **On Hit** on floor — often more reliable for golf.

#### Step 2 — End flight mode

> On **On Projectile Stop** (or floor **On Hit**):
> 1. **Set bInFlight** = **false**
> 2. **Deactivate** (target = **Projectile Movement**) — or **Set Active** = false
> **What Deactivate does:** Stops the component from moving the ball.
> **Why:** Lesson 9 manual roll or physics roll takes over on the ground.
> 3. (Optional) **Set bIsRolling** = **true** — you'll use this in Lesson 9.

#### Step 3 — Compile + Save

> Click **Compile** + **Save** in the Blueprint toolbar.

---

### Plain English summary
When the ball lands, projectile turns off — ready for ground roll next lesson.

---

## Part H — Manual vs Projectile (Reference)

| | Manual Velocity (L5–7) | Projectile Movement (L8) |
|---|------------------------|---------------------------|
| Learning value | See every node | Faster, fewer nodes |
| Golf tuning | You control every line | Tune fewer properties |
| Mevo+ later | Map JSON → your variables | Map JSON → Initial Velocity |
| **GolfSim recommendation** | Learn first ✓ | Use for polished **flight phase** |

---

## Part I — Quick Check

1. What does **Gravity Scale = 0** do?
2. Why must **Is Active** start false?
3. Why deactivate Projectile Movement after landing?
4. What would happen if manual Tick gravity still ran while **bInFlight** is true?

---

## Part J — Git

```powershell
cd "C:\Users\Darre\Documents\Unreal Projects\GolfSim"
git add .
git commit -m "feat: lesson 8 projectile movement component"
git push
```

---

## Troubleshooting

| Problem | Fix |
|---------|-----|
| Ball doesn't move on launch | **Activate** called? Velocity set **before** or **with** activate? |
| Double speed / jitter | Manual Tick still running — check **bInFlight** Branch. |
| Ball falls through floor | **Updated Component** must be colliding mesh; Sweep/collision from L4. |
| No arc | **Gravity Scale** > 0; velocity Z not zero. |
| On Projectile Stop never fires | Use floor **On Hit** instead; tune **Stop Simulation On Hit**. |

