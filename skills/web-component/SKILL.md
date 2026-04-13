---
name: web-components
description: Manage web components - create, style, and use custom HTML elements with shadow DOM and encapsulation. Triggered when user asks about creating custom elements, using shadow DOM, handling events, or integrating web components.
---

# Web Component Skills

Web components allow developers to create reusable, encapsulated HTML elements that can be used across different web applications. They provide a way to extend HTML with custom elements, shadow DOM, and templates, enabling better modularity and maintainability.

## Principles

1. **Encapsulation First** - Use shadow DOM to isolate component styles and markup
2. **Custom Elements** - Extend `HTMLElement` and register with `customElements.define()`
3. **Slots for Composition** - Use `<slot>` for content projection
4. **Events for Communication** - Dispatch custom events for parent-child communication
5. **Attributes for Configuration** - Use attributes for component props
6. **No Framework Lock-in** - Web components work with any framework or vanilla JS

### Creation & Basics → [creation.md](./rules/creation.md)

- **Use `customElements.define()` for registering custom elements.**
- **Use `attachShadow()` for creating shadow DOM.**
- **Use `template` element for defining component structure.**
- **Use `cloneNode(true)` for cloning template content.**
- **Use `mode: 'open'` for shadow root to make it accessible from outside.**

### Lifecycle & State Management → [lifecycle.md](./rules/lifecycle.md)

- **Use `connectedCallback()` for initialization and setup.**
- **Use `disconnectedCallback()` for cleanup and resource management.**
- **Use `attributeChangedCallback()` for observing attribute changes.**
- **Use `adoptedCallback()` for handling document adoption.**
- **Use getters and setters for property synchronization.**
- **Use `requestAnimationFrame()` for DOM updates in callbacks.**
- **Debounce expensive operations to avoid performance issues.**
