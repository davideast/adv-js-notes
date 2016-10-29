<p>
  <h1 align="center">Hoisting</h2>
  <h3 align="center">// TODO: </h2>
</p>


There is a conceptual model of how JavaScript works, and it is called "hoisting". This is not phyiscally what happens. Hositing is a mental construct, not in the spec.

```js
a;           // ???
b;           // ???
var a = b;  
var b = 2;
b;           // 2
a;           // ???
```
Lines 1 & 2 look like undeclared variables. But, the compiler runs through the code first and declares the `a` and `b` variables. So they are actually `undefined` at that point.

The hoisting way of thinking of it:

```js
var a;
var b;
a;           // ???
b;           // ???
a = b;  
b = 2;
b;           // 2
a;           // ???
```

The variables are not actually "hoisting" they are handled during the compile phase. You can think of it as the compile lines as 1 & 2, and the execution lines are 3-8.

This also goes for functions.

```js
var a = b();
var c = d();
a;             // ???
c;             // ???

function b() {
   return c;
}

var d = function() {
    return b();
};
```

Line 1 will return `undefined` because `c`. Line 2 will also returned `undefined`.

Function expressions are not hoisted whereas function declarations are.

This is how the "hoisted" version looks:

```js
function b() {
   return c;
}
var a;
var c;
var b;
a = b();
c = d();
a;                // ???
c;                // ???
d = function() {
   return b();
}
```

Functions are "hoisted" before variables, and the code below proves it.

```js
foo(); // "foo"

var foo = 2;

function foo() {
   console.log("bar");
}

function foo() {
   console.log("foo");
}
```

```js
sayHello(); // What is printed?
var sayHello = 'lol hi';
function sayHello() {
   console.log(sayHello);
}
function sayHello() {
   console.log('hi');
}
console.log(sayHello);
```