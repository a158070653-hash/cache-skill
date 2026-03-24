# TO NUMBER

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_tonumber

---

 

Caché SQL Reference

TO\_NUMBER

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_todate "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_totimestamp "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [TO\_NUMBER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_tonumber "TO_NUMBER") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A string function that converts a given string expression to a value of NUMBER data type.

Synopsis

TO\_NUMBER(string-expression)

TONUMBER(string-expression)

Arguments

string-expression

The string expression to be converted. The expression can be the name of a column, a string literal, or the result of another function, where the underlying data type is of type CHAR or VARCHAR2.

Description

The names TO\_NUMBER and TONUMBER are interchangeable. They are supported for Oracle compatibility.

TO\_NUMBER converts a string to a number. TO\_NUMBER conversion takes a numeric string and converts it to a canonical number by removing leading and trailing zeros and a trailing decimal separator character, resolving plus and minus signs, and expanding exponential notation ("E" or "e").

TO\_NUMBER halts conversion when it encounters a nonnumeric character (such as a letter or a numeric group separator). Thus the string '7dwarves' converts to 7. TO\_NUMBER does not resolve arithmetic operations. Thus the string '2+4' converts to 2. If the first character of string-expression is a nonnumeric string, TO\_NUMBER returns 0. If string-expression is an empty string (''), TO\_NUMBER returns 0. If NULL is specified for string-expression, TO\_NUMBER returns null.

Related SQL Functions

*   TO\_NUMBER converts a string to a number.
    
*   [TO\_CHAR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_tochar) performs the reverse operation; it converts a number to a string.
    
*   [TO\_DATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_todate) converts a formatted date string to a date integer.
    
*   [TO\_TIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_totimestamp) converts a formatted date and time string to a standard timestamp.
    
*   [CAST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_cast) and [CONVERT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_convert) perform NUMBER data type conversions.
    

Examples

Note:

The following examples use a period (.) as the decimal separator character and a comma (,) as the numeric group separator character.

The following two examples show how TO\_NUMBER converts a string to a number: removing leading and trailing zeros, resolving multiple signs and removing plus signs, and truncating the number when it encounters a nonnumeric character:

SELECT TO\_NUMBER('+-+-01000.00+') AS Num

 

returns: 1000

However, because a comma is not considered a numeric character:

SELECT TO\_NUMBER('+-+-01,000.00+') AS Num

 

returns: 1

The following example resolves exponential notation:

SELECT TO\_NUMBER('+7E-2') AS Num

 

returns: .07

The following example shows how to use TO\_NUMBER to list street addresses ordered in ascending numerical order:

SELECT Home\_Street,Name
FROM Sample.Person
ORDER BY TO\_NUMBER(Home\_Street)

 

Compare the results with the same data ordered in ascending string order:

SELECT Home\_Street,Name
FROM Sample.Person
ORDER BY Home\_Street

 

See Also

*   [TO\_CHAR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_tochar)
    
*   [TO\_DATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_todate)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_todate "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_totimestamp "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:38**

Source: **RSQL\_tonumber.xml**