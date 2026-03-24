# LOG

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_log

---

 

Caché SQL Reference

LOG

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listtostring "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_log10 "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [LOG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_log "LOG") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A scalar numeric function that returns the natural logarithm of a given numeric expression.

Synopsis

{fn LOG(float-expression)}

Arguments

float-expression

An expression of type FLOAT.

Description

LOG returns the natural logarithm (base e) of float-expression. LOG returns a value of data type FLOAT with a precision of 21 and a scale of 18.

LOG can only be used as an ODBC scalar function (with the curly brace syntax).

Examples

The following example returns the natural logarithm of an integer:

SELECT {fn LOG(5)} AS Logarithm

 

returns 1.60943791...

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
     WRITE !,"Error code ",SQLCODE
     QUIT }
   ELSE {
     WRITE !,"Exponential of log ",b," = ",c
     SET a\=a+1 }
   }

 

Note that the ROUND function is needed here to correct for very small discrepancies caused by system calculation limitations. In the above example, ROUND is set arbitrarily to 12 decimal digits for this purpose.

See Also

*   SQL functions: [EXP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_exp), [LOG10](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_log10), [ROUND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_round)
    
*   Caché ObjectScript function: [$ZLN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzln)
    

 

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listtostring "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_log10 "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_log.xml**