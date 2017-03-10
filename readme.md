# qb-standard

The Quicbit coding standard

Prioritizing quality over conformity.

qb-standard is comprised of:
 
 * [A Testing Standard](#testing)
 * [A Build Performance Standard](#build-times)
 * [A Production and Development Dependency Standard](#production-and-development-dependencies)
 * [Required Style](doc/required-style.md)
 * [Recommended (Weighted) Style](doc/recommended-style.md) 

## A Quick Note About Our Current Status

While some tools that we use to achieve
qb-standard like [test-kit](github/test-kit) have been 
made available, others, such as dependency measurement are not ready for publication.
As we work out practical matters, more tools will be published and details
will be fleshed out.

However, we feel it is important to share the standards
we are following, though enforcement be through clunky and manual processes 
at the moment.

The 100% 
test coverage requirement is real and active.  We use tap --coverage option
to watch coverage with each release.  With the help of 
[test-kit](../test-kit) and
with practice, the cost of achieving near 100% coverage on all 
software has become manageable to a point that it makes sense as a requirement, 
especially in light of the benefits that it brings requiring less stringency
and work elsewhere.  [quickbit-js](..) has several 
[examples](../qb-json-tok/test.js) of 
100% code coverage using data-driven testing.

We hope you find this document useful, or at least some of the ideas interesting
and thought-provoking.

# Overview

**Inversion of Strictness of Style and Testing**

Many standards today loosely enforce testing with a recommended score
and then enforce style, including format guidelines strictly using lint or 
similar tool requiring 100% compliance.

**qb-standard** inverts this traditional emphasis so that:

* test coverage is strictly enforced (like lint)
* style rules, other than the obvious safety rules, are scored and tracked 

qb-standard emphasis on testing is in line with what is important to successful
projects.  Untested code is a serious risk and very difficult to manage 
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

**Consistency Scores**

qb-standard uses scores that attribute greater penalty for inconsistency than for other types of 
violations which can make it easier to spot unintended changes.

For example, non-standard but consistent indentation throughout a file will cost some style points, 
but different levels of indentation within the same file incurs a heavier penalty.
Emphasis on consistency in scores can help catch conflicting styles when
they get introduced.

(Tooling for this is clunky at the moment... not publicly available)

**Dependency Scores**

Quicbit standard requires that the incremental cost of a module is
measured and recorded with each change, this includes tracking cost of both 
production code *and* build tool dependencies.

(Tooling for this is clunky at the moment... not publicly available)

# The Standard

## Simplicity

Above all, create software that is simple, clean and understandable.

## Testing

In Quicbit, 100% or very near 100% branch and line coverage is a requirement.
Requirements for both branch and line coverage are:

    100%                             OK                           (green)
    Below 100%                       Caution - missing coverage   (yellow)
    Below 97%                        Not covered. Not standard    (red)

Test coverage below 97% is an outright Quicbit standards violation. 

**Having trouble achieving these levels of coverage?**
... then take a careful look at the code in question -
 
* Does the code employ stateless functions for 
  complex logic so that it can be easily tested?  
* Do tests use 
  [data-driven tools and techniques](../test-kit) to ease the 
  burden of writing comprehensive test cases?  
* Is complex logic 
  tightly integrated with objects that require heavy amount of 
  state and mocking?  Try moving this logic into testable functions
  which is then used by the classes.
* Can't trigger a safety assertion?  Consider moving logic into small modules
  where all cases can be tried.


Just like 
[style standards allow you to silence warnings](../../feross/standard#how-do-i-hide-a-certain-warning) with
inserted comments and rules, qb-standard may introduce controls that
allow test violations to be documented and skipped during checks. We 
want to be cautious about how this is done and so will be 
working out the details as we gain experience.

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
code one is writing.  In other words, the [browserified](../../substack/node-browserify) 
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
   [uglified](../../mishoo/UglifyJS2)) byte cost instead.

2. Track this metric as you would a performance metric - over time and 
   against changes.  Set limits to what is acceptable change.  Sudden doubling
   or spikes of say 30KB should be investigated.

3. Now do perform steps 1 and 2 for the test-and-build dependencies, possibly 
   less frequently to track how the
   required tool set is changing - hopefully staying small and simple!
   

### Recommended Dev Tools

These recommended tools are not part of the standard, but are included here to show how we achieve 
qb-standard today with javascript.
  
We like this toolset because it encourages modularization and imposes a minimum amount of 
framework and toolkit to install and test code.
 
Quicbit currently prefers

  * [npm](https://docs.npmjs.com/getting-started/what-is-npm)
    * npm [packaging](https://docs.npmjs.com/getting-started/using-a-package.json)
    * npm [scripts](https://docs.npmjs.com/misc/scripts) for build and automation
  * testing via [test-kit](../test-kit), [tape](../../substack/tape), or [tap](http://www.node-tap.org/)
   
This tool list is technically unbounded because you can npm scripts could run 
anything.  That being said, being light-weight matters and libraries should only
apply tools that make a true difference.  A heavy (complex/large) tool has to bring in that much
more benefit to justify the added weight.

Our tools for tracking incremental byte costs are currently somewhat manual 
and not yet publication-worthy.  We will update with this information
they become more mature.

## Style

Quickbit style standards are based on the 
[feross/standard](../../feross/standard), however, they are 
organized into two lists

1. [Required style](doc/required-style.md)
2. [Recommended style](doc/recommended-style.md)

(note that there were also a handful of
[rules](doc/styles-changed-or-removed.md) 
deemed trivial or in conflict with qb-standard goals and so were left out)

This separation makes clear standards that are *required* for code safety and 
quality versus those that are *recommended* because they can bring
a greater amount of consistency to the format.  *Recommended* rules should be
scored and tracked and generally followed, but they are not regarded 
in the same light as the *required* style rules which are all deemed dangerous or
just strange and confusing to break.
 
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
   
2. Quicbit allows leniency for multi-language support.
   
   While javascript
   is the our preferred prototyping and web language, it is important that
   many algorithms and APIs port cleanly to C, C++, Python and Java.
   Quicbit software is also performance-oriented.  Things like break and continue
   labels are allowed where they make a difference and in those rare cases where
   they can simplify logic.
   
   For this reason, Quicbit preferred style isn't JavaWhereEverythingIsAClass, and it
   isn't c_either.  The style allows for defining algorithms and APIs across
   languages without futzing around with format issues.  No style will look
   perfect across all of these languages, but Quicbit style should read 
   fairly well across different languages.
   
   
3. Conciseness is king.  Like 
   [Linux kernel style](https://01.org/linuxgraphics/gfx-docs/drm/process/coding-style.html#naming),
   short names are preferred.  Try using the 
   [variable-glossary](doc/variable-glossary.md) 
   to create short names and improve consistency across libraries.  While not using a particular 
   existing short-name is 
   not a violation of the standard, *egregious use of long names for local 
   variables is a violation* because it makes logic harder to spot.
   
   These functions would be considered **Not OK** because they are too 
   wordy.  The long parameter names make too big a deal of a very simple algorithm:
        
```js
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
        
   This next example is OK because it uses the parameter glossary to reduce
   the read burden.  Notice how it is easier with this version to discern the 
   algorithms and their similarit.
   
```js
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

   This next example is also nice.  The author uses concise variables and
   vertical alignment to show contrast and similarity of these two simple 
   functions.
   
   
```javascript
    function padright (s, l) { while(s.length < l) s = s + ' '; return s }
    function padleft  (s, l) { while(s.length < l) s = ' ' + s; return s }
```
       
   Notice that the author chose to change spacing after the function name as well as the 
   change to padright 
   code (from <code>s += ' '</code> to <code>s = s + ' '</code>)
   to show great similarity between these functions. 
   If there is a bug in one, then they are probably both broken.
   
   qb-standard's point-based recommendations communicate layout conformity while
   still allowing authors leeway to decide what's best.  We think that is important.
   
   We understand that some programmers hate one-line functions because 
   they are difficult to debug, but
   remember that when we shift our concern to having 100% line and branch
   test coverage, debuggable layouts become less important. 
   