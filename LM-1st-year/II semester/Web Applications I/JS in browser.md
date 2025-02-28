# Load JS in browser

To load JS code in a browser we use the `script` tag:

```html
<script src="index.js"></script>
```

Keep in mind that the script tag is a **blocking operation**, so it's preferred to put the tag at the bottom of HTML file. In this way the browser can render the web page, then loading the scripts.

![[Pasted image 20240409115925.png]]

# BOM: Browser Object Model

Properties of BOM:
- **console**: browser debug console (visible via developer tools)
- **document**: the document object
- **history**: allows access to History API (history of URLs)
- **location**: allows access to Location API (current URL, protocol, etc.). Read/write property, i.e., can be set to load a new page
- **localStorage** and **sessionStorage**: allows access to the two objects via the Web Storage API, to store (small) info locally in the browser

# DOM: Document Object Model

It's the **browser internal representation of a web page**, obtained by parsing HTML. The browser exposes a set of APIs to manipulate the DOM.
Starting from the **document node** (the root) we have different types of nodes:
- An HTML tag is an **element** node
- An attribute is an **attr** node
- The text inside a tag is a **Text** node
- The Document type declaration is a **DocumentType** node
- **Comment** node is an HTML comment

The DOM APIs can manipulate a set of nodes: this **NodeList** can be accessed and processed as a JS array.

To find DOM elements we have several methods:

```js
document.getElementById(value);
document.getElementsByTagName(value)
document.getElementsByClassName(value)
document.querySelector(css)
document.querySelectorAll(css)
```

We can use this methods above also on NodeList objects.
The **attributes** of HTML elements can be accessed as **properties** (i.e. with dot notation)

```js
elem.hasAttribute(name)
elem.getAttribute(name)
elem.setAttribute(name, value)
elem.removeAttribute(name)
elem.attributes
elem.matches(css)
```

There are methods for creating a node: `createElement` and `createTextNode`.
There are also methods for navigating NodeList and inserting new element in a NodeList.

# Event handling

Js is the browser is used as an **event-driven** programming model
Events are determined by the **target event** and the **type** of the event.
The **event listener** can be set on any DOM objects:

```js
const link = document.getElementById("#my-link");
link.addEventListener('mousedown', event =>{
	console.log("event triggered")
});
```

There are several types of event: [DOM events](https://en.wikipedia.org/wiki/DOM_events)

To prevent default behavior of an event, we can use `event.preventDefault()`

# Form control

To retrive active forms in the we page we can access `documents.forms`.
Here's a list of useful events of forms:

![[Pasted image 20240409122126.png]]
