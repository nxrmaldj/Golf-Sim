# Lesson 4: Turn On Collisions

**Project:** GolfSim  
**Prerequisite:** Lesson 3  
**Goal:** Ball stops or reacts when it hits walls, floor, or obstacles — not passing through them.

**Plain English:** Give the ball a "solid body" so the world can push back.

---

## Part A — What You Are Building

1. **Collision enabled** on the ball mesh.
2. **Sweep** turned **on** when moving (line trace along movement).
3. A simple **floor + wall** in the level to test hits.
4. **Stop on hit** (simple version before bounce in Lesson 5).

---

## Part B — New Words

| Word | Simple meaning |
|------|----------------|
| **Collision** | Two objects can't occupy the same space. |
| **Sweep** | "Check along the path while moving" — catches hits between frames. |
| **Block** | Objects physically stop each other. |
| **Overlap** | Objects detect touch but may pass through (used for triggers). |
| **Hit Result** | Info about what you bumped (location, surface, actor). |

---

## Part C — Two Places Collisions Live

### On the ball (Static Mesh component)

- **Collision Presets:** e.g. **BlockAll** or **PhysicsActor**
- **Generate Hit Events** (if you need OnComponentHit later)

### On level geometry

- Floor/wall meshes need collision too (most primitives have it by default).

---

## Part D — The Logic (Preview) — Stop on sweep hit

```
Event Tick → Branch (bIsMoving)
    ↓
Add Actor World Offset
    - Delta Location = movement vector
    - Sweep = TRUE   ← important
    - Return Value (hit?) → Branch
         ├─ Hit → Set bIsMoving = false (stop)
         └─ No hit → continue next frame
```

**Note:** Exact pins vary slightly by UE version; search **Add Actor World Offset** and use the **Sweep** checkbox and **Return Value** (boolean hit).

---

## Part E — Build Steps (When Ready)

1. Select **Static Mesh** on `BP_MovingBall` → Details → **Collision** → preset **BlockAll**.
2. Place **Cube** or **Plane** as floor/wall in level; confirm they block.
3. Enable **Sweep** on **Add Actor World Offset**.
4. On hit: **Set bIsMoving** false (stop for now).
5. Play: launch ball into wall — it should stop, not tunnel through.

---

## Part F — Quick Check

1. What does **Sweep** do that non-sweep movement misses?
2. What's the difference between **Block** and **Overlap**?

---

## Part G — Git

```powershell
git add .
git commit -m "feat: lesson 4 collision and sweep stop on hit"
git push
```
