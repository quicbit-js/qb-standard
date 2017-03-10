# Changed from standard

Two rules were changed from [feross/standard](https://github.com/feross/standard) :

* **Use 2 spaces** for indentation.

  eslint: [`indent`](http://eslint.org/docs/rules/indent)

  ```js
  function hello (name) {
    console.log('hi', name)
  }
  ```

  [qb-standard recommends 4 spaces](https://github.com/quicbit-js/qb-standard/blob/master/recommended-style.md)
  
* **Use camelcase when naming variables and functions.**

  eslint: [`camelcase`](http://eslint.org/docs/rules/camelcase)

  ```js
    function my_function () { }    // ✗ avoid
    function myFunction () { }     // ✓ ok

    var my_var = 'hello'           // ✗ avoid
    var myVar = 'hello'            // ✓ ok
  ```

  [qb-standard recommends underscore_words for some cases](https://github.com/quicbit-js/qb-standard/blob/master/recommended-style.md)


# Dropped from standard

The following rules from [feross/standard](https://github.com/feross/standard) 
were dropped

* **Wrap conditional assignments** with additional parentheses. This makes it clear that the expression is intentionally an assignment (`=`) rather than a typo for equality (`===`).

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

* **Trailing commas not allowed.**

  eslint: [`comma-dangle`](http://eslint.org/docs/rules/comma-dangle)

  ```js
    var obj = {
        message: 'hello',   // ✗ avoid
    }
  ```

* **Commas should have a space** after them.

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

* **For var declarations,** write each declaration in its own statement.

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

* **Add spaces inside single line blocks.**

  eslint: [`block-spacing`](http://eslint.org/docs/rules/block-spacing)

  ```js
    function foo () {return true}    // ✗ avoid
    function foo () { return true }  // ✓ ok
  ```

* **No label statements.**

  eslint: [`no-labels`](http://eslint.org/docs/rules/no-labels)

  ```js
  label:
      while (true) {
          break label     // ✗ avoid
      }
  ```

* **Do not use multiple spaces except for indentation.**

  eslint: [`no-multi-spaces`](http://eslint.org/docs/rules/no-multi-spaces)

  ```js
  const id =    1234    // ✗ avoid
  const id = 1234       // ✓ ok
  ```
