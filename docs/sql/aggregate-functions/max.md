# MAX

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_max

---

 

Caché SQL Reference

MAX

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_list "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_min "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Aggregate Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_AGGREGATE_FUNCTIONS "SQL Aggregate Functions") \]  >  \[ [MAX](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_max "MAX") \]

Go to: Description Changes Made During the Current Transaction Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

An aggregate function that returns the maximum data value in a specified column.

Synopsis

MAX(\[ALL | DISTINCT\] expression \[%FOREACH(col-list)\] \[%AFTERHAVING\])

Arguments

ALL

Optional — Applies the aggregate function to all values. ALL has no effect on the value returned by MAX. It is provided for SQL-92 compatibility.

DISTINCT

Optional — Specifies that each unique value is considered. DISTINCT has no effect on the value returned by MAX. It is provided for SQL-92 compatibility.

expression

Any valid expression. Usually the name of a column that contains the values from which the maximum value is to be returned.

%FOREACH(col-list)

Optional — A column name or a comma-separated list of column names. See [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select) for further information on %FOREACH.

%AFTERHAVING

Optional — Applies the condition found in the [HAVING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_having) clause.

Description

The MAX [aggregate function](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_aggregatefunctions) returns the largest (maximum) of the values of expression. Commonly, expression is the name of a field, (or an expression containing one or more field names) in the multiple rows returned by a query.

MAX can be used in a [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select) query or subquery that references either a table or a view. MAX can appear in a SELECT list or HAVING clause alongside ordinary field values.

MAX cannot be used in a WHERE clause. MAX cannot be used in the ON clause of a JOIN, unless the SELECT is a subquery.

Unlike most other aggregate functions, the ALL and DISTINCT keywords perform no operation in MAX. They are provided for SQL–92 compatibility.

Data Values

The specified field used by MAX can be numeric or nonnumeric. Maximum is defined as highest in collation sequence: thus 'Z' is the maximum alphabetic value. By default, alphabetic characters are converted to uppercase collation before comparison. Empty string ('') values are treated as zero (0).

For numeric values, the scale returned is the same as the expression scale.

NULL values in data fields are ignored when deriving a MAX aggregate function value. If no rows are returned by the query, or the data field value for all rows returned is NULL, MAX returns NULL.

Changes Made During the Current Transaction

Like all aggregate functions, MAX always returns the current state of the data, including uncommitted changes, regardless of the current transaction’s isolation level. For further details, refer to [SET TRANSACTION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_settransaction) and [START TRANSACTION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_starttransaction).

Examples

The following query returns the highest (maximum) salary in the Sample.Employee database:

SELECT '$' || MAX(Salary) As TopSalary
     FROM Sample.Employee

 

The following query returns one row for each state that contains at least one employee with a salary smaller than $25,000. Using the %AFTERHAVING keyword, each row returns the maximum employee salary smaller than $25,000. Each row also returns the minimum salary and the maximum salary for all employees in that state:

SELECT Home\_State,
       '$' || MAX(Salary %AFTERHAVING) AS MaxSalaryBelow25K,
       '$' || MIN(Salary) AS MinSalary,
       '$' || MAX(Salary) AS MaxSalary
          FROM Sample.Employee
     GROUP BY Home\_State
     HAVING Salary < 25000
     ORDER BY Home\_State

 

The following query returns the lowest (minimum) and highest (maximum) name in collation sequence found in the Sample.Employee database:

SELECT Name,MIN(Name),MAX(Name)
     FROM Sample.Employee

 

Note that MIN and MAX convert Name values to uppercase before comparison.

The following query returns the highest (maximum) salary for an employee whose Home\_State is 'VT' in the Sample.Employee database:

SELECT MAX(Salary)
     FROM Sample.Employee
     WHERE Home\_State \= 'VT'

 

The following query returns the number of employees and the highest (maximum) employee salary for each Home\_State in the Sample.Employee database:

SELECT Home\_State, 
     COUNT(Home\_State) As NumEmployees, 
     MAX(Salary) As TopSalary
     FROM Sample.Employee
     GROUP BY Home\_State
     ORDER BY TopSalary

 

See Also

*   [Aggregate Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_aggregatefunctions) overview
    
*   [MIN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_min) aggregate function
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_list "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_min "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_max.xml**