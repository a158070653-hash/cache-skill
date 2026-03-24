# TRUNCATE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_truncate

---

 

Caché SQL Reference

TRUNCATE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_trim "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_truncate_pct "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [TRUNCATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_truncate "TRUNCATE") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A scalar numeric function that truncates a number at a specified number of digits.

Synopsis

{fn TRUNCATE(numeric-expr,scale)}

Arguments

numeric-expr

The number to be truncated. A number or numeric expression.

scale

An expression that evaluates to an integer that specifies the number of places to truncate, counting from the decimal point. Can be zero, a positive integer, or a negative integer. If scale is a fractional number, Caché rounds it to the nearest integer.

Description

TRUNCATE truncates numeric-expr by truncating at the scale number of digits from the decimal point. It does not round numbers or add padding zeroes. Leading and trailing zeroes are removed before the TRUNCATE operation. TRUNCATE returns the same data type as numeric-expr.

*   If scale is a positive number, truncation is performed at that number of digits to the right of the decimal point. If scale is equal to or larger than the number of decimal digits, no truncation or zero filling occurs.
    
*   If scale is zero, the number is truncated to a whole integer. In other words, truncation is performed at zero digits to the right of the decimal point; all decimal digits and the decimal point itself are truncated.
    
*   If scale is a negative number, truncation is performed at that number of digits to the left of the decimal point. If scale is equal to or larger than the number of integer digits in the number, zero is returned.
    
*   If numeric-expr is zero (however expressed: 00.00, -0, etc.) TRUNCATE returns 0 (zero) with no decimal digits, regardless of the scale value.
    
*   If numeric-expr or scale is NULL, TRUNCATE returns NULL.
    

TRUNCATE and ROUND are numeric functions that perform similar operations; they both can be used to decrease the number of significant decimal or integer digits of a number. However, TRUNCATE does not perform rounding. TRIM can be used to perform a similar truncation operation on strings.

TRUNCATE can only be used as an ODBC scalar function (with the curly brace syntax).

Examples

The following two examples both truncate a number to two decimal digits. The first (using Dynamic SQL) specifies scale as an integer; the second (using Embedded SQL) specifies scale as a host variable that resolves to an integer:

  SET myquery \= "SELECT {fn TRUNCATE(654.321888,2)} AS trunc"
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET qStatus \= tStatement.%Prepare(myquery)
  SET rset \= tStatement.%Execute()
  DO rset.%Display()

 

  SET x\=2
  &sql(SELECT {fn TRUNCATE(654.321888,:x)}
  INTO :a)
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"truncated value is: ",a }

 

both examples return 654.32 (truncation to two decimal places).

The following Dynamic SQL example specifies a scale larger than the number of decimal digits:

  SET myquery \= "SELECT {fn TRUNCATE(654.321000,9)} AS trunc"
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET qStatus \= tStatement.%Prepare(myquery)
  SET rset \= tStatement.%Execute()
  DO rset.%Display()

 

it returns 654.321 (Caché removed the trailing zeroes before the truncation operation; no truncation or zero padding occurred).

The following Dynamic SQL example specifies a scale of zero:

  SET myquery \= "SELECT {fn TRUNCATE(654.321888,0)} AS trunc"
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET qStatus \= tStatement.%Prepare(myquery)
  SET rset \= tStatement.%Execute()
  DO rset.%Display()

 

it returns 654 (all decimal digits and the decimal point are truncated).

The following Dynamic SQL example specifies a negative scale:

  SET myquery \= "SELECT {fn TRUNCATE(654.321888,-2)} AS trunc"
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET qStatus \= tStatement.%Prepare(myquery)
  SET rset \= tStatement.%Execute()
  DO rset.%Display()

 

it returns 600 (two integer digits have been truncated and replaced by zeroes; note that no rounding has been done).

The following Dynamic SQL example specifies a negative scale as large as the integer portion of the number:

  SET myquery \= "SELECT {fn TRUNCATE(654.321888,-3)} AS trunc"
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET qStatus \= tStatement.%Prepare(myquery)
  SET rset \= tStatement.%Execute()
  DO rset.%Display()

 

it returns 0.

See Also

*   SQL functions: [ROUND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_round) [RTRIM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_rtrim) [TRIM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_trim)
    
*   Caché ObjectScript function: [$NORMALIZE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fnormalize)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_trim "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_truncate_pct "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:38**

Source: **RSQL\_truncate.xml**