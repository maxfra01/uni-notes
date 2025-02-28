# Login flow

1. A user fills out a form in the client with a unique user identifier and a password
2. Data is validated and, if ok, is sent to the server, with a POST API
3. The server receives the request and checks whether the user is already registered, and the password matches  – Password comparison exploits cryptographic hashes
4. If not, it sends back a response to the client – “Wrong username and/or password”
5. If username and password are correct, the server generates a session id
6. The server stores the session id (together with some user info retrieved by the database) in its “server session storage”
7. The server replies to the login HTTP request by creating and sending a cookie – with name = Sessi  onID, value = the generated session id, httpOnly = true, secure = true (if over HTTPS)
8. The browser receives the response with the cookie – the cookie is automatically stored by the browser – the response is handled by the web application (e.g., to say "Welcome!")

Good practice: **create a react component for login with its own state for username and password**.

# Passport js

```js
const passport = require('passport');
const LocalStrategy = require('passport-local');
passport.use(new LocalStrategy( function verify
	(username, password, callback) {
		dao.getUser(username, password).then((user) => {
	if (!user)
		return callback(null, false, { message: 'Incorrect username or password.' });
	return callback(null, user);
	});
}));
```

Username and password are automatically extracted from the body of HTTP request.
To check password:

```js
exports.getUser = (email, password) => {
	return new Promise((resolve, reject) => {
		const sql = 'SELECT * FROM user WHERE email = ?';
		db.get(sql, [email], (err, row) => {
			if (err) { reject(err); }
			else if (row === undefined) { resolve(false); }
			else {
				const user = {id: row.id, username: row.email};
				const salt = row.salt;
				crypto.scrypt(password, salt, 32, (err, hashedPassword) => {
					if (err) reject(err);
					if(!crypto.timingSafeEqual(Buffer.from(row.password, 'hex'), hashedPassword))
						resolve(false);
					else resolve(user);
				});
			}
		});
	});
};
```

After setting everything up, now we can log in a user with Passport:
– adding an Express route able to receive the “login” requests
– passing the authenticate(\<strategy\>) method as the first additional callback
-  `authenticate('local')` will look for a username and password field in `req.body`

```js
app.post('/api/login', passport.authenticate('local'), (req,res) => {
	// This function is called if authentication is successful.
	// req.user contains the authenticated user.
	res.json(req.user.username);
})
```

# Protecting routes

In this cases the browser always sends the HTTP cookie header to any API that
requires authentication.
In the server, we need to define both the credentials and the origin options, when setting up the CORS module:

```js
const corsOptions = {
	origin : "https://localhost:3000",
	credentials: true
}
app.use(cors(corsOptions))
```

All protected APIs should include the `{credentials: 'include}` object in the fetch method. The Login route is included as well.

Once we have set up a session we may protect other routes: we can use the middleware `isAuthenticated` from passport js

To perform a logout:

```js
app.post('/api/logout', (req, res) => {
	req.logout(() => {
		res.end();
	});
});
```

