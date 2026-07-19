# Loom Sort

A touch-first web game about sorting rainbow loom bands back into their organizer box. Based on a real spill, and on the discovery that re-sorting thousands of bands by color is genuinely calming.

**Play it: https://soundboy1024.github.io/loom-sort/**

Everything is one file, `index.html`. No build step, no dependencies, no network. Open it in a browser and play.

## How to play

Drag each band from the plate into the matching color cell of the organizer. Right color: it settles in with a soft bounce and a quiet click. Wrong color: it drifts gently back to the plate. No penalties, no failure.

## Modes

- **solo**: the calm one. Sort the spill at your own pace. An ambient stopwatch starts on your first pickup and the completion card shows your time, but nothing counts down and nothing pressures.
- **race**: two players, one shared plate, a full organizer box each. Both sort at the same time with multi-touch. A band scores for whoever's box it lands in. Most banked when the plate empties wins.
- **split**: two players, each with their own plate and box, identical band mixes. You can only bank into your own box. First to sort everything wins.
- **turns**: players alternate full spills on the same board. Fastest time wins, compared to the tenth of a second.

## Controls

- The **minus / plus stepper** sets the spill size, 10 to 200 bands (per player in split mode). The plate shows up to 50 at a time and refills as you sort.
- **sound** toggles the quiet synthesized clicks.
- Works with touch, mouse, touchpad, or pen. Two fingers can drag two bands at once.

## Running it

- **Laptop**: double-click `index.html`, or drag it into any browser. Landscape is the intended layout.
- **Phone**: works in mobile Chrome and iOS Safari. Portrait suits solo and turns; the two-player modes want a laptop-sized screen. Use your browser's "Add to Home screen" to launch it like an app.
- **Serving it** (optional): any static file server works, for example `python -m http.server` in this folder, then visit `http://localhost:8000`.

## Repo contents

- `index.html`: the whole game.
- `CLAUDE.md`: project instructions and design principles for agentic coding sessions.
