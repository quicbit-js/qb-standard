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

Why?  100% line and branch coverage will almost certainly catch such mistakes. 

### Trailing commas not allowed

  eslint: [`comma-dangle`](http://eslint.org/docs/rules/comma-dangle)

  ```js
    var obj = {
        message: 'hello',   // ✗ avoid
    }
  ```

Trailing commas don't cause a safety concern.  There is even a real benefit
to having them in data driven testing.  For example:

When there is an issue with chinese characters, move the chinese test(s) to
the top of the table to assert them first... without deleting the 
comma from the new bottom table row <code>[ '"abc"%', ...</code> and then adding
a comma to the row.  With 100's of tests to manage, the dangling comma is 
actually helpful.

```js
test('utf8', function(t) {
    t.tableAssert(
        [
            [ 's',                   'exp'                                  ],
            [ '',                    []                                     ],
            [ 'a',                   [0x61]                                 ],
            [ 'abc\uD801\uDC00',     [0x61,0x62,0x63,0xF0,0x90,0x90,0x80]   ],
            [ '"abc"%',              [ 34,97,98,99,34,37 ]                  ],
            [ '在嚴寒的冬日裡',        [229,156,168,229,154,180,229,175,146,231,154,132,229,134,172,230,151,165,232,163,161] ],
        ],
        utf8_from_str
    )
})
```

... and they don't cause diff-noise when moved, so make change management cleaner:

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
  
  Why?  Very long lists might be better to cram into smaller space.
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
  Why?   Not a safety or significant readability concern.

### Add spaces inside single line blocks

  eslint: [`block-spacing`](http://eslint.org/docs/rules/block-spacing)

  ```js
    function foo () {return true}    // ✗ avoid
    function foo () { return true }  // ✓ ok
  ```

  Why?   Not a safety or significant readability concern.

### No label statements

  eslint: [`no-labels`](http://eslint.org/docs/rules/no-labels)

  ```js
  label:
      while (true) {
          break label     // ✗ avoid
      }
  ```

  Some high-performance code is clearer with label use.  Inappropriate
  use is a judgement call.
  
### Do not use multiple spaces except for indentation

  eslint: [`no-multi-spaces`](http://eslint.org/docs/rules/no-multi-spaces)

  ```js
  const id =    1234    // ✗ avoid
  const id = 1234       // ✓ ok
  ```
   Why?   Not a significant readability or safety concern.  Agree it can
   be annoying when over-applied, but in some cases, alignment can help readability:
   
```js
  var padright = function (s, l) { while(s.length < l) s = s + ' '; return s }
  var padleft =  function (s, l) { while(s.length < l) s = ' ' + s; return s }
```

   Probably best to leave up to author's judgement.