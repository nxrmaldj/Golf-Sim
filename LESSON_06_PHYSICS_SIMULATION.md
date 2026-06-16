# Lesson 6: Physics Simulation vs Manual Movement

**Project:** GolfSim  
**Prerequisite:** Lesson 5  
**Goal:** Understand **Simulate Physics** and when to use engine physics instead of your manual Velocity.

**Plain English:** Unreal can be the referee (physics sim) or you can be the referee (manual). Golf games often mix both.

---

## Part A — Two Approaches

| Approach | How it works | Good for |
|----------|--------------|----------|
| **Manual** (Lessons 1–5) | You set Velocity each frame / on hit | Learning, arcade control, Mevo+ driven shots |
| **Simulate Physics** | Engine applies gravity, friction, restitution | Rolling, piles of objects, realistic bumps |

**GolfSim plan:** Manual / Projectile for **flight**, physics or custom roll for **ground** (Lesson 9).

---

## Part B — New Words

| Word | Simple meaning |
|------|----------------|
| **Simulate Physics** | Mesh obeys UE physics engine. |
| **Restitution** | Engine's bounce (0–1) on Physical Material. |
| **Impulse** | One-time shove (good for "hit" the ball). |
| **Add Impulse** | Kick the ball without setting velocity every frame. |

---

## Part C — Try Simulate Physics (Experiment)

1. On Static Mesh: **Simulate Physics** = true.
2. **Disable** your Event Tick movement (or gate with a bool).
3. On launch: **Add Impulse** in launch direction.
4. Tune **Linear Damping** (air resistance) in component details.

Watch: bounces come from **Physical Material** restitution, not your Reflect node.

---

## Part D — Compare Side by Side

Duplicate `BP_MovingBall` → `BP_PhysicsBall`:

| | Manual ball | Physics ball |
|---|-------------|--------------|
| Bounce | Your Reflect / Bounciness | Material Restitution |
| Gravity | You add to Velocity Z | Automatic |
| Control | High | Medium |

**Tradeoff:** Manual = predictable for golf shot tuning. Physics = less code for rolling but harder to match Mevo+ exactly.

---

## Part E — Quick Check

1. When would you turn **Simulate Physics** off during a drive?
2. What does **Add Impulse** do once vs every frame?

---

## Part F — Git

```powershell
git add .
git commit -m "feat: lesson 6 physics simulation experiment"
git push
```
