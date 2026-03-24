# TOP

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_top

---

 

Caché SQL Reference

TOP

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_starttransaction "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_truncatetable "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_COMMANDS "SQL Commands") \]  >  \[ [TOP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_top "TOP") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A SELECT clause that specifies how many rows to return.

Synopsis

SELECT  \[DISTINCT clause\] 
  \[TOP {\[((\]int\[))\] | ALL}\]
  select-item {,select-item2}

Arguments

int

Limits the number of rows returned to the specified integer number. The int argument can be either a positive integer, a Dynamic SQL [input parameter (?)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_dynsql#GSQL_dynsql_execute) or an Embedded SQL [host variable (:var)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_hvars) that resolve to a positive integer.

In Dynamic SQL, the int value can optionally be enclosed with single parentheses or double parentheses (double parentheses are the preferred syntax); these parentheses [suppress literal substitution](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_cachedqueries#GSQLOPT_cachedqueries_nolitsub) of the int value in the corresponding cached query.

ALL

TOP ALL is only meaningful in a subquery or in a CREATE VIEW statement. It is used to support the use of an ORDER BY clause in these situations, fulfilling the requirement that an [ORDER BY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_order) clause must be paired with a TOP clause in a subquery or a query used in a CREATE VIEW. TOP ALL does not restrict the number of rows returned.

Description

The optional TOP clause appears after the SELECT keyword and the optional DISTINCT clause, and before the first select-item.

The TOP keyword is used in [Dynamic SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_dynsql) and in [cursor-based Embedded SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_cursor). In non-cursor Embedded SQL the only meaningful use of the TOP keyword is TOP 0. Any other TOP int (where int is any non-zero integer) is valid but not meaningful because a SELECT in non-cursor Embedded SQL always returns at most one row of data.

The TOP clause of a [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select) statement limits the number of rows returned to the number specified in int. If no TOP clause is specified, the default is to display all the rows that meet the SELECT criteria. If a TOP clause is specified, the number or rows displayed is either int or all of the rows that fulfill the query predicate requirements, whichever is smaller. If you specify ALL, SELECT returns all the rows in the table that fulfill the query predicate requirements.

If no [ORDER BY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_order) clause is specified in the query, which records are returned as the “top” rows is unpredictable. If an ORDER BY clause is specified, the top rows accord to the order specified in that clause.

The [DISTINCT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_distinct) clause (if specified) is applied before TOP, specifying that (at most) int number of unique values are to be returned.

TOP short circuits when all rows have been delivered. Thus, if you select until you get SQLCODE 100, the [FETCH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_fetch) that sets SQLCODE 100 is instant.

When accessing data through a view, or through a [FROM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_from) clause subquery, you can limit the number of rows returned by using the %vid view ID, rather than (or in addition to) the TOP clause. For further details on using %vid, refer to the [Defining and Using Views](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_views) chapter of Using Caché SQL.

The TOP int Value

The int value can be an integer, or a Dynamic SQL [input parameter (?)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_dynsql#GSQL_dynsql_execute), or an Embedded SQL [host variable (:var)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_hvars) that resolve to an integer value.

When int is an integer, it specifies the number of rows to return. Permitted values are 0 and positive numbers; fractional numbers are rounded up to the next higher integer. Zero (0) is a valid int value. TOP 0 executes the query but returns no data.

An int value can be specified with or without enclosing parentheses. These parentheses affect how a Dynamic SQL query is cached (Embedded SQL queries are not cached). An int value without parentheses is converted to a ? parameter variable in the cached query. This means that repeatedly invoking the same query with different TOP int values invokes the same cached query, rather than preparing and optimizing the query each time.

Enclosing parentheses [suppress literal substitution](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_cachedqueries#GSQLOPT_cachedqueries_nolitsub). For example, TOP ((7)). When int is enclosed in parentheses, the cached query preserves the specific int value. Re-invoking the query with the same TOP int value uses the cached query; invoking the query with a different TOP int value causes SQL to prepare, optimize, and cache this new version of the query.

TOP and ORDER BY

TOP is generally used in a SELECT with an [ORDER BY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_order) clause. Note that the default ascending ORDER BY collation sequence considers NULL to be the lowest (“top”) value, followed by the empty string ('').

TOP is required in a subquery SELECT or a [CREATE VIEW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createview) SELECT when specifying an ORDER BY clause. In these cases you can specify either TOP int (to limit the number of rows to return) or TOP ALL.

TOP ALL is only used in a subquery or in a CREATE VIEW statement. It is used to support the use of an ORDER BY clause in these situations, fulfilling the requirement that an ORDER BY clause must be paired with a TOP clause in a subquery or a CREATE VIEW query. TOP ALL does not restrict the number of rows returned. TOP ALL ... ORDER BY does not change default SELECT optimization. The ALL keyword cannot be enclosed in parentheses.

TOP Optimization

By default, a SELECT optimizes for fastest time to return all data. Adding both a TOP int clause and an ORDER BY clause optimizes for fastest time to return first row. (Note that both clauses are required to change the optimization.) You can use the [%SYS.PTools.SQLStats](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYS.PTools.SQLStats) class to return the time to return first row for a query.

The following are special case optimizations:

*   You may wish to use the TOP and ORDER BY optimization strategy without limiting the number of rows returned; for example, if you are returning data that is displayed in page units. In such a case, you may want to issue a TOP clause with an int value larger than the total number of rows.
    
*   You may wish to limit the number of rows returned and specify their order without changing the default SELECT optimization. In this case, specify a TOP clause, an ORDER BY clause, and the %NOTOPOPT keyword to preserve fastest time to return all data optimization. See the [FROM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_from) clause for more details.
    

TOP with Aggregates and Functions

An aggregate function or a scalar function can only return a single value. If the query select-item list contains only aggregates and functions, the application of the TOP clause is as follows:

*   If the select-item list contains an aggregate function, for example COUNT(\*) or AVG(Age), and does not contain any field references, no more than one row is returned, regardless of the TOP int value or the presence of an ORDER BY clause. These clauses are validated, but ignored. This is shown in the following examples:
    
    SELECT TOP 5 AVG(Age),CURRENT\_TIMESTAMP(3) FROM Sample.Person
      /\* returns 1 row \*/
    
     
    
    SELECT TOP 1 AVG(Age),CURRENT\_TIMESTAMP(3) FROM Sample.Person ORDER BY Age
      /\* returns 1 row \*/
    
     
    
*   If the select-item list contains one or more scalar functions, expressions, literals (such as %TABLENAME), subqueries, or host variables, and does not contain any field references or aggregates, the TOP clause is applied. This is shown in the following example:
    
    SELECT TOP 5 ROUND(678.987,2),CURRENT\_TIMESTAMP(3) FROM Sample.Person
      /\* returns 5 identical rows \*/
    
     
    
    The actual number of rows returned depends on the number of rows in the table, even when table fields are not referenced. For example:
    
    SELECT TOP 300 CURRENT\_TIMESTAMP(3) FROM Sample.Person
      /\* returns either the number of rows in Sample.Person
         or 300 rows, whichever is smaller \*/
    
     
    
    When the query is restricted by a predicate condition, the number of rows returned is restricted by that condition, even when table fields are not referenced in the select-item list. For example:
    
    SELECT TOP 300 CURRENT\_TIMESTAMP(3) FROM Sample.Person WHERE Home\_State \= 'MA'
      /\* returns either the number of rows in Sample.Person
         where Home\_State = 'MA'
         or 300 rows, whichever is smaller \*/
    
     
    
*   If the SELECT statement does not contain a FROM clause, at most one row is returned, regardless of the TOP value. For example:
    
    SELECT TOP 5 ROUND(678.987,2),CURRENT\_TIMESTAMP(3)
      /\* returns 1 row \*/
    
     
    
*   The DISTINCT clause further limits the TOP clause. If there are fewer distinct values than the TOP value, only the rows with distinct values are returned. When only scalar functions are referenced, only one row is returned. For example:
    
    SELECT DISTINCT TOP 15 CURRENT\_TIMESTAMP(3) FROM Sample.Person
      /\* returns 1 row \*/
    
     
    
*   TOP 0 always returns no rows, regardless of the contents of the select-item list, or whether the SELECT statement contains a FROM clause or a DISTINCT clause.
    
    In non-cursor Embedded SQL, a query with TOP 0 returns no rows and sets SQLCODE=100; a non-cursor Embedded SQL query with TOP 1 (or any other TOP int value) returns one row and sets SQLCODE=0. In cursor-based Embedded SQL, completion of the fetch loop always sets SQLCODE=100, regardless of the TOP int value.
    

Examples

The following query returns the first 20 rows retrieved from Sample.Person in the order that they are stored in the database. This record order is generally not predictable.

SELECT TOP 20 Home\_State,Name FROM Sample.Person

 

The following query returns the first 20 distinct Home\_State values retrieved from Sample.Person in ascending collation sequence order.

SELECT DISTINCT TOP 20 Home\_State FROM Sample.Person ORDER BY Home\_State

 

The following query returns the first 40 distinct FavoriteColor values. The “top” rows reflect the ORDER BY clause sequencing of all of the rows in Sample.Person in descending (DESC) collation sequence. Descending collation sequence is used rather than the default ascending collation sequence because the FavoriteColors field is known to have NULLs, which would appear at the top of the ascending collation sequence.

SELECT DISTINCT TOP 40 FavoriteColors FROM Sample.Person 
      ORDER BY FavoriteColors DESC

 

Also note in the preceding example that because FavoriteColors is a list field, the collation sequence includes the element length byte. Thus six-letter elements (YELLOW, PURPLE, ORANGE) collate together, listed before five-letter elements (WHITE, GREEN, etc.).

[Dynamic SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_dynsql) can specify the int value as an input parameter (indicated by “?”). In the following example, the TOP ? input parameter is set to 10 by the %Execute method:

  SET myquery \= "SELECT TOP ? Name,Age FROM Sample.Person"
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET tStatus \= tStatement.%Prepare(myquery)
  SET rset \= tStatement.%Execute(10)
  DO rset.%Display()

 

The following [cursor-based Embedded SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_cursor) example performs the same operation:

  SET topnum\=10
  &sql(DECLARE pCursor CURSOR FOR
       SELECT TOP :topnum Name,Age INTO :name,:years FROM Sample.Person
      )
  &sql(OPEN pCursor)
  FOR { &sql(FETCH pCursor)
        QUIT:SQLCODE
        WRITE "Name=",name," Age=",years,!
      }
  &sql(CLOSE pCursor)

 

See Also

*   [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select) statement
    
*   [DISTINCT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_distinct) clause
    
*   [ORDER BY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_order) clause
    
*   “[Querying the Database](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_queries)” chapter in Using Caché SQL
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_starttransaction "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_truncatetable "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:38**

Source: **RSQL\_top.xml**