# Lesson 10: Trajectory Debug Line

**Project:** GolfSim  
**Prerequisite:** Lesson 7 or 8 (ball in flight)  
**Goal:** Draw a visible line showing where the ball is going — helps tuning shots.

**Plain English:** A temporary "laser guide" for the ball path (debug / practice overlay).

Maps to **Phase 2** trajectory tracer in your roadmap.

---

## Part A — What You Are Building

1. **Predict** a few future positions (sample points along arc).
2. **Draw Debug Line** or **Spline** / **Niagara** ribbon (start simple).
3. Show only while aiming or for 2 seconds after hit.

---

## Part B — Simple Version — Draw Debug Line

In UE Blueprint (development only):

- Node: **Draw Debug Line** (enable **Draw Debug** in editor).
- From ball position → position + Velocity × small time step.
- Loop 10–20 steps, adding gravity each step.

**Note:** Debug lines only show in editor unless you use a permanent mesh line for shipping.

---

## Part C — Visual Line for Players (Later)

- **Spline** component with points updated from prediction.
- Or **Widget** 2D tracer on HUD (advanced).

Start with debug; polish later in Phase 6.

---

## Part D — Build Steps (When Ready)

1. Function **PredictPath** → array of points.
2. On launch (or hold aim key): loop Draw Debug Line between points.
3. Tune step count and time horizon.
4. Optional: hide after ball lands.

---

## Part E — Quick Check

1. Why predict points instead of one straight line?
2. Debug line vs spline — which for final game?

---

## Part F — Git

```powershell
git add .
git commit -m "feat: lesson 10 trajectory debug line"
git push
```
