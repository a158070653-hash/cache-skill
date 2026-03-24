# FLOOR

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_floor

---

 

Caché SQL Reference

FLOOR

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_find "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_getdate "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [FLOOR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_floor "FLOOR") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A numeric function that returns the largest integer less than or equal to a given numeric expression.

Synopsis

FLOOR(numeric-expression)
{fn FLOOR(numeric-expression)}

Arguments

numeric-expression

A number whose floor is to be calculated.

Description

FLOOR returns the nearest integer value less than or equal to numeric-expression. The returned value has the same data type as numeric-expression and a scale of 0. When numeric-expression is a NULL value, an empty string (''), or a nonnumeric string, FLOOR returns NULL.

Note that FLOOR can be invoked as an ODBC scalar function (with the curly brace syntax) or as an SQL general function.

This function can also be invoked from Caché ObjectScript using the [FLOOR()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#FLOOR) method call:

$SYSTEM.SQL.FLOOR(numeric-expression)

Examples

The following examples show how FLOOR converts a fraction to its floor integer:

SELECT FLOOR(167.111) AS FloorNum1,
       FLOOR(167.456) AS FloorNum2,
       FLOOR(167.999) AS FloorNum3

 

all return 167.

SELECT {fn FLOOR(167.00)} AS FloorNum1,
       {fn FLOOR(167)} AS FloorNum2

 

return 167.

SELECT FLOOR(\-167.111) AS FloorNum1,
       FLOOR(\-167.456) AS FloorNum2,
       FLOOR(\-167.999) AS FloorNum3

 

all return -168.

SELECT FLOOR(\-167.00) AS FloorNum 

 

returns -167.

The following example uses a subquery to reduce a large table of US Zip Codes (postal codes) to one representative city for each floor Latitude integer:

SELECT City,State,FLOOR(Latitude) AS FloorLatitude 
FROM (SELECT City,State,Latitude,FLOOR(Latitude) AS FloorNum
      FROM Sample.USZipCode)
GROUP BY FloorNum
ORDER BY FloorNum DESC

 

See Also

*   [CEILING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_ceiling)
    
*   [ROUND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_round)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_find "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_getdate "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_floor.xml**