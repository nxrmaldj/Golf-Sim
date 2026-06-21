# Lesson 5: Manual Bounce (No Physics Engine)

**Project:** GolfSim  
**Prerequisite:** Lesson 4 (`BP_MovingBall` stops on sweep hit — collision, Sweep, `bIsMoving`)  
**Goal:** When the ball hits a surface, **you** reverse its direction in Blueprint — a fake bounce you control.

**Plain English:** Instead of Unreal's physics engine doing the bounce, you say: *"Keep the speed, but flip direction along the wall — and lose a little energy each time."*

This teaches **Velocity** and **reflection** — core golf ball ideas for later lessons.

---

## Part A — What You Are Building (Overview)

By the end of this lesson you will have:

1. A **`Velocity`** Vector variable — stores speed **and** direction (X, Y, Z).
2. A **`Bounciness`** Float variable — how much energy survives each bounce (`0.6` = loses some height each time).
3. Movement each frame using **Velocity × Delta Seconds** (not hard-coded +X only).
4. **On Hit** logic that flips velocity and applies Bounciness — ball **reacts when it touches** a surface instead of stopping.

**End result:** Launch the ball → it moves in 3D → **when it hits** a floor, wall, or ceiling → bounces weaker. You are learning the **hit reaction**, not full golf flight yet.

> **Important — no gravity in this lesson:** Lesson 5 does **not** pull the ball down. If you launch **straight up**, it goes up forever and **never** hits the floor — so no floor bounce. That is expected. **Lesson 7** adds gravity (arc up, fall down). For Lesson 5 floor tests, see Part F.

---

## Part B — New Words

| Word | Simple meaning |
|------|----------------|
| **Velocity** | Speed + direction together — a 3D arrow (Vector). Not just "how fast" but "which way." |
| **Vector** | Three numbers in one package: **X** (left/right), **Y** (forward/back), **Z** (up/down). |
| **Normal** | Arrow pointing straight **out** of a surface — perpendicular to the wall/floor. |
| **Reflect** | Mirror velocity across a surface — like a pool ball off a cushion. |
| **Bounciness** | Float variable YOU create. `0` = dead stop, `1` = perfect bounce, `0.6` = loses energy each hit. |
| **Break Vector** | Splits a Vector into separate X, Y, Z floats. |
| **Make Vector** | Combines X, Y, Z floats back into one Vector. |
| **On Component Hit** | Event that fires once when the mesh collides with something solid. |

---

## Part C — Build Section 1: Create Variables

### What we're building now
Two new variables that control how the ball moves and bounces.

### What you need first
- `BP_MovingBall` from Lessons 1–4.
- **`bIsMoving`** (Boolean) from Lesson 3.
- Sweep movement from Lesson 4 (you'll change what happens on hit — bounce instead of stop).

### Variables to create

Open **`BP_MovingBall`** → **My Blueprint** → **+ Variable** for each:

| Name | Type | Default | Instance Editable? | Why |
|------|------|---------|-------------------|-----|
| `Velocity` | **Vector** | `(500, 0, 200)` | Optional ✓ | Direction + speed of the ball. Change in Details to test different launches. |
| `Bounciness` | **Float** | `0.6` | Optional ✓ | Energy kept after each bounce. Lower = ball dies faster. |

**What a Vector variable does:** Holds three numbers at once — the ball's motion arrow in 3D space.

**What a Float variable does:** Holds one decimal number — your bounce energy dial.

**Why create these first:** Every later step reads or writes these variables. Without creating them here, nodes like **Get Velocity** have nothing to connect to.

### Build steps

#### Step 1 — Create `Velocity`

> 1. **+ Variable** → name **`Velocity`**, type **Vector**.
> 2. Default: **X = 500**, **Y = 0**, **Z = 200** (forward + slightly up — tune later).
> 3. Click the **eye icon** if you want to edit per-ball in the level.

#### Step 2 — Create `Bounciness`

> 1. **+ Variable** → name **`Bounciness`**, type **Float**.
> 2. Default: **`0.6`**.

#### Step 3 — Compile + Save

> Click **Compile** + **Save** in the Blueprint toolbar.

---

### Plain English summary
You now have a motion arrow (`Velocity`) and a bounce strength dial (`Bounciness`).

### Test
Both variables appear in **My Blueprint** panel with correct types.

---

## Part D — Build Section 2: Move Using Velocity

### What we're building now
Replace fixed "always move +X" movement with movement along whatever direction **Velocity** points.

### Build steps

> **Two wire types (read this once):**
> - **White arrow** = *execution* — order things run in (left → right).
> - **Colored pins** = *data* — numbers/vectors passed into nodes (Velocity, Delta Seconds, etc.).
> **Get Velocity** uses **colored data pins only** — you do **not** plug it off the white arrow.

#### Step 1 — Open your Event Tick chain

> Find this on your graph:
> ```
> Event Tick ──white──→ Branch (bIsMoving) ──white True──→ [movement nodes]
> ```
> **Why mention Branch True:** Movement should only run when **`bIsMoving`** is true (ball launched). The **True** output is the "yes, move" path. All movement nodes live on that path — not on **False**.

#### Step 2 — Remove old fixed-direction movement

> Delete or disconnect the old **Make Vector** that only used Speed on X (if that's what you had). We'll replace it with Velocity-based movement.
> Leave the **white execution wire** from **Branch → True** — you'll rebuild what comes after it.

#### Step 3 — Add Get Velocity (data node)

> 1. In **My Blueprint** (left panel), find your **`Velocity`** variable.
> 2. **Drag `Velocity` into the empty graph** near your Branch True movement area (don't drag from the white wire).
> 3. From the menu, choose **Get Velocity** — a green/yellow node appears.
>
> **What Get Velocity does:** Reads the current **Velocity** value — it does **not** run by itself; other nodes pull data from it.
>
> **Why not drag off Branch True for this:** Branch True is a **white execution** pin. **Get Velocity** is **data**. You place it separately, then connect its **output pin** to **Multiply** (next step).

#### Step 4 — Scale by Delta Seconds

> 1. Add **Multiply** (search **vector * float** or **Multiply**).
> 2. Connect **Get Velocity** output → Multiply **first input**.
> 3. From **Event Tick**, drag the **Delta Seconds** pin (green, on the event node itself) → Multiply **second input**.
>
> **What this Multiply does:** Converts "speed per second" into "distance this frame" — same idea as Lesson 2.
>
> **Why Delta Seconds comes from Event Tick:** That pin is the time slice for *this frame* — not from Branch.

#### Step 5 — Move with Sweep (execution chain)

> 1. From **Branch → True** (white pin), drag the white wire → add **Add Actor World Offset** (or connect to one you already have on that path).
> 2. Connect Multiply **result** → **Delta Location** on Add Actor World Offset (colored pin).
> 3. Check **Sweep** = ✓, **Teleport** = off.
>
> **What the white chain does:** Branch True → Add Actor World Offset = "when moving, run the move node."
>
> **What the colored wire does:** Feeds *how far* to move this frame.
>
> **Full picture:**
> ```
> Branch True ──white──→ Add Actor World Offset (Sweep on)
>                              ↑ colored
> Get Velocity ──→ Multiply ←── Delta Seconds (from Event Tick)
> ```

#### Step 6 — Remove "stop on hit" from Sweep (Lesson 4)

> If your Lesson 4 graph had:
> ```
> Sweep Hit Result → Break Hit Result → Blocking Hit → Branch → Set bIsMoving false
> ```
> **Remove or disconnect** the **Set bIsMoving false** on hit — Lesson 5 **bounces** instead of stopping. You can keep the Sweep pin connected for debugging but bounce logic lives on **On Component Hit** (next section).

#### Step 7 — Compile + Save

> Click **Compile** + **Save** in the Blueprint toolbar.

---

### Plain English summary
Each frame while moving: read Velocity, scale by Delta Seconds, move the ball that direction with Sweep on.

### Test

1. Temporarily **Set Velocity** on launch to `(500, 0, 200)` if not wired yet.
2. **Play** → launch → ball should move diagonally (forward + up), not only flat +X.

---

## Part E — Build Section 3: Set Velocity on Launch

### What we're building now
When the player presses launch, set **Velocity** once — like the club striking the ball.

### Build steps

#### Step 1 — Open your launch input event

> Find **Input Action** / **Space bar** from Lesson 3.

#### Step 2 — Set Velocity on launch

> On the launch chain, **before or with** **Set bIsMoving = true**:
> 1. **Make Vector** — **X = 500**, **Y = 0**, **Z = 200** (or use **Get Velocity** default — better: drag **Velocity** variable in and use **Set Velocity**)
> **Cleaner approach:**
> 1. Drag **Velocity** into graph → choose **Set Velocity**.
> 2. Leave value at default `(500, 0, 200)` or tune in variable defaults.
> **What Set Velocity does:** Writes a new motion arrow into the variable — one time at launch.
> **Why:** The ball doesn't move until launch (Lesson 3), then flies in the direction you set.

#### Step 3 — Keep Set bIsMoving = true

> **What bIsMoving Branch does on Event Tick:** Only runs movement when true — ball idle until launch.
> **Why:** Same Lesson 3 pattern — unchanged.

#### Step 4 — Compile + Save

> Click **Compile** + **Save** in the Blueprint toolbar.

---

### Plain English summary
Press launch → Velocity is set → bIsMoving true → Tick moves the ball along that arrow.

### Test
**Play** → ball still → press launch → ball moves in the Velocity direction.

---

## Part F — Build Section 4: Bounce on Hit (Floor — Beginner Version)

### What we're building now
When the ball **collides with the floor**, flip its **Z** direction and multiply by **Bounciness**.

This is the **axis flip** method — simpler than full 3D reflection.

> **How do you hit the floor in Lesson 5?** There is no gravity yet, so the ball won't arc down by itself. Use **one** of these tests:
> 1. **Launch downward** — start above floor, Velocity Z **negative** (aimed at ground).
> 2. **Low forward shot** — mostly X, little Z, so the ball reaches the floor quickly on a shallow path.
> 3. **Ceiling first** — ball hits ceiling, Z flips down, then hits floor (needs a ceiling in the level).
>
> **Lesson 7** adds gravity so a normal upward launch comes back down naturally.

### What you need first
- **Velocity** and **Bounciness** variables created (Part C).
- Movement using Velocity (Part D).

### Build steps

#### Step 1 — Enable hit events on the mesh

> 1. **Components** → select **Static Mesh** (sphere).
> 2. **Details** → search **Generate Hit Events** or **Simulation Generates Hit Events** → enable if available.
> 3. Confirm **Simulate Physics** = **OFF** (manual movement — not Lesson 6).
> **What Generate Hit Events does:** Tells Unreal to fire hit events when this mesh collides.
> **Why:** Without this, **On Component Hit** never runs.

#### Step 2 — Add On Component Hit event

> 1. Select **Static Mesh** in Components.
> 2. **Details** → **Events** → **On Component Hit** → click **+** to add to Event Graph.
>    **OR** right-click Event Graph → search **Add Event for Static Mesh** → **On Component Hit**.
> **What On Component Hit does:** Runs your bounce logic **once** at the moment of collision.
> **Why:** Bounce is a reaction to hitting something — not something that runs every frame on Tick.

#### Step 3 — Get current Velocity

> From **On Component Hit** execution pin:
> 1. **Get Velocity**
> **What it does:** Reads motion arrow **before** we change it.
> **Why:** We flip the existing direction — not start from zero.

#### Step 4 — Split into X, Y, Z

> 1. **Break Vector** ← **Get Velocity** return value.
> **What Break Vector does:** Gives you three separate float pins: **X**, **Y**, **Z**.
> **Why:** Floor bounce only flips **Z** (up/down). X and Y stay the same for this simple version.

#### Step 5 — Flip Z and apply Bounciness

> For floor hit, multiply **Z** by **-1** (reverse up/down), then by **Bounciness**:
> 1. **Multiply** (float): **Break Vector Z** × **-1**
> 2. **Multiply** (float): result × **Get Bounciness**
> **What × -1 does:** Going down becomes going up (and vice versa).
> **What × Bounciness does:** Each bounce weaker — `0.6` means 60% of upward speed remains.
> **Why:** Real balls don't bounce forever; neither should yours.

#### Step 6 — Rebuild Velocity

> 1. **Make Vector**:
>    - **X** ← **Break Vector X** (unchanged)
>    - **Y** ← **Break Vector Y** (unchanged)
>    - **Z** ← flipped + damped result from Step 5
> 2. **Set Velocity** ← **Make Vector** output.
> **What Make Vector does:** Packs X, Y, Z back into one Vector variable.
> **What Set Velocity does:** Saves the new motion arrow for next frame's Tick movement.
> **Why:** Next frame the ball moves **away** from the floor with reduced upward speed.

#### Step 7 — Do NOT set bIsMoving false on hit

> **Why:** Stopping was Lesson 4. Now the ball keeps moving after bounce.

#### Step 8 — Compile + Save

> Click **Compile** + **Save** in the Blueprint toolbar.

---

### Axis flip reference (walls — when ready)

| Hit surface | Flip this part of Velocity |
|-------------|----------------------------|
| Floor / ceiling | **Z** (multiply Z by -1 × Bounciness) |
| Wall facing X | **X** |
| Wall facing Y | **Y** |

Start with **floor Z flip only**. Add wall flips once floor bounce works.

---

### Plain English summary
Ball hits floor → On Component Hit fires → flip Z direction → multiply by Bounciness → save new Velocity → ball bounces up weaker.

### Test

1. **Play** → use a test that actually reaches the floor (see box above — e.g. **Velocity Z negative** or low angle toward floor).
2. On hit, ball should reverse Z direction and move away from floor with reduced speed if **Bounciness < 1**.
3. If you launch **only upward** with no ceiling — ball never hits floor; **no bounce is correct** for Lesson 5.

---

## Part G — Build Section 5: Better Bounce with Reflect Vector (Optional Upgrade)

### What we're building now
Full bounce off **any** angled surface using the hit surface normal — not just axis flip.

**Do Part F first.** Only come here when floor bounce works.

### Build steps

#### Step 1 — Get Hit Normal from hit event

> On **On Component Hit**, you have a **Hit** pin (Hit Result struct):
> 1. Drag from **Hit** → **Break Hit Result** → find **Normal** (Vector).
> **What Hit Normal does:** Points straight out of the surface that was hit.
> **Why:** Reflect Vector needs to know **which way the wall faces** to mirror velocity correctly.

#### Step 2 — Reflect Velocity

> 1. **Get Velocity**
> 2. **Reflect Vector**:
>    - **Incoming Vector** ← **Get Velocity**
>    - **Surface Normal** ← **Normal** from Break Hit Result
> **What Reflect Vector does:** Math mirror — calculates the direction the ball should travel after bounce off that surface.
> **Why:** Works on sloped surfaces and corners — not just flat floor/walls.

#### Step 3 — Apply Bounciness

> 1. **Vector × Float**: **Reflect Vector** output × **Get Bounciness**
> **What this does:** Same energy loss as Part F — bounce gets weaker each hit.

#### Step 4 — Set Velocity

> 1. **Set Velocity** ← multiply result.
---

### Plain English summary
Reflect Vector uses the wall's angle to compute bounce direction automatically — more accurate than flipping one axis.

### Test
Launch at a **wall** at an angle — ball should bounce off realistically, not slide along the wall.

---

## Part H — Quick Check

1. What is **Velocity** vs **Speed**?
2. Is **Bounciness** a variable? What type?
3. Why multiply by **Bounciness** after a bounce?
4. Why use **On Component Hit** instead of Event Tick for bounce logic?
5. Manual bounce vs **Simulate Physics** (Lesson 6) — who controls the math?

---

## Part I — Git

```powershell
cd "C:\Users\Darre\Documents\Unreal Projects\GolfSim"
git add .
git commit -m "feat: lesson 5 manual bounce with velocity"
git push
```

---

## Troubleshooting

| Problem | Fix |
|---------|-----|
| On Component Hit never fires | Enable **Generate Hit Events** on mesh. **Simulate Physics** must be OFF. Mesh must be **Root** (Lesson 4). |
| Ball stops on hit instead of bouncing | Remove **Set bIsMoving false** from Lesson 4 sweep logic. |
| Ball falls through floor | Sweep on, mesh is Root, floor has collision (Lesson 4). |
| Ball never bounces off floor | **Expected** if launching straight up — no gravity yet (Lesson 7). Test with downward launch or wall bounce instead. |
| Ball bounces forever at same height | **Bounciness** = 1.0 — lower to 0.6. |
| Ball barely bounces | **Bounciness** too low, or hit speed too small. |
| Bounce goes wrong direction | Use **Reflect Vector** (Part G) or check you flipped the correct axis. |
| Two bounce systems fighting | Use Part F **OR** Part G — not both at once. |

