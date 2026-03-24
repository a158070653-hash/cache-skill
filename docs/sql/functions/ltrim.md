# LTRIM

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_ltrim

---

 

Caché SQL Reference

LTRIM

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_lpad "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_minus_pct "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [LTRIM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_ltrim "LTRIM") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A string function that returns a string with the leading blanks removed.

Synopsis

LTRIM(string-expression)

{fn LTRIM(string-expression)}

Arguments

string-expression

A string expression, which can be the name of a column, a string literal, or the result of another scalar function, where the underlying data type can be represented as any character type (such as CHAR or VARCHAR).

Description

LTRIM removes the leading blanks from a string expression, and returns the string as type VARCHAR. If string-expression is NULL, LTRIM returns NULL. If string-expression is a string consisting entirely of blank spaces, LTRIM returns the empty string ('').

LTRIM leave trailing blanks; to remove trailing blanks, use RTRIM. To remove leading and/or trailing characters of any type, use TRIM. To pad a string with leading blanks or other characters, use LPAD. To create a string of blanks, use SPACE.

Note that LTRIM can be used as an ODBC scalar function (with the curly brace syntax) or as an SQL general function.

Examples

The following Embedded SQL example removes the five leading blanks from the string. It leaves the five trailing blanks:

   SET a\="     Test string with 5 leading and 5 trailing spaces.     "
   &sql(SELECT {fn LTRIM(:a)} INTO :b)
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"Before LTRIM",!,"start:",a,":end"
     WRITE !,"After LTRIM",!,"start:",b,":end" }

 

Returns:

Before LTRIM
start:     Test string with 5 leading and 5 trailing spaces.     :end
After LTRIM
start:Test string with 5 leading and 5 trailing spaces.     :end

See Also

[RTRIM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_rtrim) [TRIM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_trim) [LPAD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_lpad) [SPACE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_space)

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_lpad "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_minus_pct "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_ltrim.xml**