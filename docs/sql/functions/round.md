# ROUND

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_round

---

 

Caché SQL Reference

ROUND

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_right "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_rpad "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [ROUND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_round "ROUND") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A numeric function that rounds or truncates a number at a specified number of digits.

Synopsis

ROUND(numeric-expr,scale\[,flag\])

{fn ROUND(numeric-expr,scale\[,flag\])}

Arguments

numeric-expr

The number to be rounded. A numeric expression.

scale

An expression that evaluates to an integer that specifies the number of places to round to, counting from the decimal point. Can be zero, a positive integer, or a negative integer. If scale is a fractional number, Caché rounds it to the nearest integer.

flag

Optional — A boolean flag that specifies whether to round or truncate the numeric-expr: 0=round, 1=truncate. The default is 0.

Description

This function can be used to either round or truncate a number to the specified number of decimal digits.

ROUND rounds or truncates numeric-expr to scale places, counting from the decimal point. When rounding, the number 5 is always rounded up. Leading and trailing zeroes are removed before the ROUND operation. ROUND returns the same data type as numeric-expr.

*   If scale is a positive number, rounding is performed at that number of digits to the right of the decimal point. If scale is equal to or larger than the number of decimal digits, no rounding or zero filling occurs.
    
*   If scale is zero, rounding is to the closest whole integer. In other words, rounding is performed at zero digits to the right of the decimal point; all decimal digits and the decimal point itself are removed.
    
*   If scale is a negative number, rounding is performed at that number of digits to the left of the decimal point. If scale is equal to or larger than the number of integer digits in the rounded result, zero is returned.
    
*   If numeric-expr is zero (however expressed: 00.00, -0, etc.) ROUND returns 0 (zero) with no decimal digits, regardless of the scale value.
    
*   If numeric-expr or scale is NULL, ROUND returns NULL.
    

Note that the ROUND return value is always normalized, removing trailing zeros. For example, ROUND(10.004,2) returns 10, not 10.00. When a fixed number of decimal digits is important — for example, when representing monetary amounts — the user must use the SQL scale parameter to reinstate the proper number of trailing zeros following the rounding operation.

ROUND and TRUNCATE perform similar operations; they both can be used to decrease the number of significant decimal or integer digits of a number. ROUND can be used to either round or truncate a number. TRUNCATE can only be used to truncate a number.

Examples

The following example uses a scale of 0 (zero) to round several fractions to integers. It shows that 5 is always rounded up:

SELECT ROUND(5.99,0) AS RoundUp,
       ROUND(5.5,0) AS Round5,
       {fn ROUND(5.329,0)} AS Roundoff

 

The following example truncates the same fractional numbers as the previous example:

SELECT ROUND(5.99,0,1) AS Trunc1,
       ROUND(5.5,0,1) AS Trunc2,
       {fn ROUND(5.329,0,1)} AS Trunc3

 

The following ROUND functions round and truncate a negative fractional number:

SELECT ROUND(\-0.987,2,0) AS Round1,
       ROUND(\-0.987,2,1) AS Trunc1

 

The following example rounds off pi to four decimal digits:

SELECT {fn PI()} AS ExactPi, ROUND({fn PI()},4) AS ApproxPi

 

The following example specifies a scale larger than the number of decimal digits:

SELECT {fn ROUND(654.98700,9)} AS Rounded

 

it returns 654.987 (Caché removed the trailing zeroes before the rounding operation; no rounding or zero padding occurred).

The following example rounds off the value of Salary to the nearest thousand dollars:

SELECT Salary,ROUND(Salary, \-3) AS PayBracket
FROM Sample.Employee
ORDER BY Salary

 

Note that if Salary is less than five hundred dollars, it is rounded to 0 (zero).

In the following example each ROUND specifies a negative scale as large or larger than the number to be rounded:

SELECT {fn ROUND(987,\-3)} AS Round1,
       {fn ROUND(487,\-3)} AS Round2,
       {fn ROUND(987,\-4)} AS Round3,
       {fn ROUND(987,\-5)} AS Round4

 

The first ROUND function returns 1000, because the rounded result has more digits than the scale. The other three ROUND functions return 0 (zero).

See Also

*   [$JUSTIFY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_justify) function
    
*   [TRUNCATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_truncate) function
    
*   [CEILING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_ceiling) function
    
*   [FLOOR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_floor) function
    
*   [MOD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_mod) function
    
*   Caché ObjectScript functions: [$NORMALIZE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fnormalize), [$NUMBER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fnumber)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_right "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_rpad "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_round.xml**