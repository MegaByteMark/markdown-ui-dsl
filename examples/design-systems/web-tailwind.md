# Minimal SaaS Design System
*Theme Inspiration: Modern minimalist interfaces (similar to ChatGPT, Vercel, or Shadcn UI)*

This file acts as the styling instructions for the AI Agent. When this file is referenced in a `.ui.md` frontmatter, the LLM will follow these specific design rules when generating code.

## 🎨 Design Tokens

### Colors
- **Backgrounds:** Soft and subtle. Main app background `bg-gray-50`.
- **Surfaces/Cards:** Crisp white `bg-white`.
- **Primary Accent:** Emerald/Teal (`bg-emerald-600` for primary actions, `hover:bg-emerald-700`).
- **Text:** High contrast `text-gray-900` (headings/primary), subtle `text-gray-500` (body/muted).

### Typography
- **Font Family:** System sans-serif (Inter, Roboto, or standard sans).
- **Headings:**
  - `#` => `text-3xl font-bold tracking-tight text-gray-900 mb-6`
  - `##` => `text-xl font-semibold text-gray-800 mb-4`
  - `###` => `text-lg font-medium text-gray-800 mb-3`

## 🧩 Component Mappings

### Layouts & Sections
- **`||| COLUMN |||`**: Read as a vertical stack. Map to `flex flex-col gap-4`.
- **`=== ROW ===`**: Read as a horizontal line. Map to `flex flex-row items-center gap-4 flex-wrap`.
- **`::: CARD :::`**: Map to a sleek, elevated surface.
  - *Classes:* `bg-white rounded-xl border border-gray-200 shadow-sm p-6`.
- **`::: HEADER :::`**: Map to a top application bar or global navigation.
  - *Classes:* `w-full bg-gray-900 text-white px-6 py-4 flex items-center justify-between sticky top-0 z-50 shadow-md`.
- **`::: FOOTER :::`**: Map to a bottom tab bar or page footer.
  - *Classes:* `w-full bg-white border-t border-gray-200 px-6 py-4 mt-auto text-sm text-gray-500`.
- **`***` (Divider)**: Map to a subtle separator.
  - *Classes:* `w-full my-6 border-t border-gray-100`.

### Chat Interfaces (Gemini / Minimal AI Style)
- **`::: BUBBLE USER :::`**: Treat as a user prompt. Align right, simple bubble.
  - *Layout:* `flex w-full justify-end my-4`
  - *Classes:* `max-w-[70%] bg-gray-100 text-gray-900 rounded-2xl md:rounded-3xl p-4 md:px-6 md:py-4 leading-relaxed`
- **`::: BUBBLE AGENT :::`**: Treat as the AI response. Align left, clear background, often featuring complex structures like code blocks.
  - *Layout:* `flex w-full justify-start my-4`
  - *Classes:* `max-w-full md:max-w-[85%] bg-transparent text-gray-800 p-0 md:py-2 leading-relaxed prose prose-emerald prose-pre:bg-gray-900 prose-pre:text-gray-100`

### Interactive Elements
- **Buttons (`[ Button ]`)**:
  - Make them modern with rounded corners and transitions.
  - *Primary Classes:* `justify-center inline-flex bg-emerald-600 text-white px-4 py-2.5 rounded-lg font-medium hover:bg-emerald-700 transition-colors shadow-sm`.
  - *Links (`[Link](#)`) used as secondary actions:* `text-gray-600 hover:bg-gray-100 px-4 py-2.5 rounded-lg font-medium`.
  
- **Text Inputs (`[ text: ... ]`)**:
  - *Classes:* `w-full px-4 py-2.5 rounded-lg border border-gray-300 bg-white placeholder-gray-400 focus:ring-2 focus:ring-emerald-500 focus:border-emerald-500 outline-none transition-all`.

### Advanced UI Elements
- **Tabs (`|[ Active ]| Inactive |`)**:
  - Render as an underlined navigation strip.
  - *Active Tab:* `border-b-2 border-emerald-600 text-emerald-600 font-medium pb-2`.
  - *Inactive Tab:* `border-b-2 border-transparent text-gray-500 hover:text-gray-700 pb-2`.
  
- **Badges (`(( Badge ))`)**:
  - *Classes:* `inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-semibold bg-emerald-100 text-emerald-800 border border-emerald-200`.
  
- **Toggles (`[on] / [off]`)**:
  - Render as a modern pill-shaped switch (e.g., Shadcn Switch).
  - *ON State:* Green track (`bg-emerald-600`) with knob moved to the right.
  - *OFF State:* Gray track (`bg-gray-200`) with knob on the left.

## 📱 Responsive Design

### Breakpoints
| Token | Tailwind prefix | Min-width |
|-------|----------------|-----------|
| @sm   | `sm:`          | 640px     |
| @md   | `md:`          | 768px     |
| @lg   | `lg:`          | 1024px    |
| @xl   | `xl:`          | 1280px    |
| @2xl  | `2xl:`         | 1536px    |

### Layout Tokens
| Token value      | Base classes          | With breakpoint prefix example  |
|------------------|-----------------------|---------------------------------|
| `layout: stacked`| `flex flex-col`       | `flex flex-col md:flex-row`     |
| `layout: row`    | `flex flex-row`       | `flex flex-col sm:flex-row`     |
| `layout: grid-2` | `grid grid-cols-2`    | `grid grid-cols-1 md:grid-cols-2` |
| `layout: grid-3` | `grid grid-cols-3`    | `grid grid-cols-1 lg:grid-cols-3` |

### Spacing Tokens
| Token value        | Classes generated     |
|--------------------|-----------------------|
| `padding: compact` | `p-3`                 |
| `padding: default` | `p-6`                 |
| `padding: spacious`| `p-10`                |
| `gap: compact`     | `gap-2`               |
| `gap: default`     | `gap-4`               |
| `gap: spacious`    | `gap-8`               |

### Text Tokens
| Token value    | Classes generated     |
|----------------|-----------------------|
| `text: small`  | `text-sm`             |
| `text: base`   | `text-base`           |
| `text: large`  | `text-lg`             |

### Generation Rule
When you encounter `> @<breakpoint> <token>: <value>` directives on a spec block:
1. Establish the **base classes** from the default component mapping above (smallest breakpoint or no breakpoint directive = base).
2. For each larger breakpoint directive, prepend the Tailwind breakpoint prefix to the override class.
3. Combine all classes into a single class string, e.g.: `flex flex-col gap-4 p-3 md:flex-row md:gap-6 md:p-6 lg:p-10`.
4. Apply mobile-first: `@sm` sets the base when present; omitting `@sm` means the default component mapping is the base.

## 📐 General Rule of Thumb
- **Breathe:** Use generous padding (`p-6`) and spacing (`gap-6`) to achieve a clean interface. Never cramp elements.
- **Component Libraries:** If working in React and instructed to use Shadcn UI in the frontmatter, map these designs back to Shadcn components (e.g., use `<Card>`, `<Input>`, `<Badge>`, and `<Tabs>`) passing in standard Tailwind strings only as optional overrides.
