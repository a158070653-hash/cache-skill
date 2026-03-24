# LIST

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_list

---

 

Caché SQL Reference

LIST

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dlist "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_max "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Aggregate Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_AGGREGATE_FUNCTIONS "SQL Aggregate Functions") \]  >  \[ [LIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_list "LIST") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

An aggregate function that creates a comma-separated list of values.

Synopsis

LIST(\[ALL | DISTINCT\] string-expr \[%FOREACH(col-list)\] \[%AFTERHAVING\])

Arguments

ALL

Optional — Specifies that LIST returns a list of all values for string-expr. This is the default if no keyword is specified.

DISTINCT

Optional — Specifies that LIST returns a list containing only the unique string-expr values. If not specified, the default is ALL.

string-expr

An SQL expression that evaluates to a string. Usually the name of a column from the selected table.

%FOREACH(col-list)

Optional — A column name or a comma-separated list of column names. See [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select) for further information on %FOREACH.

%AFTERHAVING

Optional — Applies the condition found in the [HAVING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_having) clause.

Description

The LIST [aggregate function](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_aggregatefunctions) returns a comma-separated list of the values in the specified column.

A simple LIST (or LIST ALL) returns a string containing a comma-separated list composed of all the values for string-expr in the selected rows. Rows where string-expr is the empty string ('') are represented by a placeholder comma in the comma-separated list. Rows where string-expr is NULL are not included in the comma-separated list. If there is only one string-expr value, and it is the empty string (''), LIST returns the empty string.

A LIST DISTINCT returns a string containing a comma-separated list composed of all the different (unique) values for string-expr in the selected rows. The NULL string-expr is not included in the comma-separated list.

LIST and %SelectMode

You can use the [%SelectMode](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_dynsql#GSQL_dynsql_selectmode) property to specify the data display mode returned by LIST: 0=Logical (the default), 1=ODBC, 2=Display.

Note that LIST separates column values with commas, and ODBC mode separates elements within a %List column value with commas. Therefore, using ODBC mode when using LIST on a %List structure produces ambiguous results.

Related Aggregate Functions

*   LIST returns a comma-separated list of values.
    
*   [%DLIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dlist) returns a Caché list containing an element for each value.
    
*   [XMLAGG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_xmlagg) returns a concatenated string of values.
    

Examples

The following Embedded SQL example returns a host variable containing a comma-separated list of all of the values listed in the Home\_State column of the Sample.Person table that start with the letter “A”:

  &sql(SELECT LIST(Home\_State)
       INTO :statelist
       FROM Sample.Person
       WHERE Home\_State %STARTSWITH 'A')
  WRITE "The states are:",statelist

 

Note that this list contains duplicate values.

The following Embedded SQL example returns a host variable containing a comma-separated list of all of the distinct (unique) values listed in the Home\_State column of the Sample.Person table that start with the letter “A”:

  &sql(SELECT LIST(DISTINCT Home\_State)
       INTO :statelist
       FROM Sample.Person
       WHERE Home\_State %STARTSWITH 'A')
  WRITE "The states are:",statelist

 

The following SQL example creates a comma-separated list of all of the values found in the Home\_City column for each of the states, and a count of these city values by state. Every Home\_State row contains a list of all of the Home\_City values for that state. These lists may include duplicate city names:

SELECT Home\_State,
       LIST(Home\_City) AS AllCities,
       COUNT(Home\_City) AS CityCount
FROM Sample.Person
GROUP BY Home\_State

 

Perhaps more useful would be a comma-separated list of all of the distinct values found in the Home\_City column for each of the states, as shown in the following example:

SELECT Home\_State,
       LIST(DISTINCT Home\_City) AS CitiesList,
       COUNT(DISTINCT Home\_City) AS DistinctCities,
       COUNT(Home\_City) AS TotalCities
FROM Sample.Person
GROUP BY Home\_State

 

Note that this example returns integer counts of both the distinct city names and the total city names for each state.

The following Dynamic SQL example uses the %SelectMode property to specify the ODBC display mode for the list of values returned by the DOB date field:

  ZNSPACE "SAMPLES"
  SET myquery \= "SELECT LIST(DOB) FROM Sample.Person WHERE Name %STARTSWITH 'A'"
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET tStatement.%SelectMode\=1
  SET qStatus \= tStatement.%Prepare(myquery)
   IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
  SET rset \= tStatement.%Execute()
  DO rset.%Display()
  WRITE !,"End of data"

 

The following example uses the %AFTERHAVING keyword. It returns a row for each Home\_State that contains at least one Name value that fulfills the HAVING clause condition (a name that begins with “M”). The first LIST function returns a list of all of the names for that state. The second LIST function returns a list containing only those names that fulfill the HAVING clause condition:

SELECT Home\_State,
       LIST(Name) AS AllNames,
       LIST(Name %AFTERHAVING) AS HaveClauseNames
    FROM Sample.Person
    GROUP BY Home\_State
    HAVING Name LIKE 'M%'
    ORDER BY Home\_state

 

See Also

*   [Aggregate Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_aggregatefunctions) overview
    
*   [%DLIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dlist) aggregate function
    
*   [XMLAGG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_xmlagg) aggregate function
    
*   [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select) statement
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dlist "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_max "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_list.xml**