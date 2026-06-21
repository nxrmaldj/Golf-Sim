# Lesson 7: Gravity and Ball Arc

**Project:** GolfSim  
**Prerequisite:** Lesson 5 (`BP_MovingBall` with **Velocity**, **Bounciness**, sweep movement, bounce on hit)  
**Goal:** Ball rises, curves through the air, and falls — your first **golf-style flight arc** using manual Velocity.

**Plain English:** Each frame, pull the ball down a little (gravity). Combined with launch speed, the path becomes a parabola — like a real shot.

---

## Part A — What You Are Building (Overview)

By the end of this lesson you will have:

1. A **`Gravity`** float variable that pulls **Velocity Z** down every frame.
2. **`ShotSpeed`** and **`LaunchAngle`** variables to compute launch direction from numbers (like future Mevo+ data).
3. A ball that launches on key press, flies in an arc, hits the floor, and bounces (Lesson 5 logic still works).

**End result:** Press launch → ball flies up and forward → gravity bends the path down → lands on floor.

---

## Part B — New Words

| Word | Simple meaning |
|------|----------------|
| **Gravity** | Constant downward pull (negative Z in UE's default world). |
| **Arc / Parabola** | Curved flight path — up first, then down. |
| **Launch Angle** | Degrees upward from flat ground (e.g. 15° = low drive, 45° = high lob). |
| **Shot Speed** | How fast the ball leaves the club — one number for total launch power. |
| **Break Vector** | Splits a Vector into separate X, Y, Z floats so you can change one axis. |
| **Make Vector** | Combines X, Y, Z floats back into one Vector. |
| **Deg Sin / Deg Cos** | Blueprint math nodes — convert angle + speed into horizontal and vertical parts. |

---

## Part C — Build Section 1: Add the Gravity Variable

### What we're building now
A number that represents how hard Earth pulls the ball downward each second.

### What you need first
- `BP_MovingBall` with **Velocity** movement from Lesson 5 (Event Tick moves via Velocity × Delta Seconds, Sweep on).

### Variables to create

| Name | Type | Default | Why |
|------|------|---------|-----|
| `Gravity` | **Float** | `-980` | Downward pull in UE units/sec². Negative because Z-up world — down is negative Z. Tune by feel. |

### Build steps

#### Step 1 — Create the variable

> 1. Open **`BP_MovingBall`** → **My Blueprint** → **+ Variable**.
> 2. Name: **`Gravity`**, type: **Float**, default: **`-980`**.
> **What a Float variable does:** Stores one decimal number you can change without rewiring the graph.
> **Why `-980`:** Unreal's default gravity scale is roughly 980 cm/s² downward. Start here, adjust later.
---

### Plain English summary
You now have a tunable "how hard gravity pulls" dial.

---

## Part D — Build Section 2: Apply Gravity Every Frame

### What we're building now
Each tick, reduce **Velocity Z** by gravity — so upward motion slows, then becomes downward.

### Build steps

> **Blueprint wires (read once):**
> - **White** = execution. **Colored** = data.
> - **Get** nodes = drag variable from **My Blueprint** — not off white pins.
> - All gravity + movement nodes sit on **Branch → True** (when `bIsMoving` is true).

#### Step 1 — Locate your Event Tick movement chain

> Your graph should look roughly like:
> ```
> Event Tick → Branch (bIsMoving) → True → [move using Velocity × Delta Seconds]
> ```
> We add gravity **before** the move, still inside the **True** path.

#### Step 2 — Add Get Velocity (data node)

> 1. From **My Blueprint**, drag **`Velocity`** into the graph (near your Branch True area).
> 2. Choose **Get Velocity** from the menu.
>
> **What Get Velocity does:** Reads the current **Velocity** value.
>
> **Why it lives on the True path area:** These nodes only matter when the ball is moving — same section as your move logic. You do **not** wire Get Velocity off the white **Branch True** pin.

#### Step 3 — Split Velocity into X, Y, Z

> 1. **Break Vector** ← connect **Get Velocity** return value.
> **What Break Vector does:** Outputs three separate floats: **X**, **Y**, **Z**.
> **Why:** Gravity only affects **Z** (up/down). X and Y stay the same this frame.

#### Step 4 — Calculate gravity change this frame

> 1. Drag **`Gravity`** from **My Blueprint** → **Get Gravity**.
> 2. From **Event Tick**, drag **Delta Seconds** (green pin on event) → **Multiply** with **Get Gravity**.
>
> **What this Multiply does:** "Per second" gravity → "this frame" gravity.
>
> **Why Delta Seconds from Event Tick:** Same as Lesson 2 — not from Branch.

#### Step 5 — Add gravity to Velocity Z

> 1. **Float + Float**: **Break Vector Z** + (**Gravity × Delta Seconds**).
> **What this Add does:** New Z = old Z + gravity slice. Example: Z was 500 upward, gravity adds -16 → Z becomes 484.
> **Why:** Each frame the upward speed shrinks until the ball peaks, then Z goes negative (falling).

#### Step 6 — Rebuild Velocity and save it

> 1. **Make Vector** — **X** ← Break Vector X, **Y** ← Break Vector Y, **Z** ← Add result.
> 2. **Set Velocity** ← **Make Vector** output.
> **What Set Velocity does:** Writes the updated Vector back to the variable.
> **Why:** Next frame's movement uses the new direction.

#### Step 7 — Wire execution order (gravity then move)

> **White execution chain on Branch True:**
> ```
> Branch True ──white──→ Set Velocity (gravity updated)
>                            ↓ white
>                        Add Actor World Offset (Sweep on)
> ```
> **Colored data for move:** **Get Velocity** (after Set) → × **Delta Seconds** → **Delta Location**.
>
> **What order does:** Apply gravity to Velocity first, then move using the updated value this frame.
>
> **Why not move before Set Velocity:** Ball would move with last frame's Velocity, not gravity-adjusted Velocity.

#### Step 8 — Compile + Save

> Click **Compile** + **Save** in the Blueprint toolbar.

---

### Plain English summary
Every frame while moving: pull Velocity Z down a tiny bit, then move the ball. Upward launch + constant downward pull = arc.

### Test

1. Set launch **Velocity** manually to something like `(800, 0, 600)` on your input event (temporary).
2. **Play** → ball should rise, hang, fall — not fly in a straight line forever.

---

## Part E — Build Section 3: Launch From Speed + Angle

### What we're building now
Instead of guessing X and Z numbers, you set **ShotSpeed** and **LaunchAngle** — closer to real golf data (and future Mevo+ JSON).

### Variables to create

| Name | Type | Default | Why |
|------|------|---------|-----|
| `ShotSpeed` | **Float** | `1200` | Total launch power — tune by eye. |
| `LaunchAngle` | **Float** | `30` | Degrees up from horizontal. |

### Build steps

#### Step 1 — Open your launch input event

> Find **Input Action** / Space bar from Lesson 3.

#### Step 2 — Compute horizontal and vertical speed from angle

> Drag variables from **My Blueprint** (don't pull off white wires):
> 1. **`ShotSpeed`** → **Get ShotSpeed**
> 2. **`LaunchAngle`** → **Get LaunchAngle**
> 3. **Deg Cos** ← **LaunchAngle**; multiply output × **ShotSpeed** → horizontal speed (X).
> **What Deg Cos does:** At 0° angle, cos = 1 (all horizontal). At 90°, cos = 0 (no horizontal).
> **Why:** Golf launch angle splits speed into forward vs up components.
> 4. **Deg Sin** — input **LaunchAngle** → output × **ShotSpeed** → this is **vertical** speed (Z).
> **What Deg Sin does:** At 0°, sin = 0 (no height). At 45°, sin is large (lots of height).
> **Why:** Complement of cos — together they use full ShotSpeed correctly.

#### Step 3 — Build launch Velocity vector

> 1. **Make Vector**:
>    - **X** ← horizontal result (Cos × ShotSpeed)
>    - **Y** ← `0` (or your aim direction later)
>    - **Z** ← vertical result (Sin × ShotSpeed)
> 2. **Set Velocity** ← Make Vector output.
> **What Set Velocity does here:** Sets initial flight direction on launch — one time per shot.
> **Why:** Replaces hard-coded `(800, 0, 600)` with tunable shot parameters.

#### Step 4 — Set bIsMoving true

> Connect **Set bIsMoving** = **true** after launch (Lesson 3).
> **What the Branch on bIsMoving does:** Event Tick only runs gravity + movement while the ball is in motion.
> **Why:** Ball sits still until you press launch.

#### Step 5 — Compile + Save

> Click **Compile** + **Save** in the Blueprint toolbar.

---

### Plain English summary
Press launch → Speed + Angle become a Velocity arrow → Tick applies gravity → ball flies a golf arc.

### Test

1. **Play** → press launch.
2. Change **LaunchAngle** in Details (if Instance Editable) — higher angle = higher arc.
3. Change **ShotSpeed** — farther shot.
4. Ball should land on floor and bounce (Lesson 5) or stop (Lesson 4) depending on your bounce setup.

---

## Part F — Quick Check

1. Why is **Gravity** negative in UE's default level?
2. If **Gravity** were `0`, what shape would the path be?
3. What do **Deg Sin** and **Deg Cos** each contribute to the launch?
4. Why multiply **Gravity** by **Delta Seconds**?

---

## Part G — Git

```powershell
cd "C:\Users\Darre\Documents\Unreal Projects\GolfSim"
git add .
git commit -m "feat: lesson 7 gravity and launch arc"
git push
```

---

## Troubleshooting

| Problem | Fix |
|---------|-----|
| Ball flies straight never falls | Gravity not applied — check **Set Velocity** Z changes each Tick. |
| Ball falls instantly | Launch Z too low or Gravity too strong (try `-500` first). |
| Ball tunnels through floor | Sweep still on? Mesh is Root? (Lesson 4) |
| Angle seems wrong | **Deg Sin/Cos** expect **degrees**, not radians. |
| Arc works but no bounce | Complete Lesson 5 bounce on **On Hit** event. |

