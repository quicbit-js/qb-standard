# Variable Glossary

## Why have a glossary of parameter and variable names?

A glossary of the most common 
variable and parameter names can reduce time to understand function
signatures and implementations.  
Using the glossary isn't mandatory, it's just
a tool to consolidate terms across a large number of 
modules and functions.

Programmers well-versed in OO methodology might suggest replacing 
parameter conventions like **src**, **off**, and **lim** with objects
that capture these concepts in methods.  But wrappers introduce the
burden of wrapping and unwrapping as well as a burden on the reader 
to learn non-standard and non-trivial types.  Arrays, strings and
plain objects are preferred for their simplicity,
transparency and ubiquity.

## Why so curt?

We certainly don't advocate stripping vowels from
functions, globals, constants and prototype names.  But common parameters and local
variables with small scope are different.  Much like logicians prefer 
[symbols](https://en.wikipedia.org/wiki/List_of_logic_symbols) to speed comprehension,
software programmers can prefer terser expressions to make algorithms 
more clear.

For example, here are two versions of a function that returns ranges of
illegal utf8 bytes found in an input array:

```js

const LEFT_BIT_MASK = 0x80
const LEFT_2_BIT_MASK = 0xC0

module.exports = function (inputArray, fromIndex, upToIndex) {
    upToIndex = upToIndex == null ? inputArray.length : upToIndex
    let index = fromIndex || 0
    if (index >= upToIndex) { return [] }

    let returnValue = []
    while (index < upToIndex) {
        // skip ascii
        while (inputArray[index] < LEFT_BIT_MASK) {
            if (++index === upToIndex) return returnValue
        }

        // non-ascii (2 or more bytes)
        let startIndex = index
        let nextCharacter = inputArray[index++]
        if (nextCharacter >= LEFT_2_BIT_MASK) {
            // trailing bytes

            // count sequential 1's: 11xxxxxx
            for (let numberOfBytes = 2; (nextCharacter << numberOfBytes) & LEFT_BIT_MASK; ) {
                numberOfBytes++
            }
            // skip trailing bytes
            while (index < upToIndex && (inputArray[index] & LEFT_2_BIT_MASK) === LEFT_BIT_MASK) {
                index++
            }
            if (index - startIndex !== numberOfBytes) {
                if (index - startIndex > numberOfBytes) {
                    // excess trailing bytes
                    returnValue.push([startIndex + numberOfBytes, index])
                } else {
                    // not enough trailing bytes
                    returnValue.push([startIndex, index])
                }
            }
        } else {
            // unexpected trailing byte

            while (index < upToIndex && (inputArray[index] & LEFT_2_BIT_MASK) === LEFT_BIT_MASK) {
                index++
            }
            // skip all trailing bytes
            returnValue.push([startIndex, index])
        }
    }
    return returnValue
}

```

And here it is again using the glossary and shorter variable names:

```js

function illegal_utf8 (src, off, lim) {
    lim = lim == null ? src.length : lim
    let i = off || 0
    if (i >= lim) { return [] }

    let ret = []
    while (i < lim) {
        while (src[i] < 0x80) { if (++i === lim) return ret }       // skip ascii

        // non-ascii (2 or more bytes)
        let start = i
        let c = src[i++]
        if (c >= 0xC0) {                                            
            // has trailing bytes        
            for (let n = 2; (c << n) & 0x80; n++) {}                // count sequential 1's: 11xxxxxx
            while (i < lim && (src[i] & 0xC0) === 0x80) { i++ }     // skip trailing bytes
            if (i - start !== n) {                                  
                if (i - start > n) {                                
                    ret.push([start + n, i])                        // excess trailing bytes
                } else {                                            
                    ret.push([start, i])                            // not enough trailing bytes
                }                                                       
            }
        } else {                                                    
            // unexpected trailing byte        
            while (i < lim && (src[i] & 0xC0) === 0x80) { i++ }
            ret.push([start, i])                                    // skip trailing bytes
        }
    }
    return ret
}
```

While long parameters and variable names in the first example might provide 
an easy superficial read, the actual logic is clearer in 
the short version.  Those who are reading the code need to know the logic.  Those
want to peruse function signatures are usually better off going through a readme and
examples.

Also notice how parameters like **src** and **lim** are plugged directly
into the algorithm several times.  The parameter name length affects conciseness
as well.  Alternatively we could reassign parameters to shorter names at the beginning
of the function, 
but gratuitous levels of reassignment are another sort of badness.  Direct
is good.  Short is good.  Consistency (e.g. with help of glossary) can help us 
against the obscurity of these short names and the more we practice it the easier it gets.

In this example, there is also a good chance that a reader familiar with other
quicbit functions will quickly recognize
that function takes an array-like 
source ([src](https://github.com/quicbit-js/qb-standard/blob/master/variable-glossary.md#src-source)) 
with an inclusive lower-bound 
offset ([off](https://github.com/quicbit-js/qb-standard/blob/master/variable-glossary.md#off-offset)) and 
exclusive upper-bound limit ([lim](https://github.com/quicbit-js/qb-standard/blob/master/variable-glossary.md#lim-limit)).

**Bitmask literals?**  Isn't that heresy?!!  Not to us.  *The most important aspects
of this function is that it is clearly written, has 100% branch and line test coverage, 
is very tiny with no library dependencies, runs fast and is
documented with clear examples in the readme file*.  Those things are important.

## Arrays

Why all the focus on arrays and indexes?  
What about filter(), map(), reduce(), forEach()...?   

Array methods are fine when we aren't iterating over large byte arrays, 
but when we do work with large data arrays, traditional iteration can
be far faster.

### src (source)

Array-like object from which data can be read using bracket notation.  
A quicbit src typically contains bytes (integers in range 0 to 255). 

```js
function scan (src, off, lim) {
    for (let i = 0; i < lim; i++) {
        let byte = src[i]
        // ...
    }
}
```

### dst (destination)

Array-like object to which data can be read using bracket notation.  
A quicbit dst typically contains bytes (integers in range 0 to 255). 

```js
function repeat_byte (b, dst, off, lim) {
    for (let i = off; i < lim; i++) {
        dst[off] = b
    }
    // ...
}
```

### off (offset)

Offset into a [src](https://github.com/quicbit-js/qb-standard/blob/master/variable-glossary.md#src-source) or [dst]() array-like object to start reading or
writing data.

```js
function scan (src, off, lim) {
    for (let i = 0; i < lim; i++) {
        let byte = src[i]
        // ...
    }
}
```

### lim (limit)

The offset into a [src](https://github.com/quicbit-js/qb-standard/blob/master/variable-glossary.md#src-source) or [dst]() *before which* we stop reading or writing data.

```js
function copy (src, off, lim, dst) {
    //...
}
```

### len (length)

The number of bytes to process in a [src](https://github.com/quicbit-js/qb-standard/blob/master/variable-glossary.md#src-source) or [dst]() array.

```js
function scan (src, off, len) {
    for (let i = 0; i < off + len; i++) {
        let byte = src[i]
        // ...
    }
}
```

So which is it, copy(src, off, **lim**) or copy(src, off, **len**)?

When working with a stacks of calls that operate on array ranges, we usually find
functions with [lim](https://github.com/quicbit-js/qb-standard/blob/master/variable-glossary.md#lim-limit) 
to be more direct than [len](https://github.com/quicbit-js/qb-standard/blob/master/variable-glossary.md#len-length).  On the other hand, 
functions with the [len](https://github.com/quicbit-js/qb-standard/blob/master/variable-glossary.md#len-length) signature can be simpler when 
not already managing arrays and offsets.

### i, si, di... (array indexes)

Usually we use 'i' or, where there are multiple loops, two initials ending with 'i'.

```js
function repeat (s, dst, off, lim) {
    let scodes = s.map((c) => c.charCodeAt(0))
    let slen = s.length
    
    for (let di = off; di < lim; di++) {
        // in actual implementation, we might handle bounds etc...
        for (let si = 0; si < slen; si++) {
            dst[di + si] = scodes[si]
        }
    }
    // ...
}
```

## General

### ret (return value)

The value that is intended to be returned, ()or converted-and-returned 
in a final step) is often called 'ret'.

### v (value)

A common choice for an input value that may be a byte or integer or string...

```js
function repeat (v, dst, off, lim) {
    // here v can be provided as a string or byte
    if(v === typeof 'string') v = v.charCodeAt(0)
    
    for (let i = off; i < lim; i++) {
        dst[i] = b
    }
    // ...
}
```

### s or str (string)

For short processing functions which clearly take a string, we often use 's'.

```javascript
function padright (s, l) { while(s.length < l) s = s + ' '; return s }
function padleft  (s, l) { while(s.length < l) s = ' ' + s; return s }
```

When 's' seems a too short, 'str' is an alternative.


