# Lesson 1: Make a Sphere Move (Blueprint)

**Project:** GolfSim  
**Phase:** 1 — Setup & foundations  
**Prerequisite:** None  
**Goal:** A ball-shaped object slides across the screen when you press Play.

**Plain English:** You teach the game one rule: *"Every frame, move this object a little bit forward."*

---

## Part A — What You Are Building (Overview)

By the end of this lesson you will have:

1. **`BP_MovingBall`** — a Blueprint Actor (reusable ball object).
2. A **visible sphere** mesh on it.
3. **Event Tick** logic that moves the ball every frame.

**End result:** Press Play → sphere slides smoothly across the level.

---

## Part B — New Words

| Word | Simple meaning |
|------|----------------|
| **Actor** | Any "thing" in the level (ball, light, camera). |
| **Blueprint** | Saved instructions + parts for an Actor. |
| **Component** | A part on an Actor (mesh, collision, etc.). |
| **Event Graph** | Where you connect logic with wires. |
| **Event** | "When this happens…" (game start, every frame, etc.). |
| **Node** | One step in the logic (a box on the graph). |
| **Compile** | Unreal checks and saves your Blueprint logic. |

---

## Part C — Two Wire Types (Read Once — Used in Every Lesson)

| Wire color | Name | What it does |
|------------|------|----------------|
| **White arrow** | *Execution* | Order nodes run in (left → right). |
| **Colored pins** | *Data* | Numbers, vectors, true/false passed into nodes. |

**Plain English:** White = "go do this next." Colored = "here is the number/vector to use."

---

## Part D — Build Section 1: Create the Blueprint

### What we're building now
The ball Actor itself — before any movement logic.

#### Step 1 — Create `BP_MovingBall`

> 1. Open **GolfSim** in Unreal Editor.
> 2. **Content Browser** → **Add (+)** → **Blueprint Class** → **Actor**.
> 3. Name it **`BP_MovingBall`**.
>
> **What this does:** Creates an empty Actor template you can place in the level.
>
> **Why Actor:** Generic object — perfect for a ball with no character movement.

#### Step 2 — Add the sphere mesh

> 1. Double-click **`BP_MovingBall`**.
> 2. **Add Component** → **Static Mesh**.
> 3. Select the Static Mesh component → **Details** → **Static Mesh** → **Shape_Sphere**.
> 4. **Compile** + **Save**.
>
> **What Static Mesh does:** Gives the Actor a visible 3D shape.
>
> **Why a sphere:** Stand-in golf ball until you swap art later.

#### Step 3 — Place in level

> 1. Drag **`BP_MovingBall`** from Content Browser into **Learning_Map**.
> 2. **Save** the level.
>
> **Test so far:** Ball visible in editor — not moving yet.

---

## Part E — Build Section 2: Movement Logic

### What we're building now
Every frame, slide the ball forward on the **X** axis.

#### Step 1 — Add Event Tick

> 1. Open **Event Graph**.
> 2. Right-click → search **Event Tick** → add it.
>
> **What Event Tick does:** Fires every frame (~60× per second) while the game runs.
>
> **Why not BeginPlay alone:** BeginPlay runs once — ball would move once and stop.

#### Step 2 — Add Make Vector (data node)

> 1. Right-click → search **Make Vector**.
> 2. Set **X = 2**, **Y = 0**, **Z = 0**.
>
> **What Make Vector does:** Builds a direction + distance (2 units on X this frame).
>
> **Why X:** Simple left/right slide for Lesson 1. Lesson 2 adds a Speed variable.

#### Step 3 — Add Add Actor World Offset (execution + data)

> 1. Right-click → search **Add Actor World Offset** → add it.
> 2. **White wire:** **Event Tick** → **Add Actor World Offset**.
> 3. **Colored wire:** **Make Vector** output → **Delta Location** pin.
> 4. **Sweep** = unchecked (no collision yet — Lesson 4).
>
> **What the white wire does:** "Every frame, run the move node."
>
> **What the colored wire does:** Tells the move node *how far* to move.
>
> **Full picture:**
> ```
> Event Tick ──white──→ Add Actor World Offset
>                            ↑ colored
>                       Make Vector (2, 0, 0)
> ```

#### Step 4 — Compile + Save

> Click **Compile** + **Save**.

---

### Plain English summary
Every frame: Event Tick runs → move node shifts the ball 2 units on X → looks like smooth sliding.

### Test

1. **Play** → sphere slides across the view.
2. **Stop** → if nothing moves, check white wire from Event Tick to Add Actor World Offset.

---

## Part F — Quick Check

1. What is a **Blueprint** in one sentence?
2. Why **Event Tick** instead of only **BeginPlay**?
3. What is the difference between a **white** wire and a **colored** wire?

---

## Part G — Git

```powershell
cd "C:\Users\Darre\Documents\Unreal Projects\GolfSim"
git add .
git commit -m "feat: add BP_MovingBall lesson 1"
git push
```

---

## What Comes Next (Lesson 2)

- **Speed** variable instead of hardcoded `2`
- **Delta Seconds** so speed is the same on every monitor
