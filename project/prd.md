# PRD: Simple Calculator

## Overview
A responsive, browser-based calculator for everyday arithmetic. Clean, minimal interface with full keyboard support, built as a single self-contained HTML/CSS/JS file with no external dependencies.

## Goals
- Fast, reliable arithmetic with zero learning curve
- Works equally well on desktop (mouse + keyboard) and mobile (touch)
- Codebase structured for future extension (history, scientific mode, themes, memory)

## Target Users
Anyone needing quick everyday calculations — no account, no setup, no install.

## Core Functionality
- **Operations:** addition, subtraction, multiplication, division, decimals
- **Chained calculations:** e.g. `10 + 5 × 2` evaluates left-to-right as each operator is pressed
- **Display:** two-line — a small "previous entry + operator" line above a large "current value" line
- **Percent:** converts current value to its /100 equivalent

## Buttons
`0–9`, `.`, `+`, `−`, `×`, `÷`, `=`, `C` (clear), `⌫` (backspace), `%`

## Keyboard Support
| Key | Action |
|---|---|
| `0–9` | Enter digit |
| `.` | Decimal point |
| `+ - * /` | Operator |
| `Enter` or `=` | Calculate |
| `Escape` | Clear |
| `Backspace` | Delete last character |
| `%` | Percent |

## Error Handling
- Division by zero → displays "Error" state (styled distinctly, smaller font, warning color)
- Any non-finite result (overflow) → same Error state
- Pressing any key while in Error state clears and resumes normal input
- No raw `NaN`/`Infinity` ever shown to the user

## Design Requirements
- Dark theme, monospace numeric display, single amber accent color reserved for the equals key and active operator state
- Buttons sized for comfortable tap targets (min ~44px), with visible press feedback (scale + color shift)
- Fully responsive: single-column centered layout, scales from mobile viewport to desktop
- Respects `prefers-reduced-motion`; visible keyboard focus states for accessibility

## Technical Requirements
- Single HTML file, no build step, no external libraries
- Vanilla JS calculator engine decoupled from the DOM/rendering layer
- State model: `current`, `previous`, `operator`, `overwrite` flag, `isError` flag

## Non-Goals (v1)
- Calculation history
- Scientific functions (sin/cos/log/etc.)
- Theme switching
- Memory buttons (M+, M-, MR, MC)

## Future Extensibility
The state/compute logic is isolated from the UI so v2 features can be layered in without a rewrite:
- **History:** log each `previous operator current = result` on equals
- **Scientific mode:** add a toggle that swaps in an extra button grid + extends `compute()`
- **Themes:** swap CSS custom-property values on a `data-theme` attribute
- **Memory:** add `memory` state variable + M+/M-/MR/MC buttons

## Success Criteria
- Every button and keyboard shortcut produces correct, consistent results
- No broken/NaN display states under any input sequence
- Usable one-handed on a phone screen and via keyboard-only on desktop
