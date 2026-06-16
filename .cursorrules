# Project Context for Cursor

## What this project is

This is a golf simulation game called Golfsim.
The player swings a real golf club outdoors, a physical launch monitor captures
the shot data, and the game simulates where the ball would have gone on a
fantasy virtual golf course displayed on screen.

## The hardware

* **Launch monitor:** FlightScope Mevo+ (connects over WiFi to the Windows laptop)
* **Platform:** Windows PC / laptop
* **Data format:** The Mevo+ sends shot data via a community connector app using
the GSPro Open API format — a JSON payload delivered over a local TCP socket
on port 921. Shot data includes: ball speed, launch angle, vertical launch
angle, horizontal launch angle, total spin, spin axis, and carry distance.

## The game engine

**Unreal Engine 5 on Windows.**

The developer already knows UE5 from cinematic and animation production work.
They understand the editor interface, Landscape tools, Level Sequences, and
working with assets. What they do NOT yet know is how to write code or build
game logic.

## Primary scripting approach — BLUEPRINTS FIRST

**Always use UE5 Blueprints unless it is genuinely impossible.**

Do not suggest C++ unless a task absolutely cannot be done in Blueprints, and
even then, explain why Blueprints fall short before showing C++. The developer
is learning to code and Blueprints are the correct starting point. They provide
the same logic as C++ in a visual format that is easier to learn and debug.

When showing Blueprint logic, describe it as clearly as possible — name the
nodes, describe how they connect, and explain what each part does.

## The developer's coding experience

**Complete beginner. No prior coding experience.**

They understand 3D production concepts, spatial thinking, and how game engines
work from a content/art perspective — but have never written code before.

**How to communicate with this developer:**

* Always explain what something does before showing how to build it
* Never use a technical term without immediately defining it in plain English
* Explain WHY a thing is built a certain way, not just HOW
* Break everything into the smallest possible steps
* After showing any code or Blueprint logic, summarise what it does in one or
two plain English sentences
* Never assume prior knowledge of programming concepts
* When there are multiple ways to do something, explain the tradeoffs simply
rather than just picking one without explanation

## The game vision

Fantasy / creative golf courses with a realistic feel. The first course is a
desert oasis theme — the player hits across a desert landscape and must land
on oasis water areas to avoid sand hazards. Courses should feel like real golf
in terms of shot mechanics but exist in imaginative environments.

**Game modes to build (in order):**

1. Single player — one person plays a full hole / round
2. Local multiplayer — turn-based, up to 2 players on the same machine

**NOT building:** online multiplayer, real licensed courses, VR support.

## The project roadmap (current phase)

### Phase 1 — Setup \& foundations (Weeks 1–2) ← START HERE

* Install Visual Studio Community 2022
* Set up Git for version control
* Learn UE5 Blueprint basics: variables, events, functions
* Goal: get a sphere moving on screen from Blueprint logic

### Phase 2 — Ball flight physics (Weeks 3–6)

* UE5 Projectile Movement Component
* Ball arc using hardcoded speed, angle, spin
* Terrain collision and ball roll-out
* Trajectory tracer line in 3D

### Phase 3 — Mevo+ connection (Weeks 7–10)

* Community Mevo+ connector app on Windows
* TCP socket listener in UE5 (free plugin from Fab)
* Parse live JSON shot data into ball physics

### Phase 4 — First course: desert oasis (Weeks 11–20)

* Landscape build using UE5 Landscape tools
* Desert environment assets from Fab marketplace
* Hazard detection: sand / oasis / fairway
* Flagstick, hole collision, hole complete event

### Phase 5 — Game loop \& multiplayer (Weeks 21–30)

* Stroke counter and par system
* Scorecard HUD
* Turn-based local multiplayer
* Main menu and course select

### Phase 6 — Polish \& release (Weeks 31+)

* Sound design
* Shot data overlay (Mevo+ stats per shot)
* FlightScope partner program application
* Package for Windows, release on itch.io

## Budget

$0–$200 total. Free tools wherever possible. Paid assets from Fab marketplace
for environment art only (\~$50–100). No paid plugins unless essential and
no free alternative exists.

## How Cursor should behave in this project

1. **Explain before building.** When asked to implement something, explain the
concept and approach first. Only write Blueprint logic or code after the
developer understands what they're about to build.
2. **Small steps.** Break every task into the smallest meaningful unit. Never
build a whole system in one response — build one piece, check understanding,
then continue.
3. **Comment everything.** Any Blueprint logic or code shown should include
comments on every meaningful section explaining what it does and why.
4. **Plain English summaries.** After every technical explanation, add a
one or two sentence plain English summary of what was just built and why.
5. **No assumed knowledge.** If you use a term the developer might not know
(variable, function, node, reference, component, etc.) define it in
plain language the first time it appears.
6. **Blueprints over C++.** Default to Blueprint solutions. Only suggest C++
if it is genuinely the only viable option, and explain why.
7. **Encourage understanding.** If the developer seems to be copying without
understanding, prompt them to explain back what the code does in their own
words before moving forward.

## File structure notes (fill these in as the project grows)

* UE5 project name: \[TO BE FILLED IN]
* Main game Blueprint: \[TO BE FILLED IN]
* Ball Blueprint: \[TO BE FILLED IN]
* Course level file: \[TO BE FILLED IN]

\---

**HOW TO USE THIS FILE**

Save this file as `.cursorrules` (no other filename, no extension after it)
in the root folder of your Unreal Engine project. Cursor will automatically
read it every time you open that project folder. You will not need to
re-explain your project at the start of each session.

You can also paste the contents of this file directly into Cursor's chat
window at the start of any session if you want to remind it of the full context.

