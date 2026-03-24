# INSTR

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_instr

---

 

Caché SQL Reference

INSTR

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_ifnull "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_internal "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [INSTR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_instr "INSTR") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A string function that returns the position of a substring within a string, with optional search start point and occurrence count.

Synopsis

INSTR(string,substring\[,start\[,occurrence\]\])

Arguments

string

The string expression within which to search for substring. It can be the name of a column, a string literal, or the result of another scalar function, where the underlying data type can be represented as any character type (such as CHAR or VARCHAR2).

substring

A substring that is believed to occur within string.

start

Optional — The starting point for substring search, specified as a positive integer. A character count from the beginning of string, counting from 1. To search from the beginning of string, omit this argument or specify a start of 1. A start value of 0, the empty string, NULL, or a nonnumeric value cause INSTR to return 0. Specifying start as a negative number causes INSTR to return <null>.

occurrence

Optional — A non-zero integer that specifies which occurrence of substring to return when searching from the start position. The default is to return the position of the first occurrence.

Description

INSTR searches string for substring, and returns the position of the first character of substring. The position is returned as an integer, counting from the beginning of string. If substring is not found, 0 (zero) is returned. INSTR returns NULL if passed a NULL value for either argument.

INSTR supports specifying start as the starting point for substring search. INSTR also support specifying the substring occurrence from that starting point.

INSTR is case-sensitive. Use one of the case-conversion functions to locate both uppercase and lowercase instances of a letter or character string.

This function can also be invoked from Caché ObjectScript using the [INSTR()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#INSTR) method call:

  WRITE $SYSTEM.SQL.INSTR("The broken brown briefcase","br",6,2)

 

INSTR, CHARINDEX, POSITION, and $FIND

INSTR, CHARINDEX, POSITION, and $FIND all search a string for a specified substring and return an integer position corresponding to the first match. CHARINDEX, POSITION, and INSTR return the integer position of the first character of the matching substring. $FIND returns the integer position of the first character after the end of the matching substring. CHARINDEX, $FIND, and INSTR support specifying a starting point for substring search. INSTR also support specifying the substring occurrence from that starting point.

The following example demonstrates these four functions, specifying all optional arguments. Note that the positions of string and substring differ in these functions:

SELECT POSITION('br' IN 'The broken brown briefcase') AS Position,
       CHARINDEX('br','The broken brown briefcase',6) AS Charindex,
       $FIND('The broken brown briefcase','br',6) AS Find,
       INSTR('The broken brown briefcase','br',6,2) AS Inst

 

For a list of functions that search for a substring, refer to [String Manipulation](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_stringmanipulation).

Examples

The following example returns 11, because “b” is the 11th character in the string:

SELECT INSTR('The quick brown fox','b',1) AS PosInt

 

The following example returns the length of the last name (surname) for each name in the Sample.Person table. It locates the comma used to separate the last name from the rest of the name field, then subtracts 1 from that position:

SELECT Name,
INSTR(Name,',',1)\-1 AS LNameLen
FROM Sample.Person

 

The following example returns the position of the first instance of the letter “B” in each name in the Sample.Person table. Because INSTR is case-sensitive, the %SQLUPPER function is used to convert all name values to uppercase before performing the search. Because %SQLUPPER adds a blank space at the beginning of a string, this example subtracts 1 to get the actual letter position. Searches that do not locate the specified string return zero (0); in this example, because of the subtraction of 1, the value displayed for these searches is –1:

SELECT Name,
INSTR(%SQLUPPER(Name),'B',1)\-1 AS BPos
FROM Sample.Person

 

See Also

*   [CHARINDEX](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_charindex) function
    
*   [$FIND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_find) function
    
*   [POSITION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_position) function
    
*   [String Manipulation](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_stringmanipulation)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_ifnull "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_internal "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_instr.xml**