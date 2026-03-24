# $ZARCTAN

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fzarctan

---

 

Caché ObjectScript Reference

$ZARCTAN

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzarcsin "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzcos "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Additional Math and Time Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_MATHFUNCTIONS "Additional Math and Time Functions") \]  >  \[ [$ZARCTAN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzarctan "$ZARCTAN") \]

Go to: Description Parameter Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Inverse (arc) tangent function.

Synopsis

$ZARCTAN(n)

Parameter

n

Any positive or negative number.

Description

$ZARCTAN returns the trigonometric inverse (arc) tangent of n. Possible results range from 1.57079 (half of pi) through zero to –1.57079. The result is given in radians.

Parameter

n

Any positive or negative number. It can be specified as a value, a variable, or an expression. You can use the $ZPI special variable to specify pi. A non-numeric string is evaluated as 0.

The following are arc tangent values returned by $ZARCTAN:

2

returns 1.107148717794090502

1

returns .7853981633974483098

0

returns 0

\-1

returns -.7853981633974483098

Examples

The following example permits you to calculate the arc tangent of a number:

   READ "Input a number: ",num
   WRITE !,"the arc tangent is: ",$ZARCTAN(num)
   QUIT

The following example compares the results from Caché fractional numbers ($DECIMAL numbers) and $DOUBLE numbers. In both cases, the arc tangent of pi/2 is a fractional number (not 1), but the arc tangent of 0 is 0:

  WRITE !,"the arc tangent is: ",$ZARCTAN(0.0)
  WRITE !,"the arc tangent is: ",$ZARCTAN($DOUBLE(0.0))
  WRITE !,"the arc tangent is: ",$ZARCTAN($ZPI)
  WRITE !,"the arc tangent is: ",$ZARCTAN($DOUBLE($ZPI))
  WRITE !,"the arc tangent is: ",$ZARCTAN($ZPI/2)
  WRITE !,"the arc tangent is: ",$ZARCTAN($DOUBLE($ZPI)/2)

 

See Also

*   [$ZTAN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fztan) function
    
*   [$ZPI](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzpi) special variable
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzarcsin "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzcos "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fzarctan.xml**