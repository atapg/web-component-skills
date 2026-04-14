# Events & Communication

## Contents

- Event Dispatching
- Event Listening
- Custom Events
- Event Bubbling
- Event Composition

---

### Event binding

```typescript
class CustomButton extends HTMLElement {
	private element: HTMLElement | null = null;

	constructor() {
		super();
		this.attachShadow({ mode: 'open' });

		const button = document.createElement('button');
		button.textContent = 'Click me!';

		this.element = button; // Store reference to the button element

		if (this.shadowRoot) {
			this.shadowRoot.appendChild(button);
		}
	}

	// Add event listener when the element is connected to the DOM
	connectedCallback() {
		if (this.element) {
			this.element.addEventListener('click', this.handleClick.bind(this));
		}
	}

	// Remove event listener when the element is disconnected from the DOM
	disconnectedCallback() {
		if (this.element) {
			this.element.removeEventListener('click', this.handleClick.bind(this));
		}
	}

	// Handle click event
	private handleClick() {
		console.log('Button clicked!');
	}
}
```

### Custom events

```typescript
private handleClick() {
    console.log('Button clicked!');
    // Dispatch a custom event when the button is clicked
    this.dispatchEvent(
        new CustomEvent('button-clicked', {
            detail: { message: 'Button was clicked!' },
            bubbles: true,
            composed: true,
        }),
    );
}
```

### Event handling in the parent component

When handling custom events dispatched by a web component, you should use the `addEventListener` method to listen for the event on the component element. The event will be triggered when the component dispatches it, and you can access the event details through the `event.detail` property.

```html
<custom-button id="myButton"></custom-button>

<script>
	const myButton = document.getElementById('myButton');
	myButton.addEventListener('button-clicked', (event) => {
		console.log(event.detail.message); // Output: Button was clicked!
	});
</script>
```

### Event bubbling and composition

By setting the `bubbles` and `composed` options to `true` when dispatching the custom event, you allow the event to bubble up through the DOM and cross the shadow DOM boundary, making it accessible to parent components and other elements in the document. This is important for ensuring that your custom events can be handled by any part of the application that needs to respond to them, regardless of whether they are inside or outside the shadow DOM of the web component.

```typescript
button.addEventListener('click', () => {
	this.dispatchEvent(
		new CustomEvent('button-clicked', {
			detail: { message: 'Button was clicked!' },
			bubbles: true, // Allow the event to bubble up through the DOM
			composed: true, // Allow the event to cross the shadow DOM boundary
		}),
	);
});
```

### Event delegation

Event delegation is a technique where you attach a single event listener to a parent element instead of attaching individual listeners to each child element. This can improve performance and simplify event handling, especially when dealing with dynamic content. In the context of web components, you can use event delegation to handle events from multiple child elements within the shadow DOM.

For example, if you have multiple buttons inside your web component and want to handle click events for all of them, you can attach a single event listener to the shadow root and use event delegation to determine which button was clicked:

```typescript
class CustomButton extends HTMLElement {
	constructor() {
		super();
		this.attachShadow({ mode: 'open' });
		const template = document.createElement('template');

		template.innerHTML = `
      <button class="btn1">Button 1</button>
      <button class="btn2">Button 2</button>
    `;

		this.shadowRoot?.appendChild(template.content.cloneNode(true));
		// Attach a single event listener to the shadow root
		this.shadowRoot?.addEventListener('click', (event) => {
			const target = event.target as HTMLElement;
			if (target.classList.contains('btn1')) {
				console.log('Button 1 clicked');
			} else if (target.classList.contains('btn2')) {
				console.log('Button 2 clicked');
			}
		});
	}
}
```
