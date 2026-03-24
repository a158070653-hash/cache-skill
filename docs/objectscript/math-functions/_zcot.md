# $ZCOT

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fzcot

---

 

Caché ObjectScript Reference

$ZCOT

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzcos "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzcsc "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Additional Math and Time Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_MATHFUNCTIONS "Additional Math and Time Functions") \]  >  \[ [$ZCOT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzcot "$ZCOT") \]

Go to: Description Parameter Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Cotangent function.

Synopsis

$ZCOT(n)

Parameter

n

An angle in radians.

Description

$ZCOT returns the trigonometric cotangent of n. The result is a signed decimal number.

Parameter

n

An angle in radians, specified as a nonzero value. It can be specified as a value, a variable, or an expression. A value of 0 generates an <ILLEGAL VALUE> error. A non-numeric string is evaluated as 0.

Examples

The following example permits you to compute the cotangent of a number:

   READ "Input a number: ",num
   IF num\=0 { WRITE !,"zero is an illegal value" }
   ELSE { 
         WRITE !,"the cotangent is: ",$ZCOT(num)
        }
   QUIT

The following example compares the results from Caché fractional numbers ($DECIMAL numbers) and $DOUBLE numbers:

  WRITE !,"the cotangent is: ",$ZCOT(1.0)
  WRITE !,"the cotangent is: ",$ZCOT($DOUBLE(1.0))
  WRITE !,"the cotangent is: ",$ZCOT(\-1.0)
  WRITE !,"the cotangent is: ",$ZCOT($DOUBLE(\-1.0))
  WRITE !,"the cotangent is: ",$ZCOT($ZPI/2)
  WRITE !,"the cotangent is: ",$ZCOT($DOUBLE($ZPI)/2)

 

Note that the cotangent of pi/2 is a fractional number, not 0.

See Also

*   [$ZTAN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fztan) function
    
*   [$ZPI](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzpi) special variable
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzcos "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzcsc "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fzcot.xml**