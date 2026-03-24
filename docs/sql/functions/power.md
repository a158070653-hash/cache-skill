# POWER

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_power

---

 

Caché SQL Reference

POWER

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_position "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_quarter "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [POWER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_power "POWER") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A numeric function that returns the value of a given expression raised to the specified power.

Synopsis

POWER(numeric-expression,power)
{fn POWER(numeric-expression,power)}

Arguments

numeric-expression

The base number. Can be a positive or negative integer or fractional number.

power

The exponent, which is the power to which to raise numeric-expression. Can be a positive or negative integer or fractional number.

Description

POWER calculates one number raised to the power of another. It returns a value of data type [DECIMAL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datatype) with a precision of 36 and a scale of 18.

Note that POWER can be invoked as an ODBC scalar function (with the curly brace syntax) or as an SQL general scalar function.

POWER interprets a non-numeric string as 0 for either argument. For further details, refer to [Strings as Numbers](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_nonnumasnum). POWER returns NULL if passed a NULL value for either argument.

All combinations of numeric-expression and power are valid except:

*   POWER(0,-m): a 0 numeric-expression and a negative power results in an SQLCODE -400 error.
    
*   POWER(-n,.m): a negative numeric-expression and a fractional power results in an SQLCODE -400 error.
    

Examples

The following example raises 5 to the 3rd power:

SELECT POWER(5,3) AS Cubed

 

returns 125.

The following embedded SQL example returns the first 16 powers of 2:

   SET a\=1
   WHILE a<17 {
   &sql(SELECT {fn POWER(2,:a)}
   INTO :b)
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE
     QUIT }
   ELSE {
     WRITE !,"2 to the ",a," = ",b
     SET a\=a+1 }
   }

 

See Also

*   SQL functions: [EXP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_exp) [LOG10](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_log10) [SQRT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_sqrt) [SQUARE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_square)
    
*   Caché ObjectScript function: [$ZPOWER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzpower)
    
*   Caché ObjectScript [Exponentiation Operator (\*\*)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_operators#GCOS_operators_expon)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_position "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_quarter "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_power.xml**