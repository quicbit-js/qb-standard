# Rules changed from standard

Two rules were changed from [feross/standard](https://github.com/feross/standard): 


### Use 2 spaces for indentation

  eslint: [`indent`](http://eslint.org/docs/rules/indent)

  ```js
  function hello (name) {
    console.log('hi', name)
  }
  ```

  [qb-standard recommends 4 spaces](recommended-style.md)
  
### Use camelcase when naming variables and functions

  eslint: [`camelcase`](http://eslint.org/docs/rules/camelcase)

  ```js
    function my_function () { }    // ✗ avoid
    function myFunction () { }     // ✓ ok

    var my_var = 'hello'           // ✗ avoid
    var myVar = 'hello'            // ✓ ok
  ```

  [qb-standard recommends underscore_words for some cases](recommended-style.md)


# Rules not kept

The following rules from [feross/standard](https://github.com/feross/standard) 
were decided to be not quite impactful enough keep:

### Wrap conditional assignments with additional parentheses

  This makes it clear that the expression is intentionally an assignment (`=`) rather than a typo for equality (`===`).

  eslint: [`no-cond-assign`](http://eslint.org/docs/rules/no-cond-assign)

  ```js
  // ✓ ok
  while ((m = text.match(expr))) {
      // ...
  }

  // ✗ avoid
  while (m = text.match(expr)) {
      // ...
  }
  ```

Why not enforced?  100% line and branch coverage will almost certainly catch such mistakes. 

### Trailing commas not allowed

  eslint: [`comma-dangle`](http://eslint.org/docs/rules/comma-dangle)

  ```js
    var obj = {
        message: 'hello',   // ✗ avoid
    }
  ```

Why not enforced? 

Trailing commas don't cause a safety concern outside of IE8 and there are 
consistency and ease-of-use benefits in using them to manage vertical lists.

Here's an example of trailing commas helping reduce diff-noise:

Let's say this object:

```js
var info = {
    first_name: "Stanley",
    last_name: "Peabody",
    address: "2015 U.S. Highway Route 22, Fimblebuck, New Jersey, 03252-2355"
}
```

... is changed to this:

```js
var info = {
    first_name: "Stanley",
    last_name: "Peabody",
    address: "2015 U.S. Highway Route 22, Fimblebuck, New Jersey, 03252-2355",
    cell_phone: "377-232-0333"
}
```

The diff shows:

```
--- a/doc/t.js
+++ b/doc/t.js
@@ -1,5 +1,6 @@
 var info = {
     first_name: "Stanley",
     last_name: "Peabody",
-    address: "2015 U.S. Highway Route 22, Fimblebuck, New Jersey, 03252-2355"
+    address: "2015 U.S. Highway Route 22, Fimblebock, New Jersey, 03252-2355",
+    cell_phone: "377-232-0333"
 }
```

So cell_phone was added...

Did you notice that Fimblebuck changed spelling too?  If changing
that line was dangerous to our system, we could easily have overlooked it in 
a review.  If we had used dangling commas, the spelling change
would have been much more obvious.

So we leave use of dangling commas a author judgement call and suggest that if you
see them in someone else's file, to **not** help "clean them up".  They might be intentional.

### Commas should have a space after them.

  eslint: [`comma-spacing`](http://eslint.org/docs/rules/comma-spacing)

  ```js
  // ✓ ok
  var list = [1, 2, 3, 4]
  function greet (name, options) { ... }
  ```

  ```js
  // ✗ avoid
  var list = [1,2,3,4]
  function greet (name,options) { ... }
  ```
  
  Why not enforced?  Very long lists might be better to cram into smaller space.
  Not a safety or significant readability concern.

### For var declarations, write each declaration in its own statement.

  eslint: [`one-var`](http://eslint.org/docs/rules/one-var)

  ```js
  // ✓ ok
  var silent = true
  var verbose = true

  // ✗ avoid
  var silent = true, verbose = true

  // ✗ avoid
  var silent = true,
      verbose = true
  ```
  Why not enforced?   Not a safety or significant readability concern.

### Add spaces inside single line blocks

  eslint: [`block-spacing`](http://eslint.org/docs/rules/block-spacing)

  ```js
    function foo () {return true}    // ✗ avoid
    function foo () { return true }  // ✓ ok
  ```

  Why not enforced?  Not a safety or significant readability concern.

### No label statements

  eslint: [`no-labels`](http://eslint.org/docs/rules/no-labels)

  ```js
  label:
      while (true) {
          break label     // ✗ avoid
      }
  ```

  Why not enforced?  Some high-performance code is clearer with label use.  Inappropriate
  use is a judgement call.
  
### Do not use multiple spaces except for indentation

  eslint: [`no-multi-spaces`](http://eslint.org/docs/rules/no-multi-spaces)

  ```js
  const id =    1234    // ✗ avoid
  const id = 1234       // ✓ ok
  ```
   Why not enforced?   Not a significant readability or safety concern.  Agree it can
   be annoying when over-applied, but in some cases, alignment can help readability:
   
```js
  var padright = function (s, l) { while(s.length < l) s = s + ' '; return s }
  var padleft =  function (s, l) { while(s.length < l) s = ' ' + s; return s }
```

   Probably best to leave up to author's judgement.