# Lesson 4: Turn On Collisions

**Project:** GolfSim  
**Prerequisite:** Lesson 3 (`bIsMoving`, launch on Space, movement on Branch True)  
**Goal:** Ball stops when it hits walls or floor — no passing through.

**Plain English:** Give the ball a solid body so the world pushes back.

---

## Part A — What You Are Building (Overview)

By the end of this lesson you will have:

1. **Collision** enabled on ball and floor/walls.
2. **Sweep** on during movement.
3. **Stop on hit** — `bIsMoving = false` when sweep detects a blocking hit.

**End result:** Launch ball into wall → it stops. Lesson 5 replaces stop with bounce.

---

## Part B — New Words

| Word | Simple meaning |
|------|----------------|
| **Collision** | Two solid objects can't share the same space. |
| **Sweep** | Check along the movement path each frame — catches hits between frames. |
| **Block** | Objects physically stop each other. |
| **Hit Result** | Data package about a hit (location, normal, what was hit). |
| **Root Component** | Main component of the Actor — movement and sweep use this. |

---

## Part C — Blueprint Wires (Quick Reminder)

- **Sweep** is a **checkbox** on Add Actor World Offset — not a separate white chain.
- **Return Value** / **Sweep Hit Result** = **colored data** (did we hit something?).
- **Break Hit Result** → **Blocking Hit** → **Branch Condition** = colored bool into Branch.

---

## Part D — Build Section 1: Collision on Ball and Level

### What we're building now
Make ball and floor/walls solid before wiring stop logic.

#### Step 1 — Make sphere the Root (critical)

> 1. **`BP_MovingBall`** → **Components**.
> 2. Right-click **Static Mesh** (sphere) → **Set as Root Component**.
> 3. **Compile** + **Save**.
> 4. Delete old ball in level → drag in fresh copy.
>
> **What Root does:** Sweep and collision use the root transform. Child mesh = hits often fail.
>
> **Why first:** Fix this before debugging sweep — saves hours.

#### Step 2 — Ball collision settings

> Select **Static Mesh** → **Details**:
> - **Collision Presets** → **BlockAllDynamic**
> - **Mobility** → **Movable**
> - **Simulate Physics** → **OFF**
>
> **Why Movable:** Static objects don't sweep-move correctly.

#### Step 3 — Floor and walls in level

> 1. Place **Cube** meshes as floor/wall.
> 2. **Mobility** → **Static**, **Collision** → **BlockAll**.
> 3. **Simulate Physics** → **OFF** on cubes and ball.
>
> **Test:** Play with movement — ball should not fall through floor (may not stop yet if sweep off).

---

## Part E — Build Section 2: Enable Sweep

#### Step 1 — Turn on Sweep

> 1. **Event Graph** → **Add Actor World Offset** (on Branch True path).
> 2. Check **Sweep** = ✓.
>
> **What Sweep does:** Line-check from old position to new position each move.
>
> **Why:** Without sweep, ball can tunnel through thin walls between frames.

---

## Part F — Build Section 3: Stop on Hit

### What we're building now
If sweep hits something solid, stop the ball.

#### Step 1 — Get hit data from sweep

> 1. From **Add Actor World Offset**, drag **Sweep Hit Result** pin (colored struct).
> 2. Add **Break Hit Result**.
> 3. Drag **Blocking Hit** (bool) → new **Branch** **Condition** pin.
>
> **What Break Hit Result does:** Pulls useful pieces out of the hit package.
>
> **What Blocking Hit means:** True = solid wall/floor, not just overlap trigger.

#### Step 2 — Stop on blocking hit

> 1. **White:** from **Add Actor World Offset** execution out → **Branch (Blocking Hit?)**
> 2. **True** path → **Set bIsMoving** = **false**.
> 3. **False** path → leave empty (keep moving next frame).
>
> **What Set bIsMoving false does:** Stops Event Tick movement — ball freezes.
>
> **Why after move node:** Check hit result from *this frame's* move attempt.
>
> ```
> Branch (bIsMoving) True → Add Actor World Offset (Sweep on)
>                               ↓ white
>                          Branch (Blocking Hit?)
>                               ├─ True → Set bIsMoving false
>                               └─ False → (continue)
> ```

#### Step 3 — Compile + Save

> Click **Compile** + **Save**.

---

### Plain English summary
Move with sweep → if solid hit → stop. Lesson 5 swaps stop for bounce.

### Test

1. **Play** → launch at wall.
2. Ball should **stop** on contact, not pass through.
3. If it never stops, see Troubleshooting below.

---

## Part G — Troubleshooting

| Problem | Fix |
|---------|-----|
| Sweep never hits / Blocking Hit always false | Sphere must be **Root** (Step D1). Re-place ball in level. |
| Cubes fly away | **Simulate Physics OFF** everywhere. |
| Ball is WorldStatic | Ball → **Movable** + **BlockAllDynamic**. |
| No Sweep Hit Result pin (UE 5.7) | Use **Return Value** bool or Break Hit Result from sweep output. |
| Still fails | Ask in chat — may need line trace fallback. |

---

## Part H — Quick Check

1. What does **Sweep** catch that non-sweep misses?
2. **Block** vs **Overlap** — which stops the ball?
3. Why must the mesh be **Root**?

---

## Part I — Git

```powershell
cd "C:\Users\Darre\Documents\Unreal Projects\GolfSim"
git add .
git commit -m "feat: lesson 4 collision and sweep stop on hit"
git push
```
