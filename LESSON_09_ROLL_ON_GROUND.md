# Lesson 9: Roll on the Ground

**Project:** GolfSim  
**Prerequisite:** Lesson 8 (ball lands after flight — **bInFlight** turns false, Projectile deactivated)  
**Goal:** After landing, ball **rolls horizontally**, slows from **friction**, and stops — golf **roll-out**.

**Plain English:** Flight is over. The ball skids/rolls on the fairway and comes to rest.

---

## Part A — What You Are Building (Overview)

By the end of this lesson you will have:

1. **`bIsRolling`** bool — flight is done, roll logic runs.
2. **`Friction`** float — how fast horizontal speed bleeds off.
3. **Landing detection** — floor hit or projectile stop sets roll mode.
4. **Event Tick roll loop** — shrink Velocity X/Y until ball stops.

**End result:** Launch → arc (Lesson 8) → touch floor → roll forward → slow → stop.

---

## Part B — New Words

| Word | Simple meaning |
|------|----------------|
| **Roll-out** | Distance the ball travels **after** landing. |
| **Friction** | Resistance from grass/sand — slows horizontal motion. |
| **Clamp / Zero out Z** | Force up/down speed to 0 on ground so ball doesn't bounce forever vertically. |
| **Speed threshold** | Tiny number — when slower than this, stop completely (avoid infinite creep). |
| **Vector Length** | One number for "how fast" regardless of direction. |

---

## Part C — Blueprint Wires (Quick Reminder)

> - **White** = execution. **Colored** = data.
> - **Get bInFlight**, **Get Friction**, etc. = drag from **My Blueprint**.
> - **Get Velocity** on landing = from **Projectile Movement** component (drag component reference into graph), not your manual Velocity variable — unless you copied it in Step 4 below.

## Part D — Build Section 1: Add Roll Variables

### What we're building now
State flags and tuning numbers for ground roll.

### What you need first
- Lesson 8 complete: **bInFlight**, Projectile deactivated on land.
- Manual **Velocity** variable still exists on `BP_MovingBall` (from Lesson 5/7).

### Variables to create

| Name | Type | Default | Why |
|------|------|---------|-----|
| `bIsRolling` | **Boolean** | `false` | True = run roll friction in Tick. |
| `Friction` | **Float** | `2.0` | Higher = stops faster. Tune like grass vs sand later. |
| `StopSpeed` | **Float** | `10.0` | When slower than this, kill movement completely. |

### Build steps

#### Step 1 — Create all three variables

> 1. **My Blueprint** → **+ Variable** for each row in the table above.
> **What booleans do:** Turn entire logic sections on/off — same pattern as **bIsMoving** and **bInFlight**.
> **Why separate bIsRolling:** Flight, idle, and roll are three different phases — one bool each keeps the graph readable.

#### Step 2 — Compile + Save

> Click **Compile** + **Save** in the Blueprint toolbar.

---

### Plain English summary
You now have a "rolling phase" switch and two tuning dials.

---

## Part E — Build Section 2: Detect Landing and Enter Roll Mode

### What we're building now
When the ball touches the floor after flight, capture horizontal speed and start rolling.

### Build steps

#### Step 1 — Choose your landing event

> Use **one** of these (floor **On Hit** is most reliable for golf):
> **Option A — On Component Hit (floor)** — recommended
> 1. **Event Graph** → your **On Hit** / **Event Hit** from Lesson 4/5.
> **Option B — On Projectile Stop** from Lesson 8
> 1. Already added to **Projectile Movement** — use if it fires reliably on your floor.

#### Step 2 — Check we're landing from flight (optional but good)

> 1. From hit event → **Branch** → **Condition** = **NOT bInFlight** OR **bInFlight was true before deactivate**
> Simpler approach: on hit, if **bInFlight** is still true, you're landing from air.
> 1. **Branch** — **Condition** = **Get bInFlight**
> **What Branch does:** Only runs landing logic when coming from flight — ignores wall taps during idle tests.
> **Why:** Prevents roll mode triggering at wrong times.

#### Step 3 — End flight (if not already)

> On **True** path:
> 1. **Set bInFlight** = **false**
> 2. **Deactivate** → **Projectile Movement**
> **What Deactivate does:** Ensures projectile won't fight roll logic.
> **Why:** Only one movement system at a time.

#### Step 4 — Copy velocity into manual roll

> When using Projectile Movement, read its velocity at landing:
> 1. **Get Velocity** (target = **Projectile Movement**) — or **Get Last Update Velocity**
> **What this does:** Grabs how fast the ball was moving when it hit ground.
> **Why:** Roll continues from landing speed — realistic roll-out.
> 2. **Break Vector** → get **X**, **Y**, **Z**
> 3. **Make Vector** — **X** and **Y** unchanged, **Z** = **`0`**
> **What zeroing Z does:** Ball stays on ground — no continued bouncing upward in roll phase.
> **Why:** Roll is horizontal on fairway; vertical bounce was flight phase.
> 4. **Set Velocity** ← Make Vector (your manual variable from Lesson 5)

#### Step 5 — Enter roll state

> 1. **Set bIsRolling** = **true**
> 2. **Set bIsMoving** = **true** (if Branch still gates Tick)

#### Step 6 — Compile + Save

> Click **Compile** + **Save** in the Blueprint toolbar.

---

### Plain English summary
Touch floor after flight → projectile off → horizontal speed saved → Z cleared → roll mode on.

### Test
Partial test: launch, land — **bIsRolling** should become true (watch with **Print String** if needed).

---

## Part F — Build Section 3: Roll Friction in Event Tick

### What we're building now
Each frame while rolling, slow X and Y until the ball stops.

### Build steps

#### Step 1 — Add roll Branch to Event Tick

> Structure:
> ```
> Event Tick
>   → Branch (bIsMoving)
>       True → Branch (bInFlight)
>           True → (empty — projectile flies)
>           False → Branch (bIsRolling)
>               True → [roll friction chain below]
>               False → [optional idle / manual move]
> ```
> **What nested Branches do:** Each bool picks exactly one "mode" per frame.
> **Why:** Clean separation — you can replicate this pattern for sand vs fairway later.

#### Step 2 — Get Velocity

> Inside **bIsRolling True**:
> 1. **Get Velocity**

#### Step 3 — Apply friction to X and Y

> For each axis (X and Y separately):
> 1. **Break Vector** → **X** and **Y**
> 2. **Multiply**: **X** × **(1 - Friction × Delta Seconds)**
> **How to build `(1 - Friction × Delta Seconds)`:**
> - **Get Friction**
> - **Delta Seconds** from Event Tick
> - **Multiply** Friction × Delta Seconds
> - **1.0 - result** (float subtract)
> **What this Multiply does:** Shrinks speed a little each frame — exponential slow-down.
> **Why:** Mimics grass resistance without physics sim.
> Repeat same for **Y**.

#### Step 4 — Rebuild Velocity

> 1. **Make Vector** — **X** = friction X, **Y** = friction Y, **Z** = **0**
> 2. **Set Velocity**

#### Step 5 — Move the ball

> 1. **Get Velocity** (again) × **Delta Seconds**
> 2. **Add Actor World Offset** — **Sweep** = **true**
> **What Add Actor World Offset does:** Moves ball along ground using rolled velocity.
> **Why:** Same movement node as Lessons 4–7 — consistent pattern.

#### Step 6 — Stop when slow enough

> After **Set Velocity**:
> 1. **Vector Length** ← **Get Velocity** (or Make Vector output)
> **What Vector Length does:** Converts X,Y,Z into one "speed" number.
> **Why:** Easy to compare against **StopSpeed**.
> 2. **Branch** — **Condition** = **Vector Length < StopSpeed**
> 3. **True** path:
>    - **Set Velocity** = `(0, 0, 0)`
>    - **Set bIsRolling** = **false**
>    - **Set bIsMoving** = **false**
> **What this stop block does:** Kills tiny jitter and ends the shot.
> **Why:** Without threshold, ball creeps forever from math rounding.

#### Step 7 — Compile + Save

> Click **Compile** + **Save** in the Blueprint toolbar.

---

### Plain English summary
While rolling: shrink horizontal speed every frame, move along ground, stop when almost still.

### Test

1. **Play** on flat floor.
2. Launch with medium speed.
3. Ball lands → rolls forward → gradually stops.
4. Increase **Friction** → shorter roll-out. Decrease → longer roll.

---

## Part G — Build Section 4: Physics Roll Alternative (Optional)

### What we're building now
Optional second approach — engine physics for roll instead of manual friction.

**Skip this section** if manual roll feels good. Know it exists for later.

### Steps (summary)

1. On landing: **Set Simulate Physics** = **true** on mesh.
2. **Add Impulse** with horizontal component only (Lesson 6).
3. Tune **Linear Damping** + floor **Physical Material** friction.

**Tradeoff:** Less Blueprint math, harder to match exact Mevo+ carry + roll numbers.

**GolfSim plan:** Manual friction (Part E) for predictable tuning; physics optional for obstacles.

---

## Part H — Future: Sand vs Fairway

Later you'll use different **Friction** values per surface:

| Surface | Friction (example) | Effect |
|---------|-------------------|--------|
| Fairway | `2.0` | Normal roll |
| Sand | `8.0` | Stops quickly |
| Green | `1.0` | Long smooth roll |

Detect surface with **Break Hit Result** → **Physical Material** on hit.

---

## Part I — Quick Check

1. Why zero out **Velocity Z** when entering roll mode?
2. What does **Vector Length** tell you?
3. Why use **StopSpeed** instead of waiting for exactly zero?
4. Sand vs fairway — which variable changes?

---

## Part J — Git

```powershell
cd "C:\Users\Darre\Documents\Unreal Projects\GolfSim"
git add .
git commit -m "feat: lesson 9 ground roll and friction"
git push
```

---

## Troubleshooting

| Problem | Fix |
|---------|-----|
| Ball stops instantly on land | Friction too high — try `0.5`. |
| Ball never stops | Add **StopSpeed** Branch; increase Friction. |
| Ball slides in air | Roll Branch firing too early — gate with **bInFlight** check. |
| Ball bounces instead of rolls | Zero Z on land; disable projectile bounce for testing. |
| Roll direction wrong | Copy velocity **at landing** from Projectile component. |

