# JSON OBJECT

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_jsonobject

---

 

Caché SQL Reference

JSON\_OBJECT

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_jsonarray "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_justify "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [JSON\_OBJECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_jsonobject "JSON_OBJECT") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A conversion function that returns data as a JSON object.

Synopsis

JSON\_OBJECT(select-items \[NULL ON NULL | ABSENT ON NULL\])

Arguments

select-items

A key:value pair or a comma-separated list of key:value pairs. A key is a user-specified literal string delimited with single quotes. A value can be a column name, an aggregate function, an arithmetic expression, a numeric or string literal, or the literal NULL.

ABSENT ON NULL

NULL ON NULL

Optional — A keyword phrase specifying how to represent NULL values in the returned JSON object. NULL ON NULL (the default) represents NULL (absent) data with the word null (not quoted). ABSENT ON NULL omits NULL data from the JSON object; it removes the key:value pair when value is NULL and does not leave a placeholder comma. This keyword phrase has no effect on empty string values.

Description

JSON\_OBJECT takes a comma-separated list of key:value pairs (for example, 'mykey':colname) and returns a JSON object containing those values. You can specify any single-quoted string as a key name; JSON\_OBJECT does not enforce any naming conventions or uniqueness check for key names. You can specify for value a column name or other expression.

JSON\_OBJECT can be combined in a SELECT statement with other types of select-items. JSON\_OBJECT can be specified in other locations where an SQL function can be used, such as in a WHERE clause.

A returned JSON object has the following format:

{ "key1" : "value1" , "key2" : "value2" , "key3" : "value3" }

JSON\_OBJECT returns object values as either a string (enclosed in double quotes), or a number. Numbers are returned in canonical format. A numeric string is returned as a literal, enclosed in double quotes. All other data types (for example, Date or $List) are returned as a string, with the current [%SelectMode](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_dynsql#GSQL_dynsql_selectmode) determining the format of the returned value. JSON\_OBJECT returns both key and value values in DISPLAY or ODBC mode if that is the select mode for the query.

You can apply a collation function to a JSON\_OBJECT. Caché applies the collation after JSON object formatting. Therefore, %UPPER(JSON\_OBJECT('k1':f1,'k2':f2)) converts all the JSON object key and value strings to uppercase. %SQLUPPER inserts a space before the JSON object, not before the values within the object; therefore it does not force numbers to be parsed as strings. Collation functions that strip punctuation (such as %ALPHAUP) remove the enclosing curly braces from the returned value, making it no longer a JSON object. You cannot apply a collation function to one of the select-items within JSON\_OBJECT.

JSON\_OBJECT does not support asterisk (\*) syntax as a way to specify all fields in a table.

The returned JSON object column is labeled as an Expression (by default); you can specify a column alias for a JSON\_OBJECT.

ABSENT ON NULL

If you specify the optional ABSENT ON NULL keyword phrase, a column value which is NULL (or the NULL literal) is not included in the JSON object. No placeholder is included in the JSON object. This can result in JSON objects with different numbers of key:value pairs. For example, the following program returns JSON objects where for some records the 3rd key:value pair is Age, and for other records the 3rd key:value pair is FavoriteColors:

SELECT JSON\_OBJECT('id':%ID,'name':Name,'colors':FavoriteColors,'years':Age ABSENT ON NULL) FROM Sample.Person

If you specify no keyword phrase, the default is NULL ON NULL: NULL is represented by the word null (not delimited by quotes) as the value of the key:value pair. Thus all JSON objects returned by a JSON\_OBJECT function will have the same number of key:value pairs.

Examples

The following example applies JSON\_OBJECT to format a JSON object containing field values:

  ZNSPACE "SAMPLES"
  SET myquery \= 2
  SET myquery(1) \= "SELECT TOP 3 JSON\_OBJECT('id':%ID,'name':Name,'birth':DOB,"
  SET myquery(2) \= "'age':Age,'state':Home\_State) FROM Sample.Person"
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET qStatus \= tStatement.%Prepare(.myquery)
    IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
  SET rset \= tStatement.%Execute()
  WHILE rset.%Next() {DO rset.%Print(" ^ ")}
  WRITE !,"Total row count=",rset.%ROWCOUNT

 

The following example applies JSON\_OBJECT to format a JSON object containing literals and field values:

  ZNSPACE "SAMPLES"
  SET myquery \= 2
  SET myquery(1) \= "SELECT TOP 3 JSON\_OBJECT('lit':'Employee from','t':%TABLENAME,"
  SET myquery(2) \= "'name':Name,'num':SSN) FROM Sample.Employee"
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET qStatus \= tStatement.%Prepare(.myquery)
    IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
  SET rset \= tStatement.%Execute()
  WHILE rset.%Next() {DO rset.%Print(" ^ ")}
  WRITE !,"Total row count=",rset.%ROWCOUNT

 

The following example applies JSON\_OBJECT to format a JSON object containing nulls and field values:

  ZNSPACE "SAMPLES"
  SET myquery \= 2
  SET myquery(1) \= "SELECT JSON\_OBJECT('name':Name,'colors':FavoriteColors) FROM Sample.Person"
  SET myquery(2) \= " WHERE Name %STARTSWITH 'S'"
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET qStatus \= tStatement.%Prepare(.myquery)
    IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
  SET rset \= tStatement.%Execute()
  WHILE rset.%Next() {DO rset.%Print(" ^ ")}
  WRITE !,"Total row count=",rset.%ROWCOUNT

 

The following example applies JSON\_OBJECT to format a JSON object containing field values from joined tables:

  ZNSPACE "SAMPLES"
  SET myquery \= 2
  SET myquery(1) \= "SELECT TOP 3 JSON\_OBJECT('e.t':E.%TABLENAME,'e.name':E.Name,'c.t':C.%TABLENAME,"
  SET myquery(2) \= "'c.name':C.Name) FROM Sample.Employee AS E,Sample.Company AS C"
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET qStatus \= tStatement.%Prepare(.myquery)
    IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
  SET rset \= tStatement.%Execute()
  WHILE rset.%Next() {DO rset.%Print(" ^ ")}
  WRITE !,"Total row count=",rset.%ROWCOUNT

 

The following example uses JSON\_OBJECT in a WHERE clause to perform a Contains test on multiple columns without using OR syntax:

  ZNSPACE "SAMPLES"
  SET myquery \= 2
  SET myquery(1) \= "SELECT Name,Home\_City,Home\_State FROM Sample.Person"
  SET myquery(2) \= " WHERE JSON\_OBJECT('name':Name,'city':Home\_City,'state':Home\_State) \[ 'X'"
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET qStatus \= tStatement.%Prepare(.myquery)
    IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
  SET rset \= tStatement.%Execute()
  WHILE rset.%Next() {DO rset.%Print(" ^ ")}
  WRITE !,"Total row count=",rset.%ROWCOUNT

 

See Also

*   [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select) statement
    
*   [WHERE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_where) clause
    
*   [JSON\_ARRAY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_jsonarray) function
    
*   [Overview of Predicates](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_predicates)
    
*   “[Querying the Database](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_queries)” chapter in Using Caché SQL
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_jsonarray "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_justify "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_jsonobject.xml**