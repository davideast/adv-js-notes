<p>
  <h1 align="center">Lexical Scope</h2>
  <h3 align="center">The compiler decides what your scope is.</h2>
</p>

## What does lexical mean?

The *lex* refers to the parsing state called *lexxing* when the compiler is parsing through your code.

Lexical scope means compile time scope. At the time that you wrote your code, and the time your code got compiled, the decision for how all of the scoping will occur were made in stone at that point. No more decisions were to made beyond that lexical phase. The compiler decides what your scope is.

### Building example

Lexical scope is like a building. The current scope is the first floor, if you don't find what you need you go up one level.

### Nested scope

```js
function foo() {
   var bar = "bar";

   function baz() {
      console.log(bar); // lexical
   }
   baz();
}
foo();
```

Nested scopes are like bubbles nested within each other. `baz` is inside of `foo`, which means the scope of `baz` is inside the scope of `foo`.

### Cheating lexical scope with `eval()`

The first way to cheat lexical scoping is with `eval()`. 

```js
var bar = "bar";
function foo(str) {
   eval(str); // cheating!
   console.log(bar); // 42
}
foo("var bar = 42");
```

Using `eval()` will modifiy the existing lexical scope of `foo` and it will pretend that `bar` had been declared at compile time. A new declaration has been added at runtime.

Anytime that the JavaScript engine sees `eval()` it cannot optimize those lookups. It has assume the worst case that at runtime you're going to invaldate those assumptions. Just having `eval()` present, it disables some optimizations.

As a strict mode, when you put a variable inside an eval statement it puts it in a new scope. Techincally the strict mode code will run faster. 

### The `with` keyword

```js
var obj = {
   a: 2,
   b: 3,
   c: 4
};

obj.a = obj.b + obj.c;
obj.c = obj.b - obj.a;

with(obj) {
   a = b + c;
   d = b - a;
   d = 3; // ??
}

obj.d; // undefined;
d; // 3 -- oops!
```

The `with` keyword is how we shorthand assigning properties. There are some problems:

1. Creating auto globals. Line 13 `d = 3`. The `with` statement is treated like a lexical scope. If the property already doesn't exist it will keep looking in the outside scope, outside the object. So you can't create new properties in using `with`.
2. Creates a new lexical scope at runtime. The compiler disables optimizations.
3. With strict mode the `with` keyword is disallowed.

### IIFE 

Immediate Invoted Function Expression. Why would we use this? Scope.

```js
var foo = "foo";

(function() {

   var foo = "foo2";
   console.log(foo); // "foo2"

})();

console.log(foo); // "foo"
```

The atomic unit of scope is the function. Wraping a statement in a function gives it its own scope.

Wrapping the `function` is parens makes it an expression. Without the parens it would be a declaration.

Putting another set of parens at the end will execute it.

IIFE's are useful for function imports.

```js
(function(bar) {

})(foo);
```

Some pass in `undefined` in as a function import because in non-strict mode `undefined` is writable. 

### Block scope in ES6

The `let` keyword gives you block scope. If the compiler finds a `let` it attaches it to whatever block it's in.

```js
function foo() {
   var bar = "bar";
   for (let i=0; i<bar.length; i++) {
      console.log(bar.charAt(i));
   }
   console.log(i); // ReferenceError
}
foo(); 
```

The `i` value is only intended for the `for` loop. But in JavaScript, `var i = 0` would be attached the scope of `foo`. If we do `let i = 0` it's attached to the `for` loop.

```js
function foo(bar) {
   if (bar) {
      let baz = bar;
      if (baz) {
         let bam = baz;
      }
      console.log(bam); // Error
   }
   console.log(baz); // Error
}

foo("bar");
```

The `let` keyword will *hijack* whatever block of scope it appears in.

### Problems with the `let` keyword

The `let` keyword does not hoist. You will need to manually put your lets at the top of each block. If you declare a `let` halfway through a block it will not exist until halfway through the block.

Using `let` is implicit, it would be nicer if the block were explicit.

```js
function foo(bar) {
   /*let*/ { let baz = bar;
      console.log(baz); // "bar"
   }
   console.log(baz); // Error
}

foo("bar");
```

This is not idiomatic, but it shows how you can think about an explicit block scope.

### Dynamic Scope

```js
// theoretical dynamic scoping 
function foo() {
   console.log(bar); // dynamic!
}

function baz() {
   var bar = "bar";
   foo();
}

baz();
```

Dynamic scope will walk up the call stack to see if there's a variable available.

Dynamic scoping is a runtime decision whereas lexical scoping is an author time decision.

### Quiz Scope 

1. What kind of scoping rule(s) does JavaScirpt have? Exceptions?
 - Lexical scoping. `eval` can cheat. `with` keyword.
2. What are the different ways you can create a new scope?
 - Functions, catch blocks, curly braces with `let`.
3. What is the difference between undeclared and `undefined`?
 - `undefined` is a value, undeclared means it has not yet been registered. `undefined` means it doesn't currently have a value. 

 