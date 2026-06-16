# Lesson 1: Make a Sphere Move (Blueprint)

**Project:** GolfSim  
**Phase:** 1 — Setup & foundations  
**Goal:** A ball-shaped object slides across the screen when you press Play.

**Plain English:** You are teaching the game one simple rule: *"Every moment, move this object a little bit forward."*

---

## Part A — What You Are Building (Big Picture)

You will create three things:

1. **A Blueprint Actor** — a reusable "thing" in your game (like a prefab).
2. **A visible sphere** — so you can see it move.
3. **A tiny bit of logic** — instructions that run when the game starts and every frame after.

You will **not** write C++. Everything happens in Unreal's **Blueprint** editor (boxes and wires).

---

## Part B — New Words (Read Once)

| Word | Simple meaning |
|------|----------------|
| **Actor** | Any "thing" in the level (ball, light, camera). |
| **Blueprint** | A saved set of instructions + parts for an Actor. |
| **Component** | A part glued onto an Actor (mesh, physics, etc.). |
| **Event Graph** | The screen where you connect logic with wires. |
| **Event** | "When this happens…" (e.g. when the game starts). |
| **Node** | One step in the logic (a box on the graph). |
| **Compile** | Unreal checks your Blueprint and saves the logic. |

---

## Part C — The Plan (3 Pieces)

### Piece 1 — The Object (`BP_MovingBall`)

- You make a Blueprint based on **Actor** (generic object).
- You add a **Static Mesh** component and set it to a **sphere** shape.
- You drag one into the level so it exists when you hit Play.

**Think of it as:** an empty puppet + a sphere "skin."

### Piece 2 — When Does Logic Run?

Two events matter here:

| Event | When it fires |
|-------|----------------|
| **Event BeginPlay** | Once, right when the game starts |
| **Event Tick** | Over and over, every frame (~60× per second) |

For movement we use **Event Tick**: *"Every frame, nudge forward a little."*

If we only used BeginPlay, we would move once and stop.

### Piece 3 — How Movement Works (The Math, Simply)

Your ball has a **position** in 3D space:

- **X** — left / right
- **Y** — forward / back
- **Z** — up / down

Each frame we say:

> New position = old position + (direction × speed × tiny time slice)

In Blueprint you will use a node called **Add Actor World Offset**:

- **Delta Location** — how far to move this frame (e.g. "2 units forward on X").
- **Sweep** — leave off for now (we are not checking collisions yet).

**Why "tiny time slice"?**  
Screens run at different speeds. **Delta Seconds** = time since the last frame. Multiplying speed × Delta Seconds keeps motion smooth on fast and slow PCs.

**Simple version for Lesson 1:**  
Move **2 units on X every frame** — easy to see; we can add Delta Seconds in a later lesson.

---

## Part D — The Blueprint Logic (What You Will Wire)

Picture this chain in the **Event Graph**:

```
Event Tick
    ↓
Make Vector  (X = 2,  Y = 0,  Z = 0)     ← "move this much this frame"
    ↓
Add Actor World Offset
    - Delta Location = that vector
    - Sweep = unchecked (false)
```

**What each part does:**

1. **Event Tick** — "Go!" every frame.
2. **Make Vector** — a direction + distance (X/Y/Z). `(2, 0, 0)` = slide along X.
3. **Add Actor World Offset** — actually moves the Actor in the world.

**Plain English:** Every frame, slide the ball 2 steps to the right. It looks like smooth motion because frames happen very fast.

---

## Part E — Build Steps (Do These in the Editor)

When you are ready, work through these in order:

### Step 1 — Create the Blueprint

1. Open **GolfSim** in Unreal Editor.
2. In the **Content Browser**, click **Add (+)** → **Blueprint Class**.
3. Choose **Actor** as the parent class.
4. Name it `BP_MovingBall`.

### Step 2 — Add the Sphere Mesh

1. Double-click `BP_MovingBall` to open it.
2. Click **Add Component** → **Static Mesh**.
3. Select the new Static Mesh component.
4. In **Details**, set **Static Mesh** to **Shape_Sphere**.
5. Click **Compile**, then **Save**.

### Step 3 — Add Movement Logic

1. Open the **Event Graph** tab.
2. Right-click → search **Event Tick** → add it.
3. Right-click → search **Make Vector** → set **X = 2**, **Y = 0**, **Z = 0**.
4. Right-click → search **Add Actor World Offset** → add it.
5. Connect:
   - **Event Tick** white execution pin → **Add Actor World Offset** white execution pin
   - **Make Vector** yellow output → **Add Actor World Offset** → **Delta Location**
6. Uncheck **Sweep** on **Add Actor World Offset**.
7. Click **Compile**, then **Save**.

### Step 4 — Test in the Level

1. Drag `BP_MovingBall` from the Content Browser into the level.
2. Press **Play**.
3. The sphere should slide across your view.

---

## Part F — Quick Check (Before or After Building)

Answer in your own words:

1. What is a **Blueprint** in one sentence?
2. Why do we use **Event Tick** instead of only **BeginPlay** for movement?
3. What does **Add Actor World Offset** do?

---

## Part G — Save Your Work to GitHub

After you finish and test in Unreal:

```powershell
cd "A:\Unreal Projects\GolfSim"
git status
git add .
git commit -m "feat: add BP_MovingBall lesson 1"
git push
```

---

## What Comes Next (Lesson 2 — not started yet)

- Use **Delta Seconds** so speed is frame-rate independent
- Change direction or speed with a **variable**
- Replace the sphere with a golf ball mesh

---

*Last updated: Lesson 1 intro — explain before build.*
