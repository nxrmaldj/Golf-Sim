# Lesson 2: Speed Variable + Delta Seconds

**Project:** GolfSim  
**Prerequisite:** Lesson 1 (`BP_MovingBall` moves on Event Tick)  
**Goal:** Control speed from one dial, and move the same speed on every PC.

**Plain English:** Replace hardcoded `2` with a **Speed** variable, and multiply by **Delta Seconds** so a 120 Hz monitor doesn't make the ball twice as fast.

---

## Part A — What You Are Building (Overview)

By the end of this lesson you will have:

1. **`Speed`** float variable (default `200`).
2. Movement = **Speed × Delta Seconds** each frame.
3. Speed tweakable in the level **Details** panel (optional).

**End result:** Same ball motion as Lesson 1, but speed means **units per second** — not "per frame."

---

## Part B — New Words

| Word | Simple meaning |
|------|----------------|
| **Variable** | Named box that holds a value you can change. |
| **Float** | Number with decimals (e.g. `200.0`). |
| **Delta Seconds** | Time since last frame (~0.016 at 60 FPS). |
| **Get (variable)** | Data node that reads the variable's current value. |
| **Instance Editable** | Change the value per ball in the level without opening the Blueprint. |

---

## Part C — Blueprint Wires (Quick Reminder)

- **White** = execution order (Event Tick → move node).
- **Colored** = data (Speed, Delta Seconds, vectors).
- **Get Speed** is a **data** node — drag **`Speed`** from **My Blueprint**, don't pull it off the white wire.

---

## Part D — Build Section 1: Create the Speed Variable

### Variables to create

| Name | Type | Default | Instance Editable? |
|------|------|---------|-------------------|
| `Speed` | **Float** | `200` | Optional ✓ (eye icon) |

#### Step 1 — Create `Speed`

> 1. Open **`BP_MovingBall`** → **My Blueprint** → **+ Variable**.
> 2. Name **`Speed`**, type **Float**, default **`200`**.
> 3. Click **eye icon** if you want to tune speed in the level Details panel.
>
> **Why create it first:** **Get Speed** needs a variable to read from.

#### Step 2 — Compile + Save

> Click **Compile** + **Save**.

---

## Part E — Build Section 2: Wire Speed × Delta Seconds

### What we're building now
Replace **Make Vector X = 2** with **Speed × Delta Seconds**.

#### Step 1 — Add Get Speed (data node)

> 1. From **My Blueprint**, drag **`Speed`** into the Event Graph.
> 2. Choose **Get Speed**.
>
> **What Get Speed does:** Reads your Speed variable — data only, no white wire.

#### Step 2 — Multiply by Delta Seconds

> 1. Add **Multiply** (float × float).
> 2. **Get Speed** → Multiply first pin.
> 3. **Event Tick** → drag **Delta Seconds** pin (green, on the event) → Multiply second pin.
>
> **What Delta Seconds is:** "How long was the last frame?" — smaller on fast PCs, larger on slow PCs.
>
> **Why multiply:** `Speed` = units **per second**. × Delta Seconds = units **this frame**. All PCs end up near the same speed.

#### Step 3 — Feed into movement

> 1. **Make Vector** — **X** ← Multiply result, **Y** = 0, **Z** = 0.
> 2. **White wire:** **Event Tick** → **Add Actor World Offset** (keep from Lesson 1).
> 3. **Colored wire:** **Make Vector** → **Delta Location**.
> 4. **Sweep** still off.
>
> ```
> Event Tick ──white──→ Add Actor World Offset
>     │                        ↑
>     └── Delta Seconds ──→ Multiply ←── Get Speed
>                              ↓
>                         Make Vector → Delta Location
> ```

#### Step 4 — Compile + Save

> Click **Compile** + **Save**.

---

### Plain English summary
Speed is a dial in units per second. Delta Seconds scales it per frame so every monitor feels the same.

### Test

1. **Play** — ball still moves smoothly.
2. Select ball in level → **Details** → change **Speed** — faster/slower without opening Blueprint.
3. **Speed = 0** — ball should not move.

---

## Part F — Quick Check

1. What does a **variable** save you from doing?
2. Why multiply by **Delta Seconds**?
3. Where do you drag **Delta Seconds** from — Event Tick or Branch?

---

## Part G — Git

```powershell
cd "C:\Users\Darre\Documents\Unreal Projects\GolfSim"
git add .
git commit -m "feat: lesson 2 speed variable and delta seconds"
git push
```

---

## Reference — Delta Seconds Table

| Monitor | FPS (rough) | Delta Seconds (rough) |
|---------|-------------|------------------------|
| Slow | ~30 | ~0.033 |
| Medium | ~60 | ~0.016 |
| Fast | ~120 | ~0.008 |

At **Speed = 200**, each PC moves ~200 units per **second** — not per frame.
