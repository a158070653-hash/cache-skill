# TRIM

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_trim

---

 

Caché SQL Reference

TRIM

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_translate "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_truncate "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [TRIM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_trim "TRIM") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A string function that returns a character string with leading and/or trailing characters removed.

Synopsis

TRIM(end\_keyword string-expression-1 FROM string-expression-2)

Arguments

end\_keyword

Optional — A keyword specifying the which end of string-expression-2 to strip. Available values are LEADING, TRAILING, BOTH. The default is BOTH.

string-expression-1

Optional — The string expression specifying the characters to strip from string-expression-2. Every instance of the specified character is stripped. Thus 'abc' strips 'bbbaacaaa'. If not specified, TRIM strips spaces.

string-expression-2

The string expression which will be stripped. Both string expressions can be the name of a column, a string literal, or the result of another function, where the underlying data type can be represented as any character type (such as CHAR or VARCHAR2).

Description

TRIM strips the specified characters from the beginning or end of a supplied value. By default, stripping of letters is case-sensitive.

The optional end\_keyword argument can take the following values:

LEADING

A keyword that specifies that the characters in string-expression-1 are to be removed from the beginning of string-expression-2.

TRAILING

A keyword that specifies that the characters in string-expression-1 are to be removed from the end of string-expression-2.

BOTH

A keyword that specifies that the characters in string-expression-1 are to be removed from both the beginning and end of string-expression-2. BOTH is the default and is used if no end\_keyword is specified.

You can use LTRIM to trim leading blanks, or RTRIM to trim trailing blanks.

To pad a string with leading or trailing blanks or other characters, use LPAD or RPAD.

NULL and Empty String

TRIM returns NULL if either string expression is NULL.

TRIM returns an empty string if string-expression-2 is the empty string, or if TRIM strips all of the characters from string-expression-2.

Examples

The following example uses the end\_keyword and string-expression-1 defaults; it removes leading and trailing blanks from "abc":

SELECT TRIM('   abc   ') AS Trimmed

 

The following example removes the character "x" from the beginning of the string "xxxabcxxx", resulting in "abcxxx":

SELECT TRIM(LEADING 'x' FROM 'xxxabcxxx') AS Trimmed

 

The following example removes the character "x" from the beginning and end of "xxxabcxxx", resulting in "abc":

SELECT TRIM(BOTH 'x' FROM 'xxxabcxxx') AS Trimmed

 

The following example removes all instances of the characters "xyz" from the end of "abcxxyz", resulting in "abc":

SELECT TRIM(TRAILING 'xyz' FROM 'abcxzzxyyyyz') AS Trimmed

 

The following example removes the leading letters "B" or "R" from the FavoriteColors values. Note that you must convert a list to a string in order to apply TRIM:

SELECT TOP 15 Name,FavoriteColors,
       TRIM(LEADING 'BR' FROM $LISTTOSTRING(FavoriteColors)) AS Trimmed
       FROM Sample.Person

 

See Also

*   SQL functions: [LTRIM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_ltrim) [RTRIM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_rtrim) [LPAD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_lpad) [RPAD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_rpad)
    
*   Caché ObjectScript function: [$ZSTRIP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzstrip)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_translate "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_truncate "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:38**

Source: **RSQL\_trim.xml**