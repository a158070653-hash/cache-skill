# IFNULL

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_ifnull

---

 

Caché SQL Reference

IFNULL

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_hour "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_instr "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [IFNULL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_ifnull "IFNULL") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A function that tests for NULL and returns the appropriate expression.

Synopsis

IFNULL(expression-1,expression-2,expression-3)
{fn IFNULL(expression-1,expression-2)}

Arguments

expression-1

The expression to be evaluated to determine if it is NULL or not.

expression-2

An expression that is returned if expression-1 is NULL.

expression-3

Optional — An expression that is returned if expression-1 is not NULL. If expression-3 is not specified, a NULL value is returned when expression-1 is not NULL.

Description

Caché supports IFNULL as both an SQL general function and an ODBC scalar function. Note that while these two perform very similar operations, they are functionally different. The SQL general function supports three arguments. The ODBC scalar function supports two arguments. The two-argument forms of the SQL general function and the ODBC scalar function are not the same; they return different values when expression-1 is not null.

The SQL general function evaluates expression-1 and returns one of three values:

*   If expression-1 is NULL, expression-2 is returned.
    
*   If expression-1 is not NULL, expression-3 is returned.
    
*   If expression-1 is not NULL, and there is no expression-3, NULL is returned.
    

The ODBC scalar function evaluates expression-1 and returns one of two values:

*   If expression-1 is NULL, expression-2 is returned.
    
*   If expression-1 is not NULL, expression-1 is returned.
    

The possible data type(s) of expression-2 and expression-3 must be compatible with the data type of expression-1. If expression-2 and expression-3 have different data types, the data type IFNULL returns is the data type with the higher (more inclusive) [data type precedence](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datatype#RSQL_datatype_precedence). If expression-2 and expression-3 have different length, precision, or scale, IFNULL returns the greater length, precision, or scale of the two expressions.

Refer to [NULL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_langelements#GSQL_langelements_null) section of the “Language Elements” chapter of Using Caché SQL for further details on NULL handling.

Display-to-Logical Conversion

When executing in SELECTMODE=DISPLAY, SQL does not convert input arguments from Display to Logical. Therefore, if expression-1 is a %List field, the expression-2 field must be specified as a %List. In the following example FavoriteColors is a %List field, so the 'No Preference' value must be specified as a %List:

SELECT Name,
       IFNULL(FavoriteColors,$LISTBUILD('No Preference')) AS ColorPref
FROM Sample.Person

 

If the SELECTMODE is LOGICAL or ODBC, or SELECTMODE is RUNTIME, and RUNTIMEMODE is DISPLAY, this Display-to-Logical conversion is performed, so the expression-2 field can be specified as a string. Both Dynamic SQL and Embedded SQL perform this Display-to-Logical conversion, as shown in the following two examples:

  ZNSPACE "SAMPLES"
  SET myquery\=3
    SET myquery(1)\="SELECT Name,"
    SET myquery(2)\="IFNULL(FavoriteColors,'No Preference') AS ColorChoice "
    SET myquery(3)\="FROM Sample.Person"
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET qStatus \= tStatement.%Prepare(.myquery)
   IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
  SET rset \= tStatement.%Execute()
  DO rset.%Display()
  WRITE !,"End of data"

 

 &sql(DECLARE C1 CURSOR FOR
    SELECT Name,IFNULL(FavoriteColors,'No Preference')
    INTO :name,:colorpref
    FROM Sample.Person
    ORDER BY Name )
 &sql(OPEN C1)
 &sql(FETCH C1)
 WHILE (SQLCODE \= 0) {
     WRITE name, ":  ", colorpref,!
    &sql(FETCH C1)
 }
    &sql(CLOSE C1)

 

NULL Handling Functions Compared

The following table shows the various SQL comparison functions. Each function returns one value if the comparison tests True (A equals B) and another value if the comparison tests False (A not equal to B):

SQL Function

Comparison Test

Return Value

[NULLIF](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_nullif)

expression1 = expression2

True = NULL

False = expression1

IFNULL (2 argument form)

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

IFNULL (3 argument form)

expression1 = NULL

True = expression2

False = expression3

Examples

In the following example, the general function and the ODBC scalar function both returns the second expression (99) because the first expression is NULL:

SELECT IFNULL(NULL,99) AS NullGen,{fn IFNULL(NULL,99)} AS NullODBC

 

In the following example, the general function and the ODBC scalar function examples return different values. The general function returns <null> because the first expression is not NULL. The ODBC example returns the first expression (33) because the first expression is not NULL:

SELECT IFNULL(33,99) AS NullGen,{fn IFNULL(33,99)} AS NullODBC

 

The following Dynamic SQL example returns the string 'No Preference' if FavoriteColors is NULL; otherwise, it returns NULL:

  ZNSPACE "SAMPLES"
  SET myquery\=3
    SET myquery(1)\="SELECT Name,"
    SET myquery(2)\="IFNULL(FavoriteColors,'No Preference') AS ColorChoice "
    SET myquery(3)\="FROM Sample.Person"
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET qStatus \= tStatement.%Prepare(.myquery)
   IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
  SET rset \= tStatement.%Execute()
  DO rset.%Display()
  WRITE !,"End of data"

 

The following Dynamic SQL example returns the string 'No Preference' if FavoriteColors is NULL; otherwise, it returns the value of FavoriteColors:

  ZNSPACE "SAMPLES"
  SET myquery\=3
    SET myquery(1)\="SELECT Name,"
    SET myquery(2)\="IFNULL(FavoriteColors,'No Preference',FavoriteColors) AS ColorChoice "
    SET myquery(3)\="FROM Sample.Person"
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET tStatement.%SelectMode\=2
  SET qStatus \= tStatement.%Prepare(.myquery)
   IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
  SET rset \= tStatement.%Execute()
  DO rset.%Display()
  WRITE !,"End of data"

 

The following example returns the string 'No Preference' if FavoriteColors is NULL; otherwise, it returns the string 'Preference':

SELECT Name,
IFNULL(FavoriteColors,'No Preference','Preference') AS ColorPref
FROM Sample.Person

 

The following ODBC syntax examples return the string 'No Preference' if FavoriteColors is NULL, otherwise they return the FavoriteColors data value:

SELECT Name,
       {fn IFNULL(FavoriteColors,$LISTBUILD('No Preference'))} AS ColorPref
FROM Sample.Person

 

  ZNSPACE "SAMPLES"
  SET myquery\=3
    SET myquery(1)\="SELECT Name,"
    SET myquery(2)\="{fn IFNULL(FavoriteColors,'No Preference')} AS ColorChoice "
    SET myquery(3)\="FROM Sample.Person"
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET tStatement.%SelectMode\=1
  SET qStatus \= tStatement.%Prepare(.myquery)
   IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
  SET rset \= tStatement.%Execute()
  DO rset.%Display()
  WRITE !,"End of data"

 

The following example uses IFNULL in the WHERE clause. It selects people under the age of 21 who do not have favorite color preferences. If FavoriteColors is NULL, IFNULL returns the Age field value, which is used for the condition test:

SELECT Name,FavoriteColors,Age
FROM Sample.Person
WHERE 21 \> IFNULL(FavoriteColors,Age)
ORDER BY Age

 

Refer to the [NULL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_null) predicate (IS NULL, IS NOT NULL) for similar functionality.

See Also

*   [CASE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_case) command
    
*   [COALESCE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_coalesce) function
    
*   [ISNULL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_isnull) function
    
*   [NULLIF](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_nullif) function
    
*   [NVL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_nvl) function
    
*   [NULL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_null) predicate
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_hour "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_instr "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_ifnull.xml**