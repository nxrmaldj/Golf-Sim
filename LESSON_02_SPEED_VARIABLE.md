# Lesson 2: Speed Variable + Delta Seconds

**Project:** GolfSim  
**Prerequisite:** Lesson 1 complete (`BP_MovingBall` moves on Event Tick)  
**Goal:** Control speed from one place, and move the same speed on every PC.

**Plain English:** Replace the hardcoded `2` with a **Speed** dial, and use **Delta Seconds** so fast monitors don't make the ball zoom.

---

## Part A — What You Are Building

You will add:

1. A **float variable** called `Speed` (stores a number).
2. **Delta Seconds** from Event Tick (tiny time slice per frame).
3. **Multiply** Speed × Delta Seconds before moving.

Movement becomes: `distance this frame = Speed × Delta Seconds`

---

## Part B — New Words

| Word | Simple meaning |
|------|----------------|
| **Variable** | A named box that holds a value you can change. |
| **Float** | A number with decimals (e.g. `200.0`). |
| **Delta Seconds** | Time since last frame (~0.016 at 60 FPS). |
| **Instance Editable** | Lets you change the value per ball in the level Details panel. |

---

## Part C — The Logic (Preview)

```
Event Tick
    ↓
Speed (get variable)  ×  Delta Seconds (from Event Tick pin)
    ↓
Make Vector (X = result, Y = 0, Z = 0)   ← or your chosen axis
    ↓
Add Actor World Offset
```

**Why multiply?**  
Without Delta Seconds, "move 2 per frame" = faster on 120 FPS, slower on 30 FPS.  
With it, **Speed = units per second** (much easier to reason about).

**Suggested starting Speed:** `200` (units per second in UE — tune by eye).

---

## Part D — Build Steps (When Ready)

### Step 1 — Create the variable

1. Open `BP_MovingBall` → **Event Graph** or **My Blueprint** panel.
2. Click **+ Variable** → name `Speed`, type **Float**.
3. Default value: `200`.
4. Eye icon **on** = Instance Editable (optional but nice for testing).

### Step 2 — Wire the graph

1. Keep **Event Tick**.
2. Drag **Speed** into graph → **Get Speed**.
3. From **Event Tick**, drag **Delta Seconds** pin.
4. **Multiply** (float × float): Speed × Delta Seconds.
5. Feed result into **Make Vector** X (Y=0, Z=0 if moving on X).
6. **Add Actor World Offset** as in Lesson 1.
7. **Compile** + **Save**.

### Step 3 — Test

1. Play — ball should still move smoothly.
2. Select the ball in the level → **Details** → change **Speed** — faster/slower without opening the Blueprint.

---

## Part E — Quick Check

1. What does a **variable** save you from doing?
2. Why multiply by **Delta Seconds**?
3. If Speed = `0`, what happens?

---

## Part F — Git

```powershell
cd "A:\Unreal Projects\GolfSim"
git add .
git commit -m "feat: lesson 2 speed variable and delta seconds"
git push
```
