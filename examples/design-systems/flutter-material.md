# Flutter Material Design System
*Theme Inspiration: Google's Material Design 3 guidelines for cross-platform mobile apps.*

This file acts as the styling instructions for the AI Agent. When this file is referenced in a `.ui.md` frontmatter, the LLM will map the wireframe tokens to native Flutter widgets.

## 🎨 Design Tokens

### Colors
- **App Background:** `Theme.of(context).colorScheme.background`
- **Surfaces/Cards:** `Theme.of(context).colorScheme.surface`
- **Primary Accent:** `Theme.of(context).colorScheme.primary`
- **Text:** `Theme.of(context).textTheme.bodyLarge` or `bodyMedium`

### Typography
- **Headings:**
  - `#` => `style: Theme.of(context).textTheme.headlineLarge` (with bold weight)
  - `##` => `style: Theme.of(context).textTheme.headlineMedium`
  - `###` => `style: Theme.of(context).textTheme.titleLarge`

## 🧩 Component Mappings

### Layouts & Sections
- **`||| COLUMN |||`**: Read as a vertical stack. Map to `Column(crossAxisAlignment: CrossAxisAlignment.start, children: [...])`.
- **`=== ROW ===`**: Read as a horizontal line. Map to `Row(mainAxisAlignment: MainAxisAlignment.start, children: [...])`.
   - *Note:* If elements need spacing, use `SizedBox(width: x)` or `Spacer()`.
- **`::: CARD :::`**: Map to a Material Card.
  - *Widget:* `Card(elevation: 2, clipBehavior: Clip.antiAlias, child: Padding(padding: EdgeInsets.all(16.0), child: ...))`
- **`::: HEADER :::`**: Map to the `appBar` property of a Scaffold.
  - *Widget:* `AppBar(title: Text(...))`
- **`::: FOOTER :::`**: Map to a `BottomNavigationBar` or `NavigationBar`.
- **`***` (Divider)**: Map to `const Divider(height: 32, thickness: 1)`.

### Interactive Elements
- **Buttons (`[ Button ]`)**:
  - *Widget:* `FilledButton(onPressed: () {}, child: Text('Button'))`
  - *Secondary Actions (`[Link](#)`):* `TextButton(onPressed: () {}, child: Text('Link'))`
  
- **Text Inputs (`[ text: ... ]`)**:
  - *Widget:* `TextField(decoration: InputDecoration(border: OutlineInputBorder(), hintText: '...'))`

- **Dropdowns (`[v] Selected Option`)**:
  - *Widget:* `DropdownButtonFormField(value: 'Option', items: [...])`

- **Toggles (`[on] / [off]`)**:
  - *Widget:* `Switch(value: true/false, onChanged: (val) {})`

## 📱 Responsive Design

### Breakpoints (logical pixels via `LayoutBuilder`)
| Token | Min-width (dp) | Typical target         |
|-------|---------------|------------------------|
| @sm   | 0             | Phone portrait          |
| @md   | 600           | Phone landscape / tablet |
| @lg   | 960           | Large tablet / desktop  |
| @xl   | 1280          | Desktop / wide screen   |

### Layout Tokens
| Token value       | Flutter implementation                                                  |
|-------------------|-------------------------------------------------------------------------|
| `layout: stacked` | `Column(crossAxisAlignment: CrossAxisAlignment.stretch, children: [...])` |
| `layout: row`     | `Row(children: [...])`                                                  |
| `layout: grid-2`  | `GridView.count(crossAxisCount: 2, ...)`                                |
| `layout: grid-3`  | `GridView.count(crossAxisCount: 3, ...)`                                |

### Spacing Tokens
| Token value        | Flutter implementation          |
|--------------------|---------------------------------|
| `padding: compact` | `EdgeInsets.all(8.0)`           |
| `padding: default` | `EdgeInsets.all(16.0)`          |
| `padding: spacious`| `EdgeInsets.all(24.0)`          |
| `gap: compact`     | `SizedBox(height/width: 8.0)`   |
| `gap: default`     | `SizedBox(height/width: 16.0)`  |
| `gap: spacious`    | `SizedBox(height/width: 24.0)`  |

### Generation Rule
When you encounter `> @<breakpoint>` directives, wrap the affected layout block in a `LayoutBuilder` and use `constraints.maxWidth` to switch between implementations:

```dart
LayoutBuilder(
  builder: (context, constraints) {
    final isTablet = constraints.maxWidth >= 600;
    final isDesktop = constraints.maxWidth >= 960;

    EdgeInsets padding;
    Widget content;

    if (isDesktop) {
      // @lg tokens
      padding = const EdgeInsets.all(24.0);
      content = Row(children: [...]);
    } else if (isTablet) {
      // @md tokens
      padding = const EdgeInsets.all(16.0);
      content = Row(children: [...]);
    } else {
      // @sm tokens (base)
      padding = const EdgeInsets.all(8.0);
      content = Column(children: [...]);
    }

    return Padding(padding: padding, child: content);
  },
)
```

## 📐 General Rule of Thumb
- Use standard Material 3 components out-of-the-box.
- Ensure all screen-level components are wrapped in a `Scaffold()`.
- Add generous `16.0` or `24.0` padding to containers and screen edges to let the UI breathe using `Padding()` widgets.