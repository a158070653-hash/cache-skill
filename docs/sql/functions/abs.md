# ABS

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_abs

---

 

Caché SQL Reference

ABS

 [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_acos "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [ABS](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_abs "ABS") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A numeric function that returns the absolute value of a numeric expression.

Synopsis

ABS(numeric-expression)
{fn ABS(numeric-expression)}

Arguments

numeric-expression

A number whose absolute value is to be returned.

Description

ABS returns the absolute value, which is always zero or a positive number. ABS returns the same data type as numeric-expression. If numeric-expression is not a number (for example, the string 'abc', or the empty string '') ABS returns 0. ABS returns <null> when passed a NULL value.

Note that ABS can be used as an ODBC scalar function (with the curly brace syntax) or as an SQL general function.

This function can also be invoked from Caché ObjectScript using the [ABS()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#ABS) method call:

  WRITE $SYSTEM.SQL.ABS(\-0099)

 

Examples

The following example shows the two forms of ABS:

SELECT ABS(\-99) AS AbsGen,{fn ABS(\-99)} AS AbsODBC

 

both returns 99.

The following examples show how ABS handles some other numbers. Caché SQL converts numeric-expression to [canonical form](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_numcanonical), deleting leading and trailing zeros and evaluating exponents, before invoking ABS.

SELECT ABS(007) AS AbsoluteValue

 

returns 7.

SELECT ABS(\-0.000) AS AbsoluteValue

 

returns 0.

SELECT ABS(\-99E4) AS AbsoluteValue

 

returns 990000.

SELECT ABS(\-99E-4) AS AbsoluteValue

 

returns .0099.

See Also

*   SQL functions: [CONVERT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_convert) [TO\_NUMBER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_tonumber)
    
*   Caché ObjectScript function: [$ZABS](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzabs)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

  [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_acos "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_abs.xml**