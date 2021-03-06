# Variable Glossary

**Note: using this glossary is not part of the broader qb-standard. Use is 
only required when working within quicbit libraries**. We are just sharing 
the glossary here for those who wish to contribute
to quicbit-js libraries and as a recommended concept for companies that
create libraries and APIs.

## Why have a glossary of parameter and variable names?


A glossary of the most common 
variable and parameter names can reduce time to understand function
signatures and implementations.  


Programmers well-versed in OO methodology might suggest replacing 
parameter conventions like **src**, **off**, and **lim** with objects
that capture and enforce these concepts in methods.  But wrappers introduce the
burden of wrapping and unwrapping as well as a burden on the reader 
to learn non-standard and non-trivial types.  Arrays, strings and
plain objects can be preferred for their simplicity,
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
the short version.  Those reading and changing code need to know this logic very
well.  Those who want to peruse functions should use documentation and
examples in the comments or readme files.

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
source ([src](https://github.com/quicbit-js/qb-standard/blob/master/doc/variable-glossary.md#src-source)) 
with an inclusive lower-bound 
offset ([off](https://github.com/quicbit-js/qb-standard/blob/master/doc/variable-glossary.md#off-offset)) and 
exclusive upper-bound limit ([lim](https://github.com/quicbit-js/qb-standard/blob/master/doc/variable-glossary.md#lim-limit)).

**Bitmask literals?**  Isn't that heresy?!!  Not to us.  *The most important aspects
of this function is that it is clearly written, has 100% branch and line test coverage, 
is very tiny with no library dependencies, runs fast and is
documented with clear examples in the readme file*.  Those things are important.

## Arrays

Why focus on arrays and indexes?  Shouldn't we prefer filter(), map(), 
reduce(), forEach()...?   

A good portion of quicbit software is concerned with fast scans
over large arrays of bytes.  For these cases, for-loop iteration is
far faster and is preferred.

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

Offset into a [src](https://github.com/quicbit-js/qb-standard/blob/master/doc/variable-glossary.md#src-source) 
or [dst](https://github.com/quicbit-js/qb-standard/blob/master/doc/variable-glossary.md#dst-destination) 
array-like object to start reading or
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

The offset into a [src](https://github.com/quicbit-js/qb-standard/blob/master/doc/variable-glossary.md#src-source) 
or [dst](https://github.com/quicbit-js/qb-standard/blob/master/doc/variable-glossary.md#dst-destination) *before which* we stop reading or writing data.

```js
function copy (src, off, lim, dst) {
    //...
}
```

### len (length)

The number of bytes to process in a [src](https://github.com/quicbit-js/qb-standard/blob/master/doc/variable-glossary.md#src-source) 
or [dst](https://github.com/quicbit-js/qb-standard/blob/master/doc/variable-glossary.md#dst-destination) array.

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
functions with [lim](https://github.com/quicbit-js/qb-standard/blob/master/doc/variable-glossary.md#lim-limit) 
to be more direct than [len](https://github.com/quicbit-js/qb-standard/blob/master/doc/variable-glossary.md#len-length).  On the other hand, 
functions with the [len](https://github.com/quicbit-js/qb-standard/blob/master/doc/variable-glossary.md#len-length) signature can be simpler when 
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

## Streams

### enc (encoding)

A string encoding.  Currently 'utf8' is the only format supported and 'buffer' is used as a special indicator for object streams,
just as it is in the node streams API.  When creating a quicbit transform object, opt.enc specifies the *output* format. 

## General

### opt (options)

We often place optional arguments within a single object called 'opt'.  This
is placed at the very end of all other arguments.  

Good practice is to put all or very nearly all optional arguments in
this object rather than use variable length argument lists or force
users to inject 'null' placeholders.  For example, Douglass 
Crockford once admitted this API regret*:

    JSON.stringify (value, replacer, space)
    
Is very often called using a filler:

    JSON.stringify (obj, null, '  ')

While using var-args may be a tempting fix, var-arg logic is messy and requires
careful management of argument types.  A simple and effective approach
is to slurp all optional arguments into an options object:

    JSON.stringify (value, options)
    
And the calls become clear / to-the-purpose:

    JSON.stringify (obj, { space: '  ' }) 

* sorry, no citation available.  I heard this in one of his talks and cannot find a reference.

### cb (callback)

'cb' is used for most callbacks used in map/reduce/streams etc.

### fn (function)

'fn' is used as a generic abbreviation for a 'function', as well as a name postfix 

    compare (src1, src2, { compare_fn: function () { ... } }) 

## Tiny Variables and Parameters

### a (array)

If a parameter must be an array or array-buffer and cannot be a sequence, we often use 'arr' or simply 'a'
to indicate it must be an array with support for bracket-access.


### a and b (comparison)

quicbit often uses 'a' and 'b' to represent two generic arguments with roughly equivalent roles:

    function compare (a, b) { return a - b }

### n (number of items)

quicbit sequences often use simply 'n' to signify a number of items to copy, scan, etc.  Typically
'n' can be negative to indicate quantity from the end of an array or sequence.

### q (quantity of items)

quickbit implementations and private function parameters often use 'q' to signify an absolute quantity of items 
as opposed to a directional quantity 'n'.

### s (string)

Often we use 's' to represent a generic string in code, similar to 'a' for arrays (strings and arrays
are in fact very similar and super-common, so it makes sense to use consistent short forms in algorithms).


### r or ret (return value)

The value that is intended to be returned, (or converted-and-returned 
in a final step) is often called 'ret'.  In one-liners, we will often shorten to 'r'.

### k or key

When using maps or objects, keys are often abbreviated to 'k' and corresponding values to 'v'

### v or val (value)

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

'v' is also often used for generic object values along with 'k' for keys.

### s or str (string)

For short processing functions which clearly take a string, we often use 's'.

```javascript
function padright (s, l) { while(s.length < l) s = s + ' '; return s }
function padleft  (s, l) { while(s.length < l) s = ' ' + s; return s }
```

When 's' seems a too short, 'str' is an alternative.  'str' is also the quicbit name for string types.


