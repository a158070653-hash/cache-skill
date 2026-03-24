# EXP

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_exp

---

 

Caché SQL Reference

EXP

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_exact "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_external "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [EXP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_exp "EXP") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A scalar numeric function that returns the exponential (inverse of natural logarithm) of a number.

Synopsis

{fn EXP(float-expression)}

Arguments

float-expression

The logarithmic exponent, which is an expression of type FLOAT.

Description

EXP is the exponential function en, where e is the constant 2.718281828. Therefore, to return the value of e, you can specify {fn EXP(1)}. EXP is the inverse of the natural logarithm function [LOG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_log).

EXP returns a value of data type FLOAT with a precision of 36 and a scale of 18. EXP returns NULL if passed a NULL value.

EXP can only be used as an ODBC scalar function (with the curly brace syntax).

Examples

The following example returns the constant e:

SELECT {fn EXP(1)} AS e\_constant

 

returns 2.718281828...

The following Embedded SQL example returns the exponential values for the integers 0 through 10:

   SET a\=0
   WHILE a<11 {
   &sql(SELECT {fn EXP(:a)} INTO :b)
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE
     QUIT }
   ELSE {
     WRITE !,"Exponential of ",a," = ",b
     SET a\=a+1 }
   }

 

The following Embedded SQL example demonstrates that EXP is the inverse of LOG:

  SET x\=7
  &sql(SELECT {fn EXP(:x)} AS Exp,
              {fn LOG(:x)} AS Log,
              {fn EXP({fn LOG(:x)})} AS ExpOfLog
       INTO :a,:b,:c)
  IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE
     QUIT }
   ELSE {
     WRITE "Exponential of ",x," = ",a,!
     WRITE "Natural log of ",x," = ",b,!
     WRITE "Exp of Log of  ",x," = ",c
     }

 

Note in the third function call the small discrepancy between the number input and the calculated return value. The next example shows how to handle this computational discrepancy.

The following Embedded SQL example shows the relationship between the LOG and EXP functions for the integers 1 through 10:

   SET a\=1
   WHILE a<11 {
   &sql(SELECT {fn LOG(:a)} INTO :b)
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE
     QUIT }
   ELSE {
     WRITE !,"Logarithm of ",a," = ",b }
   &sql(SELECT ROUND({fn EXP(:b)},12) INTO :c)
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"Exponential of log ",b," = ",c 
   SET a\=a+1 }
   }

 

Note that the ROUND function is needed here to correct for very small discrepancies caused by system calculation limitations. In the above example, ROUND is set arbitrarily to 12 decimal digits for this purpose.

See Also

*   SQL functions: [LOG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_log) [LOG10](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_log10) [POWER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_power) [ROUND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_round)
    
*   Caché ObjectScript function: [$ZEXP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzexp)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_exact "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_external "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_exp.xml**