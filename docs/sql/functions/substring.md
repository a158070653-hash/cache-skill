# SUBSTRING

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_substring

---

 

Caché SQL Reference

SUBSTRING

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_substr "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_sysdate "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [SUBSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_substring "SUBSTRING") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A string function that returns a substring from a larger character string.

Synopsis

SUBSTRING(string-expression,start,length)

SUBSTRING(string-expression FROM start FOR length)

{fn SUBSTRING(string-expression,start,length)}

Arguments

string-expression

The string expression from which the substring is to be derived. An expression, which can be the name of a column, a string literal, or the result of another scalar function. The underlying data type can be a character type (such as CHAR or VARCHAR), a numeric, or a data stream.

start

An integer that specifies the position in string-expression to begin the substring. The first character in string-expression is at position 1. If the start position is higher than the length of the string, SUBSTRING returns an empty string (''). If the start position is lower than 1 (zero, or a negative number) the substring begins at position 1, but the length of the substring is reduced by the start position.

length

Optional — An integer that specifies the length of the substring to return. If length is not specified, the default is to return the rest of the string.

Description

The value of start controls the starting point of the substring:

*   If start is less than 1, the value of length is decremented by a corresponding amount. Thus, if start is 0, the value of length is diminished by 1; if start is –1, the value of length is diminished by 2.
    

The value of length controls the size of the substring:

*   If length is a positive value (1 or greater), the substring ends length number of characters to the right of the starting position. (This effective length may be diminished if the start number is less than 1.)
    
*   If length is larger than the number of character remaining in the string, all characters to the right of the starting position through the end of string-expression are returned.
    
*   If length is zero, NULL is returned.
    
*   If length is a negative number, Caché issues an SQLCODE –140 error.
    

Floating-point numbers passed as arguments to SUBSTRING are converted to integers by truncating the fractional portion.

SUBSTRING extracts a substring from the beginning of a string. SUBSTR can extract a substring from either the beginning or the end of a string. Note that these two SQL functions handle argument values differently. SUBSTRING can be used with [character stream data](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_blobs); SUBSTR cannot be used with stream data.

SUBSTRING can be used as an ODBC scalar function (with the curly brace syntax) or as an SQL general function.

Return Value

If any SUBSTRING argument value is NULL, SUBSTRING returns NULL.

If string-expression is any %String data type, the SUBSTRING return value is the same data type as the string-expression data type. This allows SUBSTRING to handle user-defined string data types with special encoding.

If string-expression is not a %String data type (for example, %Float), the SUBSTRING return value is %String.

Examples

This example returns the string “forward”:

SELECT {fn SUBSTRING( 'forward pass',1,7 )} AS SubText

 

This example returns the string “pass”:

SELECT {fn SUBSTRING( 'forward pass',9,4 )} AS SubText

 

The following example returns the first four characters of each name:

SELECT Name,SUBSTRING(Name,1,4) AS FirstFour
FROM Sample.Person

 

The following example demonstrates another syntactical form of SUBSTRING. This example is functionally the same as the previous example:

SELECT Name,SUBSTRING(Name FROM 1 FOR 4) AS FirstFour
FROM Sample.Person

 

The following example shows how the length is reduced by a start value of less than 1. (A start value of 0 reduces length by 1, a start value of -1 reduces length by 2, and so forth.) In this case, length is reduced by 3, so only one character (“A”) is returned:

SELECT {fn SUBSTRING( 'ABCDEFG',\-2,4 )} AS SubText

 

See Also

*   SQL function: [SUBSTR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_substr)
    
*   Caché ObjectScript functions: [$EXTRACT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fextract) [$PIECE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fpiece)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_substr "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_sysdate "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:38**

Source: **RSQL\_substring.xml**