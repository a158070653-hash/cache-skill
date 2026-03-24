# DISTINCT

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_distinct

---

 

Caché SQL Reference

DISTINCT

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_delete "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dropdatabase "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_COMMANDS "SQL Commands") \]  >  \[ [DISTINCT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_distinct "DISTINCT") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A SELECT clause that specifies to return only distinct values.

Synopsis

SELECT \[DISTINCT \[BY (item {,item})\] \]  |  \[ALL\]
  select-item {,select-item2}

Arguments

DISTINCT

Optional — Returns rows for which the combined select-item value(s) are unique.

DISTINCT BY (item1 {,item2})

Optional — Returns select-item values for rows for which the BY (item) value(s) are unique.

ALL

Optional — Returns all rows in the result set. The default.

Description

The optional DISTINCT clause appears after the SELECT keyword and before the optional [TOP clause](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_top) and the first select-item.

The DISTINCT clause is applied to the result set of the [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select) statement. It limits the rows returned to those that contain a distinct (non-duplicate) value. If no DISTINCT clause is specified, the default is to display all the rows that fulfill the SELECT criteria. The ALL clause is the same as specifying no DEFAULT clause; if you specify ALL, SELECT returns all the rows in the table that fulfill the SELECT criteria.

The DISTINCT clause has two forms:

*   SELECT DISTINCT: Returns one row for each unique combination of select-item values. You can specify one or more than one select-items. For example, the following query returns a row with Home\_State and Age values for each unique combination of Home\_State and Age values:
    
    SELECT DISTINCT Home\_State,Age FROM Sample.Person
    
     
    
*   SELECT DISTINCT BY (item): Returns one row for each unique combination of item values. You can specify a single item or a comma-separated list of items. The specified item or item list must be enclosed in parentheses. Spaces may be specified or omitted between the BY keyword and the parentheses. The select-item list may, but does not have to, include the specified item(s). For example, the following query returns a row with Name and Age values for each unique combination of Home\_State and Age values:
    
    SELECT DISTINCT BY (Home\_State,Age) Name,Age FROM Sample.Person
    
     
    

The DISTINCT clause is applied before the TOP clause. If both are specified, the SELECT returns only rows with unique values, the number of unique value rows specified in the TOP clause.

If the column specified in the DISTINCT clause has rows that are NULL (contain no value), DISTINCT returns one row with a value of NULL, as shown in the following examples:

SELECT DISTINCT FavoriteColors FROM Sample.Person

 

SELECT DISTINCT BY (FavoriteColors) Name,FavoriteColors FROM Sample.Person
ORDER BY FavoriteColors

 

A DISTINCT clause is not meaningful in an [Embedded SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql) simple query, because in this type of Embedded SQL a SELECT always returns only one row of data. However, an Embedded SQL [cursor–based query](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_cursor) can return multiple rows of data; in a cursor-based query a DISTINCT clause returns only unique value rows.

DISTINCT and ORDER BY

The [ORDER BY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_order) clause is applied before the DISTINCT clause. You can therefore use the combination of DISTINCT and ORDER BY to return values from specific rows.

For example, the following program returns one row for each distinct Home\_State value. Because the rows have been ordered by Name value in ascending collation sequence, the row returned for each Home\_State is the one with the highest Name value:

SELECT DISTINCT BY (Home\_State) Home\_State,Name FROM Sample.Person
ORDER BY Name

 

For further details and program examples, refer to the [ORDER BY clause](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_order) reference page.

DISTINCT and GROUP BY

DISTINCT and GROUP BY both group records by a specified field (or fields) and return one record for each unique value of that field. One significant difference between them is that DISTINCT calculates aggregate functions before grouping. GROUP BY calculates aggregate functions after grouping. This difference is shown in the following examples:

SELECT DISTINCT BY (ROUND(Age,\-1)) Age,AVG(Age) AS AvgAge FROM Sample.Person
 /\* AVG(Age) returns average of all ages in table \*/

 

SELECT Age,AVG(Age) AS AvgAge FROM Sample.Person GROUP BY ROUND(Age,\-1)
 /\* AVG(Age) returns an average age for each age group \*/

 

A DISTINCT clause can be specified with one or more aggregate function fields, though this is rarely meaningful because an aggregate function returns a single value. Thus the following example returns a single row:

SELECT DISTINCT BY (AVG(Age)) Name,Age,AVG(Age) FROM Sample.Person

 

Caution:

If a DISTINCT clause with [aggregate functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_aggregatefunctions) as the only item or select-item is used with a GROUP BY clause, the DISTINCT clause is ignored. The intended combination of DISTINCT, aggregate function, and GROUP BY can be achieved using a subquery. For further details and program examples, refer to the [GROUP BY clause](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_groupby) reference page.

Letter Case and DISTINCT Optimization

By default, DISTINCT groups together alphabetic values based on their uppercase letter collation. Field values that differ only in letter case are grouped together. Grouped field values are returned in all uppercase letters. To group values by original letter case, or to display the returned values for a grouped field in their original letter case, use the [%EXACT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_exact) collation function. This is shown in the following examples, which assume that Sample.Person contains records with Home\_City field values of ‘New York’ and ‘new york’:

SELECT DISTINCT BY (Home\_City) Name,Home\_City FROM Sample.Person
/\* groups together Home\_City values by their uppercase letter values
   returns the name of each grouped city in uppercase letters. 
   Thus, 'NEW YORK' is returned.                          \*/

 

SELECT DISTINCT BY (Home\_City) Name,%EXACT(Home\_City) FROM Sample.Person
/\* groups together Home\_City values by their uppercase letter values
   returns the name of each grouped city in original letter case. 
   Thus, 'New York' or 'new york' may be returned, but not both. \*/

 

SELECT DISTINCT BY (%EXACT(Home\_City)) Name,Home\_City FROM Sample.Person
/\* groups together Home\_City values by their original letter case
   returns the name of each grouped city in original letter case.
   Thus, both 'New York' and 'new york' are returned.
   Optimization is not used.                       \*/

 

You can optimize query performance for queries that contain a DISTINCT clause by using the Management Portal. Select \[Home\] > \[Configuration\] > \[General SQL Settings\], then view and edit the DISTINCT Optimization Turned ON option. (This optimization also works for the [GROUP BY clause](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_groupby).) The default is “Yes”. For further details, refer to [SQL configuration settings](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RACS_Settings#RACS_Category_SQL) described in Caché Advanced Configuration Settings Reference.

You can also set this system-wide option to 1 or 0 with the [SetFastDistinct()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#SetFastDistinct) method:

  WRITE $SYSTEM.SQL.SetFastDistinct(1)

This optimization takes advantage of indices for the selected field(s). It is therefore only meaningful if an index exists for one or more of the selected fields. It collates field values as they are stored in the index; alphabetic strings are returned in all uppercase letters. You can set this system-wide option, then override it for specific queries by using the %EXACT collation function to preserve letter case.

Other Uses of DISTINCT

Asterisk Syntax: The syntax DISTINCT \* is legal, but not meaningful, because all rows, by definition, contain some distinct unique identifier. The syntax DISTINCT BY (\*) is not legal.

Subquery: The use of a DISTINCT clause in a subquery is legal, but not meaningful, because a subquery returns a single value.

No Row Data Selected: The DISTINCT clause can be used with a SELECT that does not access any table data. If the SELECT contains a FROM clause, specifying DISTINCT results in one row contain these non-table values; if you do not specify DISTINCT (or TOP) the SELECT results in as many rows with identical values as the number of rows in the FROM clause table. If the SELECT does not contain a FROM clause, DISTINCT is legal but not meaningful. See [FROM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_from) clause for more details.

DISTINCT and %ROWID

Specifying the DISTINCT keyword causes a [cursor-based Embedded SQL query](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_cursor) to not set the [%ROWID](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_pcROWID) variable. %ROWID is not set even when DISTINCT does not limit the rows returned. This is shown in the following example:

   SET %ROWID\=999
   &sql(DECLARE EmpCursor CURSOR FOR 
        SELECT DISTINCT Name, Home\_State
        INTO :name,:state FROM Sample.Person
        WHERE Home\_State %STARTSWITH 'M')
   &sql(OPEN EmpCursor)
   FOR { &sql(FETCH EmpCursor)
        QUIT:SQLCODE  
        WRITE !,"RowID: ",%ROWID," row count: ",%ROWCOUNT
        WRITE " Name=",name," State=",state
  }
   &sql(CLOSE EmpCursor)

 

This change of query behavior only applies to cursor-based Embedded SQL SELECT queries. Dynamic SQL SELECT queries and non-cursor Embedded SQL SELECT queries never set %ROWID.

DISTINCT and Transaction Processing

Specifying the DISTINCT keyword causes a query to retrieve all current data, including data that has not yet been committed by the current transaction. The transaction’s READ COMMITTED isolation mode parameter (if set) is ignored; all data is retrieved in READ UNCOMMITTED mode. For further details, refer to [Transaction Processing](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_modify#GSQL_modify_transaction) in the “Modifying the Database” chapter of Using Caché SQL.

Examples

The following query returns the one row from Sample.Person for each distinct Home\_State value. Which row is retrieved is not predictable:

SELECT DISTINCT Home\_State FROM Sample.Person

 

The following query returns the first 20 distinct Home\_State values retrieved from Sample.Person in ascending collation sequence order. The “top” rows reflect the ORDER BY clause sequencing of all of the rows in Sample.Person.

SELECT DISTINCT TOP 20 Home\_State FROM Sample.Person ORDER BY Home\_State

 

The following query uses DISTINCT in both the main query and in a WHERE clause subquery. It returns the first 20 distinct Home\_State values in Sample.Person that are also in Sample.Employee. If the subquery DISTINCT was not provided, it would retrieve the distinct Home\_State values in Sample.Person that match a random selection of Home\_State values in Sample.Employee:

SELECT DISTINCT TOP 20 Home\_State FROM Sample.Person 
WHERE Home\_State IN(SELECT DISTINCT TOP 20 Home\_State FROM Sample.Employee)
ORDER BY Home\_State

 

The following query returns the first 10 distinct FavoriteColor values. This reflects the ORDER BY clause sequencing of all of the rows in Sample.Person. The FavoriteColors field is known to have NULLs, so one distinct row with FavoriteColors NULL appears at the top of the collation sequence.

SELECT DISTINCT BY (FavoriteColors) TOP 10 FavoriteColors,Name FROM Sample.Person 
      ORDER BY FavoriteColors

 

Also note in the preceding example that because FavoriteColors is a list field, the collation sequence includes the element length byte. Thus distinct list values beginning with a three-letter element (RED) are listed before list values beginning with a four-letter element (BLUE).

See Also

*   [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select) statement
    
*   [GROUP BY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_groupby) clause
    
*   [ORDER BY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_order) clause
    
*   [TOP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_top) clause
    
*   “[Querying the Database](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_queries)” chapter in Using Caché SQL
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_delete "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dropdatabase "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_distinct.xml**