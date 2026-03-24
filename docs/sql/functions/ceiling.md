# CEILING

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_ceiling

---

 

Caché SQL Reference

CEILING

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_cast "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_char "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [CEILING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_ceiling "CEILING") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A numeric function that returns the smallest integer greater than or equal to a given numeric expression.

Synopsis

CEILING(numeric-expression)
{fn CEILING(numeric-expression)}

Arguments

numeric-expression

A number whose ceiling is to be calculated.

Description

CEILING returns the nearest integer value greater than or equal to numeric-expression. The returned value has the same data type as numeric-expression and a scale of 0. When numeric-expression is a NULL value, an empty string (''), or any nonnumeric string, CEILING returns NULL.

Note that CEILING can be invoked as an ODBC scalar function (with the curly brace syntax) or as an SQL general function.

This function can also be invoked from Caché ObjectScript using the [CEILING()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#CEILING) method call:

$SYSTEM.SQL.CEILING(numeric-expression)

Examples

The following examples show how CEILING converts a fraction to its ceiling integer:

SELECT CEILING(167.111) AS CeilingNum1,
       CEILING(167.456) AS CeilingNum2,
       CEILING(167.999) AS CeilingNum3

 

all return 168.

SELECT {fn CEILING(167.00)} AS CeilingNum1,
       {fn CEILING(167.00)} AS CeilingNum2

 

return 167.

SELECT CEILING(\-167.111) AS CeilingNum1,
       CEILING(\-167.456) AS CeilingNum2,
       CEILING(\-167.999) AS CeilingNum3

 

all return -167.

SELECT CEILING(\-167.00) AS CeilingNum 

 

returns -167.

The following example uses a subquery to reduce a large table of US Zip Codes (postal codes) to one representative city for each ceiling Latitude integer:

SELECT City,State,CEILING(Latitude) AS CeilingLatitude 
FROM (SELECT City,State,Latitude,CEILING(Latitude) AS CeilingNum
      FROM Sample.USZipCode)
GROUP BY CeilingNum
ORDER BY CeilingNum DESC

 

See Also

*   [FLOOR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_floor)
    
*   [ROUND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_round)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_cast "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_char "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_ceiling.xml**