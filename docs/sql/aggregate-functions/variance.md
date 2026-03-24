# VARIANCE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_variance

---

 

Caché SQL Reference

VARIANCE, VAR\_SAMP, VAR\_POP

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_sum "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_xmlagg "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Aggregate Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_AGGREGATE_FUNCTIONS "SQL Aggregate Functions") \]  >  \[ [VARIANCE, VAR\_SAMP, VAR\_POP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_variance "VARIANCE, VAR_SAMP, VAR_POP") \]

Go to: Description Changes Made During the Current Transaction Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Aggregate functions that return the statistical variance of a data set.

Synopsis

VARIANCE(\[ALL | DISTINCT\] expression \[%FOREACH(col-list)\] \[%AFTERHAVING\])

VAR\_SAMP(\[ALL | DISTINCT\] expression \[%FOREACH(col-list)\] \[%AFTERHAVING\])

VAR\_POP(\[ALL | DISTINCT\] expression \[%FOREACH(col-list)\] \[%AFTERHAVING\])

Arguments

ALL

Optional — Specifies that VARIANCE return the variance of all values for expression. This is the default if no keyword is specified.

DISTINCT

Optional — Specifies that VARIANCE return the variance of the distinct (unique) values for expression. If not specified, the default is ALL.

expression

Any valid expression. Usually the name of a column that contains the data values to be analyzed for variance.

%FOREACH(col-list)

Optional — A column name or a comma-separated list of column names. See [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select) for further information on %FOREACH.

%AFTERHAVING

Optional — Applies the condition found in the [HAVING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_having) clause.

Description

These three variance [aggregate functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_aggregatefunctions) return the statistical variance of the values of expression, after discarding NULL values. That is, the amount of variation from the mean value of the data set, expressed as a positive number. The larger the return value, the more variation there is within the data set of values. Caché SQL also supplies aggregate functions to return the [standard deviation](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_stddev) corresponding to each of these variance functions.

There are slight variations in how this statistical variation is derived:

*   VARIANCE: Returns 0 if all of the values in the data set have the same value (no variability). Returns 0 if the data set consists of only one value (no possible variability). Returns NULL if the data set has no values.
    
    The VARIANCE calculation is:
    
    (SUM(expression\*\*2) \* COUNT(expression)) - SUM(expression\*\*2)
    \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_
    COUNT(expression) \* (COUNT(expression) - 1)
    
*   VAR\_SAMP: Sample variance. Returns 0 if all of the values in the data set have the same value (no variability). Returns NULL if the data set consists of only one value (no possible variability). Returns NULL if the data set has no values. Uses the same variant calculation as VARIANCE.
    
*   VAR\_POP: Population variance. Returns 0 if all of the values in the data set have the same value (no variability). Returns 0 if the data set consists of only one value (no possible variability). Returns NULL if the data set has no values.
    
    The VAR\_POP calculation is:
    
    (SUM(expression\*\*2) \* COUNT(expression)) - (SUM(expression) \*\*2)
    \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_
    (COUNT(expression) \*\*2 )
    

These variance aggregate functions can be used in a [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select) query or subquery that references either a table or a view. They can appear in a SELECT list or HAVING clause alongside ordinary field values.

These variance aggregate functions cannot be used in a WHERE clause. They cannot be used in the ON clause of a JOIN, unless the SELECT is a subquery.

These variance aggregate functions return a value of data type NUMERIC with a precision of 36 and a scale of 17, unless expression is data type DOUBLE in which case the function returns data type DOUBLE.

These variance aggregate functions are normally applied to a field or expression that has a numeric value. They evaluate nonnumeric values, including the empty string (''), as zero (0).

These variance aggregate functions ignore NULL values in data fields. If no rows are returned by the query, or the data field value for all rows returned is NULL, they return NULL.

Changes Made During the Current Transaction

Like all aggregate functions, the variance functions always returns the current state of the data, including uncommitted changes, regardless of the current transaction’s isolation level. For further details, refer to [SET TRANSACTION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_settransaction) and [START TRANSACTION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_starttransaction).

Examples

The following example uses VARIANCE to return the variance in the ages of the employees in Sample.Employee, and the variance in the distinct ages represented by one or more employees:

SELECT VARIANCE(Age) AS AgeVar,VARIANCE(DISTINCT Age) AS PerAgeVar
     FROM Sample.Employee

The following example uses VAR\_POP to return the population variance in the ages of the employees in Sample.Employee, and the variance in the distinct ages represented by one or more employees:

SELECT VAR\_POP(Age) AS AgePopVar,VAR\_POP(DISTINCT Age) AS PerAgePopVar
     FROM Sample.Employee

See Also

*   [Aggregate Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_aggregatefunctions) overview
    
*   [AVG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_avg) aggregate function
    
*   [COUNT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_count) aggregate function
    
*   [STDDEV, STDDEV\_SAMP, STDDEV\_POP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_stddev) aggregate functions
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_sum "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_xmlagg "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:38**

Source: **RSQL\_variance.xml**