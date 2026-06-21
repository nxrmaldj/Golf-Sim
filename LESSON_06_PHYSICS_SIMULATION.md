# Lesson 6: Physics Simulation vs Manual Movement

**Project:** GolfSim  
**Prerequisite:** Lesson 5 (`BP_MovingBall` with Velocity, Bounciness, manual bounce)  
**Goal:** Build a second ball Blueprint that uses Unreal's physics engine instead of your manual Velocity — so you understand both approaches and when to use each.

**Plain English:** So far **you** were the referee (manual Velocity). Now Unreal becomes the referee (**Simulate Physics**). You compare both side by side.

---

## Part A — What You Are Building (Overview)

By the end of this lesson you will have:

1. **`BP_MovingBall`** — unchanged manual ball (Lessons 1–5).
2. **`BP_PhysicsBall`** — a duplicate that uses **Simulate Physics** + **Add Impulse** on launch.
3. Both balls in `Learning_Map` so you can test manual vs physics in the same level.

**End result:** Press launch on each ball. Manual ball bounces using your **Reflect / Bounciness** nodes. Physics ball bounces using engine **Restitution** on materials — no Reflect node needed.

---

## Part B — New Words

| Word | Simple meaning |
|------|----------------|
| **Simulate Physics** | Checkbox on a mesh — Unreal moves it with gravity, collisions, and bounce automatically. |
| **Restitution** | How bouncy a **Physical Material** is (0 = no bounce, 1 = perfect bounce). Engine version of Bounciness. |
| **Impulse** | A one-time push — like striking the ball once, not pushing every frame. |
| **Add Impulse** | Blueprint node that shoves the ball at launch; physics takes over after that. |
| **Linear Damping** | Air resistance built into the mesh — slows the ball over time. |

---

## Part C — Blueprint Wires (Quick Reminder)

> - **White** = execution order. **Colored** = data.
> - **Get** nodes = drag variable from **My Blueprint** into the graph.
> - **Branch True** = the **white output** when condition is true — put movement nodes after that pin, not mixed into False.

## Part D — Build Section 1: Duplicate the Blueprint

### What we're building now
A copy of your manual ball so we can experiment without breaking Lesson 5 work.

### What you need first
- `BP_MovingBall` working from Lesson 5.

### Build steps

#### Step 1 — Duplicate the Blueprint

> 1. In **Content Browser** → `Content/Blueprints/` → right-click `BP_MovingBall` → **Duplicate**.
> 2. Rename to **`BP_PhysicsBall`**.
> **What this does:** Creates an independent copy. Changes here won't break your manual ball.
> **Why:** You need both versions in the level to compare behavior.

#### Step 2 — Open `BP_PhysicsBall`

> 1. Double-click to open the Blueprint editor.
---

### Plain English summary
You now have two ball Blueprints: the original manual one and a copy we'll turn into a physics ball.

### Test
`BP_PhysicsBall` appears in Content Browser with the new name.

---

## Part E — Build Section 2: Turn On Simulate Physics

### What we're building now
Tell Unreal's physics engine to control the ball mesh instead of your Event Tick movement.

### Variables to create in `BP_PhysicsBall`

| Name | Type | Default | Why |
|------|------|---------|-----|
| `bUseManualMovement` | **Boolean** | `false` | Switch to turn off Tick movement (physics ball won't use it). |

> You can skip this variable and simply **delete or disable** the Event Tick movement chain later — the bool is optional but helps you see the pattern.

### Build steps

#### Step 1 — Enable Simulate Physics on the mesh

> 1. **Components** panel → select **Static Mesh** (sphere).
> 2. **Details** panel → search **Simulate Physics** → check **Simulate Physics**.
> **What this does:** The mesh becomes a physics object. Gravity pulls it down. Collisions bounce it using material settings.
> **Why:** This is the core switch from manual to engine-controlled movement.

#### Step 2 — Set collision for physics

> 1. Still on **Static Mesh** → **Collision Presets** → **PhysicsActor** (or **BlockAllDynamic**).
> 2. **Mobility** → **Movable**.
> **What this does:** Physics objects must be **Movable** and use a preset that allows physical simulation.
> **Why:** A **Static** mesh ignores physics forces.

#### Step 3 — Tune Linear Damping (optional)

> 1. **Details** → **Linear Damping** → try `0.5` to `1.0`.
> **What this does:** Slows the ball in the air and on the ground like air drag.
> **Why:** Without damping, the ball can roll forever on a flat floor.
---

### Plain English summary
The sphere is now a real physics object. Unreal will move it — not your Velocity nodes.

### Test
**Compile** + **Save**. Do not press Play yet — Tick movement would fight physics if still connected.

---

## Part F — Build Section 3: Disable Manual Tick Movement

### What we're building now
Stop your Lesson 5 Event Tick movement from fighting the physics engine.

### Build steps

#### Step 1 — Find your Event Tick chain

> 1. **Event Graph** → locate **Event Tick** → **Branch (bIsMoving)** → movement nodes.

#### Step 2 — Disable manual movement

> **Option A (simple):** Select all movement nodes on the **Branch → True** white path → **Delete**.
> **Option B (safer):** Add **Branch** before movement:
> 1. Drag **`bUseManualMovement`** from **My Blueprint** → **Get**.
> 2. Add **Branch** — **Condition** (colored) ← **Get bUseManualMovement**.
> 3. Move your entire manual movement chain to the **True** pin only.
> 4. Leave **False** empty.
> 5. Set **`bUseManualMovement`** default to **`false`** on the physics ball.
> **What the Branch does:** Only runs manual movement when the bool is true. False = physics handles everything.
> **Why:** Two systems moving the same ball at once causes jitter, explosions, or weird speeds.

#### Step 3 — Disable Simulate Physics at start (important)

> Physics balls should start **asleep** until you launch.
> 1. **Static Mesh** → **Details** → **Simulate Physics** = on, but add this to **Event BeginPlay**:
> **Nodes:**
> 1. **Event BeginPlay**
> 2. **Set Simulate Physics** (target = Static Mesh) → **Simulate** = **false**
> **What Set Simulate Physics does:** Turns physics on/off at runtime.
> **Why:** Ball should sit still until the player presses launch — same as Lesson 3.
---

### Plain English summary
Manual Tick movement is off. The ball waits at rest until you launch it with an impulse.

---

## Part G — Build Section 4: Launch With Add Impulse

### What we're building now
On key press, wake the physics ball and shove it once — like hitting it with a club.

### Variables to create (if not already present)

| Name | Type | Default | Why |
|------|------|---------|-----|
| `LaunchImpulse` | **Vector** | `(50000, 0, 30000)` | One-time push direction + strength. Physics uses different scale than manual Speed — tune by eye. |

> **Note:** Impulse values are much larger than manual Velocity because **Add Impulse** applies force mass-scaled. Start big, reduce if the ball barely moves.

### Build steps

#### Step 1 — Reuse your launch input from Lesson 3

> 1. Find your **Input Action** / **Space bar** event (same as `BP_MovingBall`).

#### Step 2 — Enable physics on launch

> Connect in this order:
> 1. **Input Action** (Launch / Space)
>    ↓
> 2. **Set Simulate Physics** (target = Static Mesh) → **Simulate** = **true**
> **What Set Simulate Physics does here:** Wakes the ball so physics can affect it.
> **Why:** Ball was frozen on BeginPlay; launch turns the engine on.
>    ↓
> 3. **Add Impulse** (target = Static Mesh)
> **Pin connections:**
> - **Impulse** pin ← **Get LaunchImpulse** (drag **`LaunchImpulse`** from **My Blueprint** → **Get**, or use **Make Vector**)
> - **Vel Change** = check **true** (ignores mass — easier to tune for beginners)
> **What Add Impulse does:** Applies a one-frame shove. After that, gravity, collisions, and restitution take over.
> **Why:** Golf is one strike, not continuous pushing every frame. **Add Impulse** matches that idea.
>    ↓
> 4. (Optional) **Set bIsMoving** = true — only if other logic still reads it.

#### Step 3 — Compile + Save

> Click **Compile** + **Save** in the Blueprint toolbar.

---

### Plain English summary
Press Space → physics turns on → ball gets one push → Unreal handles the rest (fall, bounce, roll).

### Test

1. Place **`BP_PhysicsBall`** in `Learning_Map`.
2. **Play** → ball sits still.
3. Press launch key → ball flies, falls, bounces off floor/walls.
4. Bounces come from **Physical Material Restitution**, not your Reflect nodes.

---

## Part H — Build Section 5: Compare Manual vs Physics in the Level

### What we're building now
Side-by-side test so you feel the difference.

### Build steps

#### Step 1 — Place both actors

> 1. Drag **`BP_MovingBall`** and **`BP_PhysicsBall`** into the level, spaced apart.
> 2. Same floor/walls from Lesson 4.

#### Step 2 — Observe

> | | Manual (`BP_MovingBall`) | Physics (`BP_PhysicsBall`) |
> |---|--------------------------|------------------------------|
> | Who moves the ball? | Your Event Tick + Velocity | Unreal physics engine |
> | Bounce | Your **Bounciness** + axis flip / Reflect | **Physical Material** Restitution |
> | Gravity | You add to Velocity Z (Lesson 7) | Automatic |
> | Predictability | High — you control every line | Medium — harder to match exact Mevo+ numbers |
> | Best for GolfSim | **Flight** when Mevo+ sends exact speed/angle | **Rolling**, piles of objects, bumps |

#### Step 3 — Optional: Physical Material

> 1. Content Browser → right-click → **Physics** → **Physical Material**.
> 2. Name `PM_Bouncy` → **Restitution** = `0.6`.
> 3. Assign to floor mesh → compare bounce feel.
> **What Physical Material does:** Defines surface friction and bounciness for physics objects.
> **Why:** Same as Bounciness in Lesson 5, but for the engine path.
---

### Plain English summary
Manual = you write the rules. Physics = Unreal writes the rules. GolfSim will likely use **manual/projectile for flight** and **physics or custom friction for roll** (Lesson 9).

---

## Part I — Quick Check

1. When would you turn **Simulate Physics** **off** during a scripted golf shot?
2. What does **Add Impulse** do **once** vs your Event Tick movement **every frame**?
3. If the ball jitters or flies away, what two systems might be fighting each other?

---

## Part J — Git

```powershell
cd "C:\Users\Darre\Documents\Unreal Projects\GolfSim"
git add .
git commit -m "feat: lesson 6 physics ball experiment"
git push
```

---

## Troubleshooting

| Problem | Fix |
|---------|-----|
| Ball falls through floor | Mesh must be **Root** (Lesson 4). Floor needs collision. |
| Ball doesn't move on launch | Increase **LaunchImpulse**. Check **Vel Change** = true. |
| Ball explodes / spins wildly | Manual Tick still moving — disable Event Tick movement chain. |
| Ball never stops rolling | Increase **Linear Damping** or ground friction on Physical Material. |
| No bounce | Assign Physical Material with **Restitution** > 0 to floor. |

