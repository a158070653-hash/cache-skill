# SUBSTR

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_substr

---

 

Caché SQL Reference

SUBSTR

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_stuff "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_substring "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [SUBSTR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_substr "SUBSTR") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A string function that returns a substring that is derived from a specified string expression.

Synopsis

SUBSTR(string-expression,start\[,length\])

Arguments

string-expression

The string expression from which the substring is to be derived. The expression can be the name of a column, a string literal, or the result of another scalar function, where the underlying data type can be represented as any character type (such as CHAR or VARCHAR).

start

An integer that specifies where in string-expression the substring will begin. A positive starting position specifies the number of characters from the beginning of the string. The first character in string-expression1 is at position 1. A negative starting position specifies the number of characters from the end of the string. If start is 0 (zero), it is treated as 1.

length

Optional — A positive integer that specifies the length of the substring to return. This value specifies that the substring ends length characters to the right of the starting position. If omitted, substring goes from start to the end of string-expression. If length is 0 or a negative number, Caché returns NULL.

Description

Because start can be negative, you can obtain a substring from either the beginning or end of the original string.

Floating-point numbers passed as arguments to SUBSTR are converted to integers by truncating the fractional portion.

*   If start is 0, –0, or 1, the returned substring begins with the first character of the string.
    
*   If start is a negative number the returned substring begins that number of characters from the end of the string, with -1 representing the last character of the string. If the negative number is so large that its value counted backwards from the end of the string would position before the beginning of the string, the returned substring begins with the first character of the string.
    
*   If start is past the end of the string, NULL is returned.
    
*   If length larger than the remaining characters in the string, the substring from start to the end of the string is returned.
    
*   If length is less that 1, NULL is returned.
    
*   If either start or length is NULL, NULL is returned.
    

SUBSTR cannot be used with stream data. If string-expression is a stream field, SUBSTR generates an SQLCODE -37. Use SUBSTRING to extract a substring from stream data.

SUBSTR is supported for Oracle compatibility.

Examples

The following example returns the substring CDEFG because it specifies that the substring begin at the third character (C) and continue to the end of the string:

SELECT SUBSTR('ABCDEFG',3) AS Sub

 

The following example returns the substring CDEF because it specifies that the substring begin at the third character (C) and continue for four characters (until F):

SELECT SUBSTR('ABCDEFG',3,4) AS Sub

 

The following example returns the substring CDEF because it specifies that Caché should first count five characters backwards from the end of the original string, and then return the next four characters:

SELECT SUBSTR('ABCDEFG',\-5,4) AS Sub

 

See Also

*   SQL function: [SUBSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_substring)
    
*   Caché ObjectScript functions: [$EXTRACT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fextract) [$PIECE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fpiece)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_stuff "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_substring "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:38**

Source: **RSQL\_substr.xml**