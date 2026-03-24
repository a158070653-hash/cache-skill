# SUM

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_sum

---

 

Caché SQL Reference

SUM

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_stddev "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_variance "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Aggregate Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_AGGREGATE_FUNCTIONS "SQL Aggregate Functions") \]  >  \[ [SUM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_sum "SUM") \]

Go to: Description Changes Made During the Current Transaction Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

An aggregate function that returns the sum of the values of a specified column.

Synopsis

SUM(\[ALL | DISTINCT\] expression \[%FOREACH(col-list)\] \[%AFTERHAVING\])

Arguments

ALL

Optional — Specifies that SUM return the sum of all values for expression. This is the default if no keyword is specified.

DISTINCT

Optional — Specifies that SUM return the sum of the distinct (unique) values for expression. If not specified, the default is ALL.

expression

Any valid expression. Usually the name of a column that contains the data values to be summed.

%FOREACH(col-list)

Optional — A column name or a comma-separated list of column names. See [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select) for further information on %FOREACH.

%AFTERHAVING

Optional — Applies the condition found in the [HAVING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_having) clause.

Description

The SUM [aggregate function](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_aggregatefunctions) returns the sum of the values of expression. Commonly, expression is the name of a field, (or an expression containing one or more field names) in the multiple rows returned by a query.

SUM can be used in a [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select) query or subquery that references either a table or a view. SUM can appear in a SELECT list or HAVING clause alongside ordinary field values.

SUM cannot be used in a WHERE clause. SUM cannot be used in the ON clause of a JOIN, unless the SELECT is a subquery.

Data Values

SUM returns data type INTEGER for an expression with data type INT, SMALLINT, or TINYINT. SUM returns data type BIGINT for an expression with data type BIGINT. SUM returns data type DOUBLE for an expression with data type DOUBLE. For all other numeric data types, SUM returns data type NUMERIC.

SUM returns a value with a precision of 18. The scale of the returned value is the same as the expression scale, with the following exception. If expression is a numeric value with data type VARCHAR or VARBINARY, the scale of the returned value is 8.

By default, aggregate functions use Logical (internal) data values, rather than Display values.

SUM is normally applied to a field or expression that has a numeric value. Because only minimal type checking is performed, it is possible (though rarely meaningful) to invoke it for nonnumeric fields. SUM evaluates nonnumeric values, including the empty string (''), as zero (0). If expression is data type VARCHAR, the return value to ODBC or JDBC is of data type DOUBLE.

NULL values in data fields are ignored when deriving a SUM aggregate function value. If no rows are returned by the query, or the data field value for all rows returned is NULL, SUM returns NULL.

Optimization

SQL optimization of a SUM calculation can use a [bitslice index](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_indices#GSQLOPT_indices_bitslice), if this index is defined for the field.

Changes Made During the Current Transaction

Like all aggregate functions, SUM always returns the current state of the data, including uncommitted changes, regardless of the current transaction’s isolation level. For further details, refer to [SET TRANSACTION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_settransaction) and [START TRANSACTION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_starttransaction).

Examples

In the following examples a dollar sign ($) is concatenated to Salary amounts.

The following query returns the sum of the salaries of all employees in the Sample.Employee database:

SELECT '$' || SUM(Salary) AS Total\_Payroll
     FROM Sample.Employee

 

The following query uses %AFTERHAVING to return the sum of all salaries and the sum of salaries over $80,000 for each state in which there is at least one person with a salary > $80,000:

SELECT Home\_State,
       '$' || SUM(Salary) AS Total\_Payroll,
       '$' || SUM(Salary %AFTERHAVING) AS Exec\_Payroll
     FROM Sample.Employee
     GROUP BY Home\_State
     HAVING Salary \> 80000
     ORDER BY Home\_State

 

The following query returns the sum and the average of the salaries for each job title in the Sample.Employee database:

SELECT Title,
       '$' || SUM(Salary) AS Total,
       '$' || AVG(Salary) AS Average
     FROM Sample.Employee
     GROUP BY Title
     ORDER BY Average

 

The following query shows SUM used with an arithmetic expression. For each job title in the Sample.Employee database it returns the sum of the current salaries and the sum of the salaries with a 10% increase in pay:

SELECT Title,
       '$' || SUM(Salary) AS BeforeRaises,
       '$' || SUM(Salary \* 1.1) AS AfterRaises
     FROM Sample.Employee
     GROUP BY Title
     ORDER BY Title

 

The following query shows SUM used with a logical expression using the CASE statement. It counts all of the salaried employees, and uses SUM to count all of the salaried employees earning $90,000 or more.

SELECT COUNT(Salary) As AllPaid, 
       SUM(CASE WHEN (Salary \>= 90000)
           THEN 1 ELSE 0 END) As TopPaid
       FROM Sample.Employee

 

See Also

*   [Aggregate Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_aggregatefunctions) overview
    
*   [AVG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_avg) aggregate function
    
*   [COUNT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_count) aggregate function
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_stddev "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_variance "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:38**

Source: **RSQL\_sum.xml**