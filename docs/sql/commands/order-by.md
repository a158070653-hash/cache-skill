# ORDER BY

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_order

---

 

Caché SQL Reference

ORDER BY

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_open "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_revoke "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_COMMANDS "SQL Commands") \]  >  \[ [ORDER BY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_order "ORDER BY") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A SELECT clause that specifies the sorting of rows in a result set.

Synopsis

ORDER BY ordering-item \[ASC | DESC\]{,ordering-item \[ASC | DESC\] ...}

Arguments

ordering-item

A [literal](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_langelements#GSQL_langelements_literals) or scalar expression that determines the sort order. A column name or other column reference. An ORDER BY clause can contain one or more ordering items. Multiple items are separated by commas.

ASC

DESC

Optional — Sort in either ascending order (ASC), or descending order (DESC). The default is ascending order.

Description

The ORDER BY clause sorts the records in a query result set by the data values of a specified column or a comma-separated sequence of columns. This statement operates on a single result set, either from a [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select) statement or from a [UNION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_union) of multiple [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select) statements.

An ORDER BY clause is used by a query executed in [Dynamic SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_dynsql). An ORDER BY clause performs no operation in a query executed in cursor-based Embedded SQL.

If a SELECT query does not specify an ORDER BY clause, the returned record order is not predictable.

Specifying Sort Columns

You can specify a single column on which to sort, or multiple columns as a comma-separated list. Sorting is done by the first listed column, then within that column by the second listed column, and so on.

Columns can be specified by column name, column alias, or column number. An ORDER BY clause can specify any combination of these. You can specify an expression in the select-item list by column alias or column number. If the first character of the ordering-item is a number, Caché assumes you are specifying a column number. Otherwise a column name or column alias is assumed. Note that column names and column aliases are not case sensitive.

You cannot specify a column name, column alias, or column number using a Dynamic SQL ? input parameter or an Embedded SQL :var host variable.

Column Name

The following ORDER BY clause sorts by column names:

SELECT Name,Home\_State,DOB
FROM Sample.Person
ORDER BY Home\_State,Name

 

You can sort by column name whether or not the sort column is in the select-item list. The following example returns the same records in the same order as the previous example:

SELECT Name,DOB
FROM Sample.Person
ORDER BY Home\_State,Name

 

For obvious reasons, you cannot sort by column alias or column number unless the sort column is in the select-item list.

An ORDER BY clause can use the [arrow syntax](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_implicitjoins) (–>) operator to specify a field in a table that is not the base table, as shown in the following example:

SELECT Name,Company\->Name AS CompName
FROM Sample.Employee ORDER BY Company\->Name,Name

 

For further details, refer to [Implicit Joins](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_implicitjoins) in Using Caché SQL.

Column Alias

The following ORDER BY clause sorts by [column alias](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select#RSQL_select_selectitem_calias):

SELECT Name,Home\_State AS HS,DOB
FROM Sample.Person
ORDER BY HS,Name

 

A column alias can be the same as a column name (though this is not recommended). If column aliases are provided, ORDER BY first references column alias and then references any unaliased column names. If ambiguity exists, the ORDER BY clause generates an error. However, if a column alias is the same as an aliased column name, this apparent ambiguity does not generate an error, but can produce unexpected results. This is shown in the following example:

SELECT Name AS Moniker,Home\_City AS Name
FROM Sample.Person
ORDER BY Name

 

Because aliases are referenced first, this example orders the data by Home\_City. This is probably not what was intended. If the Name column was not aliased, an error would occur. If Home\_City was given a different alias, ORDER BY would find no match on aliases, and would then check column names; it would order by the Name column.

You can use a column alias to sort by an expression in the select-item list, as shown in the following example:

SELECT Name,Age,$PIECE(AVG(Age)\-Age,'.',1) AS AgeDev
FROM Sample.Employee ORDER BY AgeDev,Name

 

Column Number

The following ORDER BY clause sorts by column number (the numeric sequence of the retrieved columns, as specified in the SELECT select-item list):

SELECT Name,Home\_State,DOB
FROM Sample.Person
ORDER BY 2,1

 

Using a column number in ORDER BY that does not correspond to a SELECT list column results in an SQLCODE -5 error.

Column numbers refer to the position in the SELECT clause list. They do not refer to the positions of columns in the table itself. You can specify a column number as an integer, or as any number. Standard truncation rules apply to resolve to an integer; for example, 1.99 resolves by truncation to 1.

You can use a column number to sort by an expression in the select-item list, as shown in the following example:

SELECT Name,Age,$PIECE(AVG(Age)\-Age,'.',1)
FROM Sample.Employee ORDER BY 3,Name

 

Specifying Collation

Sorting is done in collation sequence order. Sorting is done based on the collation specified for a column when it was created. The default for character columns is %SQLUPPER. For expressions, the default collation is %EXACT. You can override the default by using an expression such as %SQLSTRING(fieldxyz) in your ORDER BY clause.

The default ascending collation sequence considers NULL to be the lowest value, followed by the empty string (''). ORDER BY does not distinguish between the empty string and strings that consist only of blank spaces.

Sorting can be specified for each column in ascending or descending coalition sequence order, as specified by the optional ASC (ascending) or DESC (descending) keyword following the column identifier. If ASC or DESC is not specified, ORDER BY sorts that column in ascending order. You cannot specify the ASC or DESC keyword using a Dynamic SQL ? input parameter or an Embedded SQL :var host variable.

This is shown in the following example:

SELECT A,B,C,M,E,X,J
FROM LetterTable
ORDER BY 3,7 DESC,1 ASC

sorts the data values of the third-listed item (C) in the SELECT clause list in ascending order; within this, it sorts the seventh-listed item (J) values in descending order; within this, it sorts the first-listed item (A) values in ascending order.

If the collation specified for a column is alphanumeric, leading numbers are sorted in collation sequence, not integer sequence. For example, a street address commonly consists of an integer building number followed by a street name. You can use the [%PLUS](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_plus_pct) (or %MINUS) collation function to order such mixed numeric strings in numeric sequence. Compare the following two examples:

SELECT Name,Home\_Street FROM Sample.Person
ORDER BY Home\_Street

 

SELECT Name,Home\_Street FROM Sample.Person
ORDER BY %PLUS(Home\_Street)

 

NLS Collation

If you have specified a non-default NLS collation, you must make sure that all collations are aligned and use the exact same national collation sequence. This includes not only globals used by the tables, but also globals used for indexes, in temporary files such as in CACHETEMP and process-private globals. For further details, refer to “[SQL Collation and NLS Collations](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_basics#GSQL_basics_collation_sqlnls)” in Using Caché SQL.

Restrictions

If your SELECT query specifies an ORDER BY clause, the resulting data is not updateable. Thus, if you specify a subsequent DECLARE CURSOR FOR UPDATE statement, the FOR UPDATE clause is ignored, and the cursor is declared read-only.

If the ORDER BY applies to a [UNION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_union), an ordering item must be a number or a simple column name. It cannot be an expression. If a column name is used, it refers to result columns as they are named in the first SELECT list of the UNION.

When used in a subquery, an ORDER BY clause must be paired with a [TOP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_top) clause. If both the main query and its subquery contain an ORDER BY clause, the query processor executes the subquery ORDER BY clause, even when it ignores the main query ORDER BY as meaningless.

ORDER BY and Long Global References

Under certain circumstances, running a query with an ORDER BY clause may result in an error such as:

Error: \[SQLCODE: <-400>:<Fatal error occurred>\] \[Cache Error: <<SUBSCRIPT>%0ABMod+7^CacheSql4 ^||%sql.temp(36,0,"")>\] 
\[Details: <ServerLoop - Query Fetch>\]  
\[%msg: <Unexpected error occurred: <SUBSCRIPT>%0ABMod+7^CacheSql4 ^||%sql.temp(36,0,"")>\] 

This occurs because of a limitation in the maximum encoded length of a global reference, which is a fixed Caché system limit. To prevent this problem, use a truncation length in the collation setting for the field that is the basis of the ORDER BY clause. For example, if the query includes

SELECT ... ORDER BY ResourceText 

then specify the truncation limit for the ResourceText field. If you use a collation of SQLUPPER(180), Caché truncates the collated value of the field at 180 characters. Remember that if the field contents are not unique within the first 180 characters, the data may be slightly misordered, but this is unlikely to occur. If this does occur, you can attempt to avoid displaying misordered data by using a larger value for truncation; however, if a value is too large, it will result in a <SUBSCRIPT> error.

Note also that the maximum length is for the entire encoded length of the global reference, including the length of the global name. It is not simply per subscript.

Examples

The following two examples show different ways of specifying sort columns in an ORDER BY clause. The following two queries are equivalent; the first uses column names as sort items, the second uses column numbers (the sequence number of the items in the select-item list):

SELECT Name,Age,Home\_State
FROM Sample.Person
ORDER BY Home\_State,Age DESC

 

SELECT Name,Age,Home\_State
FROM Sample.Person
ORDER BY 3,2 DESC

 

Dynamic SQL can use an input parameter to supply a literal value to an ORDER BY clause; it cannot use an input parameter to supply a field name, field alias, field number, or collation keyword. The following Dynamic SQL example uses an input parameter to sort result set records by first name:

  ZNSPACE "SAMPLES"
  SET myquery \= 4
  SET myquery(1) \= "SELECT TOP ? Name,Age,"
  SET myquery(2) \= "CURRENT\_DATE AS Today"
  SET myquery(3) \= "FROM Sample.Person WHERE Age > ?"
  SET myquery(4) \= "ORDER BY $PIECE(Name,',',?)"
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET qStatus \= tStatement.%Prepare(.myquery)
    IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
  SET rset \= tStatement.%Execute(10,60,2)
  DO rset.%Display()
  WRITE !,"%Display SQLCODE=",rset.%SQLCODE

 

The following cursor-based Embedded SQL example performs the same operation:

  SET topnum\=10,agemin\=60,firstname\=2
  &sql(DECLARE pCursor CURSOR FOR
       SELECT TOP :topnum Name,Age,CURRENT\_DATE AS Today
       INTO :name,:years,:today FROM Sample.Person
       WHERE Age \> :agemin
       ORDER BY $PIECE(Name,',',:firstname) )
  &sql(OPEN pCursor)
  FOR { &sql(FETCH pCursor)
        QUIT:SQLCODE
        WRITE "Name=",name," Age=",years," today=",today,!
      }
  &sql(CLOSE pCursor)

 

See Also

*   [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select), [UNION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_union)
    
*   [TOP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_top) clause
    
*   “[Querying the Database](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_queries)” chapter in Using Caché SQL
    
*   [SQLCODE error messages](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RERR_sql) listed in the Caché Error Reference
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_open "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_revoke "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_order.xml**