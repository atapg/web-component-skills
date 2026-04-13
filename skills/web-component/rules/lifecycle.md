# Lifecycle & State Management

## Contents

- Lifecycle Callbacks
- State Management
- Attribute Observation
- Property Synchronization

---

### Lifecycle management

**Lifecycle Callbacks:**

- `connectedCallback()`: Called when the element is added to the document.
- `disconnectedCallback()`: Called when the element is removed from the document.
- `attributeChangedCallback(name, oldValue, newValue)`: Called when an observed attribute is added, removed, or changed.
- `adoptedCallback()`: Called when the element is moved to a new document.
  You can implement these callbacks in your custom element class to perform specific actions when these events occur. For example:

```typescript
class CustomButton extends HTMLElement {
	constructor() {
		super();
		this.attachShadow({ mode: 'open' });
		const button = document.createElement('button');
		button.textContent = 'Click me!';
		if (this.shadowRoot) {
			this.shadowRoot.appendChild(button);
		}
	}

	// Mounted to the DOM
	connectedCallback() {
		console.log('Custom button added to the page.');
	}

	// Unmounted from the DOM
	disconnectedCallback() {
		console.log('Custom button removed from the page.');
	}

	// Attribute changed
	attributeChangedCallback(name: string, oldValue: string, newValue: string) {
		console.log(`Attribute ${name} changed from ${oldValue} to ${newValue}.`);
	}

	// Moved to a new document
	adoptedCallback() {
		console.log('Custom button moved to a new document.');
	}
}
```

### State Management

**Attribute-based state:**

- Use `this.setAttribute()` and `this.getAttribute()` to manage state.
- Use `this.hasAttribute()` to check if an attribute exists.
- Use `this.removeAttribute()` to remove an attribute.
- Use `this.toggleAttribute()` to toggle an attribute.

### Attribute observation

**Observation configuration:**

- Use `static get observedAttributes()` to specify which attributes should trigger `attributeChangedCallback()`.
- Only observed attributes will trigger the callback when they change.

**Example:**

```typescript
static get observedAttributes() {
	return ['disabled', 'loading'];
}
```

**AttributeChangedCallback:**

- The callback receives `name`, `oldValue`, and `newValue` parameters.
- Use this to update the component's internal state or trigger re-renders.
- Always check for actual changes before updating state to avoid unnecessary work.

**Example:**

```typescript
attributeChangedCallback(name: string, oldValue: string, newValue: string) {
	if (oldValue !== newValue) {
		this.updateState(name, newValue);
	}
}
```

### Property Synchronization

- When using attributes for state, synchronize them with properties for better JavaScript API.
- Use getters and setters to maintain consistency between attribute and property values.

**Example:**

```typescript
get disabled() {
	return this.hasAttribute('disabled');
}

set disabled(value) {
	if (value) {
		this.setAttribute('disabled', '');
	} else {
		this.removeAttribute('disabled');
	}
}
```

### Lifecycle Best Practices

- **Minimize side effects** in lifecycle callbacks - keep them simple and focused.
- **Avoid heavy operations** in `connectedCallback` as it can block rendering.
- **Clean up resources** in `disconnectedCallback` to prevent memory leaks.
- **Use `requestAnimationFrame`** for DOM updates in callbacks to batch changes.
- **Debounce expensive operations** to avoid performance issues.
