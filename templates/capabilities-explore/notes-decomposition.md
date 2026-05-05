---
url: https://chatgpt.com/c/69f87cf9-bdf0-83eb-989d-476d0fe9af42
---

# Prompt

> [!NOTE] Prompt
> 
> **Given Target Project Description**
>
> "I want to build an RPN calculator web app and have a packaged desktop version later on. I am thinking of supporting add, sub, neg, mul, div, sqr, sqrt, inv, pow, abs, exp, and ln functions. I also want to have undo capability."
>
> **Run Analysis**
>
> Generate a feature/highlights list for the described target.

## GigaChat

### 📌 **Feature Highlights List for RPN Calculator Web App**

#### Core Features:

- ✅ **Reverse Polish Notation (RPN)** interface for efficient mathematical calculations without parentheses or operator precedence issues.
- ✅ **Web Application**: Accessible via browser with no installation required initially.
- ✅ **Desktop Version**: Planned release as standalone application for offline use and enhanced user experience.

---

#### Mathematical Functions Supported:

- ➕ Addition (`add`)
- ➖ Subtraction (`sub`)
- ⬇ Negate value (`neg`)
- × Multiplication (`mul`)
- ÷ Division (`div`)
- ⁿ² Square number (`sqr`)
- √ Square root (`sqrt`)
- ℑ⁻¹ Inverse (`inv`)
- ↑ Exponentiation (`pow`)
- |⟩ Absolute value (`abs`)
- ℯ Exponential function (`exp`)
- ℓ Natural logarithm (`ln`)

---

#### User Experience Enhancements:

- ↺ **Undo Capability**: Easily revert last operation in case of input errors or miscalculations.

---

This combination of features makes your project ideal for users requiring quick, precise scientific and engineering computations across different platforms.

---
---

## DeepSeek

**Feature / Highlights List**

**Core RPN Calculator**

- **Stack-based operation** with at least a 4-level visible stack (X, Y, Z, T registers)
- **Enter** key to push values and duplicate top-of-stack after entry
- **Swap (x↔y), Roll Down, Roll Up** for stack manipulation
- **Last X** register preserved for error recovery
- Clear stack, clear entry, and drop (pop) functions

**Arithmetic & Algebraic Functions**

- **Two-operand operations:** addition (`+`), subtraction (`-`), multiplication (`×`), division (`÷`)
- **Power function:** `yˣ` (Y raised to the X)
- **Sign change:** `neg` (±) negates the value in the X register
- **Inverse:** `inv` (1/x)

**Scientific Functions**

- **Square & square root:** `sqr` (x²) and `sqrt` (√x)
- **Absolute value:** `abs`
- **Exponential & natural log:** `exp` (eˣ) and `ln` (natural logarithm)
- All functions operate on the X register and replace it with the result, consistent with RPN logic

**Undo Capability**

- Unlimited multi-level undo that reverts the complete calculator state (stack, Last X, memory registers if any)
- Redo support to restore undone states
- Undo/redo shortcuts (Ctrl+Z / Ctrl+Shift+Z) and on-screen buttons

**User Interface**

- Clean, responsive web interface that mimics a classic calculator layout
- Distinct clickable buttons for all operations, including dedicated undo/redo buttons
- Input display showing the current X register and optionally the entire stack
- Key bindings for power users (Enter, operators, undo)
- Dark/light theme support for comfortable use

**Persistence & State**

- Automatic session saving so the stack and undo history survive page refresh
- Manual save/load of calculator state as a file

**Web App Features**

- Progressive Web App (PWA) capabilities: installable on desktop and mobile, works offline
- Responsive design that adapts to phone, tablet, and desktop browsers
- No account required – all data stays local in the browser

**Packaged Desktop Version**

- Cross-platform desktop app (Windows, macOS, Linux) built using the same codebase (e.g., Electron or Tauri)
- Native installers/executables, not requiring a browser
- System tray and menu bar integration for quick access
- Keyboard-driven operation enhanced with global shortcuts (optional)
- Offline-first with identical functionality to the web version

**Additional Highlights (Planned/Extensible)**

- Memory registers (store/recall/add-to-memory)
- Customizable precision and display notation (FIX, SCI, ENG)
- Programmable macros or keystroke recording
- Import/export stack and programs as text or JSON
- Clipboard interaction: copy stack value, paste numbers from clipboard
- History log showing each operation step and stack change, exportable as a report
- Unit conversions and constants library (future expansion)

---
---

## Qwen

**Model**: Qwen3.6-Plus

Here’s a structured feature/highlights list based on your project description, organized by functional domain for clarity and planning:

### 🔢 Core RPN Engine

- **Stack-based computation** following standard RPN conventions (automatic stack lift/drop, register management)
- **Real-time stack visualization** (typically 4+ levels) with clear active/inactive register highlighting
- **Safe evaluation pipeline** (no `eval()`; strict type/stack validation before execution)
- **Robust error handling** for division by zero, domain violations, stack underflow/overflow, and invalid input

### 📐 Supported Mathematical Functions

- **Binary Operations:** `add` (+), `sub` (−), `mul` (×), `div` (÷)
- **Unary/Advanced Functions:** `neg` (±), `sqr` (x²), `sqrt` (√x), `inv` (1/x), `pow` (xʸ), `abs` (|x|), `exp` (eˣ), `ln` (ln x)
- **Consistent RPN operand mapping** (1-operand vs 2-operand behavior clearly enforced)
- **Modular function registry** for easy future expansion (trigonometry, constants, memory registers, etc.)

### ↩️ Undo & State Management

- **Full undo capability** (with optional redo) for calculator state and stack contents
- **Command-pattern or incremental snapshot history** to guarantee precise, lossless state reversal
- **Non-destructive undo stack** that preserves decimal precision and operation sequence
- **Context-aware clear/reset** that optionally clears history when explicitly requested

### 🌐 Platform & Deployment Strategy

- **Web-first architecture** using modern HTML/CSS/JS or a lightweight framework (React/Vue/Svelte)
- **Responsive, mobile-friendly UI** optimized for both touch and keyboard input
- **Desktop packaging roadmap** designed for seamless migration via Tauri, Electron, or PWA-to-desktop wrappers
- **Offline-capable build** (static assets + client-side state) for reliable desktop/web parity

### 🎨 User Experience & Interface

- **Keyboard-optimized input** with intuitive shortcuts for all operations and stack manipulation (`Enter`, `↑`, `↓`, `Del`, etc.)
- **Visual operation feedback** (stack animations, button press states, clear operand highlighting)
- **Adjustable calculation settings** (decimal precision, scientific/engineering notation toggle, angle mode if expanded later)
- **Theme support** (light/dark/system) with accessibility-compliant contrast and focus states

### 🛠 Technical Highlights

- **State-driven UI architecture** enabling predictable rendering and seamless undo/redo integration
- **Strict separation of logic and presentation** for easy testing, debugging, and future feature injection
- **Lightweight dependency footprint** to ensure fast load times and smooth desktop packaging
- **Comprehensive input sanitization** and locale-aware number parsing

This breakdown covers all your explicit requirements while outlining practical implementation patterns that will keep the codebase maintainable, extensible, and ready for desktop packaging. Let me know if you'd like a tech stack recommendation, state management strategy, or a phased rollout plan.

---
---

Ok. So the concept is as follows.

Sample target inputs (description, fragment, piece, you name it; each bullet is a standalone example):

- "RPN calculator"
- "RPN calculator web app"
- "RPN calculator web app with a packaged desktop version option. The app supports add, sub, neg, mul, div, sqr, sqrt, inv, pow, abs, exp, and ln functions. The app also has the undo capability."

Then I include context, stating, for example, that the target app might be a real open source app and I want to do kind of "cleanroom" reverse engineering (perhaps this model is limited, as I would not provide, like an API or something to fulfil).

Basically, I need to set the LLM as an expert software engineer/architect, the project context scope (perhaps, GitHub repo, or an imagined project that might be representative of several real project; project developed by a highly experienced professional team (meaning the codebase, test base, docs - all follow the best practices), possibly dev platform, etc.). The role of the LLM would to help me understand potential project structure/organization, program architecture, libraries, if relevant, UIs, frameworks, and so on. Understanding different documents that should be present in professional project (the context-setting target) and that should be developed, such as architecture.md or something, possibly, api.md, contracts.md, spec.md, and so on, whatever should be expected. Infer from context potential feature decomposition starting from high level down to detailed structured lists/specs. Perform iterative development of various such docs. The ultimate goal is to prepare a comprehensive docs/specs package/suite for subsequent SDD via a coding agent.

Conceptually, most likely I would not want to reimplement a specific project. I want to use the idea of an existing professional project developed by a professional experienced expert team to set the context for LLM to infer the necessary dev documents, architecture, specs, etc.
