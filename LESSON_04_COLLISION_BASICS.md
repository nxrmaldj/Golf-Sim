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

1. **Make the sphere the Root component** (see Troubleshooting below — critical).
2. Select **Static Mesh** on `BP_MovingBall` → Details → **Collision** → preset **BlockAllDynamic** (ball is moving).
3. Set ball **Mobility** → **Movable**. Cubes/floor stay **Static** + **BlockAll**.
4. Place **Cube** as floor/wall in level; **Simulate Physics OFF** on cubes.
5. Enable **Sweep** on **Add Actor World Offset**.
6. **Sweep Hit Result** → **Break Hit Result** → **Blocking Hit** → Branch → **Set bIsMoving** false.
7. Play: launch ball into wall — it should stop, not tunnel through.

---

## Troubleshooting — "Sweep never hits / Blocking Hit always false"

### Root component (most common)

New Blueprint Actors start with an empty **Scene Component** as **Root** — no collision.

If your **sphere mesh is a child** (not Root), sweep and hit detection often **fail** even when wireframes look correct.

**Fix:**

1. Open `BP_MovingBall` → **Components** panel.
2. Right-click **Static Mesh** (sphere) → **Set as Root Component**.
3. **Compile** + **Save**.
4. Delete old ball in level → drag in a fresh copy.

**Plain English:** The ball mesh must be the "main handle" of the Actor — not a child hanging off an invisible root.

### Other checks

| Problem | Fix |
|---------|-----|
| Cubes fly away | **Simulate Physics OFF** on cubes and ball |
| Ball is WorldStatic | Ball → **Movable** + **BlockAllDynamic** |
| No red Return Value pin (UE 5.7) | Use **Break Hit Result** → **Blocking Hit** |
| Still fails | **Line Trace by Channel** before move (see chat / ask in Lesson 4) |

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
