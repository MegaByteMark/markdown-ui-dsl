# Testing Markdown-UI DSL

Because Large Language Models (LLMs) are inherently non-deterministic, classic unit testing (like strict exact-string matching) is not effective for testing this DSL. 

Instead, "testing" this project relies on **Structural Evaluation** (often called LLM Evals). If you are contributing to the parsing rules in `skill/markdown-ui-agent/SKILL.md`, you should manually test your changes by running the LLM through a simulated generation task and checking its adherence to the logic.

## 🧪 Simulation Testing Strategy

To verify the agent correctly understands the DSL syntax and your specific `design-systems/` rules, simulate a code-generation session in your AI IDE (Copilot, Cursor, etc.) or CLI using the following steps.

### 1. Test: Translation & Theming Adherence
**Objective:** Prove the agent understands layout primitives (`||| COLUMN |||`, `=== ROW ===`) and maps them to the appropriate framework tokens.

**Prompt to the AI:**
> "Using the `markdown-ui-agent.md` skill, generate the UI component described in `examples/login-form.ui.md`."

**Evaluation Checklist (Manual or Scripted):**
- [ ] Did the agent inject a spec reference comment at the top of the file? (e.g. `// UI Spec: wireframes/login-form.ui.md`)
- [ ] Did the `||| COLUMN |||` correctly map to a visual vertical stack in the target framework?
- [ ] Did the nested `=== ROW ===` block generate a valid flex-row or horizontal container?
- [ ] Were the specific design tokens mandated in the referenced `design-system.md` file applied?

### 2. Test: Two-Way Binding (Code -> Spec)
**Objective:** Prove the agent will surgically update the Markdown wireframe when a change is requested on the code side.

**Prompt to the AI:**
> "Open the generated login code from the previous step. Add a 'Login with Google' secondary button directly beneath the main login button, and be sure to sync the wireframe."

**Evaluation Checklist:**
- [ ] Was the code updated successfully with the new button?
- [ ] Did the agent use the `// UI Spec:` comment to navigate back to `login-form.ui.md`?
- [ ] Was the `.ui.md` syntax correctly updated? (e.g., did it insert `[ Login with Google ](#google)` directly below `[ Login ](#login)` without destroying the rest of the DSL layout?)

### 3. Test: Natural Language Directives
**Objective:** Prove the agent understands Markdown blockquotes (`>`) as structural hints rather than literal text.

**Prompt to the AI:**
> "Add a text input for 'Search...' at the top of the login form, but use an inline blockquote directive `> align right` above it in the wireframe. Generate the updated layout."

**Evaluation Checklist:**
- [ ] Does the `.ui.md` feature the `> align right` syntax exactly as formatted?
- [ ] Does the generated code use CSS/Framework-specific properties (e.g., `justify-end`, `text-align: right`, or `Spacer`) to physically push the search bar to the right side of the screen?

---

### Future: Automated LLM Evals
As this repository matures, we may implement automated LLM-as-a-judge pipelines (using tools like Promptfoo, Braintrust, or Langchain). If you are submitting a PR to introduce an automated eval pipeline, please ensure the evaluation strictly asserts layout semantics (checking for the existence of required components or AST trees) rather than flakey exact-string matching.