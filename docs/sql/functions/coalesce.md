# COALESCE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_coalesce

---

 

Caché SQL Reference

COALESCE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_charlength "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_concat "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [COALESCE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_coalesce "COALESCE") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A function that returns the value of the first expression that is not NULL.

Synopsis

COALESCE(expression,expression)

Arguments

expression

A series of expressions to be evaluated. Multiple expressions are specified as a comma-separated list. This expression list has a limit of 140 expressions.

Description

The COALESCE function evaluates a list of expressions in left-to-right order and returns the value of the first non-NULL expression. If all expressions evaluate to NULL, NULL is returned.

Non-numeric expressions (such as strings or dates) must all be of the same data type, and return a value of that data type. Specifying expressions with incompatible data types results in an SQLCODE -378, and a %msg error message value. You can use the [CAST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_cast) function to convert an expression to a compatible data type.

Numeric expressions may be of different data types. If you specify numeric expressions with different data types, the data type returned is the expression data type most compatible with all of the possible result values, the data type with the highest [data type precedence](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datatype#RSQL_datatype_precedence). The following data types are compatible and are specified in order of precedence (highest to lowest): DOUBLE, NUMERIC, BIGINT, INTEGER, SMALLINT, TINYINT.

A string is returned unchanged; leading and trailing blanks are retained. A number is returned in canonical form, with leading and trailing zeros removed.

For further details on NULL handling, refer to the [NULL and the Empty String](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_langelements#GSQL_langelements_null) section of “Language Elements” in Using Caché SQL.

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

COALESCE

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

The following Embedded SQL example takes a series of host variable values and returns the first (value d) that is not NULL. Note that the Caché ObjectScript empty string ("") is translated as NULL in Caché SQL:

  SET (a,b,c,e)\=""
  SET d\="firstdata"
  SET f\="nextdata"
  &sql(SELECT COALESCE(:a,:b,:c,:d,:e,:f) INTO :x)
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"The first non-null value is: ",x }

 

The following example compares the values of two columns in left-to-right order and returns the value of the first non-NULL column. The FavoriteColors column is NULL for some rows; the Home\_State column is never NULL. For COALESCE to compare the two, FavoriteColors must be cast as a string:

SELECT TOP 25 Name,FavoriteColors,Home\_State,
COALESCE(CAST(FavoriteColors AS VARCHAR),Home\_State) AS CoalesceCol
FROM Sample.Person

 

The following Dynamic SQL example compares COALESCE to the other NULL-processing functions:

  ZNSPACE "SAMPLES"
  SET myquery \= "SELECT TOP 50 %ID,"\_
                "IFNULL(FavoriteColors,'blank') AS Ifn2Col,"\_
                "IFNULL(FavoriteColors,'blank','value') AS Ifn3Col,"\_
                "COALESCE(CAST(FavoriteColors AS VARCHAR),Home\_State) AS CoalesceCol,"\_
                "ISNULL(FavoriteColors,'blank') AS IsnullCol,"\_
                "NULLIF(FavoriteColors,$LISTBUILD('Orange')) AS NullifCol,"\_
                "NVL(FavoriteColors,'blank') AS NvlCol"\_
                " FROM Sample.Person"
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET qStatus \= tStatement.%Prepare(myquery)
   IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
  SET rset \= tStatement.%Execute()
  DO rset.%Display()
  WRITE !,"End of data"

 

See Also

*   [CASE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_case) command
    
*   [IFNULL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_ifnull) function
    
*   [ISNULL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_isnull) function
    
*   [NULLIF](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_nullif) function
    
*   [NVL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_nvl) function
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_charlength "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_concat "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_coalesce.xml**