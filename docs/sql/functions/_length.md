# $LENGTH

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_d_length

---

 

Caché SQL Reference

$LENGTH

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_length "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_list "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [$LENGTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_length "$LENGTH") \]

Go to: Description Examples Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A string function that returns the number of characters or the number of delimited substrings in a string.

Synopsis

$LENGTH(expression\[,delimiter\])

Arguments

expression

The target string. It can be a numeric value, a string literal, the name of any variable, or any valid expression.

delimiter

Optional — A string that demarcates separate substrings in the target string. It must be a string literal, but can be of any length. The enclosing quotation marks are required.

Description

$LENGTH returns the number of characters in a specified string or the number of substrings in a specified string, depending on the arguments used.

*   $LENGTH(expression) returns the number of characters in the string. If the expression is an empty string (''), $LENGTH returns 0. If the expression is NULL, $LENGTH returns 0.
    
*   $LENGTH(expression,delimiter) returns the number of substrings within the string. $LENGTH returns the number of substrings separated from one another by the indicated delimiter. This number is always equal to the number of delimiter instances found in the expression string, plus one.
    

This function returns data of type SMALLINT.

NULL and Empty String Arguments

$LENGTH(expression) does not distinguish between the empty string ('') and NULL (the absence of a value). It returns a length of 0 for both an empty string ('') value and for NULL. In contrast, the [LENGTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_length) function returns a value of 0 for an empty string, and NULL for a NULL.

$LENGTH(expression,delimiter) with a non-null delimiter returns a delimited substring count of 1 if no match occurred. The full string is a single substring containing no delimiters. This is true even when expression is the empty string (''), or expression is NULL. However, an empty string does match itself, returning a value of 2.

The following table shows the possible combinations of a string ('abc'), empty string (''), or NULL expression value paired with a non-matching string ('^'), empty string (''), or NULL delimiter value:

$LENGTH(NULL) = 0

$LENGTH('') = 0

$LENGTH('abc') = 3

$LENGTH(NULL,NULL) = 0

$LENGTH('',NULL) = 0

$LENGTH(’abc‘,NULL) = 0

$LENGTH(NULL,'') = 1

$LENGTH('','') = 2

$LENGTH(’abc‘,'') = 1

$LENGTH(NULL,'^') = 1

$LENGTH(’‘,'^') = 1

$LENGTH('abc','^') = 1

Examples

The following example returns 6, the length of the string:

SELECT $LENGTH('ABCDEG') AS StringLength

 

The following example returns 3, the number of substrings within the string, as delimited by the dollar sign ($) character.

SELECT $LENGTH('ABC$DEF$EFG','$') AS SubStrings

 

If the specified delimiter is not found in the string $LENGTH returns 1, because the only substring is the string itself:

SELECT $LENGTH('ABCDEG','$') AS SubStrings

 

In the following embedded SQL example, the first $LENGTH function returns 11, the number of characters in a (including, of course, the space character). The second $LENGTH function returns 2, the number of substrings in a using b, the space character, as the substring delimiter.

   SET a\="HELLO WORLD"
   SET b\=" "
   &sql(SELECT 
   $LENGTH(:a),
   $LENGTH(:a,:b)
   INTO :a1,:a2 )
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"The input string: ",a
     WRITE !,"Number of characters: ",a1
     WRITE !,"Number of substrings: ",a2 }

 

The following example returns 0 because the string tested is the null string:

SELECT $LENGTH(NULL) AS StringLength

 

The following example returns 1 because a delimiter is specified and not found. There is one substring, which is the null string:

SELECT $LENGTH(NULL,'$') AS SubStrings

 

The following example returns 0 because the delimiter is the null string:

SELECT $LENGTH('ABCDEFG',NULL) AS SubStrings

 

Notes

$LENGTH, $PIECE, and $LIST

*   $LENGTH with one argument returns the number of characters in a string. This function can be used with the $EXTRACT function, which locates a substring by position and returns the substring value.
    
*   $LENGTH with two arguments returns the number of substrings in a string, based on a delimiter. This function can be used with the $PIECE function, which locates a substring by a delimiter and returns the substring value.
    
*   $LENGTH should not be used on encoded lists created using $LISTBUILD or $LIST. Use $LISTLENGTH to determine the number of substrings (list elements) in an encoded list string.
    

The $LENGTH, $FIND, $EXTRACT, and $PIECE functions operate on standard character strings. The various $LIST functions operate on encoded character strings, which are incompatible with standard character strings. The only exceptions are the $LISTGET function and the one-argument and two-argument forms of $LIST, which take an encoded character string as input, but output a single element value as a standard character string.

See Also

*   SQL functions: [$EXTRACT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_extract), [$FIND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_find), [LENGTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_length), [$LIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_list), [$LISTGET](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listget), [$PIECE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_piece)
    
*   Caché ObjectScript functions: [$EXTRACT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fextract), [$FIND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ffind), [$LENGTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flength), [$LIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flist), [$LISTBUILD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistbuild), [$LISTGET](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistget), [$PIECE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fpiece)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_length "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_list "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_d\_length.xml**