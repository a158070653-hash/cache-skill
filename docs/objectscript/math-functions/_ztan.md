# $ZTAN

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fztan

---

 

Caché ObjectScript Reference

$ZTAN

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzsqr "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fztime "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Additional Math and Time Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_MATHFUNCTIONS "Additional Math and Time Functions") \]  >  \[ [$ZTAN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fztan "$ZTAN") \]

Go to: Description Parameter Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Returns the trigonometric tangent of the specified angle value.

Synopsis

$ZTAN(n)

Parameter

n

An angle in radians ranging from Pi to 2 Pi (inclusive). Other supplied numeric values are converted to a value within this range.

Description

$ZTAN returns the trigonometric tangent of n. The result is a signed decimal number.

Note:

$ZTAN (like all trigonometric functions) calculates its values based on pi rounded to the number of available decimal digits. Therefore, the value returned by $ZTAN($ZPI) is –.000000000000000000462644 and $ZTAN(–$ZPI) is .000000000000000000462644. For this reason you should not perform limit tests comparing these returned values to 0. $ZTAN(0) is 0.

Parameter

n

An angle in radians ranging from 0 to 2 Pi. It can be specified as a value, a variable, or an expression.

A non-numeric string is evaluated as 0.

Examples

The following example permits you to compute the tangent of a number:

   READ "Input a number: ",num
   WRITE !,"the tangent is: ",$ZTAN(num)
   QUIT

The following example compares the results from Caché fractional numbers ($DECIMAL numbers) and $DOUBLE numbers. In both cases, the tangent of 0 is exactly 0, but the tangent of pi is a negative fractional number (not exactly 0):

  WRITE !,"the tangent is: ",$ZTAN(0.0)
  WRITE !,"the tangent is: ",$ZTAN($DOUBLE(0.0))
  WRITE !,"the tangent is: ",$ZTAN($ZPI)
  WRITE !,"the tangent is: ",$ZTAN($DOUBLE($ZPI))
  WRITE !,"the tangent is: ",$ZTAN(1.0)
  WRITE !,"the tangent is: ",$ZTAN($DOUBLE(1.0))

 

See Also

*   [$ZARCTAN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzarctan) function
    
*   [$ZSIN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzsin) function
    
*   [$ZPI](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzpi) special variable
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzsqr "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fztime "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fztan.xml**