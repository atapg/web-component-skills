# Performance optimization

## Contents

- Memory management
- Efficient DOM manipulation
- requestAnimationFrame
- Debouncing
- Lazy loading
- Eager loading
- Auto loading

---

### Memory management

```typescript
// Clean up event listeners
this.addEventListener('dispose', () => {
	this.removeEventListener('click', this.handleClick);
});

// Clean up timers
clearTimeout(this.timer);

// Clean up event listeners in disconnectedCallback
disconnectedCallback() {
  this.element?.removeEventListener('click', this.handleClick);
}
```

### Efficient DOM manipulation

```typescript
// Use document fragments for batch updates
const fragment = document.createDocumentFragment();
for (let i = 0; i < 100; i++) {
	const div = document.createElement('div');
	fragment.appendChild(div);
}
this.appendChild(fragment);
```

### requestAnimationFrame

```typescript
requestAnimationFrame(() => {
	// DOM updates
});
```

### Debouncing

```typescript
function debounce(func, delay) {
	let timeout;
	return function (...args) {
		clearTimeout(timeout);
		timeout = setTimeout(() => func.apply(this, args), delay);
	};
}
```

### Lazy loading

```html
<script
	type="module"
	src="./web-components/custom-button.js"
	loading="lazy"
></script>
```

### Eager loading

```html
<script
	type="module"
	src="./web-components/custom-button.js"
	loading="eager"
></script>
```

### Auto loading

```html
<script
	type="module"
	src="./web-components/custom-button.js"
	loading="auto"
></script>
```
