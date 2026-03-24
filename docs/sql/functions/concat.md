# CONCAT

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_concat

---

 

Caché SQL Reference

CONCAT

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_coalesce "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_convert "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [CONCAT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_concat "CONCAT") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A scalar string function that returns a character string as a result of concatenating two character expressions.

Synopsis

{fn CONCAT(string-expression1,string-expression2)}

Arguments

string-expression1, string-expression2

The string expressions to be concatenated. The expressions can be the name of a column, a string literal, a numeric, or the result of another scalar function, where the underlying data type can be represented as any character type (such as CHAR or VARCHAR).

Description

CONCAT concatenates two strings to return a concatenated string. You can perform exactly the same operation using the concatenate operator (||).

You can concatenate any combination of numerics or numeric strings; the concatenation result is a numeric string. Caché SQL converts numerics to [canonical form](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_numcanonical) (exponents are expanded and leading and trailing zeros are removed) before concatenation. Numeric strings are not converted to canonical form before concatenation.

You can concatenate leading or trailing blanks to a string. Concatenating a NULL value to a string results in a NULL; this is the industry-wide SQL standard.

The STRING function can also be used to concatenate two or more expressions into a single string.

Examples

The following example concatenates the Home\_State and Home\_City columns to create a location value. The concatenation is shown twice, using the CONCAT function and the concatenate operator:

SELECT {fn CONCAT(Home\_State,Home\_City)} AS LocationFunc,
Home\_State||Home\_City AS LocationOp
FROM Sample.Person

 

The following example shows what happens when you attempt to concatenate a string and a NULL:

SELECT {fn CONCAT(Home\_State,NULL)} AS StrNull
FROM Sample.Person

 

The following example shows that numbers are converted to canonical form before concatenation. To avoid this, you can specify the number as a string, as shown:

SELECT {fn CONCAT(Home\_State,0012.00E2)} AS StrNum,
{fn CONCAT(Home\_State,'0012.00E2')} AS StrStrNum
FROM Sample.Person

 

The following example shows that trailing blank spaces are retained:

SELECT CHAR\_LENGTH({fn CONCAT(Home\_State,'          ')}) AS StrSpace
FROM Sample.Person

 

See Also

[ASCII](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_ascii) [CHAR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_char) [STRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_string) [SUBSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_substring)

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_coalesce "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_convert "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_concat.xml**