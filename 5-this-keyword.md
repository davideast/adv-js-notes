<p>
  <h1 align="center">this</h2>
  <h3 align="center">What does the call site look like?</h2>
</p>

Every function, **while executing**, has a reference to its current execution context, called **this**.

Remeber lexical scope vs. dynamic scope? JavaScript's version of "dynamic scope" is **this**.

### Four rules of how `this` gets bound

Each rule depends on the **call site**. The **call site** is the place in code where the function gets executed, `foo()`. The `this` keyword is not always a "self" reference or an instance reference.

The only thing that matters is what the call site looks like.

#### 4 - default binding rule

```js
function foo() {
   console.log(this.bar);
}

var bar = "bar1";
var o2 = { bar: "bar2", foo: foo };
var o3 = { bar: "bar3", foo: foo };

foo();      // "bar1"
o2.foo();   // "bar2"
o3.foo();   // "bar3"
```

If you're in strict mode, default the `this` keyword to the undefined value. If you're not in strict mode, default the `this` keyword to the global object.

#### 3 - 
 