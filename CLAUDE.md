# CLAUDE.md

## Who I am
Casey. CEO/CTO and sole owner of Lioren Technologies (AI safety, currently winding down active products). Former A1 audio specialist, Google NYC. Learning agentic coding by building and testing my own IP. Actively job hunting in AI and building resume through certifications and shipped work.

## Hardware
- PC: HP OmniBook Ultra (AMD Ryzen AI 9 365, 32GB RAM, 1TB SSD)
- Phone: Pixel 10 Pro
- Research rig: local Mac Neo driving a remote Mac Studio. Limited resources, so only a few models stay loaded at a time; models are deleted and reloaded as needed. All research data syncs to Google Drive via rclone.

## Hard formatting rules
- Never use em dashes. Use commas, periods, or parentheses instead.
- Never use ampersands in drafted content. Write "and".
- Tag any claim not directly supported by a citation or quoted source with [INFER].
- No code blocks unless I ask for code.
- No hyperbole.

## Communication rules
- Peer-level dialogue. No sycophancy, no openers, no filler. Get to the point.
- Concise and direct by default.
- Push back on weak ideas. Do not soften it.
- Flag scope creep hard. If a task is growing past its original goal, say so before continuing.
- &align → respond with a brief summary of the current goal and a course correction if we have drifted.

## Working conventions
- I am learning to code with agentic coders. Explain nontrivial decisions briefly so I learn, but do not lecture.
- Before large refactors or new dependencies, state the tradeoff in one or two lines and wait for a go.
- Prefer small, testable increments over big rewrites.
- If a result can be achieved with a simpler prompt or existing tool instead of new code, say so.

## Active projects (context, not tasks)
- avbase: venue-aware knowledge base for people working rooms with technical gear. Early stage.
- Confident hallucination detector: research project. Goal includes proving what does not work. Claude Science and Claude Code coordinate by writing docs to a shared Google Drive folder.
- Town-council campaign site: first public shipped product, maintained as needed.
- Lioren IP (AEC, Audit Engine, Lioren Engine): shelved or paused. Do not propose reviving these unless I raise it.

## Do not
- Do not assume Lioren products are active or commercial.
- Do not expand project scope to make something "more complete."
- Do not use em dashes anywhere, including code comments and commit messages.

---

# Project: Loom Sort (working title)

## What this is

A relaxing, touch-first web game about sorting rainbow loom bands back into their organizer box. Inspired by a real event: a dropped loom kit, and the discovery that re-sorting thousands of bands by color is genuinely calming. The game should feel like that. It is a gift for a specific person, not a commercial product.

## Scope and constraints

- MOCKUP FIRST. The immediate goal is a playable single-screen prototype, not a finished game. Do not build menus, accounts, monetization, levels, or settings screens unless explicitly asked.
- Web only. Single HTML file if possible (HTML plus CSS plus vanilla JS or one lightweight lib). No build step, no framework, no bundler, no native wrapper. Must run by opening the file or a URL in a browser.
- Primary target: touchscreen Windows laptop in landscape (the household has two). Touch is the first-class input. Mouse and touchpad must also work but never at the expense of touch feel.
- Secondary target: phone in portrait. Layout should adapt down (stack plate above box) but do not compromise the landscape experience to get there. If responsive layout adds real complexity to the mockup, landscape-only is acceptable for v1.
- Flag scope creep hard. If a request drifts toward 3D physics, app store deployment, networked multiplayer, or level progression systems, stop and confirm before building. Local same-screen multiplayer is already built and in scope.

## Core loop

1. A pile of mixed loom bands sits in a staging area (visually: a white round plate) on one side of the screen.
2. On the other side, an open organizer box with a grid of compartments, each compartment assigned a color (visually: the clear plastic case with 20 to 24 cells).
3. Player drags a band from the pile into a compartment.
4. Correct compartment: band settles in with a soft, satisfying animation and a quiet sound. The compartment's fill count ticks up.
5. Wrong compartment: gentle rejection, band drifts back to the plate. No penalty, no harsh feedback, no failure state.
6. When the plate is empty, a calm completion moment (the full, tidy box). Offer "spill again" to restart.

## Input handling

- One code path for all inputs. Use pointer events (pointerdown, pointermove, pointerup, pointercancel). These unify touch, mouse, touchpad, and pen on Windows and mobile. Do not write separate touch and mouse handlers, and do not use the HTML5 drag-and-drop API (unreliable on touch).
- Set touch-action: none on draggable elements and the play area so the browser does not hijack drags for scrolling or zooming.
- Drag feel: the band should track the finger or cursor directly with no lag, lift slightly (scale up, soft shadow) on pickup, and drop targets should highlight when a band hovers over them.
- Hit targets sized for fingers, not cursors: compartment cells at least 64px on the laptop layout. If the full 20 to 24 cell grid gets cramped, cut to 12 to 15 colors rather than shrinking targets.

## Design principles

- Calm over challenge in solo mode. No countdowns, no lives, nothing blocks. An ambient stopwatch exists (starts on first pickup, whole seconds, never pressures) and the sorted count is ambient. The 2P modes (race, split, turns) are the competitive outlet; keep the pressure there, never in solo.
- The satisfaction is the point. Invest effort in the feel of a band landing in the right cell: slight bounce, subtle scale, soft click sound. This is the whole game.
- Sound: quiet, tactile, optional. Muted by default is acceptable for a mockup; add a mute toggle if sound is added.
- Color accuracy matters. Bands should read as distinct colors at small sizes. Include tricky near-pairs (teal vs turquoise, clear vs white, pale yellow vs cream) only after the basic loop works, since distinguishing similar colors was part of the real sorting task.
- Visual reference: two photos exist of the real kit (white plate staging area, multi-cell clear organizer with saturated color piles). Match that vibe: clean, bright, uncluttered.

## Technical notes

- Bands are simple shapes: a ring rendered as a circle with a hole (SVG or canvas), slightly irregular so a pile looks organic. No physics engine. Fake the pile with randomized offsets and rotations.
- Keep state as a plain JS object: pile contents, per-compartment color assignment and count. No storage needed for the mockup; add localStorage for resume later only if asked.
- Performance target: smooth on both household machines and a mid-range phone. Cap the visible pile (render roughly 30 to 50 bands at once, backfill from a queue) rather than drawing thousands.

## Later, maybe (do not build now)

- PWA manifest and home screen icon for the Pixel
- Difficulty via similar-color sets or bigger spills
- A "her kit" mode matching the exact colors of the real organizer
- Ambient background audio
- Stats (total bands sorted, lifetime)

## Definition of done for the mockup

One HTML file. Loads in a browser on the touchscreen laptop. Pile of roughly 40 mixed bands on a plate, 12-cell colored organizer beside it, drag to sort by touch or mouse, satisfying land animation, gentle bounce-back on mistakes, completion state, restart. Nothing else.

## Status (2026-07-18)

Mockup definition of done is met and extended, all in index.html:
- Four modes: solo (default, calm), race (shared plate, box per player, most banked wins), split (own plate and box each, first done wins), turns (alternate spills, fastest time wins, tenths shown).
- Multi-touch drag: per-pointer state in a Map, so two players sort simultaneously.
- Band count stepper, 10 to 200 in steps of 10, visible pile capped at 50 with queue backfill.
- Ambient stopwatch, sound toggle with synthesized blips, reduced motion respected.
- Portrait layout exists for solo and turns but is unverified on a real phone. 2P modes are landscape.
- Known technical guards worth keeping: setPointerCapture wrapped in try/catch, landing state committed via setTimeout fallback so a stalled animation cannot strand a band.
