# HTTP

HTTP protocol is used in modern web to send and receive information between clients and servers. Two HTTP messages are named **request** and **response**. The structure of HTTP message is the following:

![[Pasted image 20240326100645.png]]

The **initial line of request** is used to specify the **method**, the **local path** of the requested resource and the **version** of HTTP used.
For example: `GET /path/to/file.html HTTP/1.0`
Several methods are available in HTTP, but most common are GET and POST:

![[Pasted image 20240326101109.png]]

The **initial line of the response** contains 3 fields: **HTTP version**, the status **code** and a **phrase** in English that described the status.
For example: `HTTP/1.0 404 Not Found`
Status codes are grouped in categories, based on the initial number:
- Codes starting with a '2' represent **success**
- Code starting with a '1' are **informational** 
- Code starting with '3' are **redirection**
- Code starting with '4' are **client errors**
- Code starting with '5' are **server errors**

Headers lines are used to carry additional information: HTTP defines 46 header but only one is mandatory in all requests (host). There is one line per headers.

The request/response body contains the payload: the header **`Content-Type`** indicates the media type of resources.

# Express

In Node, **express is a popular framework** to activate a web server: to start a server application just type:

```
node index.js
```

Inside `index.js` we need to create an application object then use the `listen` method to start the server:

```js
import express from 'express';
const app = express();

app.get('/', (req,res) => {
		res.send("Hello world")
});

app.listen(3000, () => console.log("Server ready"));
```

To call an HTTP method we use: `app.method(path, callback)`

![[Pasted image 20240326103114.png]]

If path have parameters we can use the `:id` notation:

```js
app.get('/users/:userId/books/:bookId', (req, res) => {
	res.send(req.params)
});

//Request URL:
//http://localhost:3000/users/34/books/8989
//Results in:
req.params.userId == "34"
req.params.bookId == "8989"
```

To handle logins and validating forms we can use **morgan** and **espress-validator**.

# JSON

The goal of back-end is to communicate information between application and a database: in order to do this we need to use a comprensive text format.

![[Pasted image 20240326104053.png]]

**JSON** (JavaScript Object Notation) use a logical structure composed by three elements: **Primitive types** (number, string, true/false), **Array** (Ordered lists of values) and **Objects**.
Note that all strings must be quoted with `""` (not `''`) , including objects keys.

```json
{
	"firstName": "John",
	"lastName": "Smith",
	"address": {
		"streetAddress": "21 2nd Street",
		"city": "New York",
		"state": "NY",
		"postalCode": 10021
	},
	"phoneNumbers": [
		"212 555-1234",
		"646 555-4567"
		]
}
```

To perform **conversion** between object format and JSON format we can use **stringify** and **parse**:

```js
const myJson = JSON.strigify(myObj);

const myObj = JSON.parse(myJson);
```

### API

When interacting with a back-end we want to **retrieve single or URI collections**:

![[Pasted image 20240326105455.png]]

URI should be named using concrete nouns (not verbs).
Based on HTTP method we can get URI:

![[Pasted image 20240326105623.png]]

In Express we can use API:

![[Pasted image 20240326105751.png]]
