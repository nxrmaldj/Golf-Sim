# Lesson 8: Projectile Movement Component

**Project:** GolfSim  
**Prerequisite:** Lesson 7 (understand arc concept)  
**Goal:** Use UE's built-in **Projectile Movement** component for flight instead of hand-rolling gravity.

**Plain English:** Unreal has a pre-made "flying object" part — you feed it speed and it handles movement + optional bounce.

Maps to **Phase 2** in your roadmap.

---

## Part A — What You Are Building

1. Add **Projectile Movement** component to ball Blueprint.
2. Turn off manual Tick movement for flight phase.
3. **Set Velocity in Local Space** or **Initial Speed** + rotation on launch.
4. Optional: **Should Bounce**, **Bounciness**, **Projectile Gravity Scale**.

---

## Part B — New Words

| Word | Simple meaning |
|------|----------------|
| **Projectile Movement** | Component for bullets, balls in flight. |
| **Initial Speed** | Starting speed magnitude. |
| **Gravity Scale** | 0 = no fall, 1 = normal, 2 = heavy fall. |
| **Updated Component** | Which part moves (usually root or mesh). |

---

## Part C — Manual vs Projectile (Tradeoff)

| | Manual Velocity (L5–7) | Projectile Movement |
|---|------------------------|---------------------|
| Learning | See every step | Faster setup |
| Golf tuning | You control every line | Tune fewer properties |
| Mevo+ later | Map JSON → your variables | Map JSON → Initial Velocity vector |

**Recommendation:** Learn manual first (done), then switch flight to Projectile for polish.

---

## Part D — Build Steps (When Ready)

1. **Add Component** → **Projectile Movement**.
2. Disable manual movement when `bInFlight` true.
3. On launch: **Activate** projectile, **Set Velocity in Local Space** from shot variables.
4. Tune **Gravity Scale**, **Max Speed** if needed.
5. On **OnProjectileStop** or hit ground event: deactivate projectile, start roll (Lesson 9).

---

## Part E — Quick Check

1. What does **Gravity Scale** = 0 do?
2. Why deactivate Projectile Movement after landing?

---

## Part F — Git

```powershell
git add .
git commit -m "feat: lesson 8 projectile movement component"
git push
```
