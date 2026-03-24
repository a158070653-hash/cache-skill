# ASIN

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_asin

---

 

Caché SQL Reference

ASIN

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_ascii "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_atan "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [ASIN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_asin "ASIN") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A scalar numeric function that returns the arc-sine, in radians, of the sine of an angle.

Synopsis

{fn ASIN(float-expression)}

Arguments

float-expression

An expression of type float, whose value is between -1 and 1. This is the sine of the angle.

Description

ASIN returns the inverse (arc) of the sine of an angle as a floating point number. The value of float-expression must be a signed decimal number ranging from 1 to -1 (inclusive). A number outside of this range causes a runtime error, generating an SQLCODE -400 (fatal error occurred). ASIN returns NULL if passed a NULL value. ASIN treats nonnumeric strings, including the empty string (''), as the numeric value 0.

ASIN returns a value of data type FLOAT with a precision of 19 and a scale of 18.

ASIN can only be used as an ODBC scalar function (with the curly brace syntax).

You can use the [DEGREES](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_degrees) function to convert radians to degrees. You can use the [RADIANS](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_radians) function to convert degrees to radians.

Examples

The following examples show the effect of ASIN on two sines.

SELECT {fn ASIN(0.52)} AS ArcSine 

 

returns 0.5468509506...

SELECT {fn ASIN(\-1.00)} AS ArcSine 

 

returns -1.5707963267...

See Also

*   SQL functions: [ACOS](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_acos) [ATAN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_atan) [COS](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_cos) [COT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_cot) [SIN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_sin) [TAN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_tan)
    
*   Caché ObjectScript function: [$ZARCSIN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzarcsin)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_ascii "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_atan "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_asin.xml**