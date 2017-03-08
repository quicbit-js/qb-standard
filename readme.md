# qb-standard

The Quicbit coding standard

## A Quick Note About Our Current Status

While some tools that we use to achieve
qb-standard like [test-kit](https://github.com/quicbit-js/test-kit) have been 
made available, others, such as dependency measurement are not ready for publication.
As we work out practical matters, more tools will be published and details
will be fleshed out.

However, we feel it is important to share the standards
we are following, though enforcement be through clunky and manual processes 
at the moment.

The 100% 
test coverage requirement is real and active.  We use tap coverage options
to watch coverage with each release.  With the help of 
[test-kit](https://github.com/quicbit-js/test-kit) and
with practice, the cost of achieving near 100% coverage on all 
software has become manageable to a point that it makes sense as a requirement, 
especially in light of the benefits that it brings requiring less stringency
and work elsewhere.  [quickbit-js](https://github.com/quicbit-js) has several 
[examples](https://github.com/quicbit-js/qb-json-tok/blob/master/test.js) of 
100% code coverage using data-driven testing.


We hope you find this document useful, or at least some of the ideas interesting
and thought-provoking.

# Overview

**Inversion of Strictness of Style and Testing**

Many standards today loosely enforce testing with a recommended score
and then enforce style guidelines strictly using lint or 
similar tool.

**qb-standard** inverts this traditional emphasis so that:

* test coverage is strictly enforced 
* style violations are loosely scored and tracked


qb-standard emphasis is in line with what is most important to successful
code and projects.  Untested code is a serious risk and very difficult to manage 
while non-standard braces placement and indentation,
while worthy of some negative attention through scoring, are less serious.

**Fewer Style Restrictions**

After swapping the strictness that is traditionally applied to testing and style, 
(above) we then
look at style standards in a different light.  Style standards of this light
weight, well tested code should:

1. encourage simplicity and clarity of logic
1. encourage consistency within files and modules
1. not be overly concerned with catching and preventing bugs by hand or by eye
1. not be as draconian with style as we are with testing and dependency requirements

**Emphasis on Consistency**

qb-standard allocates a greater penalty for inconsistency than for other types of 
violations which can make it easier to spot unintended changes.

For example, non-standard but consistent indentation throughout a file will cost some style points, 
but different levels of indentation to the same file incurs a heavier score penalty.
Emphasis on consistency in scores helps catch conflicting styles when
they get introduced.

**Awareness of Dependencies**

Quicbit standard requires that the incremental cost of a module is
measured and recorded with each change, this includes tracking cost of both 
production code 
*and* build tool dependencies.

# The Standard

## Simplicity

Above all, create software that is simple, clean and understandable.

## Testing

In Quicbit, 100% or very near 100% branch and line coverage is a requirement.
Requirements for both branch and line coverage are:

    From 100%                 OK                           (green)
    From 97 to less than 100  Caution - missing coverage   (yellow)
    Below 97%                 Not covered. Not standard    (red)

Test coverage below 97% is an outright Quicbit standards violation, 
while style transgressions are scored by lint and a 
"passing style score" can be worked out by the author and
teams involved.  


**Having trouble achieving these levels of coverage?**
... then take a hard look
at the code in question - is it employing stateless functions for 
complex logic so that it can be easily tested?  Do the tests use 
[data-driven tools and techniques](https://github.com/quicbit-js/test-kit) to ease the 
burden of writing comprehensive test cases?  Is complex logic 
tightly integrated with objects that require heavy amount of 
state and mocking?  Then consider moving the that logic into testable functions
which is then used by the classes. 

Just like 
[style standards allow you to silence warnings](https://github.com/feross/standard#how-do-i-hide-a-certain-warning) with
inserted comments and rules, Quicbit standard may allow test violations to
be documented and skipped during checks... working out the details on that.

## Build Times

Track the **time it takes to unit-test-and-build** the libraries after all required libraries
are downloaded and available (e.g. after npm install).  Be sure to separate
performance tests from the unit-test-and-build process unless they fit within 
the fast build timing requirements.  
Keep this metric and note significant changes as you would a performance metric of the code
itself.

* A small library should unit-test-and-build in less than one second on a reasonably
  modern machine.
* A larger project should unit-test-and-build in a few seconds.

When build times get uncontrollably high, we scrutinize expensive tests, and/or 
make sure the project is broken into smaller and faster-building components.
Usually, we don't let modules get big in the first place.

## Production and Development Dependencies

Size and complexity of the entire required code base counts, not just the 
code one is writing.  In other words, the [browserified](https://github.com/substack/node-browserify) 
file counts, not just the simplicity of the root-level library in question.
Think critically before adding new dependencies and seek to get good benefit-per-KB 
of each added library.

For a given project, we recommend tracking the incremental cost of adding
a module like so:

1. Measure the size of the runtime software without the added library (e.g. the
   size of browserify of the project without the library by using
   a stub, if needed), and again with
   the library (browserify with project linked).  Record the incremental byte cost of
   adding the module:
   
       byte_cost = bytes_with_module - bytes_without_module
       
   Concerned about penalizing comments?  Track the condensed (e.g. 
   [uglified](https://github.com/mishoo/UglifyJS2)) byte cost instead.

2. Track this metric as you would a performance metric - over time and 
   against changes.  Set limits to what is acceptable change.  A 2x change
   or + 30K change should be a concern.

3. Now do the steps 1 and 2 for test code, possibly less frequently. to track how the
   required tool set is coming along - hopefully staying small and simple!
   

### Recommended Dev Tools

The requirement to stay light weight with dependencies applies to development
required tools as well.  While we can of course use whatever we like on our machine
to develop code, the published package should impose the minimum amount of 
framework and toolkit to install and test code. 
Quicbit currently prefers

  * [npm](https://docs.npmjs.com/getting-started/what-is-npm)
    * npm [packaging](https://docs.npmjs.com/getting-started/using-a-package.json)
    * npm [scripts](https://docs.npmjs.com/misc/scripts) for build and automation
  * testing via [test-kit](https://github.com/quicbit-js/test-kit), [tape](https://github.com/substack/tape), or [tap](http://www.node-tap.org/)
   
This tool list is technically unbounded because you can npm scripts could run 
anything.  That being said, being light-weight matters and libraries should only
apply tools that make a true difference.  A heavy (complex/large) tool has to bring in that much
more benefit to justify the added weight.

Our hacked-together tools for tracking incremental byte costs are highly manual 
and not publication worthy at this point.  We will update with this information
they become more mature.

## Format and Style

Quicbit style standard is loosely based on the 
[feross/standard](https://github.com/feross/standard). However, there are 
differences in the focus and in what is required versus scored/encouraged...
 
 
1. Quicbit recognizes an author's preference to a greater
   degree than most other styles. Style rules are stated as either *recommended*
   or *must* (required).
   
   Breaking a required *must*... rule makes a library non-standard. 
    
   Breaking *recommended* rule might make your code unpopular, but
   is never as serious as lack of clarity, poor testing, excessive
   dependency or unnecessary complexity.  Those things matter more than format.
   
   Compliance with Quicbit style standards is not absolute, it is relative/incremental.
   Breaking a some format rules on a 100% tested and clearly laid-out file
   just isn't significant.  On the other hand, breaking dozens of style rules and making
   code look weird for no particular reason *is* significant.  The owner(s)
   of a particular file should have say over how it is laid out, while staying
   mostly in line with recommended rules (a qb-standard scoring tool is pending, but 
   will be the way we manage compliance with qb-standard).
   
   Most importantly, it is not ok to change the style of an existing file
   without permission and without buy-in for converting the whole set of
   files that comes with it.  Consistency matters.
   
2. Quicbit is a multi-language style.
   
   While javascript
   is the our preferred prototyping and web language, it is important that
   many algorithms and APIs port cleanly to C, C++, Python and Java.
   Quicbit software is also performance-oriented.  Things like break and continue
   labels are allowed where they make a difference and in those rare cases where
   they can simplify logic.
   
   For this reason, Quicbit preferred style isn't JavaWhereEverythingIsAClass, and it
   isn't c_either.  The style is oriented toward defining algorithms and APIs across
   languages without futzing around with style issues.  No style will look
   perfect across all of these languages, but Quicbit style reads clearly in
   all of them, which is the objective.
   
3. Conciseness is king.  Like 
   [Linux kernel style](https://01.org/linuxgraphics/gfx-docs/drm/process/coding-style.html#naming),
   short names are preferred.  Try to use the short-name-glossary to create greater
   consistency across libraries.  Failing to use an existing short-name is 
   not a serious violation of the standard, but using names like 'aString'
   throughout the code makes algorithms harder to discern.
   
   The following functions are **NOT OK** because they are too wordy.  
   The long parameter names make too big a deal of a very simple algorithm:
        
```javascript
    function padright(aString, desiredLength) {
        while(aString.length < desiredLength) {
            aString += ' '  
        }
        return aString
    }
    
    function padleft(aString, desiredLength) {
        while(aString.length < desiredLength) {
            aString = ' ' + aString 
        }
        return aString
    }
```
        
   This next example is fine because it uses the parameter glossary to reduce
   the read burden.  Notice how we can discern the algoritm similarities and 
   differences more quickly.
   
```javascript
       function padright(s, len) {
           while(s.length < len) { 
               s += ' ' 
           } 
           return s
       }

       function padleft(s, len) {
           while(s.length < len) { 
               s = ' ' + s
           } 
           return s
       }
```

   The following example shows why we want to empower our author to break 
   a recommended rule when it improves clarity.  An author may want to 
   show how simple and similar these functions by lining them
   up like so:

```javascript
       function padright (s, l) { while(s.length < l) s = s + ' '; return s }
       function padleft  (s, l) { while(s.length < l) s = ' ' + s; return s }
```
       
   The author changed spacing after the function name as well as the 
   change to padright 
   code (from <code>s += ' '</code> to <code>s = s + ' '</code>)
   to show great similarity between these functions. 
   If there is a bug in one, then they are probably both broken.
   
   Here we see value in letting the author make layout decisions 
   in ways that help the reader.  Quicbit standard allows the author to group 
   functions together and align code in ways that convey the most information
   in the clearest possible way.  We think that is important.
   
   Some developers hate one-line functions because they are difficult to debug, but
   remember that when we shift our concern to having 100% line and branch
   test coverage, debuggable layouts become unimportant. 
   
### Indent with Spaces

Quicbit files *must* use spaces, not tabs.

4 space indentation is *recommended*
  * 4 space indentation is a fair compromise between linux kernel 8-space standard 
    and javascript 2-space standards.

Why not tabs?  Because using fixed
indentation allows authors to layout code in an intended way:

```javascript
        //...
        } else {
            if(prev_tok === COLON) {                                    // string-value-pair
                ret = cb(buf, si, slen, tok, vi, idx - vi, err_info)
            } else {
                ret = cb(buf, -1, 0, QUOTE, si, slen)                      
                if(ret > 0) {
                    idx = ret || idx; si = -1; slen = 0; continue       // cb requested index
                } else if (ret === 0) {
                    return si + slen                                    // cb requested stop
                }
```

... the alignment of comments down the right may be more clear in this case.

Not everyone feels the same way about readability and you may feel the
above sample would be better using some other form without the comments on the right, but
the *allowing the author to create a meaningful alignment* is still important.
If we used tabs, then someone editing using tabs set to 3 would likely
break the layout pattern without even knowing it.

### Place braces '{' at the end of line

```javascript

    if (a == 4) {
       //...
    }
```

### Semicolons

Quicbit [*recommends* not using semicolons](https://github.com/feross/standard/blob/master/RULES.md#semicolons).  
Semicolons are not needed except when a line begins with '[', `'`, or '('.  In
simple software, it is a small matter to avoid starting lines with these tokens.

... more updates pending
