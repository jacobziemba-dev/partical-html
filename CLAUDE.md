# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Single-file interactive particle simulation built with vanilla HTML5 Canvas and JavaScript. No build tools, dependencies, or package manager — just open `particle-playground.html` in a browser.

## Architecture

Everything lives in `particle-playground.html` (~995 lines):

- **CSS** (lines 7-191): Dark theme UI with CSS custom properties (`--accent`, `--secondary`) that change per color theme. Collapsible control panel with sliders, dropdowns, and toggle buttons.
- **HTML** (lines 193-316): Canvas element + control panel with collapsible sections (Mode, Particles, Physics, Visuals, Actions).
- **JavaScript** (lines 318-992): Organized into labeled sections:

### Key Sections

- **CONFIGURATION** (lines 321-361): `THEMES` object (6 color themes), `MODES` array (8 modes), `config` object (all tunable settings), `state` object (runtime state).
- **SPATIAL PARTITIONING** (lines 394-428): Grid-based spatial hash (`CELL_SIZE=100`) with `buildSpatialGrid()` and `getNearby()` — reduces O(n²) particle interactions to ~O(n).
- **PARTICLE CLASS** (lines 432-706): Each particle has position, velocity, hue, life, mode, stage (for fireworks), and prevX/prevY (for trail shape). Mode physics are separate methods (`modeGravity`, `modeSwarm`, `modeFireworks`, etc.). Drawing supports 4 shapes (circle, square, star, trail) and themed colors.
- **SPAWN LOGIC** (lines 729-757): Mode-specific spawning — explosion gets radial burst velocity, fireworks get rocket stage with upward thrust, others get random spread.
- **ANIMATION LOOP** (lines 761-816): `requestAnimationFrame` loop with spatial grid rebuild each frame, FPS counter, and pause support.
- **UI WIRING** (lines 865-976): `wireSlider()` and `wireToggle()` helpers connect DOM controls to `config` object. Keyboard shortcuts: Space=pause, C=clear, M=cycle mode, T=cycle theme.

### 8 Particle Modes

| Mode | Physics |
|------|---------|
| Gravity | Mouse attraction (when held) + inter-particle attraction via spatial grid |
| Repel | Particles flee from mouse, inverse-distance force |
| Orbit | Tangential + centripetal forces around mouse cursor |
| Swarm | Boids algorithm: separation, alignment, cohesion |
| Explosion | Radial burst from spawn point, gravity pulls down, fast fade |
| Vortex | Spiral inward toward mouse, tangential force increases with proximity |
| Flow Field | Pseudo-Perlin field via `sin(x)*cos(y)`, time-varying |
| Fireworks | Two-stage: rocket ascends → explodes into spark children that fall |

### 6 Color Themes

Each theme defines `hueRange`, `bg`, `accent`, `secondary`, and `bgTint`. The `themeHue()` function maps particle hues into the theme's range. Theme changes also update CSS custom properties for UI accent colors.
