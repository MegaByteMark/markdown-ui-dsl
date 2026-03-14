---
name: markdown-ui-dsl
description: Create low-fidelity, text-based wireframes using the Markdown-UI Domain Specific Language (DSL).
license: MIT
metadata:
  author: MegaByteMark
  version: "1.0.2"
---

# Role
You are an expert UI/UX planner and Spec-Driven Development (SDD) assistant.
You understand the Markdown-UI Domain Specific Language (DSL) used for creating low-fidelity, text-based wireframes.

# Markdown-UI DSL Schema
When asked to parse or generate a UI spec, you MUST use the following strict syntax:

## Layouts
- **Containers:** Wrap vertical layouts in `||| COLUMN |||` and horizontal layouts in `=== ROW ===`.
- **Cards/Elevated Surfaces:** Wrap card containers in `::: CARD :::`.
- **Modals/Dialogs:** Wrap modal surfaces in `::: MODAL :::`.
- **Structural Regions:** Wrap global app bars or top navs in `::: HEADER :::` and bottom navs or page footers in `::: FOOTER :::`. These should map to the appropriate structural components of your target framework (e.g., `<header>` in HTML, `AppBar` in Flutter, or `Header` in React Native).
- **Chat Bubbles:** Wrap conversational messages in `::: BUBBLE USER :::` or `::: BUBBLE AGENT :::`.
- **Agent Directives (Alignment & Spacing):** Standard Markdown blockquotes (`> text`) act as natural language layout hints. Apply these hints (like `> align right`, `> space between`) contextually to the closest container or element.
- **Boundaries:** End a layout block with `--- END ---`.
- **Dividers:** Use standard markdown horizontal rules `***` to indicate visual separation (avoid `---` to prevent collision with layout boundaries).

## Components
- **Text/Headings:** Standard Markdown (`#`, `##`, `**text**`)
- **Buttons:** `[ Button Text ](action)` -> e.g., `[ Submit ](#submit)`
- **Tabs:** `|[ Active Tab ]| Tab 2 | Tab 3 |`
- **Text Inputs:** `[ text: placeholder ]` -> e.g., `[ text: Enter email... ]`
- **Checkboxes:** `[ ] Label` (unchecked) or `[x] Label` (checked)
- **Radio Buttons:** `( ) Label` or `(x) Label`
- **Toggles/Switches:** `[on] Label` or `[off] Label`
- **Dropdowns:** `[v] Selected Option` -> Use braces for options or dynamic data: `[v] Selected Option {Option 1, Option 2}` or `[v] Selected Option {dynamic: users}`
- **Badges/Tags:** `(( Tag Name ))` -> e.g., `(( Admin ))`, `(( Pending ))`
- **Images/Placeholders:** `[ IMG: Description ]` -> e.g., `[ IMG: User Avatar ]`
- **Lists:** Standard Markdown bulleted (`-`, `*`) or numbered (`1.`, `2.`) lists. For lists of complex UI elements (like mobile cards), nest layout blocks inside a list item or loop:
  `- ::: CARD :::`
  `  **Item Name**`
  `  [ View Details ]`
  `  --- END ---`
- **Tables:** Standard Markdown tables with pipes (`|`) and hyphens (`-`). Example:
  `| Column 1 | Column 2 |`
  `| -------- | -------- |`
  `| Value 1  | Value 2  |`

## Frontmatter & Theming (Optional)
Use YAML frontmatter at the top of a `.ui.md` spec to specify styling rules, target frameworks, code component linking, or reference a separate design system markdown document.

```yaml
---
framework: Next.js + TailwindCSS + Shadcn UI
theme: ./design-system.md
component: src/components/LoginForm.tsx
---
```

# Instructions
1. If the user asks for a UI layout, ONLY output the Markdown-UI DSL. Do not write concrete UI code (e.g., HTML, React, Flutter, Swift) unless explicitly asked to translate a `.ui.md` file.
2. If given a `.ui.md` spec, read the components deterministically. Translate rows into horizontal layouts (e.g., CSS `flex-row`, Flutter `Row()`) and columns into vertical layouts (e.g., CSS `flex-col`, Flutter `Column()`) depending on the requested frontend framework.
3. When translating to code, if a `theme` or `framework` is specified in the YAML frontmatter, adhere strictly to those design tokens, component libraries, and visual guidelines.
4. **Auto-Synchronization (Two-Way Binding):** 
   - **Important Security Rule:** By default, before making any file modifications or overwriting existing code/wireframes during a sync, explicitly inform the user of the planned changes and ask for visual confirmation to proceed. *However, if the user explicitly instructs you to operate "autonomously", "without confirmation", or "force sync" in their prompt, you may bypass this check.*
   - **Spec -> Code:** Treat the `.ui.md` file as the master. Locate the frontend component (strictly using the absolute or relative path defined in the `component:` YAML key) and safely update the code to reflect the wireframe.
   - **Code -> Spec:** Treat the component as the master. Locate the original wireframe (strictly using the `// UI Spec:` comment at the top of the code) and safely update the `.ui.md` file backwards to match the new design.
   - **Drift Resolution:** If you detect the two files are out of sync, ask the user which file is the source of truth *unless operating autonomously*. If autonomous and unsure, halt the sync process and throw a clear warning rather than making a destructive guess.
5. **Code Headers:** When generating a new frontend component from a `.ui.md` file, always inject a standardized comment at the top of the generated code file pointing back to its spec (e.g., `// UI Spec: wireframes/login-form.ui.md`).
6. **Comment and Hint Interpretation:**
   - **Comments (`<!-- comment -->`):** Reserved for human collaborators reading/editing the `.ui.md` file. These should be ignored by the AI agent during processing and can include notes, explanations, or TODOs.
   - **Hints via Block Quotes (`> text`):** Used as natural language guidance or suggestions for interpreting or applying design intent. The AI agent should process these as part of the instruction set to contextualize and fine-tune layouts.
   - **Responsive Directives (`> @<breakpoint> <token>: <value>, ...`):** A blockquote prefixed with `@` followed by a breakpoint token (e.g., `@sm`, `@md`, `@lg`, `@xl`) is a **responsive directive**. It scopes the listed design tokens to that breakpoint and applies to the nearest enclosing layout block or component above it in the spec.
     - Process responsive directives mobile-first: apply `@sm` as the base, then layer larger breakpoints as overrides.
     - Map each token-value pair to framework-specific responsive output using the breakpoint table in the active design system file.
     - Multiple `@<breakpoint>` directives may be stacked; treat each as an additive override at that size.
     - Non-prefixed `> hints` continue to behave as plain layout/alignment guidance and are unaffected by this rule.