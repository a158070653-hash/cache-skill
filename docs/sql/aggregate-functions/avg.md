# AVG

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_avg

---

 

Caché SQL Reference

AVG

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_aggregatefunctions "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_count "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Aggregate Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_AGGREGATE_FUNCTIONS "SQL Aggregate Functions") \]  >  \[ [AVG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_avg "AVG") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

An aggregate function that returns the average of the values of the specified column.

Synopsis

AVG(\[ALL | DISTINCT\] expression \[%FOREACH(col-list)\] \[%AFTERHAVING\])

Arguments

ALL

Optional — Specifies that AVG return the average of all values for expression. This is the default if no keyword is specified.

DISTINCT

Optional — Specifies that AVG calculate the average on only the unique instances of a value. If not specified, the default is ALL.

expression

Any valid expression. Usually the name of a column that contains the data values to be averaged.

%FOREACH(col-list)

Optional — A column name or a comma-separated list of column names. See [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select) for further information on %FOREACH.

%AFTERHAVING

Optional — Applies the condition found in the [HAVING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_having) clause.

Description

The AVG [aggregate function](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_aggregatefunctions) returns the average of the values of expression. Commonly, expression is the name of a field, (or an expression containing one or more field names) in the multiple rows returned by a query.

AVG can be used in a [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select) query or subquery that references either a table or a view. AVG can appear in a SELECT list or HAVING clause alongside ordinary field values.

AVG cannot be used in a WHERE clause. AVG cannot be used in the ON clause of a JOIN, unless the SELECT is a subquery.

The DISTINCT keyword with AVG performs the aggregate operation on only those fields having distinct (unique) values. The ALL keyword is optional. The default is to average all values.

Data Values

AVG returns either NUMERIC data type values or DOUBLE data type values. If expression is data type DOUBLE, AVG returns DOUBLE; otherwise, it returns NUMERIC.

For non-DOUBLE expression values, AVG returns a double-precision floating point number. The precision of the value returned by AVG is 18. The scale of the returned value depends upon the precision and scale of expression: the scale of the value returned by AVG is equal to 18 minus the expression precision, plus the expression scale (as=ap-ep+es).

For DOUBLE expression values, the scale is 0.

AVG is normally applied to a field or expression that has a numeric value, such as a number field or a date field. By default, aggregate functions use Logical (internal) data values, rather than Display values. Because no type checking is performed, it is possible (though rarely meaningful) to invoke it for nonnumeric fields; AVG evaluates nonnumeric values, including the empty string (''), as zero (0). If expression is data type VARCHAR, the return value to ODBC or JDBC is of data type DOUBLE.

NULL values in data fields are ignored when deriving an AVG aggregate function value. If no rows are returned by the query, or the data field value for all rows returned is NULL, AVG returns NULL.

Averaging a Single Value

If all of the expression values supplied to AVG are the same, the resulting average depends on the number of accessed rows in the table (the divisor). For example, if all of the rows in the table have the same value for a specific column, the average value of that column is a calculated value, which may differ slightly from the value in the individual columns. To avoid this descrepancy, you can use the DISTINCT keyword.

The following example shows how a slight inequality can result from the calculation of an average. The first query does not reference table rows, so AVG calculates by dividing by 1. The second query references table rows, so AVG calculates by dividing by the number of rows in the table. The third query references table rows, but averages the DISTINCT values of a single value; in this case AVG calculates by dividing by 1.

  SET pi\=$ZPI
  &sql(SELECT :pi,AVG(:pi) INTO :p,:av FROM Sample.Person)
  WRITE p," the value of pi",!
  WRITE av," avg of pi/1",!
  &sql(SELECT Name,:pi,AVG(:pi) INTO :n,:p,:av FROM Sample.Person)
  WRITE av," avg calculated using numrows",!
  &sql(SELECT Name,:pi,AVG(DISTINCT :pi) INTO :n,:p,:av FROM Sample.Person)
  WRITE av," avg of pi/1"

 

Optimization

SQL optimization of an AVG calculation can use a [bitslice index](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_indices#GSQLOPT_indices_bitslice), if this index is defined for the field.

Changes Made During the Current Transaction

Like all aggregate functions, AVG always returns the current state of the data, including uncommitted changes, regardless of the current transaction’s isolation level. For further details, refer to [SET TRANSACTION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_settransaction) and [START TRANSACTION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_starttransaction).

Examples

The following query lists the average salary for all employees in the Sample.Employee database. Because all rows returned by the query would have identical values for this average, this query only returns a single row, consisting of the average salary. For display purposes, this query concatenates a dollar sign to the value (using the || operator), and uses the AS clause to label the column:

SELECT '$' || AVG(Salary) AS AverageSalary
     FROM Sample.Employee

 

The following query lists each state with the average salary for the employees in that state:

SELECT Home\_State,'$' || AVG(Salary) AS AverageSalary
     FROM Sample.Employee
GROUP BY Home\_State

 

The following query lists the name and salary for those employees whose salary is greater than the average salary. It also lists the average salary for all employees; this value is the same for all rows returned by the query:

SELECT Name,Salary,
       '$' || AVG(Salary) AS AverageAllSalary
FROM Sample.Employee
HAVING Salary\>AVG(Salary)
ORDER BY Salary

 

The following query lists the name and salary for those employees whose salary is greater than the average salary. It also lists the average salary for those employees with above-average salaries; this value is the same for all rows returned by the query:

SELECT Name,Salary,
       '$' || AVG(Salary %AFTERHAVING) AS AverageHighSalary
FROM Sample.Employee
HAVING Salary\>AVG(Salary)
ORDER BY Salary

 

The following query lists those states containing more than three employees with the average salary of that state's employees, and the average salary of that state's employees earning more than $20,000:

SELECT Home\_State,
       '$' || AVG(Salary) AS AvgStateSalary,
       '$' || AVG(Salary %AFTERHAVING) AS AvgLargerSalaries
FROM Sample.Employee
GROUP BY Home\_State
HAVING COUNT(\*) \> 3 AND Salary \> 20000
ORDER BY Home\_State

 

The following query uses both the %FOREACH and the %AFTERHAVING keywords. It returns a row for those states containing people whose names start with “A”, “M”, or “W” (HAVING clause and GROUP BY clause). Each state row contains the following values:

*   LIST(Age %FOREACH(Home\_State)): a list of the ages of all of the people in the state.
    
*   AVG(Age %FOREACH(Home\_State)): the average age of all of the people in the state.
    
*   AVG(Age %AFTERHAVING): the average age of all of the people in the database that meet the HAVING clause criteria. (This number is the same for all rows.)
    
*   LIST(Age %FOREACH(Home\_State) %AFTERHAVING): a list of the ages of all of the people in the state that meet the HAVING clause criteria.
    
*   AVG(Age %FOREACH(Home\_State) %AFTERHAVING): the average age of all of the people in the state that meet the HAVING clause criteria.
    

SELECT Home\_State,
       LIST(Age %FOREACH(Home\_State)) AS StateAgeList,
       AVG(Age %FOREACH(Home\_State)) AS StateAgeAvg,
       AVG(Age %AFTERHAVING ) AS AgeAvgHaving,
       LIST(Age %FOREACH(Home\_State)%AFTERHAVING ) AS StateAgeListHaving,
       AVG(Age %FOREACH(Home\_State)%AFTERHAVING ) AS StateAgeAvgHaving
FROM Sample.Person
GROUP BY Home\_State
HAVING Name LIKE 'A%' OR Name LIKE 'M%' OR Name LIKE 'W%'
ORDER BY Home\_State

 

See Also

*   [Aggregate Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_aggregatefunctions) overview
    
*   [COUNT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_count) aggregate function
    
*   [SUM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_sum) aggregate function
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_aggregatefunctions "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_count "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_avg.xml**