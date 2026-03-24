# $ZSEC

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fzsec

---

 

Caché ObjectScript Reference

$ZSEC

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzpower "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzsin "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Additional Math and Time Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_MATHFUNCTIONS "Additional Math and Time Functions") \]  >  \[ [$ZSEC](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzsec "$ZSEC") \]

Go to: Description Parameter Example See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Returns the trigonometric secant of the specified angle value.

Synopsis

$ZSEC(n)

Parameter

n

Angle in radians ranging from 0 to 2 Pi. It can be specified as a value, a variable, or an expression.

Description

$ZSEC returns the trigonometric secant of n. The result is a signed decimal number. The secant of 0 is 1. The secant of pi is -1.

Note:

Caché uses the host operating system’s routines to calculate trigonometric functions. For this reason, results obtained from different operating systems may not precisely match.

Parameter

n

An angle in radians ranging from Pi to 2 Pi (inclusive). It can be specified as a value, a variable, or an expression. You can specify the value Pi by using the $ZPI special variable. You can specify positive or negative values smaller than Pi or larger than 2 Pi; Caché resolve these values to the corresponding multiple of Pi. For example, 3 Pi is equivalent to Pi, and negative Pi is equivalent to Pi.

A non-numeric string is evaluated as 0, and therefore $ZSEC returns 1.

Example

The following example permits you to compute the secant of a number:

   READ "Input a number: ",num
   IF $ZABS(num)\>(2\*$ZPI) { WRITE !,"number is a larger than 2 pi" }
   ELSE { 
         WRITE !,"the secant is: ",$ZSEC(num)
        }
   QUIT

See Also

*   [$ZCSC](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzcsc) function
    
*   [$ZPI](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzpi) special variable
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzpower "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzsin "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fzsec.xml**