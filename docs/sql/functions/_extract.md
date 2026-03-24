# $EXTRACT

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_d_extract

---

 

Caché SQL Reference

$EXTRACT

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_external "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_find "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [$EXTRACT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_extract "$EXTRACT") \]

Go to: Description Arguments Examples Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A string function that extracts characters from a string by position.

Synopsis

$EXTRACT(string\[,from\[,to\]\])

Arguments

string

The target string from which the substring is to be extracted.

from

Optional — The position within the target string for a single character, or the beginning of a range of characters (inclusive), to be extracted. Specified as a positive integer counting from 1.

to

Optional — The end position (inclusive) for a range of characters to be extracted. Specified as a positive integer counting from 1.

Description

$EXTRACT returns a substring from a specified position in string. The nature of the substring returned depends on the arguments used.

*   $EXTRACT(string) extracts the first character in the string.
    
*   $EXTRACT(string,from) extracts the character in the position specified by from. For example, if variable var1 contains the string “ABCD”, the following command extracts “B” (the second character):
    
    SELECT $EXTRACT('ABCD',2) AS Extracted
    
     
    
*   $EXTRACT(string,from,to) extracts the range of characters starting with the from position and ending with the to position. For example, the following command extracts the string “Alabama” (that is, all characters from position 5 to position 11, inclusive) from the string “1234Alabama567”:
    
    SELECT $EXTRACT('1234Alabama567',5,11) AS Extracted
    
     
    

This function returns data of type VARCHAR.

Arguments

string

The string value can be a variable name, a numeric value, a string literal, or any valid expression.

from

The from value must be a positive integer (however, see Note). If a fractional number, the fraction is truncated and only the integer portion is used.

If the from value is greater than the number of characters in the string, $EXTRACT returns a null string.

If from is specified without the to argument, it extracts the single specified character.

If used with the to argument, it identifies the start of the range to be extracted and must be less than the value of to. If from equals to, $EXTRACT returns the single character at the specified position. If from is greater than to, $EXTRACT returns a null string.

to

The to argument must be used with the from argument. It must be a positive integer. If a fractional number, the fraction is truncated and only the integer portion is used.

If the to value is greater than or equal to the from value, $EXTRACT returns the specified substring. If to is greater than the length of the string, $EXTRACT returns the substring from the from position to the end of the string. If to is less than from, $EXTRACT returns a null string.

Examples

The following example returns “S”, the fourth character in the string:

SELECT $EXTRACT('THIS IS A TEST',4) AS Extracted

 

The following example returns a substring “THIS IS” which is composed of the first through seventh characters.

SELECT $EXTRACT('THIS IS A TEST',1,7) AS Extracted

 

The following Embedded SQL example extracts the second character (“B” ) from a and assigns this value to variable y.

   SET a\="ABCD"
   &sql(SELECT $EXTRACT(:a,2) INTO :y)
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"The extract returns ",y }

 

The following Embedded SQL example shows that the one-argument format is equivalent to the two-argument format when the from value is “1”. Both $EXTRACT functions return “H”.

   SET a\="HELLO"
   &sql(SELECT $EXTRACT(:a),$EXTRACT(:a,1) INTO :b,:c)
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"The one-arg form returns ",b
     WRITE !,"The two-arg form returns ",c }

 

Notes

$EXTRACT Compared with $PIECE and $LIST

$EXTRACT returns a substring of characters by integer position from a string. $PIECE and $LIST both work on specially formatted strings.

$PIECE returns a substring from a standard character string using delimiter characters within the string.

$LIST returns a sublist of elements from an encoded list by the integer position of elements (not characters). $LIST cannot be used on ordinary strings, and $EXTRACT cannot be used on encoded lists.

The $EXTRACT, $FIND, $LENGTH, and $PIECE functions operate on standard character strings. The various $LIST functions operate on encoded character strings, which are incompatible with standard character strings. The only exceptions are the $LISTGET function and the one-argument and two-argument forms of $LIST, which take an encoded character string as input, but output a single element value as a standard character string.

$EXTRACT and Unicode

The $EXTRACT function operates on characters, not bytes. Therefore, Unicode strings are handled the same as ASCII strings, as shown in the following embedded SQL example using the Unicode character for “pi” ($CHAR(960)):

  IF $SYSTEM.Version.IsUnicode() {
   SET a\="QT PIE"
   SET b\=("QT "\_$CHAR(960))
   &sql(SELECT 
   $EXTRACT(:a,\-33,4),
   $EXTRACT(:a,4,4),
   $EXTRACT(:a,4,99),
   $EXTRACT(:b,\-33,4),
   $EXTRACT(:b,4,4),
   $EXTRACT(:b,4,99)
   INTO :a1,:a2,:a3,:b1,:b2,:b3)
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"ASCII form returns ",!,a1,!,a2,!,a3
     WRITE !,"Unicode form returns ",!,b1,!,b2,!,b3 }
  }
  ELSE {WRITE "This example requires a Unicode installation of Caché"}

 

Null and Invalid Arguments

*   When string is a null string, a null string is returned.
    
*   When from is a number larger than the string length, a null string is returned.
    
*   When from is zero or a negative number, and no to is specified, a null string is returned.
    
*   When to is zero, a negative number, or a number smaller than from, a null string is returned.
    
*   When to is a valid value, from can be zero or a negative number. $EXTRACT treats such from values as 1.
    

No SQLCODE error is generated for invalid argument values.

In following example, the negative from value is evaluated as 1; $EXTRACT returns the substring “THIS IS” composed of the first through seventh characters.

SELECT $EXTRACT('THIS IS A TEST',\-7,7)

 

In following embedded SQL example, all $EXTRACT function calls return the null string:

   SET a\="THIS IS A TEST"
   SET b\=""
   &sql(SELECT 
   $EXTRACT(:a,33),
   $EXTRACT(:a,\-7),
   $EXTRACT(:a,3,2),
   $EXTRACT(:a,\-7,0),
   $EXTRACT(:a,\-7,\-10),
   $EXTRACT(:b,\-33,4),
   $EXTRACT(:b,4,4),
   $EXTRACT(:b,4,99),
   $EXTRACT(NULL,\-33,4),
   $EXTRACT(NULL,4,4),
   $EXTRACT(NULL,4,99)
   INTO :a1,:a2,:a3,:a4,:a5,:b1,:b2,:b3,:c1,:c2,:c3)
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"FROM too big: ",a1
     WRITE !,"FROM negative, no TO: ",a2
     WRITE !,"TO smaller than FROM: ",a3
     WRITE !,"TO not a positive integer: ",a4,a5
     WRITE !,"LIST is null string: ",b1,b2,b3,c1,c2,c3 }

 

See Also

*   SQL functions: [$FIND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_find) [$LENGTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_length) [$LIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_list) [$LISTGET](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listget) [$PIECE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_piece)
    
*   Caché ObjectScript functions: [$EXTRACT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fextract) [$FIND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ffind) [$LENGTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flength) [$LIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flist) [$LISTBUILD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistbuild) [$LISTGET](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistget) [$PIECE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fpiece)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_external "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_find "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_d\_extract.xml**