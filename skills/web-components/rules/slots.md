# Slots and content projection

## Contents

- Slots
- Named slots
- Fallback content
- Accessing projected content
- Listening to slot changes

---

### Slots

```html
<button>
	<slot>
		<!-- This is where the projected content will go -->
	</slot>
</button>
```

### Named slots

```html
<div>
	<slot name="header"></slot>
	<slot name="content"></slot>
	<slot name="footer"></slot>
</div>
```

**Usage:**

```html
<web-component>
	<button slot="header">Header</button>
	<button slot="content">Content</button>
	<button slot="footer">Footer</button>
</web-component>
```

**Fallback content:**

```html
<slot>
	<!-- This is the fallback content -->
</slot>
```

### Accessing projected content

```typescript
const slot = this.shadowRoot.querySelector('slot');
const assignedElements = slot.assignedElements();
const assignedNodes = slot.assignedNodes();
```

### Listening to slot changes

```typescript
const slot = this.shadowRoot.querySelector('slot');
slot.addEventListener('slotchange', () => {
	console.log('Slot changed');
});
```
