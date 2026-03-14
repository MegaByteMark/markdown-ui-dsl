# Markdown UI DSL for AI Agents

[![Star on GitHub](https://img.shields.io/github/stars/MegaByteMark/markdown-ui-dsl.svg?style=social)](https://github.com/MegaByteMark/markdown-ui-dsl/stargazers)
[![OpenClaw Skill](https://img.shields.io/badge/OpenClaw-Hub-blue?logo=github&style=flat-square)](https://openclaw.com/skills/markdown-ui-dsl)

A lightweight, text-based wireframing standard designed specifically for AI coding agents and Spec-Driven Development (SDD).

> ⭐️ **If you find this project useful for your AI tooling, please consider giving it a star!** ⭐️

## Table of Contents
- [The Problem](#the-problem)
- [The Solution](#the-solution)
- [Installation & Setup](#installation--setup)
- [Recommended Project Structure](#recommended-project-structure)
- [Workflow](#workflow)
- [Syntax Quick Reference](#syntax-quick-reference)
- [Design Theming](#design-theming)
- [Responsive Design](#responsive-design)
- [Events & Interactivity](#events--interactivity)
- [Contributing](#contributing)
- [License](#license)

## The Problem
Visual wireframes (Figma, PNGs) are token-heavy, prone to AI hallucination, and hard to version control. Natural language prompts often lack deterministic structure, leading to inconsistent UI generation.

## The Solution
This project provides a strict Markdown-based Intermediate Representation (IR). Product managers can write easily readable low-fidelity specs, and AI agents can parse them with deterministic accuracy to generate React, Vue, HTML, or any other UI code.

## Installation & Setup
For the smoothest experience, you should integrate the skill file into your AI agent's specific instruction architecture.

### 1. OpenClaw Hub (Recommended)
If your agent supports OpenClaw registry integration, you can install this skill directly via the marketplace. Visit the [Markdown-UI DSL page on OpenClaw Hub](https://openclaw.com/skills/markdown-ui-dsl) for one-click installation or use their CLI:
```bash
claw install markdown-ui-dsl
```

### 2. GitHub Copilot (Agent Mode)
Copilot Agent mode works best when using its explicit Skill architecture. Set it up using these steps:

1. Initialise your `copilot-instructions.md` by asking Copilot to setup your project for AI agent assistance, or manually create `.github/copilot-instructions.md`.
2. Create a folder for the skill and download the DSL instructions:
```bash
mkdir -p .github/skills/markdown-ui-dsl
curl -o .github/skills/markdown-ui-dsl/SKILL.md https://raw.githubusercontent.com/MegaByteMark/markdown-ui-dsl/main/skills/markdown-ui-dsl/SKILL.md
```
3. Open your `.github/copilot-instructions.md` file and add the following mapping:
```markdown
## Skills
- For interpreting `ui.md` files, use the `markdown-ui-dsl` skill to generate components based on the specifications provided.
```

### 2. Cursor / Roo Code / Cline
For agents that use single flat rules files, you can append the skill directly:
```bash
# For Cursor (picks up from .cursorrules)
curl -o .cursorrules https://raw.githubusercontent.com/MegaByteMark/markdown-ui-dsl/main/skills/markdown-ui-dsl/SKILL.md

# For Cline/Roo Code (picks up from .clinerules)
curl -o .clinerules https://raw.githubusercontent.com/MegaByteMark/markdown-ui-dsl/main/skills/markdown-ui-dsl/SKILL.md
```

### 3. Claude Code
Claude Code automatically looks for a `CLAUDE.md` file in the root of your project to understand project-specific rules and conventions. You can append the skill directly to it:
```bash
curl -s https://raw.githubusercontent.com/MegaByteMark/markdown-ui-dsl/main/skills/markdown-ui-dsl/SKILL.md >> CLAUDE.md
```

### 4. Gemini CLI
For Gemini CLI, download the skill into your project and pass it as a system prompt or context flag when running your generation commands:
```bash
mkdir -p .ai/skills
curl -o .ai/skills/markdown-ui-dsl.md https://raw.githubusercontent.com/MegaByteMark/markdown-ui-dsl/main/skills/markdown-ui-dsl/SKILL.md

# Example usage:
# gemini query "Build the login UI" --system-prompt .ai/skills/markdown-ui-dsl.md
```

### 5. GPT Codex (and other CLI agents)
Download the instruction file into your workspace and reference it as a system prompt when initiating your AI codebase session:
```bash
mkdir -p .ai/skills
curl -o .ai/skills/markdown-ui-dsl.md https://raw.githubusercontent.com/MegaByteMark/markdown-ui-dsl/main/skills/markdown-ui-dsl/SKILL.md
```

## Recommended Project Structure
To keep your specs organized and cleanly separated from your application code, we highly recommend creating a `wireframes` folder at the root of your project to hold your DSL designs and your design system file:

```text
my-app/
├── .github/
│   ├── copilot-instructions.md
│   └── skills/markdown-ui-dsl/SKILL.md
├── src/
│   └── components/
└── wireframes/
    ├── design-systems/
    │   └── web-tailwind.md
    └── login-form.ui.md
```

*(Alternative for Dev-Heavy Teams: You can also choose to **Colocate** your `.ui.md` files directly next to their resulting components in the `src/` directory. If you do this, we recommend moving the rules from `design-system.md` directly into your global agent instructions file so you don't have to manage complex relative pathing in your frontmatter).*

## Workflow
1. **Spec Generation:** Create a new `.ui.md` file in your `wireframes/` folder or ask your AI: "Plan a login page using the Markdown-UI DSL and save it to `wireframes/login.ui.md`."
2. **Human Review:** A PM or developer reviews the generated `.ui.md` file and edits the text layout easily.
3. **Code Generation:** Open an Agent Chat and prompt it: "Create the opening page of the app as a login page and create a component for it from `wireframes/login-form.ui.md`."
4. **Iterative Syncing:** When you need to make changes, don't edit the code! Ask the agent: "Add a 'Forgot Password' link to `login-form.ui.md`" — the built-in skill instructions will force the agent to automatically update *both* your UI spec and the targeted code component simultaneously.

## Syntax Quick Reference
Here is a brief overview of the Markdown-UI DSL syntax. Because it relies heavily on natural visual metaphors, it's incredibly fast to read and write without looking at a manual. For the exact AI instruction set, see the [skills/markdown-ui-dsl/SKILL.md](skills/markdown-ui-dsl/SKILL.md) file.

### Layouts
- **Containers:** `||| COLUMN |||` for vertical stacking, `=== ROW ===` for flex-row alignment.
- **Surfaces:** `::: CARD :::` or `::: MODAL :::` for elevated containers, `::: BUBBLE USER :::` or `::: BUBBLE AGENT :::` for chat interfaces, and `::: HEADER :::` or `::: FOOTER :::` for structural app navigation.
- **Boundaries:** End *any* layout block with `--- END ---`.
- **Dividers:** `***` for horizontal visual breaks.
- **Agent Directives (Alignment & Spacing):** Use standard Markdown blockquotes (`>`) to give the AI specific layout, alignment, or stylistic hints within a flow.
  *Example: `> align items to the right` or `> push the avatar to the far right edge`.*

### Components
- **Text & Lists:** Use standard Markdown (`#`, `**bold**`, `- list item`). Everything naturally parses.
- **Buttons / Actions:** `[ Button Name ](#action)`
- **Text Inputs:** `[ text: Placeholder content ]`
- **Checkboxes:** `[ ] Unchecked` or `[x] Checked`
- **Radio Buttons:** `( ) Option` or `(x) Selected`
- **Toggles:** `[on] Enabled` or `[off] Disabled`
- **Dropdowns / Selects:** `[v] Selected Value {Option 1, Option 2}`
- **Tabs:** `|[ Active Tab ]| Tab 2 | Tab 3 |`
- **Badges / Tags:** `(( Premium ))`
- **Images:** `[ IMG: User Avatar (rounded) ]`

## Design Theming
The Markdown-UI DSL strongly separates structure from style. You can use YAML frontmatter at the top of your `.ui.md` specs to instruct the AI to follow a specific UI framework or custom design system document:

```yaml
---
framework: Next.js + TailwindCSS + Shadcn UI
theme: ./design-system.md
---
```

By writing your styling rules, design tokens, and standard CSS classes into a single `design-system.md` file, the AI agent will consistently apply your branding and layout guidelines across every component it generates. Check out the `examples/design-system.md` file for inspiration.

## Responsive Design
The DSL supports media-query-style responsive behaviour through **responsive directives** — a natural extension of the existing blockquote hint syntax. Prefix any design directive with `@<breakpoint>` to scope it to a specific viewport size:

```markdown
> @sm layout: stacked, padding: compact
> @md layout: row, padding: default
> @lg padding: spacious
```

The AI agent applies these directives at the specified breakpoints, mapping each token to the correct framework-specific output (e.g., Tailwind responsive prefixes, Flutter `LayoutBuilder`, Bootstrap responsive infixes). The available breakpoint tokens and their token-to-class mappings are defined in your design system file — see the `examples/design-systems/` folder for reference.

**Rules:**
- Responsive directives always apply to the nearest enclosing layout block or component above them.
- Multiple `@<breakpoint>` lines can be stacked; they are applied additively from smallest to largest (mobile-first).
- Omitting a breakpoint directive means the base design system styles apply at that size unchanged.
- Non-responsive `>` directives (without a `@` prefix) continue to work exactly as before.

**Example — a card that stacks on mobile and goes side-by-side on desktop:**
```markdown
::: CARD :::
> @sm layout: stacked, padding: compact
> @lg layout: row, padding: default

[ IMG: Product Photo ]

||| COLUMN |||
## Product Title
Some description text.
[ Buy Now ](#purchase)
--- END ---
--- END ---
```

For a full working example see [`examples/responsive-layout.ui.md`](examples/responsive-layout.ui.md).

## Events & Interactivity
You do **not** need to map complex state logic, API calls, or event handlers in the UI DSL. That falls outside the scope of a wireframe and belongs in your overarching SDD (Spec-Driven Development) principles or user stories. 

The DSL captures the *intent* of an action using standard markdown link syntax on buttons:
* `[ Login ](#login)` -> Indicates a trigger/event handler needs to be mapped.
* `[ Forgot Password ](/reset-password)` -> Indicates a route change.

When you pass the `.ui.md` file to the AI Agent, you should provide the behavior alongside it in your prompt or a separate requirements document, like so:
*"Generate the component from `login-form.ui.md`. When `#login` is clicked, mock an API call setting `isLoading` to true, and route to `/dashboard` on success."*

## Contributing
Contributions and community feedback are highly encouraged! Since this DSL is an evolving standard, your input naturally makes it better.

If you want to contribute:
1. **Fork the Project**
2. **Create your Feature Branch** (`git checkout -b feature/amazing-feature`)
3. **Commit your Changes** (`git commit -m 'Add some amazing feature'`)
4. **Push to the Branch** (`git push origin feature/amazing-feature`)
5. **Open a Pull Request**

If you have ideas, want to request a new DSL primitive, or find an issue, please [open an issue](https://github.com/MegaByteMark/markdown-ui-dsl/issues).

## License
This project is open-source and available under the terms of the project's [LICENSE](LICENSE).