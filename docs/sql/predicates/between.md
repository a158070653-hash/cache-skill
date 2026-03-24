# BETWEEN

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_between

---

 

Caché SQL Reference

BETWEEN

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_any "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_contains "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Predicate Conditions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_PREDICATE_CONDITONS "SQL Predicate Conditions") \]  >  \[ [BETWEEN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_between "BETWEEN") \]

Go to: Description See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Matches a value to a range of values.

Synopsis

scalar-expression BETWEEN lowval AND highval

Arguments

scalar-expression

A scalar expression (most commonly a data column) whose values are being compared with the range of values between lowval and highval (inclusive).

lowval

Expression that resolves to the low collation sequence value specifying the beginning of a range of values to match with each value in scalar-expression.

highval

Expression that resolves to the high collation sequence value specifying the end of a range of values to match with each value in scalar-expression.

Description

The BETWEEN predicate allows you to select those data values that are in the range specified by lowval and highval. This range is inclusive of the lowval and highval values themselves. This is equivalent to a paired greater than or equal to operator and a less than or equal to operator. This comparison is shown in the following example:

SELECT Name,Age FROM Sample.Person
WHERE Age BETWEEN 18 AND 21
ORDER BY Age

 

This returns all the records in the Sample.Person table with an Age value between 18 and 21, inclusive of those values. Note that you must specify the BETWEEN values in ascending order; a predicate such as BETWEEN 21 AND 18 would return the null string. If none of the scalar expression values fall within the specified range, BETWEEN returns the null string.

Like most predicates, BETWEEN can be inverted using the NOT logical operator. Neither BETWEEN nor NOT BETWEEN can be used to return NULL fields. To return NULL fields use [IS NULL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_isnull). NOT BETWEEN is shown in the following example:

SELECT Name,Age FROM Sample.Person
WHERE Age NOT BETWEEN 20 AND 55
ORDER BY Age

 

This returns all the records in the Sample.Person table with an Age value less than 20 or greater than 55, exclusive of those values.

BETWEEN can be used wherever a [predicate condition](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_predicates) can be specified, as described in the [Overview of Predicates](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_predicates) page of this manual.

Collation Types

BETWEEN is commonly used for a range of numeric values, which collate in numeric order. However, BETWEEN can be used for a collation sequence range of values of any data type.

BETWEEN uses the same collation type as the column it is matching against. By default, string data types collate as not case-sensitive.

If you assign a different collation type to the column, you must also apply this collation type to the BETWEEN substring. This is shown in the following examples:

In the following example, BETWEEN uses the fields’ default letter case collation, which is not case-sensitive. It returns records where Name is higher in alphabetical order than Home\_State, and Home\_State is higher in alphabetical order than Home\_City:

SELECT Name,Home\_State,Home\_City
FROM Sample.Person
WHERE Home\_State BETWEEN Name AND Home\_City
ORDER BY Home\_State

 

In the following example, BETWEEN string comparisons are not case-sensitive by default. This means that the lowval and highval are functionally identical, selecting 'MA' in any lettercase:

SELECT Name,Home\_State FROM Sample.Person
WHERE Home\_State
   BETWEEN 'MA' AND 'Ma'
ORDER BY Home\_State

 

In the following example, the %SQLSTRING collation function causes BETWEEN string comparisons to be case-sensitive. It selects those records with Home\_State values of 'MA' through 'Ma', which in this data set includes 'MA', 'MD', 'ME', 'MO', 'MS', and 'MT':

SELECT Name,Home\_State FROM Sample.Person
WHERE %SQLSTRING(Home\_State) 
   BETWEEN %SQLSTRING('MA') AND %SQLSTRING('Ma')
ORDER BY Home\_State

 

In the following example, the BETWEEN string comparison is not case-sensitive and ignores blank spaces and punctuation marks:

SELECT Name FROM Sample.Person
WHERE %STRING(Name) BETWEEN %STRING('OA') AND %STRING('OZ')
ORDER BY Name

 

Using %STRING, this example can select Odem, O'Donnell, and Olsen. Without the %STRING collation type, O'Donnell would not be selected.

Refer to [%SQLUPPER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_sqlupper) for further information on case transformation functions.

The following example shows BETWEEN used in an INNER JOIN operation ON clause. It is performing a string comparison which is not case sensitive:

SELECT P.Name AS PersonName,E.Name AS EmpName 
FROM Sample.Person AS P INNER JOIN Sample.Employee AS E
ON P.Name BETWEEN 'an' AND 'ch' AND P.Name\=E.Name

 

%SelectMode

If [%SelectMode](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_dynsql#GSQL_dynsql_selectmode) is set to a value other than Logical format, the BETWEEN predicate values must be specified in the %SelectMode format (ODBC or Display). This applies mainly to dates, times, and Caché format lists (%List). Specifying predicate value(s) in Logical format commonly results in an SQLCODE error. For example, SQLCODE -146 “Unable to convert date input to a valid logical date value”.

In the following Dynamic SQL example, the BETWEEN predicate must specify dates in %SelectMode=1 (ODBC) format:

  ZNSPACE "SAMPLES"
  SET q1 \= "SELECT Name,DOB FROM Sample.Person "
  SET q2 \= "WHERE DOB BETWEEN '1950-01-01' AND '1960-01-01'"
  SET myquery \= q1\_q2
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET tStatement.%SelectMode\=1
  SET qStatus \= tStatement.%Prepare(myquery)
   IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
  SET rset \= tStatement.%Execute()
  DO rset.%Display()
  WRITE !,"End of data"

 

See Also

*   [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select) statement [HAVING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_having) clause [WHERE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_where) clause
    
*   [Overview of Predicates](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_predicates)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_any "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_contains "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_between.xml**