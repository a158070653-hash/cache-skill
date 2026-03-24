# JSON ARRAY

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_jsonarray

---

 

Caché SQL Reference

JSON\_ARRAY

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_isnumeric "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_jsonobject "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [JSON\_ARRAY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_jsonarray "JSON_ARRAY") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A conversion function that returns data as a JSON array.

Synopsis

JSON\_ARRAY(select-items \[NULL ON NULL | ABSENT ON NULL\])

Arguments

select-items

An expression or a comma-separated list of expressions. These expressions can include column names, aggregate functions, arithmetic expressions, literals, and the literal NULL.

ABSENT ON NULL

NULL ON NULL

Optional — A keyword phrase specifying how to represent NULL values in the returned JSON array. NULL ON NULL (the default) represents NULL (absent) data with the word null (not quoted). ABSENT ON NULL omits NULL data from the JSON array; it does not leave a placeholder comma. This keyword phrase has no effect on empty string values.

Description

JSON\_ARRAY takes an expression or (more commonly) a comma-separated list of expressions and returns a JSON array containing those values. JSON\_ARRAY can be combined in a SELECT statement with other types of select-items. JSON\_ARRAY can be specified in other locations where an SQL function can be used, such as in a WHERE clause.

The returned JSON array has the following format:

\[ element1 , element2 , element3 \]

JSON\_ARRAY returns each array element value as either a string (enclosed in double quotes), or a number. Numbers are returned in canonical format. A numeric string is returned as a literal, enclosed in double quotes. All other data types (for example, Date or $List) are returned as a string, with the current %SelectMode determining the format of the returned value.

You can apply a collation function to a JSON\_ARRAY. Caché applies the collation after JSON array formatting. Therefore, %UPPER(JSON\_ARRAY(f1,f2)) converts all the JSON array element values to uppercase. %SQLUPPER inserts a space before the JSON array, not before the elements of the array; therefore it does not force numbers to be parsed as strings. Collation functions that strip punctuation (such as %ALPHAUP) remove the enclosing brackets from the returned value, making it no longer a JSON array. You cannot apply a collation function to one of the select-items within JSON\_ARRAY.

JSON\_ARRAY does not support asterisk (\*) syntax as a way to specify all fields in a table. It does support the COUNT(\*) aggregate function.

The returned JSON array column is labeled as an Expression (by default); you can specify a column alias for a JSON\_ARRAY.

ABSENT ON NULL

If you specify the optional ABSENT ON NULL keyword phrase, a column value which is NULL (or the NULL literal) is not included in the JSON array. No placeholder is included in the JSON array. This can result in JSON arrays with different numbers of elements. For example, the following program returns JSON arrays where for some records the 3rd array element is Age, and for other records the 3rd element is FavoriteColors:

SELECT JSON\_ARRAY(%ID,Name,FavoriteColors,Age ABSENT ON NULL) FROM Sample.Person

If you specify no keyword phrase, the default is NULL ON NULL: NULL is represented by the word null (not delimited by quotes) as a comma-separated array element. Thus all JSON arrays returned by a JSON\_ARRAY function will have the same number of array elements.

Examples

The following example applies JSON\_ARRAY to format a JSON array containing a comma-separated list of field values:

SELECT TOP 3 JSON\_ARRAY(%ID,Name,DOB,Age,Home\_State) FROM Sample.Person

 

The following example applies JSON\_ARRAY to format a JSON array with a single element containing the Name field values:

SELECT TOP 3 JSON\_ARRAY(Name) FROM Sample.Person

 

The following example applies JSON\_ARRAY to format a JSON array containing literals and field values:

SELECT TOP 3 JSON\_ARRAY('Employee from',%TABLENAME,Name,SSN) FROM Sample.Employee

 

The following example applies JSON\_ARRAY to format a JSON array containing nulls and field values:

SELECT JSON\_ARRAY(Name,FavoriteColors) FROM Sample.Person
WHERE Name %STARTSWITH 'S'

 

The following example applies JSON\_ARRAY to format a JSON array containing field values from joined tables:

SELECT TOP 3 JSON\_ARRAY(E.%TABLENAME,E.Name,C.%TABLENAME,C.Name) 
FROM Sample.Employee AS E,Sample.Company AS C

 

The following example uses JSON\_ARRAY in a WHERE clause to perform a Contains test on multiple columns without using OR syntax:

SELECT Name,Home\_City,Home\_State FROM Sample.Person
WHERE JSON\_ARRAY(Name,Home\_City,Home\_State) \[ 'X'

 

See Also

*   [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select) statement
    
*   [WHERE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_where) clause
    
*   [JSON\_OBJECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_jsonobject) function
    
*   [Overview of Predicates](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_predicates)
    
*   “[Querying the Database](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_queries)” chapter in Using Caché SQL
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_isnumeric "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_jsonobject "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_jsonarray.xml**