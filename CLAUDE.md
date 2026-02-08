# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Single-file interactive particle simulation built with vanilla HTML5 Canvas and JavaScript. No build tools, dependencies, or package manager — just open `particle-playground.html` in a browser.

## Architecture

Everything lives in `particle-playground.html`:

- **CSS** (lines 7-74): Inline styles for dark theme UI, control panel overlay, and neon-green accent color scheme
- **HTML** (lines 76-96): Canvas element + absolute-positioned control panel with mode toggle, clear button, and particle count
- **JavaScript** (lines 98-334): All logic in a single `<script>` block

### Key Components

- **`Particle` class**: Each particle has position, velocity, radius, HSL hue, life (opacity that fades over time), mode, and mass. Handles its own physics (`update`), rendering (`draw`), and connection lines to nearby particles (`drawConnection`).
- **Three interaction modes** (`gravity`, `repel`, `orbit`): Determine how particles respond to the mouse cursor and each other. Cycled via the "Change Mode" button.
- **Animation loop** (`animate`): Uses `requestAnimationFrame`. Applies a semi-transparent fill each frame for trail effects. Removes dead particles (life <= 0). Draws connection lines between particles within 80px.
- **Particle cap**: 500 max particles, enforced during continuous spawn (mouse hold).
