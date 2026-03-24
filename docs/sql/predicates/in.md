# IN

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_in

---

 

Caché SQL Reference

IN

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_forsomeelement "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_inlist "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Predicate Conditions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_PREDICATE_CONDITONS "SQL Predicate Conditions") \]  >  \[ [IN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_in "IN") \]

Go to: Description See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Matches a value to items in an unstructured comma-separated list.

Synopsis

scalar-expression IN (item1,item2\[,...\])

scalar-expression IN (subquery)

Description

The IN predicate is used for matching a value to an unstructured series of items. Typically, it compares column data values to a comma-separated list of values. IN can perform [equality comparisons](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_in#RSQL_in_equality) and [subquery comparisons](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_in#RSQL_in_subquery).

Like most predicates, IN can be inverted using the NOT logical operator. Neither IN nor NOT IN can be used to return NULL fields. To return NULL fields use [IS NULL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_isnull).

IN can be used wherever a [predicate condition](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_predicates) can be specified, as described in the [Overview of Predicates](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_predicates) page of this manual.

Equality Comparison

The IN predicate can serve as shorthand for the use of multiple equality comparisons linked together with the OR operator. For instance:

SELECT Name, Home\_State FROM Sample.Person
WHERE Home\_State IN ('ME','NH','VT','MA','RI','CT') 

 

evaluates true if Home\_State equals any of the values in the comma-separated list. The listed items can be constants or expressions.

IN comparisons use the [collation type](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_basics#GSQL_basics_collation) of scalar-expression, regardless of the collation type of the individual items. By default, comparisons to a string data field are not case-sensitive. The following two examples show that collation matching is based on the left-hand scalar-expression collation, not the right-hand item collation. An item in these examples specifies New Hampshire as “nH”, rather than “NH”. The first example does not return NH Home\_State values, the second example does return NH Home\_State values:

SELECT Name, Home\_State FROM Sample.Person
WHERE %EXACT(Home\_State) IN ('ME','nH','VT','MA','RI','CT') 

 

SELECT Name, Home\_State FROM Sample.Person
WHERE Home\_State IN ('ME',%EXACT('nH'),'VT','MA','RI','CT') 

 

It is not meaningful to include NULL in the list of values. NULL is the absence of a value, and therefore fails all equality tests. Specifying an IN predicate (or any other predicate) eliminates any instances of the specified field that are NULL. This is shown in the following incorrect (but executable) example:

SELECT FavoriteColors FROM Sample.Person
WHERE FavoriteColors IN ($LISTBUILD('Red'),$LISTBUILD('Blue'),NULL)
  /\* NULL here is meaningless. No FavoriteColor NULL fields returned \*/

 

The only way to include a field with NULL in the predicate result set is to specify the IS NULL predicate, as shown in the following example:

SELECT FavoriteColors FROM Sample.Person
WHERE FavoriteColors IN ($LISTBUILD('Red'),$LISTBUILD('Blue')) OR FavoriteColors IS NULL

 

When dates or times are used for IN predicate equality comparisons, the appropriate data type conversions are automatically performed. If the WHERE field is type TimeStamp, values of type Date or Time are converted to Timestamp. If the WHERE field is type Date, values of type TimeStamp or String are converted to Date. If the WHERE field is type Time, values of type TimeStamp or String are converted to Time.

The following examples both perform the same equality comparisons and return the same data. The DOB field is of data type Date:

SELECT Name,DOB FROM Sample.Person 
WHERE DOB IN ({d '1951-02-02'},{d '1987-02-28'})

 

SELECT Name,DOB FROM Sample.Person 
WHERE DOB IN ({ts '1951-02-02 02:37:00'},{ts '1987-02-28 16:58:10'})

 

For further details refer to [Date and Time Constructs](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dateconstruct).

%SelectMode

If [%SelectMode](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_dynsql#GSQL_dynsql_selectmode) is set to a value other than Logical format, the IN predicate values must be specified in the %SelectMode format (ODBC or Display). This applies mainly to dates, times, and Caché format lists (%List). Specifying predicate values in Logical format commonly results in an SQLCODE error. For example, SQLCODE -146 “Unable to convert date input to a valid logical date value”.

In the following Dynamic SQL example, the IN predicate must specify dates in %SelectMode=1 (ODBC) format:

  ZNSPACE "SAMPLES"
  SET q1 \= "SELECT Name,DOB FROM Sample.Person "
  SET q2 \= "WHERE DOB IN('1956-03-05','1956-04-08','1956-04-18','1956-12-08')"
  SET myquery \= q1\_q2
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET tStatement.%SelectMode\=1
  SET qStatus \= tStatement.%Prepare(myquery)
   IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
  SET rset \= tStatement.%Execute()
  DO rset.%Display()
  WRITE !,"End of data"

 

Subquery Comparison

You can use the IN predicate with a subquery to test whether a column value (or any other expression) equals any of the subquery row values. For example:

SELECT Name,Home\_State FROM Sample.Person
WHERE Name IN 
   (SELECT Name FROM Sample.Employee
    HAVING Salary < 50000)

 

Note that the subquery must have exactly one select-item in the SELECT list.

The following example uses an IN subquery to return the Employee states that are not Vendor states:

SELECT Home\_State
FROM Sample.Employee
WHERE Home\_State NOT IN (SELECT Address\_State FROM Sample.Vendor)
GROUP BY Home\_State

 

The following example matches a collation function expression to an IN predicate with a subquery:

SELECT Name,Id FROM Sample.Person
WHERE %EXACT(Spouse) NOT IN
   (SELECT Id FROM Sample.Person
    WHERE Age < 65)

 

An IN cannot specify both a subquery and a comma-separated list of literal values.

Literal Substitution Override

You can override literal substitution during compile pre-parsing by enclosing each IN predicate argument with parentheses. For example, WHERE Home\_State IN (('ME'),('NH'),('VT'),('MA'),('RI'),('CT')). This may improve query performance by improving overall selectivity and/or subscript bounding selectivity. However, it should be avoided when the same query is called multiple times with different values, as it will result in the creation of a separate cached query for each query call. For further details, refer to [Literal Substitution](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_cachedqueries#GSQLOPT_cachedqueries_litsub) in the “Cached Queries” chapter of the Caché SQL Optimization Guide.

IN and %INLIST

Both the IN and %INLIST predicates can be used to supply multiple values to use for OR equality comparisons. The %INLIST predicate is used for matching a value to the elements of a %List structure. In Dynamic SQL you can supply the %INLIST predicate values as a single host variable. You must supply the IN predicate values as individual host variables. Therefore, changing the number of IN predicate values results in the creation of a separate cached query. %INLIST takes a single predicate value, a %List with multiple elements; changing the number of %List elements does not result in the creation of a separate cached query. %INLIST also provides an order-of-magnitude SIZE argument that SQL uses to optimize performance. For these reasons it is often advantageous to use %INLIST(%LISTFROMSTRING(val)) rather than IN(val1,val2,val3,..valn).

%INLIST can perform equality comparisons; it cannot perform subquery comparisons.

For further details, refer to [%INLIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_inlist).

See Also

*   [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select) statement [HAVING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_having) clause [WHERE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_where) clause
    
*   [%INLIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_inlist) predicate
    
*   [Overview of Predicates](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_predicates)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_forsomeelement "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_inlist "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_in.xml**