# NVL

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_nvl

---

 

Caché SQL Reference

NVL

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_nullif "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_object "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [NVL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_nvl "NVL") \]

Go to: Description Example See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A function that tests for NULL and returns the appropriate expression.

Synopsis

NVL(check-expression,replace-expression)

Arguments

check-expression

The expression to be evaluated.

replace-expression

The expression that is returned if check-expression is NULL.

Description

NVL evaluates check-expression and returns one of two values:

*   If check-expression is NULL, replace-expression is returned.
    
*   If check-expression is not NULL, check-expression is returned.
    

The arguments check-expression and replace-expression can have any data type. If their data types are different, Caché converts replace-expression to the data type of check-expression before comparing them. The data type of the return value is always the same as the data type of check-expression, unless check-expression is character data, in which case the return value’s data type is VARCHAR2.

Note that NVL is supported for Oracle compatibility, and is the same as the ISNULL function.

Refer to [NULL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_langelements#GSQL_langelements_null) section of the “Language Elements” chapter of Using Caché SQL for further details on NULL handling.

NULL Handling Functions Compared

The following table shows the various SQL comparison functions. Each function returns one value if the comparison tests True (A equals B) and another value if the comparison tests False (A not equal to B):

SQL Function

Comparison Test

Return Value

[NULLIF](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_nullif)

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

NVL

expression1 = NULL

True = expression2

False = expression1

[IFNULL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_ifnull) (3 argument form)

expression1 = NULL

True = expression2

False = expression3

Example

This following example returns the replace-expression (99) because the check-expression is NULL:

SELECT NVL(NULL,99) AS NullTest

 

This following example returns the check-expression (33) because check-expression is not NULL:

SELECT NVL(33,99) AS NullTest

 

The following Dynamic SQL example returns the string 'No Preference' if FavoriteColors is NULL; otherwise, it returns the value of FavoriteColors:

  ZNSPACE "SAMPLES"
  SET myquery\=3
    SET myquery(1)\="SELECT Name,"
    SET myquery(2)\="NVL(FavoriteColors,'No Preference') AS ColorChoice "
    SET myquery(3)\="FROM Sample.Person"
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET qStatus \= tStatement.%Prepare(.myquery)
   IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
  SET rset \= tStatement.%Execute()
  DO rset.%Display()
  WRITE !,"End of data"

 

See Also

*   [CASE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_case) command
    
*   [COALESCE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_coalesce) function
    
*   [IFNULL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_ifnull) function
    
*   [ISNULL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_isnull) function
    
*   [NULLIF](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_nullif) function
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_nullif "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_object "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_nvl.xml**