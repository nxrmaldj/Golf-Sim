# Lesson 5: Manual Bounce (No Physics Engine)

**Project:** GolfSim  
**Prerequisite:** Lesson 4  
**Goal:** When the ball hits a surface, **you** flip its direction in code — a fake bounce you control.

**Plain English:** Instead of Unreal's physics sim doing the bounce, you say: *"Speed stays the same, but reverse the direction along the wall."*

This teaches **velocity** and **reflection** — core golf ball ideas later.

---

## Part A — What You Are Building

1. A **Velocity** vector variable (X, Y, Z speed direction).
2. Each tick: move by Velocity × Delta Seconds (instead of only +X).
3. On hit: **reflect** velocity off the surface normal (simplified: flip the right axis).

---

## Part B — New Words

| Word | Simple meaning |
|------|----------------|
| **Velocity** | Speed + direction (a 3D arrow). |
| **Normal** | Arrow pointing straight out of a surface (perpendicular). |
| **Reflect** | Mirror velocity across the surface — like a pool ball off a cushion. |
| **Bounciness** | 0 = dead stop, 1 = perfect bounce, 0.5 = loses half energy. |

---

## Part C — Simple Bounce (Beginner Version)

Before full 3D reflection, try **axis flip** on floor/wall:

| Hit surface | Flip this part of Velocity |
|-------------|----------------------------|
| Floor/ceiling | Flip **Z** (multiply Z by -1) |
| Wall facing X | Flip **X** |
| Wall facing Y | Flip **Y** |

Multiply by **Bounciness** float (e.g. `0.6`) to lose energy each bounce.

```
On Component Hit (or from Sweep Hit Result)
    ↓
Get Velocity
    ↓
Flip appropriate axis (× -1 × Bounciness)
    ↓
Set Velocity
```

---

## Part D — Better Bounce (When Ready)

Use **Reflect Vector** node:

- **In Velocity** = current Velocity  
- **In Normal** = Hit Normal from hit result  
- **Out** = new Velocity after bounce  

Then: `Set Velocity = Reflect result × Bounciness`

---

## Part E — Switch Movement Style

Replace **Add Actor World Offset** constant direction with:

```
New Location = Current Location + (Velocity × Delta Seconds)
```

Use **Set Actor Location** or **Add Actor World Offset** with Velocity × Delta Seconds.

Enable **Sweep** still — on hit, adjust Velocity instead of only stopping.

---

## Part F — Build Steps (When Ready)

1. Add variables: `Velocity` (Vector), `Bounciness` (Float, default `0.6`).
2. On launch (Lesson 3 input): **Set Velocity** e.g. `(500, 0, 200)` for a lob.
3. Event Tick: move using Velocity × Delta Seconds, Sweep on.
4. Add **On Component Hit** event on Static Mesh (need **Simulation Generates Hit Events** or hit-compatible collision).
5. On hit: **Reflect Vector** or simple axis flip → **Set Velocity**.
6. Test: ball bounces off floor and walls with decreasing height if Bounciness < 1.

---

## Part G — Quick Check

1. What is **Velocity** vs **Speed**?
2. Why multiply by **Bounciness** after a bounce?
3. Manual bounce vs **Simulate Physics** — who controls the math?

---

## Part H — Git

```powershell
git add .
git commit -m "feat: lesson 5 manual bounce with velocity"
git push
```
