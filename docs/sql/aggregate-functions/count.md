# COUNT

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_count

---

 

Caché SQL Reference

COUNT

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_avg "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dlist "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Aggregate Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_AGGREGATE_FUNCTIONS "SQL Aggregate Functions") \]  >  \[ [COUNT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_count "COUNT") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

An aggregate function that returns the number of rows in a table or a specified column.

Synopsis

COUNT(\*)

COUNT(\[ALL | DISTINCT\] expression \[%FOREACH(col-list)\] \[%AFTERHAVING\])

Arguments

\*

Specifies that all rows should be counted to return the total number of rows in the specified table. COUNT(\*) takes no other arguments and cannot be used with the ALL or DISTINCT keywords. COUNT(\*) does not take an expression argument, and does not use information about any particular column. COUNT(\*) returns the number of rows in a specified table or view without eliminating duplicates. It counts each row separately, including rows that contain NULL values.

ALL

Optional — Specifies that COUNT return the count of all values for expression. This is the default if no keyword is specified.

DISTINCT

Optional — Specifies that COUNT return the count of the distinct (unique) values for expression. Cannot be used with a stream field. If not specified, the default is ALL.

expression

Any valid expression. Usually the name of a column that contains the data values to be counted.

%FOREACH(col-list)

Optional — A column name or a comma-separated list of column names. See [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select) for further information on %FOREACH. The col-list cannot contain a stream field.

%AFTERHAVING

Optional — Applies the condition found in the [HAVING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_having) clause.

Description

The COUNT [aggregate function](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_aggregatefunctions) has two forms:

*   COUNT(expression) returns the count of the number of values in expression as an integer. Commonly, expression is the name of a field, (or an expression containing one or more field names) in the multiple rows returned by a query. COUNT(expression) does not count NULL values. It can optionally count or not count duplicate field values. COUNT always returns type INTEGER with length 4, precision 10, and scale 0.
    
*   COUNT(\*) returns the count of the number of rows in the table as an integer. COUNT(\*) counts all rows, regardless of the presence of duplicate field values or NULL values.
    

COUNT can be used in a [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select) query or subquery that references either a table or a view. COUNT can appear in a SELECT list or HAVING clause alongside ordinary field values.

COUNT cannot be used in a WHERE clause. COUNT cannot be used in the ON clause of a JOIN, unless the SELECT is a subquery.

COUNT(expression) can take a DISTINCT keyword or an ALL keyword. The DISTINCT keyword counts only those fields having distinct (unique) values; a field with duplicate values is only counted once. COUNT DISTINCT does not count NULL as a distinct value. The ALL keyword counts all non-NULL values, including all duplicates. ALL is the default behavior.

No Rows Returned

If no rows are selected, COUNT either returns 0 or NULL, depending on the query:

*   COUNT returns 0 if the select-list does not contain any references to fields in the FROM clause table(s), other than fields supplied to aggregate functions. Only the COUNT aggregate function returns 0; other aggregate functions return NULL. The query returns a %ROWCOUNT of 1. This is shown in the following example:
    
      SET myquery \= 3
      SET myquery(1) \= "SELECT COUNT(\*) AS Recs,COUNT(Name) AS People,"
      SET myquery(2) \= "AVG(Age) AS AvgAge,MAX(Age) AS MaxAge,CURRENT\_TIMESTAMP AS Now"
      SET myquery(3) \= " FROM Sample.Employee WHERE Name %STARTSWITH 'ZZZ'"
      SET tStatement \= ##class(%SQL.Statement).%New()
      SET qStatus \= tStatement.%Prepare(.myquery)
       IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
      SET rset \= tStatement.%Execute()
      DO rset.%Display()
      WRITE !,"Rowcount:",rset.%ROWCOUNT
    
     
    
*   COUNT returns NULL if the select-list contains any direct reference to a field in a FROM clause table, or if TOP 0 is specified. The query returns a %ROWCOUNT of 0. The following example does not return a COUNT value because the %ROWCOUNT value is 0:
    
      SET myquery \= 2
      SET myquery(1) \= "SELECT COUNT(\*) AS Recs,COUNT(Name) AS People,$LENGTH(Name) AS NameLen"
      SET myquery(2) \= " FROM Sample.Employee WHERE Name %STARTSWITH 'ZZZ'"
      SET tStatement \= ##class(%SQL.Statement).%New()
      SET qStatus \= tStatement.%Prepare(.myquery)
       IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
      SET rset \= tStatement.%Execute()
      DO rset.%Display()
      WRITE !,"Rowcount:",rset.%ROWCOUNT
    
     
    
*   COUNT(\*) returns 1 if no table is specified. The query returns a %ROWCOUNT of 1. This is shown in the following example:
    
      SET myquery \= "SELECT COUNT(\*) AS Recs"
      SET tStatement \= ##class(%SQL.Statement).%New()
      SET qStatus \= tStatement.%Prepare(myquery)
       IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
      SET rset \= tStatement.%Execute()
      DO rset.%Display()
      WRITE !,"Rowcount:",rset.%ROWCOUNT
    
     
    

Stream Fields

You can use COUNT(expression) to count [stream field](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_blobs) values, with some restrictions. COUNT(streamfield) counts all non-NULL values. It does not check for duplicate values.

You cannot specify the COUNT function’s DISTINCT keyword when expression is a stream field. Attempting to use a DISTINCT keyword with a stream field results in an SQLCODE -37 error.

You cannot specify a stream field in a %FOREACH col-list. Attempting to do so results in an SQLCODE -37 error.

The following example shows valid uses of the COUNT function, where Title is a string field and Notes and Picture are stream fields:

SELECT DISTINCT Title,COUNT(Notes),COUNT(Picture %FOREACH(Title))
FROM Sample.Employee

 

The following examples are not valid when Title is a string field and Notes and Picture are stream fields:

\-- Invalid: DISTINCT keyword with stream field
SELECT Title,COUNT(DISTINCT Notes) FROM Sample.Employee

\-- Invalid: %FOREACH col-list contains stream field
SELECT Title,COUNT(Notes %FOREACH(Picture))
FROM Sample.Employee

Privileges

To use COUNT(\*) you must have table-level SELECT privilege for the specified table. To use COUNT(column-name) you must have column-level SELECT privilege for the specified column, or table-level SELECT privilege for the specified table. You can determine if the current user has SELECT privilege by invoking the [%CHECKPRIV](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_checkpriv) command. You can determine if a specified user has table-level SELECT privilege by invoking the [$SYSTEM.SQL.CheckPriv()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#CheckPriv) method. For privilege assignment, refer to the [GRANT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_grant) command.

Performance

For optimal COUNT performance, you should define indices as follows:

*   For COUNT(\*), define a [bitmap extent index](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_indices#GSQLOPT_indices_bitmapextent), if needed. This index may have been automatically defined when the table was created.
    
*   For COUNT(fieldname), define a [bitslice index](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_indices#GSQLOPT_indices_bitslice) for the specified field.
    

Changes Made by Uncommitted Transactions

Like all aggregate functions, COUNT always returns the current state of the data, including uncommitted changes, regardless of the current transaction's isolation level, as follows:

*   COUNT counts inserted and updated records, even though those changes have not been committed and may be rolled back.
    
*   COUNT does not count deleted records, even though those deletions have not been committed and may be rolled back.
    

For further details, refer to [SET TRANSACTION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_settransaction) and [START TRANSACTION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_starttransaction).

Examples

The following example returns the total number of rows in Sample.Person:

SELECT COUNT(\*) AS TotalPersons
     FROM Sample.Person

 

The following example returns the count of names, spouses, and favorite colors in Sample.Person. These counts differ because some Spouse and FavoriteColors fields have NULL; COUNT does not count nulls:

SELECT COUNT(Name) AS People,
       COUNT(Spouse) AS PeopleWithSpouses,
       COUNT(FavoriteColors) AS PeopleWithColorPref
FROM Sample.Person

 

The following example returns three values: the total number of rows, the total number of non-NULL values in the FavoriteColors field, and the total number of distinct non-NULL values in the FavoriteColors field:

SELECT COUNT(\*) As TotalPersons,
       COUNT(FavoriteColors) AS WithColorPref,
       COUNT(DISTINCT FavoriteColors) AS ColorPrefs
       FROM Sample.Person

 

The following example uses COUNT DISTINCT to return the count of distinct FavoriteColors values in Sample.Person. It also uses the DISTINCT clause to return one row for each distinct favorite color. The row count and the COUNT DISTINCT count differ, because DISTINCT returns a row for NULL, but COUNT DISTINCT does not count NULL:

SELECT DISTINCT FavoriteColors,
       COUNT(DISTINCT FavoriteColors) AS PeopleWithColorPref
FROM Sample.Person

 

The following example use GROUP BY to return a row for each FavoriteColors value, including a row for NULL. Associated with each row are two counts. The first counts the number or records with that FavoriteColors option; records with NULL are not counted. The second counts the number of names associated with each FavoriteColor choice; since Name does not include NULL values, this enables a count of FavoriteColors with NULL:

SELECT FavoriteColors,
       COUNT(FavoriteColors) AS ColorPreference,
       COUNT(Name) AS People
       FROM Sample.Person
       GROUP BY FavoriteColors

 

The following example returns the count of person records for each Home\_State value in Sample.Person:

SELECT Home\_State, COUNT(\*) AS AllPersons
     FROM Sample.Person
     GROUP BY Home\_State

 

The following example uses %AFTERHAVING to return the count of person records and the count of persons over 65 for each state in which there is at least one person over 65:

SELECT Home\_State, COUNT(Name) AS AllPersons,
     COUNT(Name %AFTERHAVING) AS Seniors
     FROM Sample.Person
     GROUP BY Home\_State
     HAVING Age \> 65
     ORDER BY Home\_State

 

The following example uses both the %FOREACH and the %AFTERHAVING keywords. It returns a row for those states containing people whose names start with “A”, “M”, or “W” (HAVING clause and GROUP BY clause). Each state row contains the following values:

*   COUNT(Name): a count of all of the people in the database. (This number is the same for all rows.)
    
*   COUNT(Name %FOREACH(Home\_State)): a count of all of the people in the state.
    
*   COUNT(Name %AFTERHAVING): a count of all of the people in the database that meet the HAVING clause criteria. (This number is the same for all rows.)
    
*   COUNT(Name %FOREACH(Home\_State) %AFTERHAVING): a count of all of the people in the state that meet the HAVING clause criteria.
    

SELECT Home\_State,
       COUNT(Name) AS NameCount,
       COUNT(Name %FOREACH(Home\_State)) AS StateNameCount,
       COUNT(Name %AFTERHAVING) AS NameCountHaving,
       COUNT(Name %FOREACH(Home\_State) %AFTERHAVING) AS StateNameCountHaving
FROM Sample.Person
GROUP BY Home\_State
HAVING Name LIKE 'A%' OR Name LIKE 'M%' OR Name LIKE 'W%'
ORDER BY Home\_State

 

The following example shows COUNT with a concatenation expression. It returns the total number of non-NULL values in the FavoriteColors field, and the total number of non-NULL values in FavoriteColors concatenated with two other fields, using the concatenate operator (||):

SELECT COUNT(FavoriteColors) AS Color,
       COUNT(FavoriteColors||Home\_State) AS ColorState,
       COUNT(FavoriteColors||Spouse) AS ColorSpouse
       FROM Sample.Person

 

When two fields are concatenated, COUNT counts only those rows in which neither field has a NULL value. Because every row in Sample.Person has a non-NULL Home\_State value, the concatenation FavoriteColors||Home\_State returns the same count as FavoriteColors. Because some rows in Sample.Person have a NULL value for Spouse, the concatenation FavoriteColors||Spouse returns the count of rows which have non-NULL values for both FavoriteColors and Spouse.

See Also

*   [Aggregate Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_aggregatefunctions) overview
    
*   [AVG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_avg) aggregate function
    
*   [SUM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_sum) aggregate function
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_avg "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dlist "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_count.xml**