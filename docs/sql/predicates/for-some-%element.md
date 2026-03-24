# FOR SOME %ELEMENT

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_forsomeelement

---

 

Caché SQL Reference

FOR SOME %ELEMENT

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_forsome "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_in "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Predicate Conditions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_PREDICATE_CONDITONS "SQL Predicate Conditions") \]  >  \[ [FOR SOME %ELEMENT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_forsomeelement "FOR SOME %ELEMENT") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Matches list element values or the number of list elements with a predicate.

Synopsis

FOR SOME %ELEMENT(field) \[\[AS\] e-alias\] (predicate)

Arguments

field

A scalar expression (most commonly a data column) whose elements are being compared with predicate.

AS e-alias

Optional — An element alias used to qualify %KEY or %VALUE within the predicate. Commonly, this alias is used when the predicate contains a nested FOR SOME %ELEMENT condition. The alias must be a valid [identifier](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_identifiers). For further details see the “Identifiers” chapter of Using Caché SQL. The AS keyword is optional.

(predicate)

A predicate condition, enclosed in parentheses. Within this condition use %VALUE and/or %KEY to determine what the condition is matching. %VALUE matches the element value (%VALUE=’Red’). %KEY matches the minimum number of elements (%KEY=2). Within this condition, %VALUE and %KEY may be optionally qualified if you have specified an e-alias. This predicate can consist of multiple condition expressions with AND and OR logical operators.

Description

The FOR SOME %ELEMENT predicate matches the list elements in field with the specified predicate. The SOME keyword specifies that at least one of the elements in the field must satisfy the specified predicate clause.

The predicate clause must contain either the %VALUE or the %KEY keyword, followed by a predicate condition. These keywords are not case sensitive.

The use of %VALUE and %KEY is explained in the following examples:

*   (%VALUE=’Red’) matches all field values that contain the value Red as one of their list elements. The field may only contain the single element Red, or it may contain multiple elements, one of which is the element Red.
    
*   (%KEY=2) matches all field values that contain at least 2 elements. The field may contain exactly two elements or it may contain more than two elements. The %KEY value must be a positive integer. (%KEY=0) does not match any field values.
    

FOR SOME %ELEMENT cannot be used to match a field that is NULL.

The predicate clause can use any [predicate condition](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_predicates), not just the equality condition. The following are some examples of predicate clauses:

(%VALUE='Red')
(%VALUE > 21)
(%VALUE %STARTSWITH 'R')
(%VALUE \[ 'e')
(%VALUE IN ('Red','Blue')
(%VALUE IS NOT NULL)
(%KEY=3)
(%KEY > 1)
(%KEY IS NOT NULL)

For performance reasons, the predicate %STARTSWITH 'abc' is preferable to the equivalent predicate LIKE 'abc%'.

You can specify multiple predicate conditions using AND, OR, and NOT [logical operators](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_langelements#GSQL_langelements_ops_logical). Caché applies the combined predicate conditions to each element. Therefore, it is not meaningful to apply two %VALUE or two %KEY predicates using an AND test.

For example, using FOR SOME %ELEMENT to match a field containing the values Red, Green, Red Green, Black Red, Green Yellow Red, Green Black, Yellow, or Black Yellow:

*   (%VALUE='Red') matches any field containing the element Red: Red, Red Green, Black Red, and Red Yellow Green.
    
*   (%VALUE='Red' OR %VALUE='Green') matches any field containing either element (or both, in any order): Red, Green, Red Green, Black Red, Green Yellow Red, Green Black. This is functionally identical to (%VALUE IN('Red','Green')).
    
*   (%VALUE='Red' AND %VALUE='Green') matches no field values because it matches each element against both Red and Green, and no element can have the value Red and the value Green. This predicate does not match the two-element value Red Green.
    
*   (%VALUE='Red' AND %KEY=2) matches Red Green, Black Red, Green Yellow Red.
    
*   (%VALUE='Red' OR %KEY=2) matches Red, Red Green, Black Red, Green Yellow Red, Green Black, Black Yellow.
    

FOR SOME %ELEMENT is a collection predicate. It can be used in most contexts where a [predicate condition](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_predicates) can be specified, as described in the [Overview of Predicates](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_predicates) page of this manual. It is subject to the following restrictions:

*   You cannot use FOR SOME %ELEMENT in a HAVING clause.
    
*   You cannot use FOR SOME %ELEMENT as a predicate that selects fields for a JOIN operation.
    
*   You cannot associate FOR SOME %ELEMENT with another predicate condition using the OR logical operator if the two predicates reference fields in different tables. For example:
    
    WHERE FOR SOME %ELEMENT(t1.FavoriteColors) (%VALUE='purple') OR t2.Age < 65
    
    Because this restriction depends on how the optimizer uses indices, SQL may only enforce this restriction when indices are added to a table. It is strongly suggested that this type of logic be avoided in all queries.
    

Collection Index

An important use of FOR SOME %ELEMENT is to select elements using a collection index. If the appropriate KEYS or ELEMENTS index is defined for field, Caché uses this index rather than directly referencing the field value elements.

If the following collection index is defined:

 INDEX fcIDX1 ON FavoriteColors(ELEMENTS);

The following query uses this index:

SELECT Name,FavoriteColors FROM Sample.Person
WHERE FOR SOME %ELEMENT(FavoriteColors) (%VALUE\='Red')

If the following collection index is defined:

 INDEX fcIDX2 ON FavoriteColors(KEYS) \[ Type \= bitmap \];

The following query uses this index:

SELECT Name,FavoriteColors FROM Sample.Person
WHERE FOR SOME %ELEMENT(FavoriteColors) (%KEY\=2)

For further details on FOR SOME %ELEMENT with collection indices, refer to [Collection Indexing and Querying Collections through SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_queries#GSQL_queries_collindex) in the “Querying the Database” chapter of Using Caché SQL.

Examples

The following example uses FOR SOME %ELEMENT to return those rows where the FavoriteColors list contains the element 'Red':

SELECT Name,FavoriteColors
FROM Sample.Person
WHERE FOR SOME %ELEMENT(FavoriteColors) (%VALUE\='Red')

 

In the following example, the %VALUE predicate contains an IN statement specifying a comma-separated list. This example returns those rows where the FavoriteColors list contains either the element 'Red' or the element 'Blue' (or both):

SELECT Name,FavoriteColors
FROM Sample.Person
WHERE FOR SOME %ELEMENT(FavoriteColors) (%VALUE IN ('Red','Blue'))

 

The following example uses a predicate clause with two Contains operators (\[). It returns those rows where the FavoriteColors list has an element that contains a lowercase 'l' and a lowercase 'e' (the contains operator is case-sensitive). In this case, the elements 'Blue', 'Yellow', and 'Purple':

SELECT Name,FavoriteColors AS Preferences
FROM Sample.Person
WHERE FOR SOME %ELEMENT(FavoriteColors) AS fc (fc.%VALUE \[ 'l' AND fc.%VALUE \[ 'e')

 

This example also demonstrates how an element alias (e-alias) is used.

The following Dynamic SQL example uses %KEY to return rows based on the number of elements in FavoriteColors. The first %Execute() sets %KEY=1, returning all rows that have one or more FavoriteColors elements. The second %Execute() sets %KEY=2, returning all rows that have two or more FavoriteColors elements:

  ZNSPACE "SAMPLES"
  SET q1 \= "SELECT %ID,Name,FavoriteColors FROM Sample.Person "
  SET q2 \= "WHERE FOR SOME %ELEMENT(FavoriteColors) (%KEY=?)"
  SET myquery \= q1\_q2
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET tStatement.%SelectMode\=1
  SET qStatus \= tStatement.%Prepare(myquery)
   IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
  SET rset \= tStatement.%Execute(1)
  DO rset.%Display()
  WRITE !,"End of data %KEY 1",!!
  SET rset \= tStatement.%Execute(2)
  DO rset.%Display()
  WRITE !,"End of data %KEY 2"

 

See Also

*   [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select) statement, [WHERE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_where) clause
    
*   [Overview of Predicates](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_predicates)
    
*   [FOR SOME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_forsome) predicate
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_forsome "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_in "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_forsomeelement.xml**