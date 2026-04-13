# Creation & Basics

## Contents

- Basics
- Render Styles and Templates
- Use the custom element in your HTML like any other element.
- Extending built-in elements
- The scalable way to create a web component

---

### Create a web component

You can create a web component by creating a class that extends `HTMLElement` and defining it using `customElements.define()`.

Simple component example:

```typescript
class CustomComponent extends HTMLElement {
	constructor() {
		super();
		this.innerHTML = `<p>Hello, I'm a custom component!</p>`;
	}
}

customElements.define('custom-component', CustomComponent);
```

### Create within template and shadow DOM

```typescript
class CustomComponent extends HTMLElement {
	constructor() {
		super();
		// Create a shadow root and append a template to it
		const shadow = this.attachShadow({ mode: 'open' }); // Has to be open to be accessible from outside
		const template = document.createElement('template');
		template.innerHTML = `<p>Hello, I'm a custom component with shadow DOM!</p>`;
		shadow.appendChild(template.content.cloneNode(true));
	}
}

customElements.define('custom-component', CustomComponent);
```

### Render Styles and Templates

You can also include styles and templates in your web component (Styles are scoped to the shadow DOM and won't affect the rest of the page). For example:

```typescript
class CustomComponent extends HTMLElement {
	constructor() {
		super();
		// Create a shadow root and append a template to it
		const shadow = this.attachShadow({ mode: 'open' }); // Has to be open to be accessible from outside
		const template = document.createElement('template');
		template.innerHTML = `
			<style>
				p {
					color: blue;
				}
			</style>
			<p>Hello, I'm a custom component with shadow DOM!</p>
		`;
		shadow.appendChild(template.content.cloneNode(true));
	}
}

customElements.define('custom-component', CustomComponent);
```

### Use the custom element in your HTML like any other element.

Import your web component in your main application file (e.g., `app.ts` or `main.ts`) to ensure it is registered before you use it in your HTML.

```typescript
import './web-components/custom-component.ts';
```

Then you can use the custom element in your HTML like this:

```html
<custom-component></custom-component>
```

### Extending built-in elements

You can also extend built-in HTML elements to create custom elements with additional functionality. For example, to create a custom button that extends the native `<button>` element:

```typescript
class ExpandableList extends HTMLUListElement {
	constructor() {
		super();
		this.textContent = 'Click me!';
	}
}

customElements.define('expandable-list', ExpandableList, { extends: 'ul' });
```

You can then use this custom element in your HTML like this:

```html
<ul is="expandable-list"></ul>
```

### The clean and scalable way to create a web component

The clean and scalable way to create a web component is to use a template and shadow DOM (Use this approach for all web components if user asks for clean, maintainable code or doesn't specify a particular approach). For example:

```typescript
const template = `
  <div>
    Hello World!
  </div>

  <style>

  </style>
`;

class WebComponent extends HTMLElement {
	private template: HTMLTemplateElement;

	constructor() {
		super();
		this.attachShadow({ mode: 'open' });
		this.template = document.createElement('template');
		this.template.innerHTML = template;
		this.render();
	}

	private render() {
		const fragment = document.createDocumentFragment();
		fragment.appendChild(this.template.content.cloneNode(true));
		this.shadowRoot && this.shadowRoot.appendChild(fragment);
	}

	static get observedAttributes() {
		return [];
	}

	private connectedCallback() {}

	private disconnectedCallback() {}

	private attributeChangedCallback(
		name: string,
		oldValue: string,
		newValue: string,
	) {
		if (oldValue === newValue) return;

		switch (name) {
			default:
				break;
		}
	}
}

customElements.define('web-component', WebComponent);
```
