# Fetch API

We now consider a classic client-server architecture where communication between them is based on JSON/HTTP messages.

The `fetch()` API allows us to send an HTTP request and returns a **Promise which is rejected only in case of network errors**. In case of successful fetch, the promise is resolved to a `Response` object. Any error code must be checked through the property `response.status` or `response.ok`. Other properties for `Response` are available.

```js
fetch(url).then(response => {
	if (!response.ok) { throw Error(response.statusText) }
	let type = response.headers.get('Content-Type');
	if (type !== 'application/json') {
		//then() returns a rejected promise if something is thrown
		throw new TypeError(`Expected JSON, got ${type}`)
	}
	return response;
	})
	.then(response => {
		//...
		})
	.catch(err => console.log(err)) // either the throw value or other errors
```

The **default option for fetch** is a **GET request**, but we can customize the behavior of the method:

```js
fetch(url, {
	method: 'POST',
	headers: {
	'Content-Type': 'application/json',
	},
	body: JSON.stringify(objectToSend), // Conversion in JSON format
})
```

To **read the body** we have different methods, but they can only be called once, because the body is consumed.

To run multiple fetch **sequentially** we can use an `async` block:

![[Pasted image 20240521094936.png]]

To run multiple fetch in **parallel** we use `Promise.all`:

![[Pasted image 20240521095013.png]]

Alternative to fetch is using a library: **Axios**

# Servers setup

We will use two separate servers: one for react and one for APIs, then we will use **CORS** ( Cross-origin resource sharing).

![[Pasted image 20240521095509.png]]

In the **API server** (only the API server) enable CORS:

```js
// index.js (node express server)
var cors = require('cors') ; // npm install cors
//Enable All CORS Requests (for this server)
app.use(cors());
//Use ONLY for development, otherwise restrict domain
```

Let's make and example:
In the react application we'll have an API file that makes the calls:

```js
const APIURL=new URL('http://localhost:3000');
async function getCourses() {
	return fetch(new URL('/courses', APIURL))
	.then((response)=>{
		if(response.ok) {
			return response.json() ;
		} 
		else {
			throw response.statusText;
		}
	})
	.catch((error)=>{
		throw error;
	});
}
```

While in the server (DAO):

```js
const express = require('express');
const port = 3000;
const cors = require('cors');
const app = express();
app.use(cors());
app.get('/courses', (req, res) => {
	dao.listCourses()
	.then((courses) => res.json(courses))
	.catch((dbErrorObj)=>
		res.status(503).json(dbErrorObj));
});
app.listen(port, () => console.log(`Example app
listening at http://localhost:${port}`));
```

