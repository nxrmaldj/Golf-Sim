# GolfSim — Ball Lessons (Blueprint Track)

**How to use these files**

1. Read **Part A — What You Are Building (Overview)** first.
2. Create every variable in the **Variables to create** table before wiring nodes.
3. Read **Blueprint wires** box in each lesson (white = execution, colored = data).
4. Follow **#### Step** sections in order — details are indented underneath.
5. Run the **Test** at the end of each build section before moving on.
6. `git add` / `commit` / `push` when the lesson is done.

**Lesson format (all lessons 1–10):**

| Section | Purpose |
|---------|---------|
| **Part A — Overview** | End result in plain English |
| **Blueprint wires** | White vs colored pins; how to **Get** variables |
| **Variables to create** | Name, type, default — before any node wiring |
| **#### Step N** | Where → what to add → how to connect → what it does → why |
| **Plain English summary** | 1–2 sentences |
| **Test** | What you should see in Play mode |

**Step styling:** `#### Step N — Title` headings with indented blockquote content underneath.

**Important habits:**
- **Get Variable** = drag from **My Blueprint** — not off white execution pins.
- **Branch True** = white output when condition is true — movement lives there.
- Don't expect physics from a later lesson (e.g. no gravity arc in Lesson 5).

---

| # | File | Status | What you learn |
|---|------|--------|----------------|
| 1 | [LESSON_01_MOVING_SPHERE.md](LESSON_01_MOVING_SPHERE.md) | **Done** | Blueprint Actor, Event Tick, movement |
| 2 | [LESSON_02_SPEED_VARIABLE.md](LESSON_02_SPEED_VARIABLE.md) | **Done** | Variables, Delta Seconds, tweakable speed |
| 3 | [LESSON_03_STOP_AND_LAUNCH.md](LESSON_03_STOP_AND_LAUNCH.md) | **Done** | Stop moving, one-shot "hit" with a key |
| 4 | [LESSON_04_COLLISION_BASICS.md](LESSON_04_COLLISION_BASICS.md) | **Done** | Collision on/off, Sweep, hit walls & floor |
| 5 | [LESSON_05_MANUAL_BOUNCE.md](LESSON_05_MANUAL_BOUNCE.md) | **In progress** | Velocity, Bounciness, bounce on hit |
| 6 | [LESSON_06_PHYSICS_SIMULATION.md](LESSON_06_PHYSICS_SIMULATION.md) | Planned | Simulate Physics vs manual |
| 7 | [LESSON_07_GRAVITY_AND_ARC.md](LESSON_07_GRAVITY_AND_ARC.md) | Planned | Gravity, launch angle, golf arc |
| 8 | [LESSON_08_PROJECTILE_MOVEMENT.md](LESSON_08_PROJECTILE_MOVEMENT.md) | Planned | UE Projectile Movement Component |
| 9 | [LESSON_09_ROLL_ON_GROUND.md](LESSON_09_ROLL_ON_GROUND.md) | Planned | Landing, friction, roll-out |
| 10 | [LESSON_10_TRAJECTORY_LINE.md](LESSON_10_TRAJECTORY_LINE.md) | Planned | Debug line showing ball path |

**Phase 3+ (later):** Mevo+ JSON → **ShotSpeed** / **LaunchAngle** variables (Lessons 7–8).

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

**Plain English:** Learn to move, control speed, hit once, collide, bounce by hand, then add gravity, flight tools, roll, and debug path.
