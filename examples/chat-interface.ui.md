---
framework: Next.js + TailwindCSS + React Markdown
theme: ./design-system.md
component: src/components/ChatInterface.tsx
---

||| COLUMN |||

## AI Assistant
Let's build something amazing together.

***

::: BUBBLE USER :::
Hi! Can you help me write a python script to reverse a string?
--- END ---

::: BUBBLE AGENT :::
Of course! Here is a simple Python script to reverse a string using slicing:

```python
def reverse_string(text):
    return text[::-1]

print(reverse_string("Hello, World!"))
```

Let me know if you need help with anything else!

=== ROW ===
[ Copy Code ](#copy)
[ Run Code ](#run)
--- END ---
--- END ---

::: BUBBLE USER :::
Awesome, that works. What about using a loop instead?
--- END ---

::: BUBBLE AGENT :::
Sure, if you prefer an iterative approach, you can do it like this:

```python
def reverse_string_loop(text):
    reversed_text = ""
    for char in text:
        reversed_text = char + reversed_text
    return reversed_text
```
--- END ---

***

=== ROW ===
[ text: Message the AI assistant... ]
[ Send ](#send)
--- END ---

||| COLUMN |||
