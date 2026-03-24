# %FIND

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_find

---

 

Caché SQL Reference

%FIND

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_exists "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_forsome "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Predicate Conditions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_PREDICATE_CONDITONS "SQL Predicate Conditions") \]  >  \[ [%FIND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_find "%FIND") \]

Go to: Description See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Matches a value to a set of generated values with bitmap chunks iteration.

Synopsis

scalar-expression %FIND valueset \[SIZE ((nn))\]

Arguments

scalar-expression

A scalar expression (most commonly the RowId field of a table) whose values are being compared with valueset.

valueset

An object reference (oref) to a user-defined object that implements bitmap chunks iteration methods and the ContainsItem() method. This method takes a set of data values and returns a boolean when there is a match with a value in scalar-expression.

SIZE ((nn))

Optional — An order-of-magnitude integer (10, 100, 1000, etc.) used for query optimization.

Description

The %FIND predicate allows you to filter a result set by selecting those data values that match the values specified in valueset, iterating through values in a sequence of bitmap chunks. This match is successful when a scalar-expression value matches a value in valueset. If the valueset values do not match any of the scalar expression values, %FIND returns the null string. This match is always performed on the logical (internal storage) data value, regardless of the display mode.

%FIND, like the other comparison conditions, is used in the WHERE clause or the HAVING clause of a SELECT statement.

%FIND enables filtering of field values using an abstract, programmatically specified set of matching values. Specifically, it enables filtering of RowId field values using an abstract, programmatically specified bitmap, where valueset behaves similar to the subscript layer of a bitmap index.

The user-defined class is derived from the abstract class [%SQL.AbstractFind](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SQL.AbstractFind). this abstract class defines the [ContainsItem()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SQL.AbstractFind#ContainsItem) boolean method. The ContainsItem() method matches the scalar-expression values to the valueset values.

Iteration through values in a sequence of bitmap chunks is performed using the following three methods:

*   [GetChunk(c)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SQL.AbstractFind#GetChunk), which returns the bitmap chunk with chunk number c.
    
*   [NextChunk(.c)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SQL.AbstractFind#NextChunk), which returns the first bitmap chunk with chunk number > c.
    
*   [PreviousChunk(.c)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SQL.AbstractFind#PreviousChunk), which returns the first bitmap chunk with chunk number < c.
    

Refer to [%SQL.AbstractFind](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SQL.AbstractFind) for further details concerning these four methods.

Collation Types

%FIND uses the same collation type as the column it is matched against. By default, string data types collate as not case-sensitive. If you assign a different collation type to the column, you must also apply this collation type to the %FIND substring. Refer to [%SQLUPPER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_sqlupper) for further information on case transformation functions.

SIZE Clause

The optional %FIND SIZE clause provides the integer nn, which specifies an order-of-magnitude estimate of the number of values in valueset. Caché uses this order-of-magnitude estimate to determine the optimal query plan. Specify nn as one of the following literals: 10, 100, 1000, 10000, etc. Because nn must be available as a constant value at compile time, it must be specified as a literal in all SQL code. Note that nesting parentheses must be specified as shown for all SQL, with the exception of Embedded SQL.

%FIND and %INSET Compared

*   %INSET is the simplest and most general interface. It supports the ContainsItem() method.
    
*   %FIND supports iteration over bitmap chunks using a [bitmap index](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_indices#GSQLOPT_indices_bitmap). It emulates the functionality of the Caché ObjectScript [$ORDER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_forder) function, supporting NextChunk(), PreviousChunk(), and GetChunk() iteration methods, as well as the ContainsItem() method.
    

See Also

*   [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select) statement [HAVING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_having) clause [WHERE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_where) clause
    
*   [%INSET](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_inset) predicate
    
*   [Overview of Predicates](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_predicates)
    
*   [SEARCH\_INDEX](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_searchindex) function
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_exists "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_forsome "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_find.xml**