# $ZARCSIN

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fzarcsin

---

 

Caché ObjectScript Reference

$ZARCSIN

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzarccos "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzarctan "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Additional Math and Time Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_MATHFUNCTIONS "Additional Math and Time Functions") \]  >  \[ [$ZARCSIN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzarcsin "$ZARCSIN") \]

Go to: Description Parameter Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Inverse (arc) sine function.

Synopsis

$ZARCSIN(n)

Parameter

n

A signed decimal number.

Description

$ZARCSIN returns the trigonometric inverse (arc) sine of n. The result is given in radians.

Parameter

n

Signed decimal number ranging from 1 to -1 (inclusive). It can be specified as a value, a variable, or an expression. Numbers outside the range generate an <ILLEGAL VALUE> error. A non-numeric string is evaluated as 0.

The following are arc sine values returned by $ZARCSIN:

1

returns 1.570796326794896619

0

returns 0

\-1

returns -1.570796326794896619

Examples

The following example permits you to compare the arc sine and the arc cosine of a number:

   READ "Input a number: ",num
   IF num\>1 { WRITE !,"ILLEGAL VALUE: number too big" }
   ELSEIF num<\-1 { WRITE !,"ILLEGAL VALUE: number too small" }
   ELSE { 
         WRITE !,"the arc sine is: ",$ZARCSIN(num)
         WRITE !,"the arc cosine is: ",$ZARCCOS(num)
        }
   QUIT

The following example compares the results from Caché fractional numbers ($DECIMAL numbers) and $DOUBLE numbers. In both cases, the arc sine of 0 is exactly 0:

  WRITE !,"the arc sine is: ",$ZARCSIN(0.0)
  WRITE !,"the arc sine is: ",$ZARCSIN($DOUBLE(0.0))
  WRITE !,"the arc sine is: ",$ZARCSIN(1.0)
  WRITE !,"the arc sine is: ",$ZARCSIN($DOUBLE(1.0))
  WRITE !,"the arc sine is: ",$ZARCSIN(\-1.0)
  WRITE !,"the arc sine is: ",$ZARCSIN($DOUBLE(\-1.0))

 

See Also

*   [$ZSIN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzsin) function
    
*   [$ZPI](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzpi) special variable
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzarccos "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzarctan "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fzarcsin.xml**