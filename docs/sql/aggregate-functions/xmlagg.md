# XMLAGG

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_xmlagg

---

 

Caché SQL Reference

XMLAGG

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_variance "Go back to the previous section.") 

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Aggregate Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_AGGREGATE_FUNCTIONS "SQL Aggregate Functions") \]  >  \[ [XMLAGG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_xmlagg "XMLAGG") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

An aggregate function that creates a concatenated string of values.

Synopsis

XMLAGG(\[ALL | DISTINCT\] string-expr \[%FOREACH(col-list)\] \[%AFTERHAVING\])

Arguments

ALL

Optional — Specifies that XMLAGG returns a concatenated string of all values for string-expr. This is the default if no keyword is specified.

DISTINCT

Optional — Specifies that XMLAGG returns a concatenated string containing only the unique string-expr values. If not specified, the default is ALL.

string-expr

An SQL expression that evaluates to a string. Commonly this is the name of a column from which to retrieve data.

%FOREACH(col-list)

Optional — A column name or a comma-separated list of column names. See [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select) for further information on %FOREACH.

%AFTERHAVING

Optional — Applies the condition found in the [HAVING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_having) clause.

Description

The XMLAGG [aggregate function](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_aggregatefunctions) returns a concatenated string of all values from string-expr. The return value is of data type VARCHAR, with a default length of 4096.

*   A simple XMLAGG (or XMLAGG ALL) returns a string containing a concatenated string composed of all the values for string-expr in the selected rows. Rows where string-expr is NULL are ignored.
    
    The following two examples both return the same single value, a concatenated string of all of the values listed in the Home\_State column of the Sample.Person table.
    
    SELECT XMLAGG(Home\_State) AS All\_State\_Values
    FROM Sample.Person
    
     
    
    SELECT XMLAGG(ALL Home\_State) AS ALL\_State\_Values
    FROM Sample.Person
    
     
    
    Note that this concatenated string contains duplicate values.
    
*   An XMLAGG DISTINCT returns a concatenated string composed of all the distinct (unique) values for string-expr. Rows where string-expr is NULL are ignored.
    
    The following example creates a concatenated string of all of the distinct (unique) values listed in the Home\_State column of the Sample.Person table:
    
    SELECT XMLAGG(DISTINCT Home\_State) AS DISTINCT\_State\_Values
    FROM Sample.Person
    
     
    

Rows where string-expr is NULL are omitted from the return value. Rows where string-expr is the empty string ('') are omitted from the return value if at least one non-empty string value is returned. If the only non-NULL string-expr values are the empty string (''), the return value is a single empty string.

XMLAGG does not support data stream fields. Specifying a stream field for string-expr results in an SQLCODE -37.

XML and XMLAGG

One common use of XMLAGG is to tag each data item from a column. This is done by combining XMLAGG and XMLELEMENT as shown in the following example:

SELECT XMLAGG(XMLELEMENT("para",Home\_State))
FROM Sample.Person

This results in an output string such as the following:

<para>LA</para><para>MN</para><para>LA</para><para>NH</para><para>ME</para>...

Related Aggregate Functions

*   XMLAGG returns a concatenated string of values.
    
*   [LIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_list) returns a comma-separated list of values.
    
*   [%DLIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dlist) returns a Caché list containing an element for each value.
    

Examples

The following example creates a concatenated string of all of the distinct values found in the FavoriteColors column of the Sample.Person table. Thus every row has the same value for the All\_Colors column. Note that while some rows have a NULL value for FavoriteColors, this value is not included in the concatenated string. Data values are returned in internal format.

SELECT Name,FavoriteColors,
   XMLAGG(DISTINCT FavoriteColors) AS All\_Colors\_In\_Table
FROM Sample.Person
ORDER BY FavoriteColors

 

The following example creates a concatenated string of all of the distinct values found in the Home\_City column for each of the states. Every row from the same state contains a list of all of the distinct city values for that state:

SELECT Home\_State, Home\_City,
   XMLAGG(DISTINCT Home\_City %FOREACH(Home\_State)) AS All\_Cities\_In\_State
FROM Sample.Person
ORDER BY Home\_State

 

The following example uses the %AFTERHAVING keyword. It returns a row for each Home\_State that contains at least one Name value that fulfills the HAVING clause condition (a name that begins with either “C” or “K”). The first XMLAGG function returns a concatenated string consisting of all of the names for that state. The second XMLAGG function returns a concatenated string consisting of only those names that fulfill the HAVING clause condition:

SELECT Home\_State,
       XMLAGG(Name) AS AllNames,
       XMLAGG(Name %AFTERHAVING) AS HaveClauseNames
    FROM Sample.Person
    GROUP BY Home\_State
    HAVING Name LIKE 'C%' OR Name LIKE 'K%' 
    ORDER BY Home\_state

 

For the following examples, suppose we have the following table, AutoClub:

Name

Make

Model

Year

Smith,Joe

Pontiac

Firebird

1971

Smith,Joe

Saturn

SW2

1997

Smith,Joe

Pontiac

Bonneville

1999

Jones,Scott

Ford

Mustang

1966

Jones,Scott

Mazda

Miata

2000

The query:

SELECT DISTINCT Name, XMLAGG(Make) AS String\_Of\_Makes
FROM AutoClub WHERE Name \= 'Smith,Joe'

returns:

Name

String\_Of\_Makes

Smith,Joe

PontiacSaturnPontiac

The query:

SELECT DISTINCT Name, XMLAGG(DISTINCT Make) AS String\_Of\_Makes
FROM AutoClub WHERE Name \= 'Smith,Joe'

returns:

Name

String\_Of\_Makes

Smith,Joe

PontiacSaturn

See Also

*   [Aggregate Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_aggregatefunctions) overview
    
*   [%DLIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dlist) aggregate function
    
*   [LIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_list) aggregate function
    
*   [XMLELEMENT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_xmlelement) function
    
*   [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select) statement
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_variance "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:38**

Source: **RSQL\_xmlagg.xml**