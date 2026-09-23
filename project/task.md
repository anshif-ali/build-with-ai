# Execution Plan: Simple Calculator Implementation

This execution plan breaks down the development of the Simple Calculator into modular, testable tasks for an automated or pair-programming coding agent.

---

## Task 1: Foundation & Layout (`index.html` & CSS Architecture)
- [x] Create a single self-contained `index.html` file (or structured HTML/CSS/JS if split, but keeping single-file output ready).
- [x] Set up CSS custom properties (variables) for theme styling:
  - Dark mode backgrounds (`#121212`, `#1e1e1e`, `#2a2a2a`)
  - Text colors (primary white, muted secondary for entry line)
  - Accent colors (`#ff9500` / amber for `=` and active operator states)
  - Error state styling (distinct red/warning accent for error mode)
- [x] Build the HTML layout:
  - Container element centered on screen (responsive scaling from mobile viewports to desktop).
  - Two-line display module: upper small line for `<previous> <operator>`, lower large line for `<current>`.
  - Button grid (4x5 layout) with appropriate semantic HTML tags (`<button type="button">`), ARIA attributes, and clear class names.
  - Buttons: `C`, `⌫`, `%`, `÷`, `7`, `8`, `9`, `×`, `4`, `5`, `6`, `−`, `1`, `2`, `3`, `+`, `0`, `.`, `=`
- [x] Add accessibility & responsive styles:
  - Minimum tap target size of 44px x 44px.
  - Keyboard focus rings (`:focus-visible`).
  - `:active` press feedback effects (smooth scale down + color shift).
  - Media query for `@media (prefers-reduced-motion: reduce)`.

---

## Task 2: Core Calculator Engine (Decoupled JavaScript Logic)
- [x] Implement a clean, decoupled `CalculatorEngine` class/module in JS with internal state:
  - `current`: string (represents current input/value being typed)
  - `previous`: string (represents stored previous value)
  - `operator`: string | null (current pending operator `+`, `-`, `*`, `/`)
  - `overwrite`: boolean (flag indicating if next digit replaces current input)
  - `isError`: boolean (flag indicating calculator is in error state)
- [x] Implement engine methods:
  - `appendDigit(digit)`: Handles digit inputs `0-9` and decimal point `.`. Prevents duplicate decimal points. Resets error state if active.
  - `chooseOperator(op)`: Sets current operator, evaluates chained operations if an operator & previous value already exist.
  - `compute()`: Performs arithmetic calculation based on `previous`, `current`, and `operator`.
    - Handle standard operations: `+`, `-`, `*`, `/`.
    - Handle Division by Zero: set `isError = true`, set display to `"Error"`.
    - Handle floating point precision issues (e.g. `0.1 + 0.2 = 0.3` using precision formatting).
    - Handle overflow / non-finite results.
  - `percent()`: Divides `current` value by 100.
  - `deleteLast()`: Removes last character from `current` input (backspace).
  - `clear()`: Fully resets state to initial default.

---

## Task 3: DOM Binding & UI State Synchronization
- [x] Create UI controller to bind `CalculatorEngine` to the DOM:
  - Query display elements (`#previous-display`, `#current-display`).
  - Implement `updateDisplay()` function to render current engine state to screen:
    - Render upper line (previous entry + operator).
    - Render lower line (current value or "Error").
    - Toggle CSS error class on display when `isError` is true.
  - Highlight active operator button visually when an operator is chosen and pending.
- [x] Bind click events to all calculator buttons via event delegation or individual handlers:
  - Route digit button clicks to `appendDigit()`.
  - Route operator button clicks to `chooseOperator()`.
  - Route `=`, `C`, `⌫`, `%` to their respective engine methods.

---

## Task 4: Keyboard Navigation & Shortcuts
- [x] Add global `keydown` event listener to document.
- [x] Map keyboard inputs to calculator actions:
  - `0–9` -> Append digit
  - `.` -> Decimal point
  - `+`, `-`, `*`, `/` -> Map to corresponding operators (`+`, `−`, `×`, `÷`)
  - `Enter` or `=` -> Compute result (`=`)
  - `Escape` -> Clear (`C`)
  - `Backspace` -> Delete (`⌫`)
  - `%` -> Percent (`%`)
- [x] Prevent default browser actions for captured keys (e.g., preventing `/` from opening quick find or `Enter` submitting forms).
- [x] Add visual key press animation trigger on UI buttons when matching physical key is pressed.

---

## Task 5: Edge Case Verification & Polish
- [x] Test & verify edge cases:
  - [x] Division by zero (`5 ÷ 0` displays "Error").
  - [x] Sequential keypress after Error state clears error and starts fresh input.
  - [x] Chained operations evaluate left-to-right on consecutive operator presses (`10 + 5 × 2`).
  - [x] Repeated `=` presses handle gracefully.
  - [x] Decimal entry logic (e.g., `0.1.2` should remain `0.12`).
  - [x] Multiple leading zeros handled properly (e.g. `000` -> `0`).
- [x] Verify zero dependencies and standalone compatibility in standard modern browsers.
