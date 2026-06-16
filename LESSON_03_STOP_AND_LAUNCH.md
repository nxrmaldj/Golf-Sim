# Lesson 3: Stop the Ball + Launch on Key Press

**Project:** GolfSim  
**Prerequisite:** Lesson 2  
**Goal:** Ball sits still until you press a key, then moves once (or starts moving) — like a practice "swing."

**Plain English:** Golf isn't a ball sliding forever. You need **idle** and **go**.

---

## Part A — What You Are Building

1. A **boolean variable** `bIsMoving` (true/false).
2. **Event Tick** only moves when `bIsMoving` is true.
3. **Input** (e.g. Space bar) sets `bIsMoving` true and optionally sets direction/speed once.

---

## Part B — New Words

| Word | Simple meaning |
|------|----------------|
| **Boolean** | True or false only. |
| **Branch** | If true → do A, if false → do B. |
| **Input Action** | "When player presses this key…" |

---

## Part C — The Logic (Preview)

```
Event Tick
    ↓
Branch (Condition = bIsMoving)
    ├─ True  → [your movement from Lesson 2]
    └─ False → (nothing)

Input Action Jump (or custom "Launch")
    ↓
Set bIsMoving = true
```

Optional: on launch, **Set** Speed or direction from variables you pick.

---

## Part D — Build Steps (When Ready)

1. Add variable `bIsMoving` (Boolean), default **false**.
2. Wrap movement nodes after **Branch** → only **True** path moves.
3. **Project Settings → Input** or use **Input Action** node in Blueprint.
4. Map **Space** (or **L**) to fire launch event.
5. On press: **Set bIsMoving** = true.
6. Test: Play → ball still → press key → ball moves.

---

## Part E — Quick Check

1. What is a **Branch** for?
2. Why start with `bIsMoving` = false?

---

## Part F — Git

```powershell
git add .
git commit -m "feat: lesson 3 stop and launch on input"
git push
```
