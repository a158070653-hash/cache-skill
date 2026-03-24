# NULLIF

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_nullif

---

 

Caché SQL Reference

NULLIF

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_now "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_nvl "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [NULLIF](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_nullif "NULLIF") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A function that returns NULL if an expression is true.

Synopsis

NULLIF(expression1,expression2)

Arguments

expression1

An SQL expression.

expression2

An SQL expression.

Description

The NULLIF function returns NULL if expression1 is equal to expression2, otherwise it returns expression1. The data type returned in DISPLAY mode or ODBC mode is determined by the data type of expression1.

NULLIF is equivalent to:

SELECT CASE 
WHEN value1 \= value2 THEN NULL
ELSE value1
END
FROM MyTable

NULL Handling Functions Compared

The following table shows the various SQL comparison functions. Each function returns one value if the comparison tests True (A equals B) and another value if the comparison tests False (A not equal to B):

SQL Function

Comparison Test

Return Value

NULLIF

expression1 = expression2

True = NULL

False = expression1

[IFNULL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_ifnull) (2 argument form)

expression1 = NULL

True = expression2

False = NULL

[COALESCE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_coalesce)

expression1 = NULL, expression2 = NULL, ...

True = test expression2

False = expression1

[ISNULL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_isnull)

expression1 = NULL

True = expression2

False = expression1

[NVL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_nvl)

expression1 = NULL

True = expression2

False = expression1

[IFNULL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_ifnull) (3 argument form)

expression1 = NULL

True = expression2

False = expression3

Examples

The following example uses the NULLIF function to set to null the display field of all records with Age=20:

SELECT Name,Age,NULLIF(Age,20) AS Nulled20
FROM Sample.Person

 

See Also

*   [CASE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_case) command
    
*   [COALESCE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_coalesce) function
    
*   [IFNULL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_ifnull) function
    
*   [ISNULL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_isnull) function
    
*   [NVL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_nvl) function
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_now "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_nvl "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_nullif.xml**