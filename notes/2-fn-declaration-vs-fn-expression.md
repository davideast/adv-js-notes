<p>
  <h1 align="center">Function Expressions vs Function Declarations</h2>
  <h3 align="center">It's all about scope</h2>
</p>

## Definition

The way you know determine between a function declaration vs. a function expression is whether the word `function` is the first is the statement. If it is the first word, then it's a declaration, otherwise it's an expression.

```js
var foo = function bar() {
  var foo = "baz";

  function baz(foo) {
    foo = bar;
    foo; // function...
  }
  baz();
};

foo();
bar();
```

Functions are expressions are a lot more common than you realize.

Is line 1 a function declaration? No. Function is not the first word.

A function declaration is declared in the enclosing scope.

A function expression is enclosed within its own scope. If you look at line 12 where `bar()` is called, it will result in a reference error because `bar()` does not exist outside its own scope. The named function bar exists from lines 1-9 inside of it's self. When we try to reference bar, it will not exist and result in a reference error. 

### Named vs Unamed

The three negatives to a unamed/anonymous function expression:
1. No way to refer to ourself. No way to recurse etc...
2. Stack traces. Anonymous functions suck when debugging. When you give it a name, it's used in the stack trace.
3. Self documenting code. Give it a name "handler" it makes it a bit more understandable.

## Block scope

```js
var foo;
try {
  foo.length;
} catch (err) {
  console.log(err); // TypeError
}
console.log(err); // ReferenceError
```

As of ES3 (IE5&6 days), when `try/catch` was added to the language it was specified that the `catch` clause was block scoped. Meaning, the variable declared in the `catch` clause is only available inside and not outside.

We can reference `err` inside of the `catch` clause, but outside will give us a reference error.

 
 
