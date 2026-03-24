# $ZEXP

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fzexp

---

 

Caché ObjectScript Reference

$ZEXP

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetimeh "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzhex "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Additional Math and Time Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_MATHFUNCTIONS "Additional Math and Time Functions") \]  >  \[ [$ZEXP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzexp "$ZEXP") \]

Go to: Description Parameter Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Exponential function (inverse of natural logarithm).

Synopsis

$ZEXP(n)

Parameter

n

A number of any type. A number larger than 335.6 results in a <MAXNUMBER> error. A number smaller than -295.4 returns 0.

Description

$ZEXP is the exponential function en, where e is the constant 2.718281828. Therefore, to return the value of e, you can specify $ZEXP(1). $ZEXP is the inverse of the natural logarithm function [$ZLN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzln).

Parameter

n

Any number. It can be specified as a value, a variable, or an expression. A positive value larger than 335.6 or smaller than -4944763837 results in a <MAXNUMBER> error. A negative value smaller than -295.4 returns 0. A value of zero (0) returns 1. A non-numeric string is evaluated as 0 and therefore returns 1.

Examples

The following example demonstrates that $ZEXP is the inverse of $ZLN:

  SET x\=7
  WRITE $ZEXP(x),!
  WRITE $ZLN(x),!
  WRITE $ZEXP($ZLN(x))

 

The following example returns $ZEXP for negative and positive integers and for zero. This example returns the constant e as $ZEXP(1):

   FOR x\=\-3:1:3 {
     WRITE !,"The exponential of ",x," = ",$ZEXP(x) 
     }
   QUIT

 

returns:

The exponential of -3 = .04978706836786394297
The exponential of -2 = .1353352832366126919
The exponential of -1 = .3678794411714423216
The exponential of 0 = 1
The exponential of 1 = 2.718281828459045236
The exponential of 2 = 7.389056098930650228
The exponential of 3 = 20.08553692318766774

The following example uses IEEE floating point numbers ([$DOUBLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fdouble) numbers). The first $ZEXP returns a numeric value, the second $ZEXP returns “INF” (or <MAXNUMBER> depending on the [IEEEError()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process#IEEEError) method setting):

   SET rtn\=##class(%SYSTEM.Process).IEEEError(0)
   WRITE $ZEXP($DOUBLE(1.0E2)),!
   WRITE $ZEXP($DOUBLE(1.0E3))

 

The following example demonstrates that the empty string or a nonnumeric value is treated as 0:

   WRITE $ZEXP(""),!
   WRITE $ZEXP("INF")

 

both return 1.

See Also

*   [$ZLN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzln) function
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetimeh "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzhex "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fzexp.xml**