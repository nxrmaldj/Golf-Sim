# GolfSim — Ball Lessons (Blueprint Track)

**How to use these files**

1. Read the lesson **explain** sections first.
2. Answer the **quick check** (or say "ready to build" in chat).
3. Build step-by-step in Unreal.
4. `git add` / `commit` / `push` when done.

| # | File | Status | What you learn |
|---|------|--------|----------------|
| 1 | [LESSON_01_MOVING_SPHERE.md](LESSON_01_MOVING_SPHERE.md) | **Done** | Blueprint Actor, Event Tick, movement |
| 2 | [LESSON_02_SPEED_VARIABLE.md](LESSON_02_SPEED_VARIABLE.md) | Next | Variables, Delta Seconds, tweakable speed |
| 3 | [LESSON_03_STOP_AND_LAUNCH.md](LESSON_03_STOP_AND_LAUNCH.md) | Planned | Stop moving, one-shot "hit" with a key |
| 4 | [LESSON_04_COLLISION_BASICS.md](LESSON_04_COLLISION_BASICS.md) | Planned | Collision on/off, Sweep, hit walls & floor |
| 5 | [LESSON_05_MANUAL_BOUNCE.md](LESSON_05_MANUAL_BOUNCE.md) | Planned | Bounce by flipping velocity (no physics engine) |
| 6 | [LESSON_06_PHYSICS_SIMULATION.md](LESSON_06_PHYSICS_SIMULATION.md) | Planned | Simulate Physics vs manual — when to use each |
| 7 | [LESSON_07_GRAVITY_AND_ARC.md](LESSON_07_GRAVITY_AND_ARC.md) | Planned | Gravity, upward launch, golf-style arc |
| 8 | [LESSON_08_PROJECTILE_MOVEMENT.md](LESSON_08_PROJECTILE_MOVEMENT.md) | Planned | UE Projectile Movement Component |
| 9 | [LESSON_09_ROLL_ON_GROUND.md](LESSON_09_ROLL_ON_GROUND.md) | Planned | Landing, friction, roll-out |
| 10 | [LESSON_10_TRAJECTORY_LINE.md](LESSON_10_TRAJECTORY_LINE.md) | Planned | Debug line showing ball path |

**Phase 3+ (later):** Mevo+ JSON → your Speed / Angle variables (not separate ball lessons).

---

## How this maps to your project roadmap

| Roadmap phase | These lessons |
|---------------|---------------|
| Phase 1 — Foundations | Lessons 1–3 |
| Phase 2 — Ball flight physics | Lessons 4–10 |
| Phase 3 — Mevo+ | Feed real shot data into Lesson 7–8 style variables |

---

## Lesson flow (simple diagram)

```
Move (L1) → Speed dial (L2) → Stop / Launch once (L3)
    → Collisions (L4) → Manual bounce (L5)
    → Physics sim option (L6) → Gravity arc (L7)
    → Projectile component (L8) → Roll (L9) → Debug line (L10)
```

**Plain English:** Learn to move, then control speed, then hit once, then bump into things, then bounce by hand, then add gravity and real golf flight tools.
