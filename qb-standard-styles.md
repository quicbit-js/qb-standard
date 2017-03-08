## Format and Style

qb-standard-styles is loosely based on the 
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
   test coverage, debuggable layouts become much less relevant. 
   
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
