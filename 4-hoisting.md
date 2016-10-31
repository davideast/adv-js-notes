<p>
  <h1 align="center">Hoisting</h2>
  <h3 align="center">The concept is real, but it's actually just the compiler.</h2>
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

Why does JavaScript "hoist" why doesn't it work in a top-down fashion?

Recursion is when a function calls itself. Mutual recursion refers two or more functions calling each other. Mutual recursion would be impossible in a language without hoisting. One of the functions would always be declared too late. A would be declared before B, but B requires A.

```js
a(1);       // ???

function a(foo) {
   if(foo > 20) return foo;
   return b(foo+2);
}

function b(foo) {
   return c(foo) + 1;
}

function c(foo) {
   return a(foo *2);
}
```

C lang header files are manual hoisting. Declarations are at the top of what the compiler handles, because the compiler doesn't automatically hoist.

### Temporal Dead Zone
`let` doesn't "hoist".

```js
function boo(bar) {
   if(bar) {
      console.log(baz); // ReferenceError
      let baz = bar;
   }
}
```