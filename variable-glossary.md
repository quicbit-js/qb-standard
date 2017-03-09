# Parameter Glossary

## Why have a glossary of parameter names?

With the creation of many small modules comes a challenge - how do we
keep consistency among them?  A simple glossary of the most common 
parameter names can keep code concise and speed up understanding of
function signatures.  Using the glossary certainly isn't mandatory, it's just
a tool to consolidate terms across a large number of module functions.

Classically trained OO programmers sometimes suggest replacing the glossary
with objects that capture the glossary of terms in methods.  But doing 
this adds a layer of obscurity and creates a burden on readers 
to learn non-trivial and non-standard types.  Arrays, strings 
and plain objects are transparent and well-understood and so preferred when 
they can do the job.

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
const LEFT_2BIT_MASK = 0xC0

module.exports = function (inputArray, fromIndex, upToIndex) {
    upToIndex = upToIndex == null ? inputArray.length : upToIndex
    var index = fromIndex || 0
    if (index >= upToIndex) { return [] }

    var returnValue = []
    while (index < upToIndex) {
        // skip ascii
        while (inputArray[index] < LEFT_BIT_MASK) {
            if (++index === upToIndex) return returnValue
        }

        // non-ascii (2 or more bytes)
        var start = index
        var nextCharacter = inputArray[index++]
        if (nextCharacter >= LEFT_2BIT_MASK) {
            // trailing bytes

            // count sequential 1's: 11xxxxxx
            for (var numberOfBytes = 2; (nextCharacter << numberOfBytes) & LEFT_BIT_MASK; ) {
                numberOfBytes++
            }
            // skip trailing bytes
            while (index < upToIndex && (inputArray[index] & LEFT_2BIT_MASK) === LEFT_BIT_MASK) {
                index++
            }
            if (index - start !== numberOfBytes) {
                if (index - start > numberOfBytes) {
                    // excess trailing bytes
                    returnValue.push([start + numberOfBytes, index])
                } else {
                    // not enough trailing bytes
                    returnValue.push([start, index])
                }
            }
        } else {
            // unexpected trailing byte
            
            while (index < upToIndex && (inputArray[index] & LEFT_2BIT_MASK) === LEFT_BIT_MASK) {
                index++
            }
            // skip all trailing bytes
            returnValue.push([start, index])
        }
    }
    return returnValue
}

```

And here it is again using the glossary and shorter variable names:

```js

function illegal_utf8 (src, off, lim) {
    lim = lim == null ? src.length : lim
    var i = off || 0
    if (i >= lim) { return [] }

    var ret = []
    while (i < lim) {
        while (src[i] < 0x80) { if (++i === lim) return ret }   // skip ascii

        // non-ascii (2 or more bytes)
        var start = i
        var c = src[i++]
        if (c >= 0xC0) {                                            
            // has trailing bytes        
            for (var n = 2; (c << n) & 0x80; n++) {}                // count sequential 1's: 11xxxxxx
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
            ret.push([start, i])                                    // skip all trailing bytes
        }
    }
    return ret
}

```

While the parameter names might be nicer to read for a newbie in the first 
example, understanding the actual logic and making improvements 
is far easier in the shorter version.  qb-standard is more concerned with
those who need to know the logic, not those casually perusing method signatures -
that's what the readme is for.

Someone who has worked with Quicbit APIs in the past will very quickly recognize
the function signature as a common way of passing [source]() array with an
[offset]() and [limit]().

**Bitmask literals?**  Isn't that heresy?!!  Not to us.  *The most important aspects
of this function is that it is clearly written, has 100% branch and line test coverage, 
is very tiny with no library dependencies, runs fast and is
documented with clear examples in the readme file*.  Those things are important.

## Arrays

### src (source)

Array-like object from which data can be read using bracket notation.  
A quicbit src typically contains bytes (integers in range 0 to 255). 

```js
function scan (src, off, lim) {
    for (var i = 0; i < lim; i++) {
        var byte = src[i]
        // ...
    }
}
```

### dst (destination)

Array-like object to which data can be read using bracket notation.  
A quicbit dst typically contains bytes (integers in range 0 to 255). 

```js
function repeat_byte (b, dst, off, lim) {
    for (var i = off; i < lim; i++) {
        dst[off] = b
    }
    // ...
}
```

### off (offset)

Offset into a [src]() or [dst]() array-like object to start reading or
writing data.

```js
function scan (src, off, lim) {
    for (var i = 0; i < lim; i++) {
        var byte = src[i]
        // ...
    }
}
```

### lim (limit)

The offset into a [src]() or [dst]() *before which* we stop reading or writing data.

```js
function copy (src, off, lim, dst) {
    //...
}
```

### len (length)

The number of bytes to process in a [src]() or [dst]() array.

```js
function scan (src, off, lim) {
    for (var i = 0; i < lim; i++) {
        var byte = src[i]
        // ...
    }
}
```

We typically prefer [lim]() because it is more direct, but len is a good
common shorthand when we want number of bytes processed.


## General

### ret (return value)

The value that is intended to be returned, ()or converted-and-returned 
in a final step) is often called 'ret'.

### v (value)

A common choice for an input value that may be a byte or integer or string...

```js

// v can be a string from which we read a character or byte value.
function repeat (v, dst, off, lim) {
    if(v === typeof 'string') v = v.charCodeAt(0)
    
    for (var i = off; i < lim; i++) {
        dst[off] = b
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

If that seems a bit too short for a function, we may us 'str' to mean 'a string input'



### i, si, ri (indexes)

Really?   What about filter(), reduce(), forEach()...?   Those are 
all great when we aren't iterating over large byte arrays, but when we do
care about iteration speed, we typically use 'i' or some specific variant
like 'si' for string index.


