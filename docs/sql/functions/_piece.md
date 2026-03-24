# $PIECE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_d_piece

---

 

Caché SQL Reference

$PIECE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_pi "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_plus_pct "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [$PIECE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_piece "$PIECE") \]

Go to: Description Arguments Examples Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A string function that returns a substring identified by a delimiter.

Synopsis

$PIECE(plist,delimiter\[,from\[,to\]\])

Arguments

plist

The target string from which a substring is to be returned.

delimiter

A delimiter used to identify substrings.

from

Optional — An integer that specifies the substring, or beginning of a range of substrings, to return from the target string. Substrings are separated by a delimiter, and counted from 1. If omitted, the first substring is returned.

to

Optional — An integer that specifies the ending substring for a range of substrings to return from the target string. Must be used with from.

Description

$PIECE returns the specified substring (piece) from plist. The substring returned depends on the arguments used:

*   $PIECE(plist,delimiter) returns the first substring in plist. If delimiter occurs in plist, this is the substring that precedes the first occurrence of delimiter. If delimiter does not occur in plist, the returned substring is plist.
    
*   $PIECE(plist,delimiter,from) returns the substring which is the nth piece of plist, where the integer n is specified by the from argument, and pieces are separated by a delimiter. The delimiter is not returned.
    
*   $PIECE(plist,delimiter,from,to) returns a range of substrings including the substring specified in from through the substring specified in to. This four-argument form of $PIECE returns a string that includes any intermediate occurrences of delimiter that occur between the from and to substrings. If to is greater than the number of substrings, the returned substring includes all substrings to the end of the plist string.
    

Arguments

plist

The target string from which the substring is to be returned. It can be a string literal, a variable name, or any valid expression that evaluates to a string.

A target string usually contains instances of a character (or character string) which are used as delimiters. This character or string cannot also be used as a data value within plist.

If you specify the null string (NULL) as the target string, $PIECE returns <null>, the null string.

delimiter

The search string to be used to delimit substrings within plist. It can be a numeric or string literal (enclosed in quotation marks), the name of a variable, or an expression that evaluates to a string.

Commonly, a delimiter is a designated character which is never used within string data, but is set aside solely for use as a delimiter separating substrings. A delimiter can also be a multi-character search string, the individual characters of which can be used within string data.

If you specify the null string (NULL) as the delimiter, $PIECE returns <null>, the null string.

from

The number of a substring within plist, counting from 1. It must be a positive integer, the name of an integer variable, or an expression that evaluates to a positive integer. Substrings are separated by delimiters.

*   If the from argument is omitted or set to 1, $PIECE returns the first substring of plist. If plist does not contain the specified delimiter, a from value of 1 returns plist.
    
*   If the from argument identifies by count the last substring in plist, this substring is returned, regardless of whether it is followed by a delimiter.
    
*   If the value of from is NULL, the empty string, zero, or a negative number, and no to argument is specified, $PIECE returns a null string. However, if a to argument is specified, $PIECE treats these from values the same as from\=1.
    
*   If the value of from is greater than the number of substrings in plist, $PIECE returns a null string.
    

If the from argument is used with the to argument, it identifies the start of a range of substrings to be returned as a string, and should be less than the value of to.

to

The number of the substring within plist that ends the range initiated by the from argument. The returned string includes both the from and to substrings, as well as any intermediate substrings and the delimiters separating them. The to argument must be a positive integer, the name of an integer variable, or an expression that evaluates to a positive integer. The to argument must be used with from and should be greater than the value of from.

*   If from is less than to, $PIECE returns a string consisting of all of the delimited substrings within this range, including the from and to substrings. This returned string contains the substrings and the delimiters within this range.
    
*   If to is greater than the number of delimited substrings, the returned string contains all the string data (substrings and delimiters) beginning with the from substring and continuing to the end of the plist string.
    
*   If from is equal to to, the from substring is returned.
    
*   If from is greater than to, $PIECE returns a null string.
    
*   If to is the null string (NULL), $PIECE returns a null string.
    

Examples

The following example returns 'Red', the first substring as identified by the "," delimiter:

SELECT $PIECE('Red,Green,Blue,Yellow,Orange,Black',',')

 

The following example returns 'Blue', the third substring as identified by the "," delimiters:

SELECT $PIECE('Red,Green,Blue,Yellow,Orange,Black',',',3)

 

The following example returns 'Blue,Yellow,Orange', the third through fifth elements in colorlist, as delimited by ",":

SELECT $PIECE('Red,Green,Blue,Yellow,Orange,Black',',',3,5)

 

The following $PIECE functions both return '123', showing that the two-argument form is equivalent to the three-argument form when from is 1:

SELECT $PIECE('123#456#789','#') AS TwoArg

 

SELECT $PIECE('123#456#789','#',1) AS ThreeArg

 

The following example uses the multi-character delimiter string '#-#' to return the third substring '789'. Here, the component characters of the delimiter string, '#' and '-', can be used as data values; only the specified sequence of characters (#-#) is set aside:

SELECT $PIECE('1#2-3#-#45##6#-#789','#-#',3)

 

The following example returns 'MAR;APR;MAY'. These comprise the third through the fifth substrings, as identified by the ';' delimiter:

SELECT $PIECE('JAN;FEB;MAR;APR;MAY;JUN',';',3,5)

 

The following example uses $PIECE to extract the surname from employee names and vendor contact names, and then perform a JOIN which return instances where an employee has the same surname as a vendor contact:

SELECT E.Name,V.Contact 
FROM Sample.Employee AS E INNER JOIN Sample.Vendor AS V 
ON $PIECE(E.Name,',')\=$PIECE(V.Contact,',')

 

Notes

Using $PIECE to Unpack Data Values

$PIECE is typically used to "unpack" data values that contain multiple fields delimited by a separator character. Typical delimiter characters include the slash (/), the comma (,), the space ( ), and the semicolon (;). The following sample values are good candidates for use with $PIECE:

'John Jones/29 River St./Boston MA, 02095'
'Mumps;Measles;Chicken Pox;Diptheria'
'45.23,52.76,89.05,48.27'

$PIECE and $LENGTH

The two-argument form of $LENGTH returns the number of substrings in a string, based on a delimiter. Use $LENGTH to determine the number of substrings in a string, and then use $PIECE to extract individual substrings.

$PIECE and $LIST

The data storage techniques used by $PIECE and the $LIST functions are incompatible and should not be combined. For example, attempted to use $PIECE on a list created using $LISTBUILD yields unpredictable results and should be avoided. This is true for both SQL functions and the corresponding Caché ObjectScript functions.

The $LIST functions specify substrings without using a designated delimiter. If setting aside a delimiter character or character sequence is not appropriate to the type of data (for example, bitstring data), you should use the $LISTBUILD and $LIST SQL functions to store and retrieve substrings.

Null Values

$PIECE does not distinguish between a delimited substring with a null string value (NULL), and a nonexistent substring. Both return <null>, the null string value. For example, the following examples both return the null string for a from value of 7:

SELECT $PIECE('Red,Green,Blue,Yellow,Orange,Black',',',7)

 

SELECT $PIECE('Red,Green,Blue,Yellow,Orange,Black,',',',7)

 

In the first case, there is no seventh substring; a null string is returned. In the second case there is a seventh substring, as indicted by the delimiter at the end of the plist string; the value of this seventh substring is the null string.

The following example shows null values within a plist. It extracts substrings 3. This substring exists, but contains a null string:

SELECT $PIECE('Red,Green,,Blue,Yellow,Orange,Black,',',',3)

 

The following examples also returns a null string, because the specified substrings do not exist:

SELECT $PIECE('Red,Green,,Blue,Yellow,Orange,Black,',',',0)

 

SELECT $PIECE('Red,Green,,Blue,Yellow,Orange,Black,',',',8,20)

 

In the following example, the $PIECE function returns the entire plist string, because there are no occurrences of delimiter in the plist string:

SELECT $PIECE('Red,Green,Blue,Yellow,Orange,Black,','#')

 

Nested $PIECE Operations

To perform complex extractions, you can nest $PIECE references within each other. The inner $PIECE returns a substring that is operated on by the outer $PIECE. Each $PIECE uses its own delimiter. For example, the following returns the state abbreviation 'MA':

SELECT $PIECE($PIECE('John Jones/29 River St./Boston MA 02095','/',3),' ',2)

 

The following is another example of nested $PIECE operations, using a hierarchy of delimiters. First, the inner $PIECE uses the caret (^) delimiter to find the second piece, 'A,B,C', of the string. Then the outer $PIECE uses the comma (,) delimiter to return the first and second pieces ('A,B') of the substring 'A,B,C':

SELECT $PIECE($PIECE('1,2,3^A,B,C^@#!','^',2),',',1,2)

 

See Also

*   SQL functions: [$EXTRACT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_extract) [$FIND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_find) [$LENGTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_length) [$LIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_list)
    
*   Caché ObjectScript functions: [$EXTRACT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fextract) [$FIND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ffind) [$LENGTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flength) [$LIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flist) [$PIECE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fpiece)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_pi "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_plus_pct "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_d\_piece.xml**