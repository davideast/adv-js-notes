<p>
  <h1 align="center">Scope and the JavaScript Compiler</h2>
  <h3 align="center">Think more like the JavaScript compiler engine</h2>
</p>

## Scope
Briefly put, scope is *"where to look for things"*. But this brings up two questions. Who is doing the *looking*, and what are they looking *for*?

### Who
The compiler is the one doing the *looking*. The compiler looks for **blocks of scope**. The smallest atomic unit of scope that JavaScript has is the *function* (except for ES2015 `let` for block scope).

### Be the compiler
Finding declarations of **variables** and **functions**, and putting them in their appropriate scope slots.

The code below will be passed once through by the compiler, first, and then it will get a second pass through where it is executed.

```js
var foo = "bar";

function bar() {
  var foo = "baz";
}

function baz(foo) {
  foo = "bam";
  bam = "yay";
}
``` 

### Compiler pass #1

#### Line 1
```js
var foo = "bar";
```

You might think of this line as one operation, but it's actually two.

1. Declaration - `var foo`
2. Initialization - `foo = "bar"`

These operations happen at different times.

The compiler will see this line and say, *"I see a variable declaration for `foo`, which scope am I in?"* The current answer is that we are in the **global scope**. The compiler will register the `foo` indentifier into the **global scope**.

#### Line 3

```js
function bar() {
```
The compiler sees a function with an identifier of `bar`. This identifier is assigned the value of a function, which is different than variables whose values are assigned later. The compiler asks *"Which scope am I in?"* And then registers the function `bar` into the **global scope**.

Now that the compiler sees that this a function, it will recursively decend into that function and compile the contents.

#### Line 4-6
```js
  var foo = "baz";
}
```
The compiler will find a variable declaration for `foo`. *What scope am I in?* In the scope of `bar`. *Do I have any more identifiers?* No, let's move on.

### Line 7
```js
function baz(foo) {
```
The compiler will find a function declaration for `baz`. *What scope am I in?* **Global scope**.

Now this part is tricky, because we have a named parameter for `foo`. This named parameter `foo` will be treated like a local variable to the function. *What scope am I in?* `baz` scope.

Now the compiler asks, *Do we see any more declarations in this function?"* Which the answer is no. The rest are variable **assignments** not declarations.

At this point the initial pass of the compiler has finished. We are done *"compiling"* our program.

### Compiler pass #2
Microseconds pass, and the code is now being excuted somewhere else in the program.

### Line 1
```js
var foo = "bar";
```
The variable declaration was handled in pass one. This means there's no more `var` to handle anymore. What's left is the assignment of the value, `foo = "bar"`.



------
## Brain Dump (organize later)

**MYTH**: "JavaScript is not a compiled language."

**FACT**: "JavaScript is a compiled language. It's not compiled like how C++ or Java is compiled. The difference is that we do not distribute a binary compiled form of the program. C++ compiles to an .exe, and Java compiles to bytecode than runs on the JVM. **JavaScript is compiled every single time it is run.**"

### Compiled language vs. Interpreted Language
Bash is an interpreted language, which means when it is running line 3, it doesn't know what line 4 is. Interpreter runs from top to bottom.

A compiler does an initial run through the code to compile it, and then it does at least one more pass through the code, and on this final pass is where it does the execution. A compiled language has looked at line 4 before it starts to run line 3.

### JIT Compiler
A JIT Compiler will do a scan through the program and if it doesn't notice the use of a function it won't compile the contents. It will only compile the function if it's asked to be executed. Defer the compilation, and compile it *"just in time"*.

### LHS vs RHS
LHS stands for **Left Hand side** and RHS stands for **Right Hand Side**. The LHS is the target and the RHS is the source. This comes from looking at the `=` operator.

```js
var cash = "money";
```

In the sample above `cash` is the LHS target, and the value `"money"` is the source.