# $FIND

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_d_find

---

 

Caché SQL Reference

$FIND

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_extract "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_floor "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [$FIND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_find "$FIND") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A string function that returns the end position of a substring within a string, with optional search start point.

Synopsis

$FIND(string,substring\[,start\])

Arguments

string

The target string that is to be searched. It can be a variable name, a numeric value, a string literal, or any valid expression.

substring

The substring that is to be searched for. It can be a variable name, a numeric value, a string literal, or any valid expression.

start

Optional — The starting point for substring search, specified as a positive integer. A character count from the beginning of string, counting from 1. To search from the beginning of string, omit this argument or specify a start of 0 or 1. A negative number, the empty string, or a nonnumeric value is treated as 0. Specifying start as NULL causes $FIND to return <null>.

Description

$FIND returns an integer specifying the end position of a substring within a string. $FIND searches string for substring. If substring is found, $FIND returns the integer position of the first character following substring. If substring is not found, $FIND returns a value of 0.

You can include the start option to specify a starting position for the search. If start is greater than the number of characters in string, $FIND returns a value of 0. If start is omitted, string position 1 is the default. If start is zero, a negative number, or a nonnumeric string, position 1 is the default.

$FIND is case-sensitive. Use one of the case-conversion functions to locate both uppercase and lowercase instances of a letter or character string.

This function returns data of type SMALLINT.

$FIND, POSITION, CHARINDEX, and INSTR

$FIND, POSITION, CHARINDEX, and INSTR all search a string for a specified substring and return an integer position corresponding to the first match. $FIND returns the integer position of the first character after the end of the matching substring. CHARINDEX, POSITION, and INSTR return the integer position of the first character of the matching substring. CHARINDEX, $FIND, and INSTR support specifying a starting point for substring search. INSTR also support specifying the substring occurrence from that starting point.

The following example demonstrates these four functions, specifying all optional arguments. Note that the positions of string and substring differ in these functions:

SELECT POSITION('br' IN 'The broken brown briefcase') AS Position,
       CHARINDEX('br','The broken brown briefcase',6) AS Charindex,
       $FIND('The broken brown briefcase','br',6) AS Find,
       INSTR('The broken brown briefcase','br',6,2) AS Inst

 

For a list of functions that search for a substring, refer to [String Manipulation](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_stringmanipulation).

Examples

In the following example, string contains the string “ABCDEFG” and substring contains the string “BCD”. The $FIND function returns the value 5, indicating the position of the character (“E”) that follows “BCD”:

SELECT $FIND('ABCDEG','BCD') AS SubPoint

 

The following example searches the numeric 987654321 for the number 7. It returns 4, the position following the substring:

SELECT $FIND(987654321,7) AS SubPoint

 

The following example returns 3, the position of the character that follow the first instance of the substring “AA”:

SELECT $FIND('AAAAAA','AA') AS SubPoint

 

In the following example, $FIND searches for a substring that is not in the string. It returns zero (0):

SELECT $FIND('AABBCCDD','AC') AS SubPoint

 

In the following example, $FIND begins its search with the seventh character. This example returns 14, the position of the character that follows the next occurrence of “R”:

SELECT $FIND('EVERGREEN FOREST','R',7) AS SubPoint

 

In the following example, $FIND begins its search after the last character in string. It returns zero (0):

SELECT $FIND('ABCDEFG','G',10) AS SubPoint

 

The following Embedded SQL example shows that a start less than 1 is treated as 1:

   SET a\="ABCDEFG"
   SET b\="F"
   &sql(SELECT 
    $FIND(:a,:b),
    $FIND(:a,:b,1),
    $FIND(:a,:b,0),
    $FIND(:a,:b,\-35)
   INTO :a1,:a2,:a3,:a4)
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"The input string: ",a
     WRITE !,"Two-arg: ",a1
     WRITE !,"3rd arg 1: ",a2
     WRITE !,"3rd arg 0: ",a3
     WRITE !,"3rd arg negative: ",a4 }

 

The following Embedded SQL example uses $FIND to search a string containing the Unicode character for pi, $CHAR(960). The first $FIND returns 5, the character following pi. The second $FIND also returns 5; it begins its search at character 4, which happens to be pi, the character sought. The third $FIND begins its search at character 5; it returns 13, the position following the next occurrence of pi. Note that position 13 is returned, even though position 12 is the last character in the string:

  IF $SYSTEM.Version.IsUnicode() {
   SET a\="QT "\_$CHAR(960)\_" HONEY "\_$CHAR(960)
   SET b\=$CHAR(960)
   &sql(SELECT 
    $FIND(:a,:b),
    $FIND(:a,:b,4),
    $FIND(:a,:b,5)
   INTO :a1,:a2,:a3)
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"The input string: ",a
     WRITE !,"From beginning: ",a1
     WRITE !,"From position 4: ",a2
     WRITE !,"From position 5: ",a3 }
  }
  ELSE {WRITE "This example requires a Unicode installation of Caché"}

 

See Also

*   [CHARINDEX](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_charindex) function
    
*   [INSTR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_instr) function
    
*   [POSITION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_position) function
    
*   [String Manipulation](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_stringmanipulation)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_extract "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_floor "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_d\_find.xml**