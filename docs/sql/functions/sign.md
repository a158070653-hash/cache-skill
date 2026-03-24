# SIGN

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_sign

---

 

Caché SQL Reference

SIGN

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_second "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_similarity "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [SIGN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_sign "SIGN") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A numeric function that returns the sign of a given numeric expression.

Synopsis

SIGN(numeric-expression)

{fn SIGN(numeric-expression)}

Arguments

numeric-expression

A number for which the sign is to be returned.

Description

SIGN returns the following:

*   \-1 if numeric-expression is less than zero.
    
*   0 (zero) if numeric-expression is zero: 0, +0, or \-0.
    
*   1 if numeric-expression is greater than zero.
    
*   NULL if numeric-expression is NULL, or if it is a non-numeric string.
    

SIGN can be used as either an ODBC scalar function (with the curly brace syntax) or as an SQL general function.

SIGN converts numeric-expression to [canonical form](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_numcanonical) before determining its value. For example, SIGN(-+-+3) and SIGN(-3+5) both return 1, indicating a positive number.

Note:

In Caché SQL, two negative signs (hyphens) are the in-line [comment](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_langelements#GSQL_langelements_comments) indicator. For this reason, a SIGN argument specifying two successive negative signs must be presented as a numeric string enclosed in quotes.

Examples

The following examples shows the effects of SIGN:

SELECT SIGN(\-49) AS PosNeg

 

returns -1.

SELECT {fn SIGN(\-0.0)} AS PosNeg

 

returns 0.

SELECT SIGN(\-+\-16.748) AS PosNeg

 

returns 1.

SELECT {fn SIGN(NULL)} AS PosNeg

 

returns <null>.

See Also

*   [+(Positive)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_positive) and [–(Negative)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_negative) unary operators
    
*   [ABS](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_abs) function
    
*   [ISNUMERIC](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_isnumeric) function
    
*   [%PLUS](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_plus_pct) and [%MINUS](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_minus_pct) collation functions
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_second "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_similarity "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_sign.xml**