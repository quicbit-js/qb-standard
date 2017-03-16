# Recommended Style (Scored)

The following rules which are primarily selected from [feross/standard](https://github.com/feross/standard)
are *recommended*, meaning they are 
scored and reported and generally followed, but if the author feels it is important enough
she can forgo the recommendation and take 
a score penalty according to the violation.  Violation of these rules is handled 
very differently from violations of [testing](../readme.md#testing)
and [required style](required-style.md)  rules which, if broken, 
cause software to be out of qb-standard.

There is also a [separate document enumerating the 
rules that were changed or removed](styles-changed-or-removed.md) from feross/standard. 

## Rules That Differ From Standard

All but a few of these rules are in line with the 
popular [feross/standard](https://github.com/feross/standard).
popular standard.  The following is the very short list of recommended
rules that differ from [feross/standard](https://github.com/feross/standard) 
along with explanation for the difference.

* **Use 2 or 4 spaces** for indentation.

  eslint: [`indent`](http://eslint.org/docs/rules/indent)

  ```js
  function hello (name) {
      console.log('hi', name)      // ✓ ok
  }
  ```

  ```js
  function hello (name) {
    console.log('hi', name)        // ✓ ok
  }
  ```

  **Why?**  
  
  We found as we converted some libraries over to feross/standard, that 
  two spaces was great in most cases, but in some high-performance
  functions with several code levels, it made blocks harder to see:
  
      .two
      .space
      .indentation
        doesn't
        show 
        .inner 
        blocks
        .clearly
          especially
          .when
          .using 
          .certain
          characters
        .which
        .hide
        .the
        .line
  
      .but
      .four
      .space
      .indentation
          does 
          show 
          .inner 
          blocks
          .clearly
              even
              .when
              .using 
              .certain
              characters
          .which
          .hide
          .the
          .line

  While we agree that indentation should be consistent within a file, we 
  don't feel that small-indentation should be enforced universally.
  
  Note that for quicbit, we sometimes find it beneficial when our software looks
  similar across languages.  Four spaces can be a good balance between 
  Linux kernel's eight spaces requirement and js standard's two spaces - and
  it fits well with Python PEP-8, Java, and most C that we find.
  
  So we plan to release simple means to check qb-standard for compliance with
  either 2 or 4 space indentation.  
  
  **Wouldn't tabs solve some of these problems?**  Well, yes, but having adjustable 
  whitespace creates other problems. The benefit of being able to change indentation 
  per one's preference isn't worth the hassle that can come
  from different users seeing different alignment, nice though that ability is!
  
* **Use pascalCase for object functions and snake_case for stand-alone functions.**
  
  Use pascalCaseForObjectFunctions that access the 'this' 
  parameter and use_snake_case for other functions.  
  
  ```js
  class Dog {
      constructor (breed) {
          this.breed = breed
          //...
      }
      chaseTail (radians) {
          //...
      }
      fetchStick (stick) {
          let dist = object_distance(this, stick) 
          // ...
      }
  }

  
  function object_distance(obj1, obj2) {
      // ...  
  }

  module.exports = {
      object_distance,
      create_dog: (breed) => { return new Dog(breed) }
  }
  ```
  Why? Using snake_case helps with portability to C and Python.
  Using pascalCase for classes helps with portability to Java
  and is consistent with popular javascript.
  
  There is also a readability benefit from being able to 
  quickly distinguish stand-alone functions from 
  object functions.
  
  Again, tooling for this is a bit behind, but we feel this is a 
  good standard to make code across several languages more similar,
  or at least less unnecessarily dissimilar.
  
* **Use snake_case for properties and parameters** that are 
    seen outside of code.

  ```js
  // public settings exposed in the world
  let settings = {
      valueOne: 1,      // ✗ avoid
      value_two: 2      // ✓ ok
  }
  ```
  
  This recommendation includes function object properties, parameters in APIs, 
  restful JSON interfaces and so on.
  
  **Why?**
  pascalCaseProperties and parameters work well
  as symbols in code but
  go awry when they go forth into the world in public settings, 
  JSON, urls, file names, etc.
  Symbols in pascalCase can't be easily indexed or split or managed
  in any way other than as lump-identities whose case must be 
  maintained.  In contrast, snake_case
  work effectively in other contexts and whether case information 
  is present or not.  
    
* **Use either pascalCase or snake_case for local parameters
    and variables** a.k.a.
  "the awareness and sensitivity standard"
  
  pascalCase and snake_case are both legitimate styles for
  local parameters and variables.
  
  As a software developer, one should feel capable of
  working with either and both these formats without blinking (or scoffing).
  
  ```js
  function launch(client_name, port) {        // ✓ ok
      let a_value = 0                         // ✓ ok
      //...
  }

  // in another file...
  function process(clientName, port) {        // ✓ ok
      let aValue = 0                          // ✓ ok
  }
  ```

  qb-standard *recommends* that whichever naming approach is used be kept 
  consistent throughout a module or across team code.  Also consider
  creating and using a
  [variable-glossary](variable-glossary.md)
  to improve consistency across modules and libraries.

  **Why leave this item open?**  Won't developers quarrel over it?
  
  We certainly hope not.  We feel that acknowledging which standards are unimportant
  is itself important.  While we recognize the importance of 
  standards that govern names of
  functions, prototypes, modules, and constants, we feel that 
  standards governing the choice of local variable names of very little value.

  At the same time, we recognize that some developers will find a small
  amount of satisfaction choosing local variables that works best for 
  them.  Our primary objective is *not* to make all code look exactly the same, but to 
  enforce standards that make a real difference in productivity.
  
  Can you be aware of and sensitive to the local 
  variable naming style of your fellow programmers when working in 
  a module they created?  We leave this local variable naming flexible to acknowledge 
  that ability to accept and blend in with very slightly different styles
  is as important as any rule you may find in the standard.

## Recommendations that match  [feross/standard](https://github.com/feross/standard)

These recommendations are in line with [feross/standard](https://github.com/feross/standard)
but in contrast to that standard and in contrast to qb-standard [required styles](required-style.md), 
these rules may be broken
for a format score penalty without completely breaking the standard.  IOW, 
a high level of compliance is expected, but 100% compliance is required.

* **Use single quotes for strings** except to avoid escaping.

  eslint: [`quotes`](http://eslint.org/docs/rules/quotes)

  ```js
  console.log('hello there')
  $("<div class='box'>")
  ```

* **Add a space after keywords.**

  eslint: [`keyword-spacing`](http://eslint.org/docs/rules/keyword-spacing)

  ```js
  if (condition) { ... }   // ✓ ok
  if(condition) { ... }    // ✗ avoid
  ```

* **Add a space before a function declaration's parentheses.**

  eslint: [`space-before-function-paren`](http://eslint.org/docs/rules/space-before-function-paren)

  ```js
  function name (arg) { ... }   // ✓ ok
  function name(arg) { ... }    // ✗ avoid

  run(function () { ... })      // ✓ ok
  run(function() { ... })       // ✗ avoid
  ```

* **Infix operators** should be spaced.

  eslint: [`space-infix-ops`](http://eslint.org/docs/rules/space-infix-ops)

  ```js
  // ✓ ok
  var x = 2
  var message = 'hello, ' + name + '!'
  ```

  ```js
  // ✗ avoid
  var x=2
  var message = 'hello, '+name+'!'
  ```

* **Keep else statements** on the same line as their curly braces.

  eslint: [`brace-style`](http://eslint.org/docs/rules/brace-style)

  ```js
  // ✓ ok
  if (condition) {
      // ...
  } else {
      // ...
  }
  ```

  ```js
  // ✗ avoid
  if (condition) {
      // ...
  }
  else {
      // ...
  }
  ```

* **For multi-line if statements,** use curly braces.

  eslint: [`curly`](http://eslint.org/docs/rules/curly)

  ```js
  // ✓ ok
  if (options.quiet !== true) console.log('done')
  ```

  ```js
  // ✓ ok
  if (options.quiet !== true) {
      console.log('done')
  }
  ```

  ```js
  // ✗ avoid
  if (options.quiet !== true)
      console.log('done')
  ```

* **Avoid multiple blank lines.**

  eslint: [`no-multiple-empty-lines`](http://eslint.org/docs/rules/no-multiple-empty-lines)

  ```js
  // ✓ ok
  var value = 'hello world'
  console.log(value)
  ```

  ```js
  // ✗ avoid
  var value = 'hello world'


  console.log(value)
  ```

* **For the ternary operator** in a multi-line setting, place `?` and `:` on their own lines.

  eslint: [`operator-linebreak`](http://eslint.org/docs/rules/operator-linebreak)

  ```js
  // ✓ ok
  var location = env.development ? 'localhost' : 'www.api.com'

  // ✓ ok
  var location = env.development
      ? 'localhost'
      : 'www.api.com'

  // ✗ avoid
  var location = env.development ?
      'localhost' :
      'www.api.com'
  ```

* **Commas must be placed at the end of the current line.**

  eslint: [`comma-style`](http://eslint.org/docs/rules/comma-style)

  ```js
    var obj = {
        foo: 'foo'
        ,bar: 'bar'   // ✗ avoid
    }

    var obj = {
        foo: 'foo',
        bar: 'bar'   // ✓ ok
    }
  ```

* **Dot should be on the same line as property.**

  eslint: [`dot-location`](http://eslint.org/docs/rules/dot-location)

  ```js
      console.
          log('hello')  // ✗ avoid
      
      console
          .log('hello') // ✓ ok
  ```

* **No space between function identifiers and their invocations.**

  eslint: [`func-call-spacing`](http://eslint.org/docs/rules/func-call-spacing)

  ```js
  console.log ('hello') // ✗ avoid
  console.log('hello')  // ✓ ok
  ```

* **Add space between colon and value in key value pairs.**

  eslint: [`key-spacing`](http://eslint.org/docs/rules/key-spacing)

  ```js
  var obj = { 'key' : 'value' }    // ✗ avoid
  var obj = { 'key' :'value' }     // ✗ avoid
  var obj = { 'key':'value' }      // ✗ avoid
  var obj = { 'key': 'value' }     // ✓ ok
  ```

* **Constructors of derived classes should call `super`.**

  eslint: [`constructor-super`](http://eslint.org/docs/rules/constructor-super)

  ```js
  class Dog {
      constructor () {
          super()   // ✗ avoid
      }
  }

  class Dog extends Mammal {
      constructor () {
          super()   // ✓ ok
      }
  }
  ```

* **Use a single import statement per module.**

  eslint: [`no-duplicate-imports`](http://eslint.org/docs/rules/no-duplicate-imports)

  ```js
  import { myFunc1 } from 'module'
  import { myFunc2 } from 'module'          // ✗ avoid

  import { myFunc1, myFunc2 } from 'module' // ✓ ok
  ```

* **No unnecessary parentheses around function expressions.**

  eslint: [`no-extra-parens`](http://eslint.org/docs/rules/no-extra-parens)

  ```js
  const myFunc = (function () { })   // ✗ avoid
  const myFunc = function () { }     // ✓ ok
  ```

* **Use `break` to prevent fallthrough in `switch` cases.**

  eslint: [`no-fallthrough`](http://eslint.org/docs/rules/no-fallthrough)

  ```js
  switch (filter) {
      case 1:
          doSomething()    // ✗ avoid
      case 2:
          doSomethingElse()
  }

  switch (filter) {
      case 1:
          doSomething()
          break           // ✓ ok
      case 2:
          doSomethingElse()
  }

  switch (filter) {
      case 1:
          doSomething()
          // fallthrough  // ✓ ok
      case 2:
          doSomethingElse()
  }
  ```

* **No floating decimals.**

  eslint: [`no-floating-decimal`](http://eslint.org/docs/rules/no-floating-decimal)

  ```js
  const discount = .5      // ✗ avoid
  const discount = 0.5     // ✓ ok
  ```

* **No multiline strings.**

  eslint: [`no-multi-str`](http://eslint.org/docs/rules/no-multi-str)

  ```js
  const message = 'Hello \
                   world'     // ✗ avoid
  ```

* **Avoid multiple spaces in regular expression literals.**

  eslint: [`no-regex-spaces`](http://eslint.org/docs/rules/no-regex-spaces)

  ```js
  const regexp = /test   value/   // ✗ avoid

  const regexp = /test {3}value/  // ✓ ok
  const regexp = /test value/     // ✓ ok
  ```

* **Sparse arrays are not allowed.**

  eslint: [`no-sparse-arrays`](http://eslint.org/docs/rules/no-sparse-arrays)

  ```js
  let fruits = ['apple',, 'orange']       // ✗ avoid
  ```

* **Maintain consistency of newlines between object properties.**

  eslint: [`object-property-newline`](http://eslint.org/docs/rules/object-property-newline)

  ```js
  const user = {
      name: 'Jane Doe', age: 30,
      username: 'jdoe86'            // ✗ avoid
  }

  const user = { name: 'Jane Doe', age: 30, username: 'jdoe86' }    // ✓ ok

  const user = {
      name: 'Jane Doe',
      age: 30,
      username: 'jdoe86'
  }                                                                 // ✓ ok
  ```

* **No padding within blocks.**

  eslint: [`padded-blocks`](http://eslint.org/docs/rules/padded-blocks)

  ```js
  if (user) {
                              // ✗ avoid
      const name = getName()

  }

  if (user) {
      const name = getName()    // ✓ ok
  }
  ```

* **Semicolons must have a space after and no space before.**

  eslint: [`semi-spacing`](http://eslint.org/docs/rules/semi-spacing)

  ```js
  for (let i = 0 ;i < items.length ;i++) {...}    // ✗ avoid
  for (let i = 0; i < items.length; i++) {...}    // ✓ ok
  ```

* **Must have a space before blocks.**

  eslint: [`space-before-blocks`](http://eslint.org/docs/rules/space-before-blocks)

  ```js
  if (admin){...}     // ✗ avoid
  if (admin) {...}    // ✓ ok
  ```

* **No spaces inside parentheses.**

  eslint: [`space-in-parens`](http://eslint.org/docs/rules/space-in-parens)

  ```js
  getName( name )     // ✗ avoid
  getName(name)       // ✓ ok
  ```

* **Use spaces inside comments.**

  eslint: [`spaced-comment`](http://eslint.org/docs/rules/spaced-comment)

  ```js
  //comment           // ✗ avoid
  // comment          // ✓ ok

  /*comment*/         // ✗ avoid
  /* comment */       // ✓ ok
  ```

* **The `*` in `yield*`expressions must have a space before and after.**

  eslint: [`yield-star-spacing`](http://eslint.org/docs/rules/yield-star-spacing)

  ```js
  yield* increment()    // ✗ avoid
  yield * increment()   // ✓ ok
  ```

* **Avoid Yoda conditions.**

  eslint: [`yoda`](http://eslint.org/docs/rules/yoda)

  ```js
  if (42 === age) { }    // ✗ avoid
  if (age === 42) { }    // ✓ ok
  ```

## Semicolons

* No semicolons. (see: [1](http://blog.izs.me/post/2353458699/an-open-letter-to-javascript-leaders-regarding), [2](http://inimino.org/%7Einimino/blog/javascript_semicolons), [3](https://www.youtube.com/watch?v=gsfbh17Ax9I))

  eslint: [`semi`](http://eslint.org/docs/rules/semi)

  ```js
  window.alert('hi')   // ✓ ok
  window.alert('hi');  // ✗ avoid
  ```

* Never start a line with `(`, `[`, or `` ` ``. This is the only gotcha with omitting semicolons, and standard protects you from this potential issue.

  eslint: [`no-unexpected-multiline`](http://eslint.org/docs/rules/no-unexpected-multiline)

  ```js
  // ✓ ok
  ;(function () {
      window.alert('ok')
  }())

  // ✗ avoid
  (function () {
      window.alert('ok')
  }())
  ```

  ```js
  // ✓ ok
  ;[1, 2, 3].forEach(bar)

  // ✗ avoid
  [1, 2, 3].forEach(bar)
  ```

  ```js
  // ✓ ok
  ;`hello`.indexOf('o')

  // ✗ avoid
  `hello`.indexOf('o')
  ```

  Note: If you're often writing code like this, you may be trying to be too clever.

  Clever short-hands are discouraged, in favor of clear and readable expressions, whenever
  possible.

  Instead of this:

  ```js
  ;[1, 2, 3].forEach(bar)
  ```

  This is strongly preferred:

  ```js
  var nums = [1, 2, 3]
  nums.forEach(bar)
  ```


## Helpful reading

- [An Open Letter to JavaScript Leaders Regarding Semicolons][1]
- [JavaScript Semicolon Insertion – Everything you need to know][2]

##### And a helpful video:

- [Are Semicolons Necessary in JavaScript? - YouTube][3]

All popular code minifiers in use today use AST-based minification, so they can
handle semicolon-less JavaScript with no issues (since semicolons are not required
in JavaScript).

##### Excerpt from *["An Open Letter to JavaScript Leaders Regarding Semicolons"][1]*:

> [Relying on automatic semicolon insertion] is quite safe, and perfectly valid JS that every browser understands. Closure compiler, yuicompressor, packer, and jsmin all can properly minify it. There is no performance impact anywhere.
>
> I am sorry that, instead of educating you, the leaders in this language community have given you lies and fear.  That was shameful. I recommend learning how statements in JS are actually terminated (and in which cases they are not terminated), so that you can write code that you find beautiful.
>
> In general, `\n` ends a statement unless:
>   1. The statement has an unclosed paren, array literal, or object literal or ends in some
>      other way that is not a valid way to end a statement. (For instance, ending with `.`
>      or `,`.)
>   2. The line is `--` or `++` (in which case it will decrement/increment the next token.)
>   3. It is a `for()`, `while()`, `do`, `if()`, or `else`, and there is no `{`
>   4. The next line starts with `[`, `(`, `+`, `*`, `/`, `-`, `,`, `.`, or some other
>      binary operator that can only be found between two tokens in a single expression.
>
> The first is pretty obvious. Even JSLint is ok with `\n` chars in JSON and parenthesized constructs, and with `var` statements that span multiple lines ending in `,`.
>
> The second is super weird. I’ve never seen a case (outside of these sorts of conversations) where you’d want to do write `i\n++\nj`, but, point of fact, that’s parsed as `i; ++j`, not `i++; j`.
>
> The third is well understood, if generally despised. `if (x)\ny()` is equivalent to `if (x) { y() }`. The construct doesn’t end until it reaches either a block, or a statement.
>
> `;` is a valid JavaScript statement, so `if(x);` is equivalent to `if(x){}` or, “If x, do nothing.” This is more commonly applied to loops where the loop check also is the update function. Unusual, but not unheard of.
>
> The fourth is generally the fud-inducing “oh noes, you need semicolons!” case. But, as it turns out, it’s quite easy to *prefix* those lines with semicolons if you don’t mean them to be continuations of the previous line. For example, instead of this:
>
> ```js
> foo();
> [1,2,3].forEach(bar);
> ```
>
> you could do this:
>
> ```js
> foo()
> ;[1,2,3].forEach(bar)
> ```
>
> The advantage is that the prefixes are easier to notice, once you are accustomed to never seeing lines starting with `(` or `[` without semis.

[1]: http://blog.izs.me/post/2353458699/an-open-letter-to-javascript-leaders-regarding
[2]: http://inimino.org/~inimino/blog/javascript_semicolons
[3]: https://www.youtube.com/watch?v=gsfbh17Ax9I

