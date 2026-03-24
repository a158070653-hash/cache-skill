# Predicates Overview

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_predicates

---

 

Caché SQL Reference

Overview of Predicates

 [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_all "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Predicate Conditions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_PREDICATE_CONDITONS "SQL Predicate Conditions") \]  >  \[ [Overview of Predicates](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_predicates "Overview of Predicates") \]

Go to: Use of Predicates List of Predicates NULL Collation Compound Predicates Collection Predicates with OR Predicates and %SelectMode Suppress Literal Substitution Example See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Describes logical conditions that evaluate to either true or false.

Use of Predicates

A predicate is a condition expression that evaluates to a boolean value, either true or false.

Predicates can be used as follows:

*   In a [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select) statement's [WHERE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_where) clause or [HAVING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_having) clause to determine which rows are relevant to a particular query. Note that not all predicates can be used in a HAVING clause.
    
*   In a [JOIN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_join) operation’s ON clause to determine which rows are relevant to the join operation.
    
*   In an [UPDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_update) or [DELETE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_delete) statement's WHERE clause, to determine which rows are to be modified.
    
*   In a [WHERE CURRENT OF](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_wherecurrentof) statement's AND clause.
    
*   In a [CREATE TRIGGER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtrigger) statement's WHEN clause to determine when to apply triggered action code.
    

List of Predicates

Every predicate contains one or more comparison operators, either symbols or keyword clauses. Caché SQL supports the following comparison operators:

Comparison Operator

Description

\= (equals)

<> (does not equal)

!= (does not equal)

\> (is greater than)

\>= (is greater than or equal to)

< (is less than)

<= (is less than or equal to)

Equality comparison conditions. Can be used for numeric comparisons or string collation order comparisons. For numeric comparisons, an empty string value ('') is evaluated as 0. A NULL in any equality comparison always returns the empty set; use the IS NULL predicate instead. See [Relational Operators](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_langelements#GSQL_langelements_ops_relational) in Using Caché SQL.

IS \[NOT\] NULL

Tests whether a field has undefined (NULL) values. See [NULL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_null).

EXISTS (subquery)

Uses a subquery to test a specified table for existence of one or more rows. See [EXISTS](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_exists).

BETWEEN x AND y

A BETWEEN condition uses >= and <= comparison conditions together. Match must be between two specified range limit values (inclusive). See [BETWEEN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_between).

IN (item1,item2\[...,itemn\])

IN (subquery)

An equality condition that matches a field value to any of the items in a comma-separated list, or any of the items returned by a subquery. See [IN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_in).

%INLIST listfield

An equality condition that matches a field value to any of the elements in a %List structured list. See [%INLIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_inlist).

\[

[Contains operator](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_langelements#GSQL_langelements_ops_relational). Match must contain the specified string. The Contains operator uses EXACT collation, and is therefore case-sensitive. Must specify value in Logical format.

\]

[Follows operator](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_langelements#GSQL_langelements_ops_relational). Match must appear after the specified item in collation sequence. Must specify value in Logical format.

%STARTSWITH string

Match must begin with the specified string. See [%STARTSWITH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_startswith).

%CONTAINS

%CONTAINSTERM

Match word-aware strings with complex text analysis. Match must contain all of the specified single-word or multi-word strings. WHERE clause only; cannot be used in a HAVING clause. See [%CONTAINS](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_contains) and [%CONTAINSTERM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_containsterm).

FOR SOME

A boolean comparison condition. The FOR SOME condition must be true for at least one data value of the specified field. See [FOR SOME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_forsome).

FOR SOME %ELEMENT

A list element comparison condition with a %VALUE or %KEY predicate clause. %VALUE must match the value of at least one element of the list. %KEY must be less than or equal to the number of elements in the list. %VALUE and %KEY clauses can use any of the other comparison operators. See [FOR SOME %ELEMENT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_forsomeelement).

LIKE

A pattern match condition using literals and wildcards. Use LIKE when you wish to return data values that contain a known substring of literal characters, or contain several known substrings in a known sequence. LIKE uses the collation of its target for letter case comparisons. (Contrast with the Contains operator, which uses EXACT collation.) See [LIKE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_like).

%MATCHES

A pattern match condition using literals, wildcards, and lists and ranges. Use %MATCHES when you wish to return data values that contain a known substring of literal characters, or contain one or more literal characters that fall within a list or range of possible characters, or contain several such substrings in a known sequence. %MATCHES uses EXACT collation for letter case comparisons. See [%MATCHES](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_matches).

%PATTERN

A pattern match condition using character types. For example, '1U4L1",".A' (1 uppercase letter, 4 lowercase letters, one literal comma, followed by any number of letter characters of either case). Use %PATTERN when you wish to return data values that contain a known sequence of character types. %PATTERN can specify known literal characters, but is especially useful when the data value is unimportant, but the character type format of those values is significant. See [%PATTERN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_pattern).

ALL

ANY

SOME

A quantified-comparison condition. See [ALL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_all), [ANY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_any), and [SOME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_some).

%INSET

%FIND

Field value comparison conditions that enable filtering of RowId field values using an abstract, programmatically specified temp-file or bitmap index. [%INSET](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_inset) supports simple comparisons. [%FIND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_find) supports comparisons involving a bitmap index.

NULL

A NULL is the absence of any value. By definition, it fails all boolean tests: no value is equal to NULL, no value is unequal to NULL, no value is greater than or less than NULL. Even NULL=NULL fails as a predicate. Because the IN predicate is a series of OR’ed equality tests, it is not meaningful to specify NULL in the IN value list. Therefore, specifying any predicate condition eliminates any instances of that field that are NULL. The only way to include NULL fields in the result set from a predicate condition is to use the IS NULL predicate. This is shown in the following example:

SELECT FavoriteColors FROM Sample.Person
WHERE FavoriteColors \= $LISTBUILD('Red') OR FavoriteColors IS NULL

 

Collation

Most predicate comparisons use the field’s [default collation](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_basics#GSQL_basics_collation). By default, strings values are collated as not case-sensitive. If you specify a collation type, you must specify it on both sides of the comparison. Specifying a collation type can affect index usage; for further details, refer to [Index Collation](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_indices#GSQLOPT_indices_collation) in the “Defining and Building Indices” chapter of the Caché SQL Optimization Guide.

Certain predicate comparisons can involve substrings embedded within a string: the Contains operator (\[), the %MATCHES predicate, and the %PATTERN predicate. These predicates always uses EXACT collation, and are therefore always case-sensitive. Because some collations append a blank space to a string, these predicates could not perform their function if they followed the field’s default collation. However, the LIKE predicate can use wildcards to match substrings embedded within a string. LIKE uses the field’s default collation, which by default is not case-sensitive.

Compound Predicates

A predicate is the simplest version of a condition expression; a condition expression can consist of one or more predicates. You can link multiple predicates together with the AND and OR [logical operators](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_langelements#GSQL_langelements_ops_logical). You can invert the sense of a predicate by placing the NOT unary operator before the predicate. The NOT unary operator only affects the predicate that immediately follows it. Predicates are evaluated in strict left-to-right order. You can use parentheses to group predicates. You can place a NOT unary operator before the opening parentheses to invert the sense of a group of predicates. Spaces are not required before or after parentheses, or between parentheses and logical operators.

The IN and %INLIST predicates are functionally equivalent to multiple OR equality predicates. The following examples are equivalent:

  SET q1\="SELECT Name,Home\_State FROM Sample.Person "
  SET q2\="WHERE Home\_State='MA' OR Home\_State='VT' OR Home\_State='NH'"
  SET myquery\=q1\_q2
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET qStatus \= tStatement.%Prepare(myquery)
  SET rset \= tStatement.%Execute()
  DO rset.%Display()

 

  SET q1\="SELECT Name,Home\_State FROM Sample.Person "
  SET q2\="WHERE Home\_State IN('MA','VT','NH')"
  SET myquery\=q1\_q2
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET qStatus \= tStatement.%Prepare(myquery)
  SET rset \= tStatement.%Execute()
  DO rset.%Display()

 

  SET list\=$LISTBUILD("MA","VT","NH")
  SET q1\="SELECT Name,Home\_State FROM Sample.Person "
  SET q2\="WHERE Home\_State %INLIST(?)"
  SET myquery\=q1\_q2
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET qStatus \= tStatement.%Prepare(myquery)
  SET rset \= tStatement.%Execute(list)
  DO rset.%Display()

 

The FOR SOME %ELEMENT predicate can contain logical operators, as well as be linked to other predicates using logical operators. This is shown in the following example:

SELECT Name,FavoriteColors FROM Sample.Person
WHERE FOR SOME %ELEMENT(FavoriteColors)(%VALUE\='Red' OR %Value\='White' 
      OR %Value %STARTSWITH 'B') 
      AND (Name BETWEEN 'A' AND 'F' OR Name %STARTSWITH 'S')
ORDER BY Name 

 

Note the parentheses around (Name BETWEEN 'A' AND 'F' OR Name %STARTSWITH 'S'); without these grouping parentheses, the FOR SOME %ELEMENT condition would not apply to Name %STARTSWITH 'S'.

Collection Predicates with OR

FOR SOME %ELEMENT, %CONTAINS, and %CONTAINSTERM are Collection Predicates. The use of these predicates with the OR logical operator is restricted, as follows. The OR logical operator cannot be used to associate a Collection Predicate that references a table field with a predicate that a references a field in a different table. For example,

WHERE FOR SOME %ELEMENT(t1.FavoriteColors) (%VALUE='purple') 
OR t2.Age < 65

Because this restriction depends on how the optimizer uses indices, SQL may only enforce this restriction when indices are added to a table. It is strongly suggested that this type of logic be avoided in all queries.

Predicates and %SelectMode

All predicates perform their comparisons using Logical (internal storage) data values. However, some predicates can perform format mode conversion on the predicate value(s), converting it from ODBC or Display format to Logical format. Other predicates cannot perform format mode conversion, and therefore must always specify the predicate value in Logical format.

Predicates that perform format mode conversion determine whether conversion is required from the data type (such as DATE or %List) of the matching field and determine the type of conversion from the [%SelectMode setting](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_dynsql#GSQL_dynsql_selectmode). If %SelectMode is set to a value other than Logical format (such as %SelectMode=ODBC or %SelectMode=Display) the predicate value(s) must be specified in the correct ODBC or Display format.

*   Equality predicate perform format mode conversion. Caché converts the predicate value to Logical format, then matches it with the field values. If %SelectMode is set to a mode other than Logical format, the predicate value(s) must be specified in the %SelectMode format (ODBC or Display) for data types whose display value differs from the Logical storage value. For example, dates, times, and %List-formatted strings. Because Caché automatically performs this format conversion, specifying this type of predicate value in Logical format commonly results in an SQLCODE error. For example, SQLCODE -146 “Unable to convert date input to a valid logical date value” (Caché assumes the supplied Logical value is an ODBC or Display value and attempts to convert it to a Logical value — which doesn’t succeed.) Affected predicates include =, <, >, BETWEEN, and IN.
    
*   Pattern predicates cannot perform format mode conversion, because Caché cannot meaningfully convert the predicate value. Therefore, the predicate value must be specified in Logical format, regardless of the %SelectMode setting. Specifying predicate value(s) in ODBC or Display format commonly results in no data matches or unintended data matches. Affected predicates include %INLIST, LIKE, %MATCHES, %PATTERN, %STARTSWITH, \[ (the Contains operator), and \] (the Follows operator).
    

You can use the [%INTERNAL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_internal), [%EXTERNAL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_external), or [%ODBCOUT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_odbcout) format-transform functions to transform the field that the predicate operates upon. This allows you to specify the predicate value in another format. For example, WHERE %ODBCOut(DOB) %STARTSWITH '1955-'. However, specifying a format-transform function on a matching field prevents the use of an index for the field. This can have a significant negative effect upon performance.

In the following Dynamic SQL example, the BETWEEN predicate (an equality predicate) must specify dates in %SelectMode=1 (ODBC) format:

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

 

In the following Dynamic SQL examples, the %STARTSWITH predicate (a pattern predicate) cannot perform format mode conversion. The first example attempts to specify a %STARTSWITH for dates in the %SelectMode=ODBC format for years in the 1950s. However, because the table does not contain birth dates that begin with $HOROLOG 195 (dates in the year 1894), no rows are selected:

  ZNSPACE "SAMPLES"
  SET q1 \= "SELECT Name,DOB FROM Sample.Person "
  SET q2 \= "WHERE DOB %STARTSWITH '195'"
  SET myquery \= q1\_q2
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET tStatement.%SelectMode\=1
  SET qStatus \= tStatement.%Prepare(myquery)
   IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
  SET rset \= tStatement.%Execute()
  DO rset.%Display()
  WRITE !,"End of data"

 

The following example uses the %ODBCOut format-transform function on the matching DOB field so that %STARTSWITH can be used to select for years in the 1950s in ODBC format. However, note that this usage prevents the use of an index on the DOB field.

  ZNSPACE "SAMPLES"
  SET q1 \= "SELECT Name,DOB FROM Sample.Person "
  SET q2 \= "WHERE %ODBCOut(DOB) %STARTSWITH '195'"
  SET myquery \= q1\_q2
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET tStatement.%SelectMode\=1
  SET qStatus \= tStatement.%Prepare(myquery)
   IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
  SET rset \= tStatement.%Execute()
  DO rset.%Display()
  WRITE !,"End of data"

 

In the following example the %STARTSWITH predicate specifies a %STARTSWITH for dates in Logical (internal) format. Rows with DOB Logical values beginning with 41 (dates from April 4 1953 ($HOROLOG 41000) through December 28 1955 ($HOROLOG 41999)) are selected. The DOB field index is used:

  ZNSPACE "SAMPLES"
  SET q1 \= "SELECT Name,DOB FROM Sample.Person "
  SET q2 \= "WHERE DOB %STARTSWITH '41'"
  SET myquery \= q1\_q2
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET tStatement.%SelectMode\=1
  SET qStatus \= tStatement.%Prepare(myquery)
   IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
  SET rset \= tStatement.%Execute()
  DO rset.%Display()
  WRITE !,"End of data"

 

Suppress Literal Substitution

You can [suppress literal substitution](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_cachedqueries#GSQLOPT_cachedqueries_nolitsub) during compile pre-parsing by enclosing the predicate argument in double parentheses. For example, LIKE(('abc%')). This may improve query performance by improving overall selectivity and/or subscript bounding selectivity. However, it should be avoided when the same query is called multiple times with different values, as it will result in the creation of a separate cached query for each query call.

Example

The following example uses a variety of conditions in the WHERE clause of a query:

SELECT PurchaseOrder FROM MyTable 
   WHERE OrderTotal \>= 1000 
   AND ItemName %STARTSWITH :partname 
   AND AnnualOrders BETWEEN 50000 AND 100000 
   AND City LIKE 'Ch%' 
   AND CustomerNumber IN 
      (SELECT CustNum FROM TheTop100 
       WHERE TheTop100.City\='Boston') 
   AND :minorder \> SOME 
      (SELECT OrderTotal FROM Orders 
       WHERE Orders.Customer \= :cust)

See Also

*   [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select) statement, [HAVING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_having) clause, [WHERE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_where) clause
    
*   [CREATE TRIGGER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtrigger)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

  [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_all "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_predicates.xml**