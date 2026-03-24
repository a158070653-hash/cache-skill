# CHARINDEX

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_charindex

---

 

Caché SQL Reference

CHARINDEX

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_characterlength "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_charlength "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [CHARINDEX](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_charindex "CHARINDEX") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A string function that returns the position of a substring within a string, with optional search start point.

Synopsis

CHARINDEX(substring,string\[,start\])

Arguments

substring

A substring to match within string.

string

A string expression that is the target for the substring search.

start

Optional — The starting point for substring search, specified as a positive integer. A character count from the beginning of string, counting from 1. To search from the beginning of string, omit this argument or specify a start of 0 or 1. A negative number, the empty string, NULL, or a nonnumeric value is treated as 0.

Description

CHARINDEX searches a string for a substring. If a match is found, it returns the starting position of the first matching substring, counting from 1. If the substring cannot be found, CHARINDEX returns 0.

The empty string is a string value. You can, therefore, use the empty string for either string argument value. The start argument treats an empty string value as 0. However, note that the Caché ObjectScript empty string is passed to Caché SQL as NULL.

NULL is not a string value in Caché SQL. For this reason, specifying NULL for either CHARINDEX string argument returns NULL.

CHARINDEX is case-sensitive. Use one of the case-conversion functions to locate both uppercase and lowercase instances of a letter or character string.

This function provides compatibility with Transact-SQL implementations.

CHARINDEX, POSITION, $FIND, and INSTR

CHARINDEX, POSITION, $FIND, and INSTR all search a string for a specified substring and return an integer position corresponding to the first match. CHARINDEX, POSITION, and INSTR return the integer position of the first character of the matching substring. $FIND returns the integer position of the first character after the end of the matching substring. CHARINDEX, $FIND, and INSTR support specifying a starting point for substring search. INSTR also support specifying the substring occurrence from that starting point.

The following example demonstrates these four functions, specifying all optional arguments. Note that the positions of string and substring differ in these functions:

SELECT POSITION('br' IN 'The broken brown briefcase') AS Position,
       CHARINDEX('br','The broken brown briefcase',6) AS Charindex,
       $FIND('The broken brown briefcase','br',6) AS Find,
       INSTR('The broken brown briefcase','br',6,2) AS Inst

 

For a list of functions that search for a substring, refer to [String Manipulation](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_stringmanipulation).

Examples

The following example searches for the substring KONG. It returns 6, the character position of this substring within the string:

SELECT CHARINDEX('KONG','KING KONG')

 

The following example searches for all Name field values that contain the substring 'Fred':

SELECT Name
FROM Sample.Person
WHERE CHARINDEX('Fred',Name)\>0

 

The following example matches a substring after the first 10 characters:

SELECT CHARINDEX('Re','Reduce, Reuse, Recycle',10)

 

it returns 16.

The following example specifies a start location beyond the length of the string:

SELECT CHARINDEX('Re','Reduce, Reuse, Recycle',99)

 

it returns 0.

The following example shows that CHARINDEX handles the empty string ('') just like any other string value:

SELECT CHARINDEX('','King Kong'),
       CHARINDEX('K',''),
       CHARINDEX('','')

 

In the above example, the first and second CHARINDEX functions return 0 (no match). The third returns 1, because the empty string matches the empty string at position 1.

The following example shows that CHARINDEX does not treat NULL as a string value. Specifying NULL for either string always returns NULL:

SELECT CHARINDEX(NULL,'King Kong'),
       CHARINDEX('K',NULL),
       CHARINDEX(NULL,NULL)

 

See Also

*   [$FIND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_find) function
    
*   [INSTR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_instr) function
    
*   [POSITION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_position) function
    
*   [String Manipulation](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_stringmanipulation)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_characterlength "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_charlength "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_charindex.xml**