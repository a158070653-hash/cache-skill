# RTRIM

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_rtrim

---

 

Caché SQL Reference

RTRIM

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_rpad "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_searchindex "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [RTRIM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_rtrim "RTRIM") \]

Go to: Description Example See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A string function that returns a string with the trailing blanks removed.

Synopsis

RTRIM(string-expression)

{fn RTRIM(string-expression)}

Arguments

string-expression

A string expression, which can be the name of a column, a string literal, or the result of another scalar function, where the underlying data type can be represented as any character type (such as CHAR or VARCHAR).

Description

RTRIM removes the trailing blanks from a string expression, and returns the string as type VARCHAR. If string-expression is NULL, RTRIM returns NULL. If string-expression is a string consisting entirely of blank spaces, RTRIM returns the empty string ('').

RTRIM leave leading blanks; to remove leading blanks, use LTRIM. To remove leading and/or trailing characters of any type, use TRIM. To pad a string with trailing blanks or other characters, use RPAD. To create a string of blanks, use SPACE.

Note that RTRIM can be used as an ODBC scalar function (with the curly brace syntax) or as an SQL general function.

Example

The following Embedded SQL example removes the five trailing blanks from the string. It leaves the five leading blanks:

   SET a\="     Test string with 5 leading and 5 trailing spaces.     "
   &sql(SELECT {fn RTRIM(:a)} INTO :b)
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"Before RTRIM",!,"start:",a,":end"
     WRITE !,"After RTRIM",!,"start:",b,":end" }

 

Returns:

Before RTRIM
start:     Test string with 5 leading and 5 trailing spaces.     :end
After RTRIM
start:     Test string with 5 leading and 5 trailing spaces.:end

See Also

[LTRIM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_ltrim) [TRIM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_trim) [RPAD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_rpad) [SPACE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_space)

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_rpad "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_searchindex "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_rtrim.xml**