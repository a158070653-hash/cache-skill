# UNION

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_union

---

 

Caché SQL Reference

UNION

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_truncatetable "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_unlock "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_COMMANDS "SQL Commands") \]  >  \[ [UNION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_union "UNION") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Combines two or more SELECT statements.

Synopsis

select-statement {UNION \[ALL\] \[%PARALLEL\] select-statement}

select-statement {UNION \[ALL\]  \[%PARALLEL\] (query)}

(query) {UNION \[ALL\]  \[%PARALLEL\] select-statement}

(query) {UNION \[ALL\]  \[%PARALLEL\] (query)}

Arguments

ALL

Optional — A keyword literal. If specified, duplicate data values are returned. If omitted, duplicate data values are suppressed.

%PARALLEL

Optional — The [%PARALLEL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_union#RSQL_union_parallel) keyword. If specified, each side of the union is run in parallel as a separate process.

select-statement

A SELECT statement, which retrieves data from a database.

query

A query that combines one or more SELECT statements.

Description

A UNION combines two or more queries into a single query that retrieves data into a result. The queries that are combined by a UNION can be simple queries, consisting of a single [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select) statement, or compound queries.

For a union to be possible between SELECT statements, the number of columns specified in each must match. Specifying SELECTs with different numbers of columns results in an SQLCODE -9 error. You can specify a NULL column in one SELECT to pair with a data column in another SELECT in order to match the number of columns. This use of NULL is shown in the “Examples” section below.

Caution:

To use the SELECT \* syntax in a UNION, the tables must contain the same number of columns. Therefore, future changes to the table definition by adding or deleting a column may cause unforeseen errors in unions of this sort.

Result column names and data types are taken from the column names and data types of the first leg of the UNION query. In situations where the corresponding columns in the two legs do not have the same names, it is may be useful to use the AS clause to identify the result columns.

An ordinary UNION eliminates duplicate rows (all values identical) from the result. A UNION ALL preserves duplicate rows in the result.

String fields in the UNION result have the collation of the corresponding SELECT fields, but are assigned EXACT collation if the field collations do not match.

TOP and ORDER BY Clauses

A UNION statement can conclude with an [ORDER BY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_order) clause which orders the result. This ORDER BY applies to the whole statement; it must be part of the outermost query, not a subquery. It does not have to be paired with a TOP clause. The following example shows this use of ORDER BY: the two SELECT statements select data, the data is combined by the UNION, then the ORDER BY sequences the results:

SELECT Name,Home\_Zip FROM Sample.Person
  WHERE Home\_Zip %STARTSWITH 9
UNION
SELECT Name,Office\_Zip FROM Sample.Employee
  WHERE Office\_Zip %STARTSWITH 8
ORDER BY Home\_Zip 

 

Using a column number in ORDER BY that does not correspond to a SELECT list column results in an SQLCODE -5 error. Using a column name in ORDER BY that does not correspond to a SELECT list column results in an SQLCODE -6 error.

Either SELECT statements (or both) in a union can also contain an [ORDER BY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_order) clause, but it must be paired with a [TOP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_top) clause. This ORDER BY is applied to determine which rows are selected by the TOP clause. The following example shows this use of ORDER BY: the two SELECT statements each use an ORDER BY to sequence their rows, which determines which rows are selected as the top rows. The selected data is combined by the UNION, then the final ORDER BY sequences the results:

SELECT TOP 5 Name,Home\_Zip FROM Sample.Person
  WHERE Home\_Zip %STARTSWITH 9
  ORDER BY Name
UNION
SELECT TOP 5 Name,Office\_Zip FROM Sample.Employee
  WHERE Office\_Zip %STARTSWITH 8
  ORDER BY Office\_Zip
ORDER BY Home\_Zip

 

TOP may apply to the first SELECT in the union, or to the result of the union, depending on the placement of the ORDER BY clause:

*   TOP...ORDER BY applies to UNION result: If the UNION is within a FROM clause subquery, and TOP and ORDER BY are applied to the results of the UNION. For example:
    
    SELECT TOP 10 Name,Home\_Zip
      FROM (SELECT Name,Home\_Zip FROM Sample.Person
              WHERE Name %STARTSWITH 'A'
            UNION
            SELECT Name,Home\_Zip FROM Sample.Person
              WHERE Home\_Zip %STARTSWITH 8)
    ORDER BY Home\_Zip
    
     
    
*   TOP applies to first SELECT; ORDER BY applies to UNION result. For example:
    
    SELECT TOP 10 Name,Home\_Zip 
      FROM Sample.Person
      WHERE Name %STARTSWITH 'A'
    UNION
    SELECT Name,Home\_Zip FROM Sample.Person
      WHERE Home\_Zip %STARTSWITH 8
    ORDER BY Home\_Zip
    
     
    

Enclosing Parentheses

UNION supports optional enclosing parentheses for either or both of its SELECT statements, or for the entire UNION statement. You may specify one or more pairs of enclosing parentheses. The following are all valid uses of enclosing parentheses:

(SELECT ...) UNION SELECT ...
(SELECT ...) UNION (SELECT ...)
((SELECT ...)) UNION ((SELECT ...))
(SELECT ... UNION SELECT ...)
((SELECT ...) UNION (SELECT ...))

Each use of parentheses generates a separate [cached Query](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_cachedqueries).

UNION/OR Optimization

By default, SQL automatic optimization transforms UNION subqueries to OR conditions, where deemed appropriate. This UNION/OR transformation allows EXISTS and other low-level predicates to migrate to top-level conditions where they are available to Caché query optimizer indexing. This default transformation is desirable in most situations. However, in some situations this UNION/OR transformation imposes a significant overhead burden. The %NOUNIONOROPT query optimization option disables this automatic UNION/OR transformation for all conditions in the WHERE clause associated with the FROM clause. Thus, in a complex query, you can disable automatic UNION/OR optimization for one subquery while allowing it in other subqueries. For further information on %NOUNIONOROPT, refer to the [FROM clause](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_from).

If a condition involving a subquery is applied to a UNION, it is applied within each union operand, rather than at the end. This allows subquery optimizations to be applied in each UNION operand. For descriptions of subquery optimization options, refer to the [FROM clause](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_from). In the following example, the WHERE clause condition is applied to each of the subqueries in the union, rather than to the result of the union:

SELECT Name,Age FROM 
  (SELECT Name,Age FROM Sample.Person
   UNION SELECT Name,Age FROM Sample.Employee)
WHERE Age IN (SELECT TOP 5 Age FROM Sample.Employee WHERE Age\>55 ORDER BY Age)

 

UNION ALL Aggregate Optimization

SQL automatic optimization of a UNION ALL pushes a top-level aggregate into the legs of the union. This can result in significantly improved performance with or without the %PARALLEL keyword, For example:

SELECT COUNT(\*) FROM (SELECT item1 FROM table1 UNION ALL SELECT item2 FROM table2) 

is optimized as:

SELECT SUM(y) FROM (SELECT COUNT(\*) AS y FROM table1 UNION ALL SELECT COUNT(\*) AS y FROM table2) 

This optimization applies to all top-level aggregate functions (not just COUNT), including queries with multiple top-level aggregate functions. For this optimization to be applied, the outer query must be a "onerow" query, with no WHERE or GROUP BY clause, it cannot reference %VID, and the UNION ALL must be the only stream in its FROM clause. The aggregates cannot be nested, and any aggregate function used cannot use %FOREACH() grouping or DISTINCT.

Parallel Processing

The %PARALLEL keyword supports parallelism and distributed processing on a multiprocessor system. It causes Caché to perform parallel processing on the UNION queries, assigning each query to a separate process on the same machine. In some cases that process will send the query to a different machine to be processed. These processes communicate via pipes, with Caché creating one or more temporary files to hold subquery results. The main process combines the resulting rows and returns the final results. For further details, refer to the [Show Plan](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_optquery#GSQLOPT_optquery_showplan) for a UNION query, comparing the Show Plan with and without the %PARALLEL keyword.

In general, the more effort expended to produce each row, the more beneficial %PARALLEL becomes.

The following examples show the use of the %PARALLEL keyword:

SELECT Name FROM Sample.Employee WHERE Name %STARTSWITH 'A'
UNION %PARALLEL
SELECT Name FROM Sample.Person WHERE Name %STARTSWITH 'A'
ORDER BY Name

 

SELECT Name FROM Sample.Employee WHERE Name %STARTSWITH 'A'
UNION ALL %PARALLEL
SELECT Name FROM Sample.Person WHERE Name %STARTSWITH 'A'
ORDER BY Name

 

%PARALLEL is intended for SELECT queries and their subqueries. An [INSERT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_insert#RSQL_insertselect) command subquery cannot use %PARALLEL.

Adding the %PARALLEL keyword may not be appropriate for all UNION queries, and may result in an error. The following SQL constructs generally do not support UNION %PARALLEL execution: an outer join, a correlated field, an IN predicate condition containing a subquery, an EXISTS predicate condition, or a collection predicate. To determine if a UNION query can successfully use %PARALLEL, test each leg of the UNION separately. Separately test each leg query by adding a [FROM %PARALLEL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_from#RSQL_from_parallel) keyword. If one of the FROM %PARALLEL queries generates a query plan that does not show parallelization, then the UNION query will not support %PARALLEL.

UNION ALL and Aggregate Functions

SQL automatic optimization pushes UNION ALL [aggregate functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_aggregatefunctions) into the union leg subqueries. SQL calculates the aggregate value for each subquery, and then combines the results to return the original aggregate value. For example:

SELECT COUNT(Name) FROM (SELECT Name FROM Sample.Person
                    UNION ALL SELECT Name FROM Sample.Employee)

 

Is optimized as:

SELECT SUM(y) FROM (SELECT COUNT(Name) AS y FROM Sample.Person
                    UNION ALL SELECT COUNT(Name) AS y FROM Sample.Employee)

 

This can result in substantial performance improvement. This optimization is applied with or without the [%PARALLEL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_union#RSQL_union_parallel) keyword. This optimization is applied to multiple aggregate functions.

This optimization transform only occurs under the following circumstances:

*   The outer query FROM clause must contain only a UNION ALL statement.
    
*   The outer query cannot contain a WHERE clause or a GROUP BY clause.
    
*   The outer query cannot contain a [%VID](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_views) (view ID) field.
    
*   Aggregate functions cannot contain a DISTINCT or %FOREACH keyword.
    
*   Aggregate functions cannot be nested.
    

Examples

The following example creates a result that contains a row for every Name found in each of the two tables; if a Name is found in both tables, two rows are created. When the Name is an employee, it lists the office location, concatenated with the word “office” as State, and the employee’s Title. When Name is a person, it lists the home location, concatenated with the word “home” as State, and <null> for Title. The ORDER BY clause operates on the result; the combined rows are ordered by Name:

SELECT Name,Office\_State||' office' AS State,Title 
FROM Sample.Employee
UNION
SELECT Name,Home\_State||' home',NULL
FROM Sample.Person
ORDER BY Name

 

The following two examples show the effects of the ALL keyword. In the first example, UNION returns only unique values. In the second example, UNION ALL returns all values, including duplicates:

SELECT Name
FROM Sample.Employee
WHERE Name %STARTSWITH 'A'
UNION
SELECT Name
FROM Sample.Person
WHERE Name %STARTSWITH 'A'
ORDER BY Name

 

SELECT Name
FROM Sample.Employee
WHERE Name %STARTSWITH 'A'
UNION ALL
SELECT Name
FROM Sample.Person
WHERE Name %STARTSWITH 'A'
ORDER BY Name

 

See Also

*   [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select)
    
*   [ORDER BY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_order) clause, [TOP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_top) clause
    
*   [CREATE QUERY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createquery), [CREATE PROCEDURE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createprocedure)
    
*   “[Querying the Database](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_queries)” chapter in Using Caché SQL
    
*   [SQLCODE error messages](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RERR_sql) listed in the Caché Error Reference
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_truncatetable "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_unlock "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:38**

Source: **RSQL\_union.xml**