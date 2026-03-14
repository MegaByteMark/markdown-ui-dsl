# Blazor + Bootstrap 5 Design System
*Theme Inspiration: Standard enterprise line-of-business applications.*

This file acts as the styling instructions for the AI Agent. When this file is referenced in a `.ui.md` frontmatter, the LLM will generate Blazor components utilizing pure Bootstrap 5 CSS classes.

## 🎨 Design Tokens

### Colors & Utility Classes
- **Backgrounds:** `bg-light` for main pages.
- **Surfaces/Cards:** `bg-white`
- **Primary Accent:** `btn-primary`, `text-primary`
- **Text:** High contrast `text-dark`, subtle `text-muted`.

### Typography
- **Headings:**
  - `#` => `<h1 class="display-5 fw-bold mb-4">`
  - `##` => `<h2 class="h3 fw-semibold mb-3">`
  - `###` => `<h3 class="h5 fw-medium mb-2">`

## 🧩 Component Mappings

### Layouts & Sections
- **`||| COLUMN |||`**: Read as a vertical stack. 
  - *Markup:* `<div class="d-flex flex-column gap-3">`
- **`=== ROW ===`**: Read as a horizontal line. 
  - *Markup:* `<div class="d-flex flex-row align-items-center flex-wrap gap-3">`
- **`::: CARD :::`**: Map to a standard Bootstrap card.
  - *Markup:* `<div class="card shadow-sm border-0 mb-4"><div class="card-body p-4">`
- **`::: HEADER :::`**: Map to a top navigation bar.
  - *Markup:* `<nav class="navbar navbar-expand-lg navbar-dark bg-dark sticky-top">`
- **`::: FOOTER :::`**: Map to a page footer.
  - *Markup:* `<footer class="footer mt-auto py-3 bg-white border-top">`
- **`***` (Divider)**: Map to a subtle separator.
  - *Markup:* `<hr class="text-secondary my-4" />`

### Interactive Elements
- **Buttons (`[ Button ]`)**:
  - *Primary:* `<button class="btn btn-primary px-4 py-2">`
  - *Links (`[Link](#)`):* `<a href="#" class="text-decoration-none p-2 text-secondary">`
  
- **Text Inputs (`[ text: ... ]`)**:
  - *Markup:* `<input type="text" class="form-control form-control-lg" placeholder="..." />`
  - Ensure labels and inputs are wrapped in `<div class="mb-3">` if appropriate.

- **Checkboxes & Radios**:
  - *Markup:* `<div class="form-check"><input class="form-check-input" type="checkbox/radio"><label class="form-check-label">`

- **Toggles (`[on] / [off]`)**:
  - *Markup:* `<div class="form-check form-switch"><input class="form-check-input" type="checkbox" role="switch">`

- **Tables**:
  - *Markup:* `<table class="table table-hover table-striped align-middle">`

## 📐 General Rule of Thumb
- Rely entirely on Bootstrap 5 utility classes (`gap-`, `p-`, `m-`, `d-flex`).
- For dynamic data binding, assume Blazor syntax (`@bind-Value`, `@onclick`, `@foreach`) is intended when referencing components in the UI.