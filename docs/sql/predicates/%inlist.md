# %INLIST

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_inlist

---

 

Caché SQL Reference

%INLIST

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_in "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_inset "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Predicate Conditions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_PREDICATE_CONDITONS "SQL Predicate Conditions") \]  >  \[ [%INLIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_inlist "%INLIST") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Matches a value to the elements in a %List structured list.

Synopsis

scalar-expression %INLIST list \[SIZE ((nn))\]

Arguments

scalar-expression

A scalar expression (most commonly a data column) whose values are being compared with list elements.

list

A %List structure containing one or more elements.

SIZE ((nn))

Optional — An integer specifying an order-of-magnitude estimate of the number of elements in list. Must be specified as a literal with one of the following values: 10, 100, 1000, 10000, and so forth.

Description

The %INLIST predicate is a Caché extension for matching the values of a field with the elements of a list structure. Both %INLIST and IN allow you to perform such equality comparisons with multiple specified values. %INLIST specifies these multiple values as the elements of a single list argument. Therefore, %INLIST allows you to vary the number of values to match without creating a separate cached query.

The optional %INLIST SIZE clause provides the integer nn, which specifies an order-of-magnitude estimate of the number of list elements in list. Caché uses this order-of-magnitude estimate to determine the optimal query plan. Because the same cached query is used regardless of the number of elements in list, specifying SIZE allows you to create a cached query optimized for the anticipated approximate number of elements in list. Changing the SIZE literal creates a separate cached query. Specify nn as one of the following literals: 10, 100, 1000, 10000, etc. Because nn must be available as a constant value at compile time, it must be specified as a literal in all SQL code. Note that double parentheses must be specified as shown for all compiled SQL (Dynamic SQL). Double parentheses are not used with Embedded SQL. For further details, refer to the “[Cached Queries](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_cachedqueries)” chapter in Caché SQL Optimization Guide.

%INLIST performs an equality comparison with each of the elements of list. %INLIST comparisons use the collation type of scalar-expression. Therefore, comparisons of list elements may be case-sensitive or not case-sensitive, depending on the collation of scalar-expression.

It is not meaningful to specify NULL as a comparison value. NULL is the absence of a value, and therefore fails all equality tests. Specifying an %INLIST predicate (or any other predicate) eliminates any instances of the specified field that are NULL. You must specify the IS NULL predicate to include fields with NULL in the predicate result set.

Like most predicates, %INLIST can be inverted using the NOT logical operator. Neither %INLIST nor NOT %INLIST can be used to return NULL fields. To return NULL fields use [IS NULL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_isnull).

If the match expression is not in %List format, %INLIST generates an SQLCODE -400 error. For example, if the class is a MultiValue class (specified with %MV.Adaptor), or the SqlListType of the collection property is DELIMITED, the logical value of the list field is not in %List format. For further details on list structures, see the SQL [$LIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_list) function.

%INLIST can be used wherever a [predicate condition](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_predicates) can be specified, as described in the [Overview of Predicates](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_predicates) page of this manual.

For matching a value to an unstructured series of items, such as a comma-separated list of values, use the [IN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_in) predicate. IN can perform equality comparisons and subquery comparisons.

%SelectMode

The %INLIST predicate does not use the current [%SelectMode](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_dynsql#GSQL_dynsql_selectmode) setting. The elements of list should be specified in Logical format, regardless of the %SelectMode setting. Attempting to specify list elements in ODBC format or Display format commonly results in no data matches or unintended data matches.

You can use the [%EXTERNAL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_external) or [%ODBCOUT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_odbcout) format-transform functions to transform the scalar-expression field that the predicate operates upon. This allows you to specify the list elements in Display format or ODBC format. However, using a format-transform function prevents the use of the index for the field, and can thus have a significant performance impact.

In the following Dynamic SQL example, the %INLIST predicate specifies a list containing date value elements for the year 1978 in Logical format, not in %SelectMode=1 (ODBC) format. Dates that correspond to these $HOROLOG format dates are selected:

  ZNSPACE "SAMPLES"
  SET bday\=$LISTBUILD(50039)
  FOR i\=50039:1:50403 {SET bday\=bday\_$LISTBUILD(i) }
  SET q1 \= "SELECT Name,DOB FROM Sample.Person "
  SET q2 \= "WHERE DOB %INLIST ?"
  SET myquery \= q1\_q2
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET tStatement.%SelectMode\=1
  SET qStatus \= tStatement.%Prepare(myquery)
   IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
  SET rset \= tStatement.%Execute(bday)
  DO rset.%Display()

 

The following Dynamic SQL example uses the [%ODBCOUT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_odbcout) format-transform function to transform the DOB field matched by the predicate. This allows you to specify the %INLIST list elements in ODBC format. However, specifying the format-transform function prevents the use of an index for DOB field values:

  ZNSPACE "SAMPLES"
  SET births\=$LISTBUILD("1978-01-15","1978-08-22","1978-10-01")
  SET q1 \= "SELECT Name,DOB FROM Sample.Person "
  SET q2 \= "WHERE %ODBCOUT(DOB) %INLIST ?"
  SET myquery \= q1\_q2
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET tStatement.%SelectMode\=1
  SET qStatus \= tStatement.%Prepare(myquery)
   IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
  SET rset \= tStatement.%Execute(births)
  DO rset.%Display()

 

%INLIST and IN

Both the %INLIST and IN predicates can be used to supply multiple values to use for equality comparisons. The following Dynamic SQL examples return the same results:

  ZNSPACE "SAMPLES"
  SET states\=$LISTBUILD("VT","NH","ME")
  SET myquery \= "SELECT Name,Home\_State FROM Sample.Person WHERE Home\_State %INLIST ?"
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET qStatus \= tStatement.%Prepare(myquery)
  IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
  SET rset \= tStatement.%Execute(states)
  DO rset.%Display()

 

  ZNSPACE "SAMPLES"
  SET s1\="VT"
  SET s2\="NH"
  SET s3\="ME"
  SET myquery \= "SELECT Name,Home\_State FROM Sample.Person WHERE Home\_State IN(?,?,?)"
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET qStatus \= tStatement.%Prepare(myquery)
  IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
  SET rset \= tStatement.%Execute(s1,s2,s3)
  DO rset.%Display()

 

However, in Dynamic SQL you can supply the %INLIST predicate values as a single host variable; you must supply the IN predicate values as individual host variables. Therefore, changing the number of IN predicate values results in the creation of a separate cached query. Changing the number of %INLIST predicate values does not result in the creation of a separate cached query. For further details, refer to the “[Cached Queries](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_cachedqueries)” chapter in Caché SQL Optimization Guide.

Examples

The following example matches Home\_State column values to the elements of a structured list of northern New England states:

  ZNSPACE "SAMPLES"
  SET states\=$LISTBUILD("VT","NH","ME")
  SET myquery\="SELECT Name,Home\_State FROM Sample.Person "\_
              "WHERE Home\_State %INLIST ?"
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET qStatus \= tStatement.%Prepare(myquery)
  IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
  SET rset \= tStatement.%Execute(states)
  DO rset.%Display()

 

The following two examples show that collation matching is based on the left-hand scalar-expression collation, not the right-hand list element collation. The list in these examples specifies New Hampshire as “nH”, rather than “NH”. The first example does not return NH Home\_State values, the second example does return NH Home\_State values:

  ZNSPACE "SAMPLES"
  SET states\=$LISTBUILD("VT","nH","ME")
  SET myquery\="SELECT Name,Home\_State FROM Sample.Person "\_
              "WHERE %EXACT(Home\_State) %INLIST ?"
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET qStatus \= tStatement.%Prepare(myquery)
  IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
  SET rset \= tStatement.%Execute(states)
  DO rset.%Display()

 

  ZNSPACE "SAMPLES"
  SET states\=$LISTBUILD("VT","nH","ME")
  SET myquery\="SELECT Name,Home\_State FROM Sample.Person "\_
              "WHERE Home\_State %INLIST ?"
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET qStatus \= tStatement.%Prepare(myquery)
  IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
  SET rset \= tStatement.%Execute(states)
  DO rset.%Display()

 

The following example creates a cached query with a SIZE literal of 10. Specifying SIZE 10 is optimal for this query, because 10 corresponds in order-of-magnitude to the actual number of elements in the list. Changing the number of elements in the list does not create a separate cached query. Changing the SIZE literal does create a separate cached query:

  ZNSPACE "SAMPLES"
  SET states\=$LISTBUILD("VT","NH","ME")
  SET myquery\="SELECT Name,Home\_State FROM Sample.Person "\_
              "WHERE Home\_State %INLIST ? SIZE ((10))"
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET qStatus \= tStatement.%Prepare(myquery)
  IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
  SET rset \= tStatement.%Execute(states)
  DO rset.%Display()

 

  ZNSPACE "SAMPLES"
  SET states\=$LISTBUILD("VT","nH","ME")
  SET myquery\="SELECT Name,Home\_State FROM Sample.Person "\_
              "WHERE Home\_State %INLIST ?"
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET qStatus \= tStatement.%Prepare(myquery)
  IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
  SET rset \= tStatement.%Execute(states)
  DO rset.%Display()

 

The following example creates a cached query with a SIZE literal of 10. Specifying SIZE 10 is optimal for this query, because 10 corresponds in order-of-magnitude to the actual number of elements in the list. Changing the number of elements in the list does not create a separate cached query. Changing the SIZE literal does create a separate cached query:

  ZNSPACE "SAMPLES"
  SET states\=$LISTBUILD("VT","NH","ME")
  SET myquery\="SELECT Name,Home\_State FROM Sample.Person "\_
              "WHERE Home\_State %INLIST ? SIZE ((10))"
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET qStatus \= tStatement.%Prepare(myquery)
  IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
  SET rset \= tStatement.%Execute(states)
  DO rset.%Display()

 

See Also

*   [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select) statement, [HAVING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_having) clause, [WHERE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_where) clause
    
*   [$LISTBUILD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listbuild) function
    
*   [IN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_in) predicate
    
*   [Overview of Predicates](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_predicates)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_in "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_inset "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_inlist.xml**