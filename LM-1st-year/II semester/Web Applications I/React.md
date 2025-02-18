# Introduction

React uses a **declarative approach**, which means that we don't manipulate the DOM in a direct way: we simply define **components** and how they will be rendered in the page.
React is also based on a functional paradigm: this means that **components** **are functions** and on every change the virtual DOM will update itself.

Components have **props and state**, the first ones are immutable and they describe the properties of a component, while the state are useful to keep track of information during the execution of the app: the state cannot be edit directly, but it can be modified through special calls. **Props** are passed to a component by its parent. State's changes don't affect parents component, just the children: state are often **moved up**, so they can be shared between components.
Another property of React is the **pureness** of function: a function has no side effects besides the return value.

Every time a change is made, React will **re-render** all components from scratch, even with small modification: this does not affect performance since React uses a **virtual DOM** optimized.

To initialize the react virtual DOM we use the `createRoot` React function:

```js
const container = document.getElementById('root');
const root = createRoot(container);
root.render(<h1>Hello, world!</h1>);
```

As we can see above, the argument of the `render` function is similar to HTML, but without quotes. It is called **JSX syntax** which is basically translated (with Babel) to React components invocation.

![[Pasted image 20240423102551.png]]

To **define a custom component** we simply define a function and return a graphical element with JSX syntax: components can also be **dynamic**, so it receives properties that can be printed in the component using the curly brackets notation

```js
	//WITHOUT PROPERTIES
const BlogPostExcerpt = () =>{
	return (
		<div>
			<h1>Title of the blog post</h1>
			<p>Text of the blog post</p>
		</div>
	)
}

 //WITH PROPERTIES
const BlogPostContent = (props) => {
	return (
		<div>
		<p>{props.content}</p>
		</div>
	)
}
```

We can split the components in two families: **presentation components** just displays information, while **container components** interacts with the backend.

### Use state

```js
import { useState } from "react";
function Button(props) {
	let [buttonLang, setButtonLang] = useState(props.lang) ;
	if (buttonLang === 'it')
		return <button onClick={()=>setButtonLang('en')}>Ciao!</button>;
	else
		return <button onClick={()=>setButtonLang('it')}>Hello!</button>;
}
export default Button;
```

To use a state we can define it using the `useState()` method, it return a 2-length array where the second item is a special function to edit the state. 
# React Elements

An **element** is a plain object describing a component instance or DOM node and its desired properties. A React element is a representation of a DOM element in the Virtual DOM.
It contains information about:
- the component **type**
- its **properties**
- any child element inside it

To create a React element we use the `createElement` method:

```js
React.createElement(type, props, children);
```

The **type is a string**, which represent the name of the element, the **props is an object** (defined in `{}`) and **children** is the list of child elements.

```ad-note
title: Convention on naming
Remember that DOM elements are always **lowercase**: div, li, ...
React components are always upper camel case: NavigationBar, BlogPost, ...

```

# Hooks

A Hook is a set of additions to function components by access **advanced features**.
Most popular examples of hooks:

![[Pasted image 20240430101718.png]]

We also need to define the **Context of a component**, which is a sort of "global and implicit property" that is passed to all interested components.

Now let's consider a piece of text with a dynamic behavior: first of all we use the usestate hook that return a **reference to che current value and a function to update the state value.**

```js
import React, { useState } from 'react';

function ShortText(props) {
	const [hidden, setHidden] = useState(true);
	return (
		<span>
		{hidden ?
			'${props.text.substr(0, props.maxLength)}' : props.text }
		{hidden ? (
			<a onClick={() => setHidden(false)}>more</a>
		) : ( <a onClick={() => setHidden(true)}>less</a>
		)}
		</span>
	} 
	);
```

It's important to **never modify a state in a directly way**. We always want to use the update state function provided by the hook: that is because it will apply the modification asynchronously.
If the update function depends on the current value of the state, **always use a callback function**: 

```js
setCounter( (cnt) => cnt+1)

//DO NOT DO THIS
setCounter(cnt+1)
```

It's a good practice to **implement stateless components** which are more reusable and efficient: we try to "**lift up" states** to parent components and pass states to children through props.

# Context

As we said before, to pass props to nested components we need to write a lot of code.
We can instead use a **context**: a set of "global" props that are automatically available to lower components.
Context can be useful to share information about the light/dark theme, the logged in/out, the multi-language support: information that are common to all components.

First of all, we need to create a `Context` object:

```js
const example = React.createContext(defaultValue);
```

This `example` contains two props: a **Provider** and a **Consumer**.
The provider injects the context into all child component, while the consumer (or the **`useContext`** hook) made the value of the context accessible.
Thus, each component need to **subscribe** to the context to use the value inside: the value come from the closest Provider ancestor.
It's important to note that each time the value changes, all consumer will re-render.

![[Pasted image 20240514093347.png]]

A much faster way to gain access to the value of the context is using the `useContext` hook: it simply takes as argument the name of Context object and returns the value.

```ad-warning
title: Hooks and context
THe `useContext` is used to access the context value. There is no way to creare a context (or a provider) usign hooks!
```

If a child component need to update a context value, we need to pass him a callback function to perform the update.

Recommendation: do not put everything in context! It reduces purity of components and also defeats component portability.

# Routes

Whatever page we visit React will always load the same page (**index.js / index.html**). The page to render is passed to the App component.
React doesn't have a default route manager, so we can install it via npm `react-router-dom`

The object that send the user to another page is called a Link, while the page itself is a Route:

![[Pasted image 20240514100154.png]]

The path (URL) can be static, dynamic (for example `/profile/:userId`) or we can use the star notation (`/docs/*`). In case of multiple match, the most specific one is selected.
Paths are by default case-insensitive (can be changed but it's not recommended).

`Route` components can be nested as well: in that case the URL will be concatenate and the matched components will be displayed in the `Outlet` component (Not forgive the outlet, or nothing will be rendered)

![[Pasted image 20240514100813.png]]

Special routes:
- **Index route**: `<Route index elemeent={<Home />} />` is a child route with no path that render in the parent's outlet
- **Layout route**: useful to wrap with a common layout its children routes
- **No match route**: `path="*"` will match only when no other routes do

![[Pasted image 20240514101346.png]]

To navigate between pages we have two methods:
Remember that all path are relative, unless they start with "/".

```js
//FIRST METHOD
<Link to="/about"> About </Link>


//SECOND METHOD

const navigate = useNavigate();
<NewInvoiceForm onSubmit={(event) => {
	const newInvoice = create(event.target);
	navigate(`/invoices/${newInvoice.id}`);
}}
/>
```

```ad-error
title: \<a\> and action
Never use a “plain hyperlink” \<a\>.
Never use a “form submission”	\<form action='…'\>
They will reload the whole application (and kill the current state)
```

when creating menus or navigation elements, it is useful to see which item is the currently selected one:
-  `<NavLink>` behaves like `<Link>`, but knows whether it is “active”: It adds the `active` class to the rendered link (to be customized with CSS)
-  You may create a callback in `className={}` that receives the `isActive` status and decides which class to apply

If routes have dynamic argument (notation `:userId`) we can retrive the parameters using the **`useParams` hook**:

```js
function Post( props ) {
	const params = useParams();
	//each parameter can be accessed with dot notation
}
```

If a URL has a search query we can use `useSearchParams()`.
