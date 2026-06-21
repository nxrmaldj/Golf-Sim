# Lesson 3: Stop the Ball + Launch on Key Press

**Project:** GolfSim  
**Prerequisite:** Lesson 2 (`Speed`, Delta Seconds, movement on Event Tick)  
**Goal:** Ball sits still until you press a key, then moves — like a practice swing.

**Plain English:** Golf needs **idle** and **go**. No sliding forever before you "hit."

---

## Part A — What You Are Building (Overview)

By the end of this lesson you will have:

1. **`bIsMoving`** Boolean variable (default `false`).
2. **Branch** on Event Tick — movement only on **True** path.
3. **Space bar** (or Jump action) sets **`bIsMoving = true`** to launch.

**End result:** Play → ball still → press Space → ball moves.

---

## Part B — New Words

| Word | Simple meaning |
|------|----------------|
| **Boolean** | True or false only. |
| **Branch** | If condition true → run True path; else False path (often empty). |
| **Input Action** | Event that fires when player presses a mapped key. |
| **Set (variable)** | Writes a new value into a variable. |

---

## Part C — Blueprint Wires (Quick Reminder)

- **Branch** has a **white input** (when to decide) and **white True/False outputs** (which path runs next).
- **bIsMoving** connects to Branch **Condition** (colored **bool** pin) — not the white wire.
- Movement nodes live on the **True** white output only.

---

## Part D — Build Section 1: Create bIsMoving

### Variables to create

| Name | Type | Default | Why |
|------|------|---------|-----|
| `bIsMoving` | **Boolean** | `false` | Ball idle until launch. |

#### Step 1 — Create the variable

> 1. **My Blueprint** → **+ Variable** → **`bIsMoving`**, type **Boolean**, default **false**.
>
> **Why false at start:** Ball should not move until player presses launch.

#### Step 2 — Compile + Save

> Click **Compile** + **Save**.

---

## Part E — Build Section 2: Branch Before Movement

### What we're building now
Wrap Lesson 2 movement so it only runs when `bIsMoving` is true.

#### Step 1 — Add Branch on Event Tick

> 1. **Event Graph** — find **Event Tick** → **Add Actor World Offset** white chain.
> 2. Break the white wire between them (select wire → Delete).
> 3. Add **Branch** node.
> 4. **White:** **Event Tick** → **Branch** → **True** → **Add Actor World Offset**.
> 5. **Colored:** Drag **`bIsMoving`** into graph → **Get bIsMoving** → **Branch Condition** pin.
> 6. Leave **False** empty (nothing happens when idle).
>
> **What Branch does:** Only runs movement when `bIsMoving` is true.
>
> **Why mention True:** All Lesson 2 movement nodes stay on the **True** white pin — not False, not Condition.
>
> ```
> Event Tick ──white──→ Branch (bIsMoving?)
>                          ├─ True ──white──→ [movement from Lesson 2]
>                          └─ False → (nothing)
> ```

#### Step 2 — Compile + Save

> Click **Compile** + **Save**.

---

## Part F — Build Section 3: Space Bar Launch

### What we're building now
Press Space → set `bIsMoving` true.

#### Step 1 — Map Space to Jump (if not already)

> 1. **Edit → Project Settings → Engine → Input**.
> 2. Under **Action Mappings**, find **Jump** (or add action **Launch**).
> 3. Bind **Space Bar**.
>
> **Why Jump:** UE's default action often already mapped to Space in blank projects.

#### Step 2 — Add Input Action event

> 1. In **Event Graph**, right-click → **Input → Jump Action** (or your Launch action).
> 2. Use the **Pressed** pin (white) — fires once per key press.
>
> **What Input Action Jump does:** Listens for Space (via Jump mapping) — separate from Event Tick.
>
> **Why separate event:** Launch is one-time on press; movement is every frame on Tick.

#### Step 3 — Set bIsMoving on press

> 1. Drag **`bIsMoving`** into graph → choose **Set bIsMoving**.
> 2. **White:** **Input Action Jump Pressed** → **Set bIsMoving**.
> 3. Check the bool on **Set bIsMoving** = **true**.
>
> **What Set bIsMoving does:** Flips ball from idle to moving.
>
> **Why chain from Pressed white pin:** Input events use white execution wires like Event Tick.

#### Step 4 — Compile + Save

> Click **Compile** + **Save**.

---

### Plain English summary
Tick checks bIsMoving — only moves when true. Space sets it true once.

### Test

1. **Play** → ball still.
2. Press **Space** → ball moves.
3. Without pressing Space, ball never moves.

---

## Part G — Quick Check

1. What does **Branch** do with true vs false?
2. Why is **bIsMoving** false at level start?
3. Is **Get bIsMoving** a white or colored connection to Branch?

---

## Part H — Git

```powershell
cd "C:\Users\Darre\Documents\Unreal Projects\GolfSim"
git add .
git commit -m "feat: lesson 3 stop and launch on input"
git push
```

---

## Note — Level Blueprint vs ball Blueprint

Put **Input Action** and **Set bIsMoving** inside **`BP_MovingBall`** for this lesson. If Space doesn't fire, select the ball in the level → **Details** → **Auto Receive Input** → **Player 0**.

For **two balls** on one key, use Level Blueprint and call a **LaunchBall** function on each (see Lesson 6 chat notes).
