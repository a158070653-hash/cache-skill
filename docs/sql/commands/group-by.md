# GROUP BY

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_groupby

---

 

Caché SQL Reference

GROUP BY

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_grant "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_having "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_COMMANDS "SQL Commands") \]  >  \[ [GROUP BY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_groupby "GROUP BY") \]

Go to: Description See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A SELECT clause that groups the resulting rows of a query according to one or more columns.

Synopsis

SELECT ...
GROUP BY field

Arguments

field

One or more fields from which data is being retrieved. Either a single field name or a comma-separated list of field names.

Description

GROUP BY is a clause of the [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select) statement. The optional GROUP BY clause appears after the FROM clause and the optional WHERE clause, and before the optional HAVING and ORDER BY clauses.

The GROUP BY clause takes the resulting rows of a query and breaks them up into individual groups according to one or more database columns. When you use SELECT in conjunction with GROUP BY, one row is retrieved for each distinct value of the GROUP BY fields. GROUP BY treats fields with NULL (no specified value) as a separate distinct value group.

The GROUP BY clause is conceptually similar to the Caché extension %FOREACH, but GROUP BY operates on an entire query, while %FOREACH allows selection of aggregates on sub-populations without restricting the entire query population.

Specifying a Field

The simplest form of a GROUP BY clause specifies a single field, such as GROUP BY Home\_State. The field must be specified by its column name. You cannot specify a field by column alias or by column number. You cannot specify an aggregate field; attempting to do so generates an SQLCODE -19 error.

Valid field values include the following: A column name (GROUP BY Home\_State); an %ID (which returns all rows); a scalar function specifying a column name (GROUP BY ROUND(Age,-1)); a collation function specifying a column name (GROUP BY %EXACT(Home\_City)).

A GROUP BY clause can use the arrow syntax (–>) operator to specify a field in a table that is not the base table. For example: GROUP BY Company->Name. For further details, refer to [Implicit Joins](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_implicitjoins) in Using Caché SQL.

Specifying a literal in a GROUP BY clause returns 1 row; which row is returned is indeterminate. Thus, specifying 7, 'Chicago', '', 0, or NULL all return 1 row.

Aggregate Functions with GROUP BY and DISTINCT BY

The GROUP BY clause is applied before [aggregate functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_aggregatefunctions) are calculated. In the following example, the COUNT aggregate function counts the number of rows in each GROUP BY group:

SELECT Home\_State,COUNT(Home\_State)
FROM Sample.Person
GROUP BY Home\_State

 

The [DISTINCT BY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_distinct) clause is applied after aggregate functions are calculated. In the following example, the COUNT aggregate function counts the number of rows in the entire table:

SELECT DISTINCT BY(Home\_State) Home\_State,COUNT(Home\_State)
FROM Sample.Person

 

In order to calculate an aggregate function for the entire table, rather than a GROUP BY group, you can specify a select-item subquery:

SELECT Home\_State,(SELECT COUNT(Home\_State) FROM Sample.Person)
FROM Sample.Person
GROUP BY Home\_State

 

A GROUP BY clause should not be used with a [DISTINCT clause](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_distinct) when the select list consists of an aggregate field. For example, the following query is intended to return the distinct numbers of people who share the same Home\_State:

/\* This query DOES NOT apply the DISTINCT keyword \*/
/\* It is provided as a cautionary example  \*/
SELECT DISTINCT COUNT(\*) AS mynum
FROM Sample.Person 
GROUP BY Home\_State
ORDER BY mynum

 

This query did not return the expected results because it did not apply the DISTINCT keyword. To apply both a DISTINCT aggregate and a GROUP BY clause, use a subquery as shown in the following example:

SELECT DISTINCT \*
FROM (SELECT COUNT(\*) AS mynum
      FROM Sample.Person 
      GROUP BY Home\_State) AS Sub
ORDER BY Sub.mynum

 

This example successfully returns the distinct numbers of people who share the same Home\_State. For instance, if any Home\_State is shared by 8 people, the query returns an 8.

Letter Case and Optimization

By default, GROUP BY groups together alphabetic values based on their uppercase letter collation. Field values that differ only in letter case are grouped together. Grouped field values are returned in all uppercase letters. To group values by original letter case, or to display the returned values for a grouped field in their original letter case, use the [%EXACT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_exact) collation function. This is shown in the following examples, which assume that Sample.Person contains records with Home\_City field values of ‘New York’ and ‘new york’:

SELECT Home\_City FROM Sample.Person GROUP BY Home\_City
/\* groups together Home\_City values by their uppercase letter values
   returns the name of each grouped city in uppercase letters. 
   Thus, 'NEW YORK' is returned.                          \*/

 

SELECT %EXACT(Home\_City) FROM Sample.Person GROUP BY Home\_City
/\* groups together Home\_City values by their uppercase letter values
   returns the name of each grouped city in original letter case. 
   Thus, 'New York' or 'new york' may be returned, but not both. \*/

 

SELECT Home\_City FROM Sample.Person GROUP BY %EXACT(Home\_City)
/\* groups together Home\_City values by their original letter case
   returns the name of each grouped city in original letter case. 
   Thus, both 'New York' and 'new york' are returned.
   Optimization is not used. \*/

 

You can configure optimization for queries that contain a GROUP BY clause using the Management Portal. Select \[Home\] > \[Configuration\] > \[General SQL Settings\]. View and set the DISTINCT Optimization Turned ON option. (This optimization also works for the [DISTINCT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_distinct) clause.) The default is “Yes”. When this optimization is in effect, grouping of alphabetic values is done using their uppercase letter collation. For further details, refer to [SQL configuration settings](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RACS_Settings#RACS_Category_SQL) described in Caché Advanced Configuration Settings Reference.

You can also set this system-wide option to 1 or 0 with the [$SYSTEM.SQL.SetFastDistinct()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#SetFastDistinct) method:

  WRITE $SYSTEM.SQL.SetFastDistinct(1)

This optimization takes advantage of indices for the selected field(s). It is therefore only meaningful if an index exists for one or more of the selected fields. It collates field values as they are stored in the index; alphabetic strings are returned in all uppercase letters. You can set this system-wide option, then override it for specific queries by using the %EXACT collation function to preserve letter case.

%ROWID

Specifying a GROUP BY clause causes a [cursor-based Embedded SQL query](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_cursor) to not set the [%ROWID](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_pcROWID) variable. %ROWID is not set even when GROUP BY does not limit the rows returned. This is shown in the following example:

   SET %ROWID\=999
   &sql(DECLARE EmpCursor CURSOR FOR 
        SELECT Name, Home\_State
        INTO :name,:state FROM Sample.Person
        WHERE Home\_State %STARTSWITH 'M' 
        GROUP BY Home\_State)
   &sql(OPEN EmpCursor)
   FOR { &sql(FETCH EmpCursor)
        QUIT:SQLCODE  
        WRITE !,"RowID: ",%ROWID," row count: ",%ROWCOUNT
        WRITE " Name=",name," State=",state
  }
   &sql(CLOSE EmpCursor)

 

This change of query behavior only applies to cursor-based Embedded SQL SELECT queries. Dynamic SQL SELECT queries and non-cursor Embedded SQL SELECT queries never set %ROWID.

Transaction Committed Changes

A query containing a GROUP BY clause does not support READ COMMITTED isolation level. In a transaction defined as READ COMMITTED, a SELECT statement without a GROUP BY clause returns only data modifications that have been committed; in other words, it returns the state of the data before the current transaction. A SELECT statement with a GROUP BY clause returns all data modifications made, whether or not they have been committed.

See Also

*   [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select)
    
*   [DISTINCT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_distinct) clause
    
*   [JOIN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_join)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_grant "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_having "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_groupby.xml**