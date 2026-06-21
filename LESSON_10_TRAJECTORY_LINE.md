# Lesson 10: Trajectory Debug Line

**Project:** GolfSim  
**Prerequisite:** Lesson 7 or 8 (ball flies in an arc — manual gravity or Projectile Movement)  
**Goal:** Draw a visible **predicted path** before or after launch — helps you tune speed and angle.

**Plain English:** A temporary "guide rope" showing where the ball **will** go — like a practice overlay. Starts simple with debug lines in the editor.

Maps to **Phase 2** trajectory tracer in your roadmap.

---

## Part A — What You Are Building (Overview)

By the end of this lesson you will have:

1. **`PredictPath`** function — returns an array of 3D points along the arc.
2. **Draw Debug Line** loop — connects points into a visible path in Play mode.
3. Lines show on **launch** (or while holding aim key) for ~2 seconds.

**End result:** Press launch (or aim) → colored line shows predicted arc → ball follows roughly the same shape.

---

## Part B — New Words

| Word | Simple meaning |
|------|----------------|
| **Prediction** | Simulate future frames in math without moving the real ball. |
| **Array of Vectors** | List of 3D points — point 0, point 1, point 2… |
| **Draw Debug Line** | Editor/development node — draws a line between two points for testing. |
| **Time Step** | Small slice (e.g. 0.05 sec) used when stepping prediction forward. |
| **Function** | Reusable graph block — call **PredictPath** from multiple places. |

---

## Part C — Blueprint Wires (Quick Reminder)

> - **White** = execution. **Colored** = data.
> - **PredictPath** = function (grouped logic) — called with white exec wire from launch input.
> - **Get ShotSpeed**, **Get Gravity**, etc. = drag from **My Blueprint** inside the function.

## Part D — Build Section 1: Create the PredictPath Function

### What we're building now
A reusable calculator that answers: "If I launch at this speed and angle, where will the ball be every 0.05 seconds?"

### What you need first
- **`ShotSpeed`**, **`LaunchAngle`**, **`Gravity`** variables from Lesson 7 (or equivalent).
- Understanding of launch math: **Cos/Sin × ShotSpeed** → initial Velocity.

### Variables to create (inside function or on Blueprint)

| Name | Type | Default | Why |
|------|------|---------|-----|
| `PredictionStepTime` | **Float** | `0.05` | Seconds per prediction step — smaller = smoother line. |
| `PredictionStepCount` | **Integer** | `30` | How many points — 30 × 0.05 = 1.5 sec of path. |

### Build steps

#### Step 1 — Create function

> 1. **My Blueprint** → **Functions** → **+ Function** → name **`PredictPath`**.
> 2. **Inputs:** none (reads Blueprint variables) OR add **ShotSpeedIn**, **LaunchAngleIn** if you want flexibility.
> 3. **Outputs:** add output **`PathPoints`** → type **Vector** → check **Array** (pin icon).
> **What a Function does:** Groups nodes you can call from anywhere without copy-pasting.
> **Why:** Same prediction for debug line, HUD, and AI later.

#### Step 2 — Compute initial velocity (same as Lesson 7 launch)

> Inside **PredictPath**:
> 1. **Deg Cos(LaunchAngle) × ShotSpeed** → **InitialVelX**
> 2. **Deg Sin(LaunchAngle) × ShotSpeed** → **InitialVelZ**
> 3. **Make Vector** (X, Y=0, Z) → promote to local variables **SimVelocity** and **SimLocation**
> **What SimVelocity / SimLocation are:** Temporary "fake" ball state for prediction only — not the real ball.
> **Why:** We simulate flight in math without moving the Actor.
> Use **local variables** in the function:
> - **`SimLocation`** (Vector) — start at **Get Actor Location** (ball's current position).
> - **`SimVelocity`** (Vector) — from Make Vector above.

#### Step 3 — Loop for each prediction step

> 1. Add **For Loop** — **First Index** = `0`, **Last Index** = **PredictionStepCount - 1**
> **What For Loop does:** Runs the same nodes N times — one per path point.
> **Why:** Arc is many small steps, not one straight line.
> **Inside Loop body — each iteration:**
> **A. Apply gravity to SimVelocity Z**
> 1. **Break Vector** (SimVelocity)
> 2. **NewZ** = **Z** + (**Gravity** × **PredictionStepTime**)
> 3. **Make Vector** → **Set SimVelocity**
> **What this does:** Same gravity idea as Lesson 7 Tick — but in fixed time steps.
> **Why:** Prediction must match real flight math to look accurate.
> **B. Move SimLocation**
> 1. **SimVelocity** × **PredictionStepTime** → delta
> 2. **SimLocation** + delta → **Set SimLocation**
> **What this does:** Advance fake position one step along the arc.
> **C. Store point in array**
> 1. **Add** (Array pin) on **PathPoints** — item = **SimLocation**
> **What Add does:** Appends one point to the output array.
> **Why:** Draw Debug Line needs a list of dots to connect.

#### Step 4 — Return array

> Connect filled **PathPoints** to function output.

#### Step 5 — Compile

---

### Plain English summary
PredictPath imagines the shot step-by-step and returns a list of dots along the arc — no ball movement yet.

---

## Part E — Build Section 2: Draw the Debug Lines

### What we're building now
Connect the predicted dots into visible lines when you launch.

### Build steps

#### Step 1 — Call PredictPath on launch

> 1. Open your **Launch input** event.
> 2. **Before** or **after** real launch (before is nicer for "preview"):
> **Nodes:**
> 1. **PredictPath** (function call)
> 2. Output **PathPoints** array

#### Step 2 — Loop through points and draw lines

> 1. **For Loop** — `0` to **Length(PathPoints) - 2** (stop at second-to-last — each line needs two points)
> **Inside loop:**
> 1. **Get** (array) **PathPoints** at index **Loop Index** → **PointA**
> 2. **Get** **PathPoints** at index **Loop Index + 1** → **PointB**
> 3. **Draw Debug Line**
> **Draw Debug Line pin connections:**
> | Pin | Connect |
> |-----|---------|
> | **Start** | **PointA** |
> | **End** | **PointB** |
> | **Color** | e.g. green `(0, 255, 0)` |
> | **Duration** | `2.0` (seconds visible) |
> | **Thickness** | `2.0` |
> **What Draw Debug Line does:** Draws one line segment in the viewport during Play.
> **Why:** Visual proof of predicted path — instant feedback when tuning ShotSpeed/LaunchAngle.
> **What the For Loop + Get array does:** Connects dot 0→1, 1→2, 2→3… into a continuous curve.
> **Why:** One Draw Debug Line = one segment; many segments = smooth arc.

#### Step 3 — Enable draw debug in editor

> 1. **Play** dropdown arrow → enable **Show Debug** / ensure game runs with debug drawing (default in editor Play).
> **Note:** **Draw Debug Line** is for development. Shipping game uses Spline or mesh (Part F).

#### Step 4 — Compile + Save

> Click **Compile** + **Save** in the Blueprint toolbar.

---

### Plain English summary
Launch → predict dots → draw lines between them → you see the arc before/for 2 seconds after hit.

### Test

1. **Play** → press launch.
2. Green arc should appear roughly matching ball flight.
3. Change **LaunchAngle** — line angle changes.
4. Change **Gravity** — line peak and landing spot shift.

---

## Part F — Build Section 3: Show Line While Aiming (Optional)

### What we're building now
Hold a key to preview path **before** hitting — like a practice range overlay.

### Build steps

#### Step 1 — Add aim input

> 1. **Project Settings → Input** → add **Action Mapping** `AimPreview` → bind **Right Mouse Button** or **Q**.

#### Step 2 — Input Axis / Action pressed

> 1. **InputAction AimPreview (Pressed)** → **PredictPath** → draw loop (same as Part D)
> **What this does:** Redraws preview each press — for hold-to-preview, use **Tick while pressed** (advanced).
> **Why:** Golf practice — see where shot might go before committing.

#### Step 2 — Clear old lines

> Debug lines auto-expire after **Duration**. Keep Duration at `0.1` for hold preview refreshed every frame (advanced) or `2.0` for single flash.
---

## Part G — Player-Facing Line Later (Reference — Not This Lesson)

For final game (Phase 6), replace Draw Debug Line with:

| Method | Use when |
|--------|----------|
| **Spline Mesh** | Permanent 3D ribbon on course |
| **Niagara ribbon** | Fancy glowing tracer |
| **Widget HUD** | 2D mini-map arc |

Start with debug lines until tuning feels good.

---

## Part H — Quick Check

1. Why predict many **points** instead of one straight line from ball to landing?
2. Why use the **same Gravity and launch math** in PredictPath as real flight?
3. **Draw Debug Line** vs **Spline** — which for final shipped game?
4. What does **PredictionStepTime** affect?

---

## Part I — Git

```powershell
cd "C:\Users\Darre\Documents\Unreal Projects\GolfSim"
git add .
git commit -m "feat: lesson 10 trajectory debug line"
git push
```

---

## Troubleshooting

| Problem | Fix |
|---------|-----|
| No lines visible | **Duration** > 0; Play in editor with debug; check line color vs sky. |
| Line doesn't match ball | Prediction math must match flight — same Gravity, same Cos/Sin launch. |
| Jagged line | Lower **PredictionStepTime** or raise **PredictionStepCount**. |
| Lines only in editor | Expected — use Spline for shipping build. |
| Function won't compile | **PathPoints** output must be **Vector Array** type. |

