# $ZSIN

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fzsin

---

 

Caché ObjectScript Reference

$ZSIN

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzsec "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzsqr "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Additional Math and Time Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_MATHFUNCTIONS "Additional Math and Time Functions") \]  >  \[ [$ZSIN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzsin "$ZSIN") \]

Go to: Description Parameter Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Returns the trigonometric sine of the specified angle value.

Synopsis

$ZSIN(n)

Parameter

n

Angle in radians ranging from Pi to 2 Pi (inclusive). Other supplied numeric values are converted to a value within this range.

Description

$ZSIN returns the trigonometric sine of n. The result is a signed decimal number ranging from 1 to -1 (see note). $ZSIN(0) returns 0. $ZSIN($ZPI/2) returns 1.

Note:

$ZSIN (like all trigonometric functions) calculates its values based on pi rounded to the number of available decimal digits. Therefore, the value returned by $ZSIN($ZPI) is .000000000000000000462644 and $ZSIN($ZPI\*2) is –.00000000000000000092529. For this reason you should not perform limit tests comparing these returned values to 0.

Parameter

n

An angle in radians ranging from Pi to 2 Pi (inclusive). It can be specified as a value, a variable, or an expression. You can specify the value Pi by using the $ZPI special variable. You can specify positive or negative values smaller than Pi or larger than 2 Pi; Caché resolve these values to the corresponding multiple of Pi. For example, 3 Pi is equivalent to Pi, and negative Pi is equivalent to Pi.

A non-numeric string is evaluated as 0.

Examples

The following example permits you to compute the sine of a number:

   READ "Input a number: ",num
   IF $ZABS(num)\>(2\*$ZPI) { WRITE !,"number is a larger than 2 pi" }
   ELSE { 
         WRITE !,"the sine is: ",$ZSIN(num)
        }
   QUIT

The following example compares the results from Caché fractional numbers ($DECIMAL numbers) and $DOUBLE numbers. In both cases, the sine of pi is a fractional number (not 0), but the sine of pi/2 is set to exactly 1:

  WRITE !,"the sine is: ",$ZSIN($ZPI)
  WRITE !,"the sine is: ",$ZSIN($DOUBLE($ZPI))
  WRITE !,"the sine is: ",$ZSIN($ZPI/2)
  WRITE !,"the sine is: ",$ZSIN($DOUBLE($ZPI)/2)

 

In the following example, all $ZSIN functions return zero (0):

  WRITE !,"the sine is: ",$ZSIN(0.0)
  WRITE !,"the sine is: ",$ZSIN(\-0.0)
  WRITE !,"the sine is: ",$ZSIN($DECIMAL(0.0))
  WRITE !,"the sine is: ",$ZSIN($DOUBLE(0.0))
  WRITE !,"the sine is: ",$ZSIN($DECIMAL(\-0.0))
  WRITE !,"the sine is: ",$ZSIN($DOUBLE(\-0.0))
  WRITE !,"the sine is: ",$ZSIN(\-$DECIMAL(0.0))
  WRITE !,"the sine is: ",$ZSIN(\-$DOUBLE(0.0))

 

This is true on all platforms, including AIX.

See Also

*   [$ZCOS](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzcos) function
    
*   [$ZARCSIN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzarcsin) function
    
*   [$ZPI](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzpi) special variable
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzsec "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzsqr "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fzsin.xml**