# Lesson 7: Gravity and Ball Arc

**Project:** GolfSim  
**Prerequisite:** Lesson 5 (manual Velocity)  
**Goal:** Ball rises then falls in an arc — first taste of golf **flight**.

**Plain English:** Each frame, pull the ball down a little (gravity), like real life.

---

## Part A — What You Are Building

1. **Gravity** float variable (e.g. `-980` UE units/sec² — tune by feel).
2. Each **Event Tick**: add Gravity × Delta Seconds to **Velocity Z**.
3. Move using Velocity (Lesson 5 style).

Result: diagonal launch + gravity = parabolic arc.

---

## Part B — The Logic (Preview)

```
Event Tick (while in flight)
    ↓
Velocity.Z += Gravity × Delta Seconds
    ↓
Move by Velocity × Delta Seconds (Sweep on)
```

**Launch:** Set Velocity X/Y from "club speed" and Velocity Z from "launch angle" (Lesson 8 simplifies angle math).

---

## Part C — Launch Angle (Simple)

Variables: `ShotSpeed`, `LaunchAngle` (degrees).

Use **Make Rotator** or trigonometry nodes:

- Forward speed = `ShotSpeed × cos(angle)`
- Up speed = `ShotSpeed × sin(angle)`

(Search nodes: **Deg Sin**, **Deg Cos** in Blueprint.)

---

## Part D — Build Steps (When Ready)

1. Add `Gravity` (Float, default negative).
2. In Tick: **Break Vector** on Velocity → modify Z → **Make Vector** → **Set Velocity**.
3. Add `ShotSpeed`, `LaunchAngle` — on input, compute Velocity vector.
4. Test: ball should land on floor (Lesson 4 collision) and stop or bounce (Lesson 5).

---

## Part E — Quick Check

1. Why is Gravity negative in UE's default up-axis world?
2. What shape is the path if Gravity = 0?

---

## Part F — Git

```powershell
git add .
git commit -m "feat: lesson 7 gravity and launch arc"
git push
```
