# Javascript features

Remember to use `"use strict" ;` at the beginning of a js file.
### Values and types

Since JS is object oriented, all values are objects, but variables are not.
A variable just refer to an object (just like a pointer in C).
All objects have a type: 

![[Pasted image 20240305115703.png]]

Two particular types are **null** and **undefined**: undefined is a variable declared but not initialized.
**Functions** are objects too.

Boolean type has only two values which are true and false: in JS only 6 values are "falsy":
- 0
- -0
- `NaN`
- `undefined`
- `null`
- `''` (the empty string)
Every other value is consider "truthy", even empty arrays or objects.

To compute a comparation between values we can use the classic `==` operator which **convert** types and then compare the result.
To compute a comparation without convert values we can use the `===` operator.


```js
console.log("3"==3) //it converts the values so it's true

console.log("3"===3) //not the same type so it's false

console.log(3+"2") //since the second value is a string the '+' symbol is a concatenation so the output will be 32

console.log(3-"1") //output is 2
```

Note that real and integers numbers are treated the same way: casting operations are performed based on the operands.

### Declaring variables

Declaring a variable is mandatory in strict mode: there are 3 ways to declare a variable.

![[Pasted image 20240305121613.png]]

### Expressions

If we compare two objects, the comparison will return true **only if they are the same object**, in every other scenario (even with same property) it will return false.

Mathematical operations are available inside **Math** library then we can compute them:

```js
const r = Math.abs(-345);
```

Logical operators are the classic ones: `&&`, `||`, `!`. We can use this operators to avoid if statements:

```js
flag && (expression) //the expression is computed only if flag is true (same as if)

value || (default_value) // if value is undefined, NaN... we assign a default value
```

All common operations with numbers and strings are supported, also conditional operator: `c ? t : f`.
### Control structures

If, For, While, Do while and switch are identical to C language.
There are two **special For loops** by using `in` and `of` keyword: to iterate over an array we can use this special loop:

![[Pasted image 20240305124102.png]]

Note the behavior of `in` for loop which iterate over the properties of an object.
We can also iterate over an array with the methods **`.forEach()`** and **`.map()`**

Exceptions handling is implemented by using a **try, catch, finally** construct.

### Arrays

Collections of **different** **types** of data, defined by square brackets.
The main property is `.length`
There are methods that modify the array, others returns new arrays.
There's no need to specify the length of an array when declaring, the length adjusts automatically.

Some of the main methods for arrays:
- `push()` adds an element at the end
- `pop()` remove an element from the end
- `unshift()` adds an element at the beginning
- `shift()` remove an element at the beginning

Since array's name is only a reference to the actual array, to create a real copy of an array we must use the `Array.from()` function.

![[Pasted image 20240305125300.png]]

We can **destructure** arrays using square brackets:

```js
let a=5;
let b=11;
[a,b] = [b,a] //swap operation
```

The **spread operator** `...` is available in JS: it basically expands an iterable object in its part:

```js
let parts = [3,4,5,6,7];
const numbers = [1,2, ...parts, 8, 9, 10];

const copy_of_parts = [...parts]; //we can also copy an array
```

### Strings

In JS **Strings** are **immutable** sequence of Unicode characters: the length of a string equals the number of char it contains. A char is a 1-length string.

All strings operations **return new strings**:

![[Pasted image 20240305125519.png]]

To print formatted string we can use the dollar sign:

```js
const name = "Jeff";
console.log(`Hello ${name}`); //output-> "Hello Jeff"
```

# Javascript objects

Javascript is object oriented, but differently from Java, an object can exist without a class.
At any time we can add, remove or modify **properties** and **methods** of a certain object.
Note that all methods and properties are **public**.

To create an object we use curly brackets, just as a python dictionary.

```js
let empty_object = {};

let book = {
	author: "Enrico",
	title: "Learning JS",
	pages: 520,
	"my review": 3
}
```

The variable `book` above is just a reference to the real object, which has no class.
To access properties we use **dot notation** or **square brackets** (when property name is an invalid identifier).

```js
book.author //output: Enrico
book["author"] //same as before
book["my review"] // output: 3
book.my title //ERROR, CANNOT ACCESS BY DOT NOTATION
```

If a property is not defined, accessing it will return `undefined`.

### Iterate on properties

Properties can be associated with an array: to iterate on properties we have two ways:
- Use the `in` keyword in a for loop.
- Use the methods `keys` and `entries`

**Keys** will return an array with the names of properties, while **entries** will return every pair key,  value of that object.

```js
for (let a in book){
	console.log(a); 
}
//"author", "title", "pages", "my review"

let prop = book.keys();
//["author", "title", "pages", "my review"]

let pair = book.entries();
//[[ 'author', 'Enrico' ], [ 'pages', 520 ], ["title", "learning JS"]... ]
```

We can **check the presence of a property** using the `in` operator, it will return a boolean. 
### Modify and copying objects

The **assign** method is very useful to manipulate object. It returns the manipulated object.

```js
Object.assign(target, source, default values, ..)
```

To copy (not creating an alias, but a real copy) we can set the target to a new object, and the source to the initial object.
Note that the new object is represented with `{}`.

```js
a_new_book = Object.assign({}, book);
```

We can also merge properties of different objects:

```js
let book2 = Object.assign({}, book, {"new_property": 4.5}); 
```

The **spread operator** is useful also for copying objects:

```js
let book2 = {...book};
```

# Javascript functions

In JS **functions are objects**: they can be assigned, passed as a argument and also returned.
Inside a function exists a privare scope.
Functions must **return only one value** (can be arrays, tuples...).

### Define a function 1

There are 3 ways: the first one is the same of may other languages.

```js
function do(params){
	//do stuff
}
```

Parameters are passed **by value**, and local variables are reference to the original values. 
If a parameter is not passed it will assume the value **undefined**: we can check undefined values with `p = p || default_value_p`.
The **rest operator** is useful to compact all remaining arguments into an array:

```js
function do(first, second, ...arr){
	for (let el of arr)
	//do stuff
}

let res = do(45,2,5,3,6);
```

### Define a function 2

```js
const fn = function(){
	//do stuff
}

const fn = function my_function(){
	//do stuff
}
```

Note that is also possible to give a name to the function

### Define a function 3

We can use the **arrow notation**:

```js
let fourth = (x) => {
	return square(x)*square(x);
}
```

In all three cases we create an **object of type function**: this means we can call that function, assign it to a variabile, store it into an array ecc...
If an arrow function has only parameter, parenthesis can be omitted.
If an arrow function has only a return line, we can avoid to write `return` and just write the expression, which is **return implicitly**.

### Nested functions

It is possible to declare nested functions: the inner function is scoped within the external functions, so we cannot call it outside. It's possible for the inner function to access variables declared in the external function.

**Closure**: it's a feature that says: a nested function executed after the execution of the outer
function can still access outer function’s scope.
Every function object remembers the scope where it is defined even after the external function in no longer active:

```js
"use strict" ;
function greeter(name) {
	const myname = name ;
	const hello = function () {
		return "Hello " + myname ;
	}
	return hello ;
}

const helloTom = greeter("Tom") ;
const helloJerry = greeter("Jerry") ;
console.log(helloTom()) ;
console.log(helloJerry()) ;
```

In the example above, each functions remember where `myname` was defined. Outside the scope, `myname` is not destroyed, but it's still accessible by the `hello` function.

If inner functions remember variables of the external variable, we can **use closures to emulate objects**:

```js
//Creating a counter using closure

"use strict" ;
function counter() {
	let value = 0 ;
	
	const getNext = () => {
		value++;
		return value;
	}
	return getNext ;
}

const count1 = counter();
console.log(count1()); //Output 1
console.log(count1()); //Output 2
console.log(count1()); //Output 3
//...
```

We can emulate object **with methods** by returning an object where each property is a function:

![[Pasted image 20240312103151.png]]

### Immediately Invoked Function Expressions (IIFE)

Sometimes functions may protect the scope of variables and inner functions: in order to do this we can call the function as soon as it's declared:

```js
(funtion(){
	 let a = 3;
	 console.log(a);
} ) ();

//OR

const num =(funtion(){
	 let a = 3;
	 return a;
} ) ();
```

This type of approach can be used to emulate objects.

### Constructor functions

To creare an object we can use a **constructor** (same as Java): to declare a constructor we just use a function.
- Its name has a capital initial letter 
- We use the keyword `this` to declare property
Then we can use the `new` keyword to build the object.

```js
function Car(make, model, year){
	this.make=make;
	this.model=model;
	this.year = year;
	this.isNew = () => (this.year > 2000);
}

let mycar = new Car('Audi', 'A3', 2016);
```

Forgetting the keyword `new` will result in a **TypeError**: the keyword `this` will be undefined and the program will crash.

# Dates

JS has a `Date` type but is not very efficient: we can store a time instant with millisecond precision, counted from Jan 1 1970.

```js
let now = Date(); 
let newYearMorning = Date(
2021, //year
0, //month (from 0, January)
1, //day
18,15,10,743 //local time
);
```

### Day.js

Popular library to manipulate date objects: to import the library just type at the top of a file/module:

```js
const dayjs = require('dayjs');
//OPPURE
import dayjs from 'dayjs'
```

![[Pasted image 20240312120058.png]]

Several methods are available to modify a date: add, subtract, start of, end of...
Documentation: https://day.js.org/

# Callbacks

A **Callback** function is a function passed into a another function as an argument, which is then invoked inside the outer function to complete some kind of action.

For example the `sort()` function is based of strings comparison: we can customize the behavior of `sort()` by passing it a callback function used as a sort criteria.

```js
let numbers = [4,5,4,1,3];
numbers.sort( (a,b) => a - b ); //this arrow function is a callback
console.log(numbers);
```

In the example above we used a **synchronous** callbacks, because the callback function was invoked  at the same time of the `sort()` function.

Another example is the `filter()` function, invoked on arrays:

```js
const bad = market.filter( (stock) => stock.var > 0)
```

### Functional programming

**Functional programming** is a paradigm where the developer mostly construct and structure code using **functions**: it also improves code's readability.

This paradigm **requires avoiding mutability**: this means we usually don't modify objects in place (if we need to change an element of an array, we return a new array).
This means also avoiding classic for loops, we can use functions!

| Functions | Behavior                                                                            |
| --------- | ----------------------------------------------------------------------------------- |
| forEach   | It resurns undefined, it's not chainable, it never modifies the original array      |
| every     | All elements must satisfy a constraint                                              |
| some      | At least one element has to satisfy a constraint                                    |
| map       | It transforms each element one by one into a new array                              |
| filter    | It creates a new array with only the elements that satisfy a constraint             |
| reduce    | It combines all elements, it has two parameters: reducer function and initial value |
| Others    | ![[Pasted image 20240312122749.png]]                                                |

# Asynchronous programming in JS

Javascript is single-threaded, so indirectly is synchronous: asynchronous callbacks can work with an execution environment (such as Node.js).
The simplest asynchronous call ca be performed with the `setTimeout()` passing as arguments a function and a milliseconds time.

```js
function hi(){
	console.log("Hello");
}

function hi_param(x){
	console.log("Hello" + x);
}

console.log("Print delayed");

setTimeout(hi, 1000); //1 second delay

setTimeout(hi, 1000, /*arguments: */ "World");
```

Since Asynchronous callbacks are very useful, **most of JS program are deeply asynchronous**: programs are **event**-driven and with **non-blocking** primitives.

We can also **delay the execution** of a function with `setInterval()`:

```js
const id = setInterval( ()=> {/*do something*/}, 2000);
//EVERY TWO SECONDS THE FUNCTION IS CALLED
//id is the handle that refers to the timer

//With clearInterval we stop the timer
clearInterval(id);
```

### Handling errors in callbacks

There is no correct way to handle errors, only best practices: typically a callback function has the first parameter for storing errors, while the second parameter is for the result.
This is valid in Node.js:

```js
fs.readFile('/file.json', (err, data) => {
	if (err !== null) {
		console.log(err);
		return;
	}
	//no errors, process data
	console.log(data);
});
```

### Database access with [SQLITE](https://github.com/mapbox/node-sqlite3/wiki)

Node.js supports many databases such as Cassandra, CouchDB, MySQL, MongoDB, Neo4J, Oracle, PostgreSQL, Elastic...
A simple solution for a small volume of data is **sqlite**.

To establish a connection to a database:

```js
const sqlite = require('sqlite3');

const db = new sqlite.Database('exams.sqlite', // DB filename
	(err) => { if (err) throw err; });

//Do stuff

db.close();
```

To create a query we store the SQL statement into a variable, then we can use sqlite method to run it:

```js
let sql = "SELECT * FROM course LEFT JOIN score ON course.code=score.coursecode" ;

db.all(sql, (err,rows)=>{
	if(err) throw err ;
	for (let row of rows) {
		console.log(row);
	}
});
```

- `db.all(sql, [params], (err, rows) => { } )`  The `all` function runs the query and (if there are no errors) **all the rows** are stored in the callback `rows` variable.

- `db.get(sql, [params], (err, row) => { } )` The `get` function runs the specified sql query, then it return into the callback `row` variable **only the first row of result**.

- `db.each(sql, [params], (err, row) => { } )` The `each` method runs the callback function once **for each result row** (no need to store them).

- `db.run(sql, [params], function (err) { } )` The `run` method is used for queries that don't return rows at all (such as UPDATE, CREATE TABLE...). 

```ad-warning
title: SQL Injection
SQL strings can have parameters: we need to use the `?` placeholder, then specify the parameters inside method's calls.
Concatenating string is not allowed: it can lead to SQL injection attacks.
```

SQL queries are executed **asynchronously**: this can lead to errors and bugs. For example queries can be in execution, but JS runs the `db.close()` line.
Also, it is not guaranteed that the completion order of queries corrisponde to the scheduled order.

### Promises

A **Promise** is an object representing the **eventual completion (or failure)** of an asynchronous operation. They also standardized a way to handle errors and provide a way for errors to propagate correctly through a chain of progress.

The constructor of a promise can call either a **resolve or a reject function**.

```js
const myPromise =  new Promise((resolve, reject) =>{

	//Do something here
	//then i call either:
	resolve(somedata);
	//or
	reject("failure");
})
```

A promise object has several methods, but the two most important are `then()` and `catch()`.
When a promise is fulfilled the `then` callback is executed, instead if a promise is rejected then `catch` is executed.
If we are interested only in the result, we can omit `catch()`.

![[Pasted image 20240319120049.png]]

We can also **concatenate** `then()` methods instead of perform nested callbacks: the only important thing is to return something, otherwise `then` can not operate.

We can execute multiple promises with `all()` and `race()` methods.

### Async / Await

Prepend **async** keyword to any functions means that it will return a promise.
Prepend **await** when calling an async function (or a function that return a promise) makes the code stops until the promise is resolved.

The async function declaration defines an asynchronous function: asynchronous functions operate in a separate order than the rest of the code (via the event loop), returning an implicit Promise as their result.

he await operator can be used to wait for a Promise. It can only be used inside
an async function: await blocks the code execution within the async function until the Promise is resolved
- When resumed, the value of the await expression is that of the fulfilled Promise
- If the Promise is rejected, the await expression throws the rejected value

```js
function resolveAfter2Seconds() {
	return new Promise(resolve => {
	setTimeout(() => {
		resolve('resolved');
	}, 2000);
	} 
});

async function asyncCall() {
	console.log('calling');
	const result = await resolveAfter2Seconds();
	console.log(result);
}

asyncCall()
```

