# $ZCOS

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fzcos

---

 

Caché ObjectScript Reference

$ZCOS

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzarctan "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzcot "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Additional Math and Time Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_MATHFUNCTIONS "Additional Math and Time Functions") \]  >  \[ [$ZCOS](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzcos "$ZCOS") \]

Go to: Description Parameter Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Cosine function.

Synopsis

$ZCOS(n)  

Parameter

n

An angle in radians ranging from Pi to 2 Pi (inclusive). Other supplied numeric values are converted to a value within this range.

Description

$ZCOS returns the trigonometric cosine of n. The result is a signed decimal number ranging from -1 to +1. $ZCOS(0) returns 1. $ZCOS($ZPI) returns -1.

Parameter

n

An angle in radians ranging from Pi to 2 Pi (inclusive). It can be specified as a value, a variable, or an expression. You can specify the value Pi by using the $ZPI special variable. You can specify positive or negative values smaller than Pi or larger than 2 Pi; Caché resolve these values to the corresponding multiple of Pi. For example, 3 Pi is equivalent to Pi, negative Pi is equivalent to Pi, and zero is equivalent to 2 Pi.

A non-numeric string is evaluated as 0.

Examples

The following example permits you to compute the cosine of a number:

   READ "Input a number: ",num
   IF $ZABS(num)\>(2\*$ZPI) { WRITE !,"number is a larger than 2 pi" }
   ELSE { 
         WRITE !,"the cosine is: ",$ZCOS(num)
        }
   QUIT

The following example compares the results from Caché fractional numbers ($DECIMAL numbers) and $DOUBLE numbers. In both cases, the cosine of 0 is exactly 1, the cosine of pi is exactly -1:

  WRITE !,"the cosine is: ",$ZCOS(0.0)
  WRITE !,"the cosine is: ",$ZCOS($DOUBLE(0.0))
  WRITE !,"the cosine is: ",$ZCOS(1.0)
  WRITE !,"the cosine is: ",$ZCOS($DOUBLE(1.0))
  WRITE !,"the cosine is: ",$ZCOS($ZPI)
  WRITE !,"the cosine is: ",$ZCOS($DOUBLE($ZPI))

 

See Also

*   [$ZSIN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzsin) function
    
*   [$ZARCCOS](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzarccos) function
    
*   [$ZPI](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzpi) special variable
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzarctan "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzcot "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fzcos.xml**