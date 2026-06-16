# Lesson 9: Roll on the Ground

**Project:** GolfSim  
**Prerequisite:** Lesson 8 (ball lands after flight)  
**Goal:** After landing, ball rolls and slows down — **roll-out** from your roadmap.

**Plain English:** Flight ends; ball skids/rolls on grass and stops.

---

## Part A — What You Are Building

1. Detect **landing** (hit floor, low height, or projectile stop).
2. Switch from **flight mode** to **roll mode**.
3. Horizontal Velocity decreases each frame (**Friction**).

---

## Part B — Manual Roll (Blueprint)

```
On land:
    Keep Velocity.X and Velocity.Y
    Set Velocity.Z = 0
    bIsRolling = true

Event Tick (rolling):
    Velocity.X *= (1 - Friction × Delta Seconds)
    Velocity.Y *= (1 - Friction × Delta Seconds)
    If speed near zero → stop completely
```

**Friction** = small float (e.g. `2.0`) — tune by feel.

---

## Part C — Physics Roll (Alternative)

Enable **Simulate Physics** on land, **Add Impulse** from remaining horizontal velocity, tune **Linear Damping** and ground **Physical Material** friction.

---

## Part D — Build Steps (When Ready)

1. Add `bIsRolling`, `Friction` variables.
2. On floor hit: enter roll state, clamp Z velocity.
3. Apply friction in Tick until speed < threshold.
4. Test: drive onto flat plane — ball should stop naturally.

---

## Part E — Quick Check

1. Why zero out **Z** velocity on landing for roll?
2. Sand vs fairway — how would you use different Friction later?

---

## Part F — Git

```powershell
git add .
git commit -m "feat: lesson 9 ground roll and friction"
git push
```
