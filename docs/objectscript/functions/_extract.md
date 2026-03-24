# $EXTRACT

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fextract

---

 

Caché ObjectScript Reference

$EXTRACT

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fdouble "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ffactor "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$EXTRACT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fextract "$EXTRACT") \]

Go to: Description Parameters Replacing a Substring Using SET $EXTRACT Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Extracts a substring from a character string by position, or replaces a substring by position.

Synopsis

$EXTRACT(string,from,to)
$E(string,from,to)

SET $EXTRACT(string,from,to)=value
SET $E(string,from,to)=value

Parameters

string

The target string in which substrings are identified. Specify string as an expression that evaluates to a quoted string or a numeric value. In SET $EXTRACT syntax, string must be a variable or a multi-dimensional property.

from

Optional — Specifies the starting position within the target string. Characters are counted from 1. Permitted values are n (a positive integer specifying the character count from the beginning of string), \* (specifying the last character in string), and \*-n (offset integer count of characters backwards from end of string). SET $EXTRACT syntax also supports \*+n (offset integer count of characters to append beyond the end of string). A from without a to specifies a single character. A from with a to specifies a range of characters. If from is not specified, it defaults to 1.

to

Optional — Specifies the end position (inclusive) for a range of characters. Must be used with from. Permitted values are n (a positive integer specifying the character count from the beginning of string), \* (specifying the last character in string), and \*-n (offset integer count of characters backwards from end of string). SET $EXTRACT syntax also supports \*+n (offset integer count of the end of a range of characters to append beyond the end of string).

Description

$EXTRACT identifies substrings within string by character count, either from the beginning of string or the end of string. A substring can be a single character or a range of characters.

$EXTRACT can be used in two ways:

*   To [return a substring](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fextract#RCOS_fextract_return) from string. This uses the $EXTRACT(string,from,to) syntax.
    
*   To [replace a substring](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fextract#RCOS_fextract_replace) within string. The replacement substring may be the same length, longer, or shorter than the original substring. This uses the SET $EXTRACT(string,from,to)=value syntax.
    

Returning a Substring

$EXTRACT returns a substring by character position from string. The nature of this substring extraction depends on the parameters used.

*   $EXTRACT(string) extracts the first character in the string.
    
       SET mystr\="ABCD"
       WRITE $EXTRACT(mystr)
    
     
    
*   $EXTRACT(string,from) extracts a single character in the position specified by from. The from value can be an integer count from the beginning of the string, an asterisk specifying the last character of the string, or an asterisk with a negative integer specifying a count backwards from the end of the string.
    
    The following example extracts single letters from the string “ABCD”:
    
       SET mystr\="ABCD"
       WRITE !,$EXTRACT(mystr,2)    // "B" the 2nd character
       WRITE !,$EXTRACT(mystr,\*)    // "D" the last character
       WRITE !,$EXTRACT(mystr,\*\-2)  // "B" the offset 2 characters from end
       WRITE !,$EXTRACT(mystr,\*\-0)  // "D" the last character by 0 offset
    
     
    
*   $EXTRACT(string,from,to) extracts the range of characters starting with the from position and ending with the to position (inclusive). For example, if variable var2 contains the string “1234Alabama567”, the following $EXTRACT functions both return the string “Alabama”:
    
       SET var2\="1234Alabama567"
       WRITE !,$EXTRACT(var2,5,11)
       WRITE !,$EXTRACT(var2,\*\-9,\*\-3)
    
     
    

Parameters

string

The target string in which the substring is identified.

When $EXTRACT is used to [return a substring](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fextract#RCOS_fextract_return), string can be a string literal enclosed in quotation marks, a canonical numeric, a variable, an object property, or any valid Caché ObjectScript expression that evaluates to a string or a numeric. If you specify a null string ("") as the target string, $EXTRACT always returns the null string, regardless of the other parameter values.

When $EXTRACT is used with SET on the left hand side of the equals sign to [replace a substring](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fextract#RCOS_fextract_replace), string can be a variable name or a [multidimensional property](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ROBJ_property_multidimensional) reference; it cannot be a non-multidimensional object property.

from

The from parameter can specify a single character, or the beginning of a range of characters.

*   If from is n (a positive integer), $EXTRACT counts characters from the beginning of string.
    
*   If from is \* (asterisk), $EXTRACT returns the last character in string.
    
*   If from is \*-n (an asterisk followed by a negative number), $EXTRACT counts characters by offset from the end of string. Thus, \*-0 is the last character in string, \*-1 is the next-to-last character in string (an offset of 1 from the end).
    
*   For SET $EXTRACT syntax only — If from is \*+n (an asterisk followed by a positive number), SET $EXTRACT appends characters by offset beyond the end of string. Thus, \*+1 appends a character beyond the end of string, \*+2 appends a character two positions beyond the end of string, padding the skipped position with a blank space. \*+0 is the last character in string.
    

If the from integer value is greater than the number of characters in the string, $EXTRACT returns a null string. With a from \*-n value, if n is equal to or greater than the number of characters in the string, $EXTRACT returns a null string. If the from value is 0 or a negative number, $EXTRACT returns a null string; however, if from is used with to, a from value of 0 or a negative number is treated as a value of 1.

If from is used with the to parameter, from identifies the start of the range to be extracted and must be less than the value of to. If from equals to, $EXTRACT returns the single character at the specified position. If from is greater than to, $EXTRACT returns a null string. If used with the to parameter, a from value less than 1 (zero, or a negative number) is treated as if it were the number 1.

to

The to parameter must be used with the from parameter. It must be a positive integer, \* (asterisk), or \*-n (an asterisk followed by a negative integer). If the to value is an integer greater than or equal to the from value, $EXTRACT returns the specified substring. If the to value is an asterisk, $EXTRACT returns the substring beginning with the from character through the end of the string. If to is an integer greater than the length of the string, $EXTRACT also returns the substring beginning with the from character through the end of the string.

If the from and to positions are the same,$EXTRACT returns a single character. If the to position is closer to the beginning of the string than the from position, $EXTRACT returns the null string.

If you omit the to parameter, only one character is returned. If from is specified, $EXTRACT returns the character identified by from. If both to and from are omitted, $EXTRACT returns the first character of string.

For SET $EXTRACT syntax only — If to is \*+n, SET $EXTRACT appends a range of characters by offset beyond the end of string, padding with blank spaces as needed. If from represents a character position after the end of string, SET $EXTRACT appends characters. If from represents a character position before the end of string, SET $EXTRACT may both replace and append characters.

Specifying \*-n and \*+n Parameter Values

When using a variable to specify \*-n or \*+n, you must always specify the asterisk and a sign character in the parameter itself.

The following are valid specifications of \*-n:

  SET count\=2
  SET alph\="abcd"
  WRITE $EXTRACT(alph,\*\-count)

 

  SET count\=\-2
  SET alph\="abcd"
  WRITE $EXTRACT(alph,\*+count)

 

The following is a valid specification of \*+n:

  SET count\=2
  SET alph\="abcd"
  SET $EXTRACT(alph,\*+count)\="F"
  WRITE alph

 

Whitespace is permitted within these parameter values.

Examples: Returning a Substring

The following example returns “D”, the fourth character in the string:

   SET x\="ABCDEFGHIJK"
   WRITE $EXTRACT(x,4)

 

The following example returns “K”, the last character in the string:

   SET x\="ABCDEFGHIJK"
   WRITE $EXTRACT(x,\*)

 

In the following example, all the $EXTRACT functions return “J” the next-to-last character in the string:

   SET n\=\-1
   SET m\=1
   SET x\="ABCDEFGHIJK"
   WRITE !,$EXTRACT(x,\*\-1)
   WRITE !,$EXTRACT(x,\*\-m)
   WRITE !,$EXTRACT(x,\*+n)
   WRITE !,$EXTRACT(x,\*\-1,\*\-1)

 

Note that a minus or plus sign is needed between the asterisk and the integer variable.

The following example shows that the one-argument format is equivalent to the two-argument format when the from value is “1”. Both $EXTRACT functions return “H”.

   SET x\="HELLO"
   WRITE !,$EXTRACT(x)
   WRITE !,$EXTRACT(x,1)

 

The following example returns a substring “THIS IS” which is composed of the first through seventh characters.

   SET x\="THIS IS A TEST"
   WRITE $EXTRACT(x,1,7)

 

The following example also returns the substring “THIS IS”. When the from variable contains a value less than 1, $EXTRACT treats that value as 1. Thus, the following example returns a substring composed of the first through seventh characters.

   SET X\="THIS IS A TEST"
   WRITE $EXTRACT(X,\-1,7)

 

The following example returns the last four characters of the string:

   SET X\="THIS IS A TEST"
   WRITE $EXTRACT(X,\*\-3,\*)

 

The following example also returns the last four characters of the string:

   SET X\="THIS IS A TEST"
   WRITE $EXTRACT(X,\*\-3,14)

 

The following example extracts a substring from an object property:

  SET tStatement \= ##class(%SQL.Statement).%New()
  SET tStatement.%SchemaPath\="MyTests,Sample,Cinema"
  WRITE "whole schema path: ",tStatement.%SchemaPath,!
  WRITE "start of schema path: ",$EXTRACT(tStatement.%SchemaPath,1,10),!

 

Replacing a Substring Using SET $EXTRACT

You can use $EXTRACT with the SET command to replace a specified character or range of characters with another value. You can also use it to append characters to the end of a string.

When $EXTRACT is used with SET on the left hand side of the equals sign, string can be a valid variable name. If the variable does not exist, SET $EXTRACT defines it. The string parameter can also be a [multidimensional property](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ROBJ_property_multidimensional) reference; it cannot be a non-multidimensional object property. Attempting to use SET $EXTRACT on a non-multidimensional object property results in an <OBJECT DISPATCH> error.

You cannot use SET (a,b,c,...)=value syntax with $EXTRACT (or $PIECE or $LIST) on the left of the equals sign, if the function uses relative offset syntax: \* representing the end of a string and \*-n or \*+n representing relative offset from the end of the string. You must instead use SET a=value,b=value,c=value,... syntax.

The simplest form of SET $EXTRACT is a one-for-one substitution:

   SET alph\="ABZD"
   SET $EXTRACT(alph,3)\="C"
   WRITE alph        ; "ABCD"

 

You can append characters to string either by specifying to as a positive integer that is 1 larger than the length of string, or by specifying to as \*+1, as shown in the following examples:

   SET alph\="ABCD"
   SET $EXTRACT(alph,5)\="E"
   WRITE alph        ; "ABCDE"

 

   SET alph\="ABCD"
   SET $EXTRACT(alph,\*+1)\="E"
   WRITE alph        ; "ABCDE"

 

If you specify to larger than the string plus 1, $EXTRACT pads with blank spaces:

   SET alph\="ABCD"
   SET len\=$LENGTH(alph)
   SET $EXTRACT(alph,len+2)\="F"
   WRITE alph        ; "ABCD F"

 

   SET alph\="ABCD"
   SET $EXTRACT(alph,\*+2)\="F"
   WRITE alph        ; "ABCD F"

 

You can also extract a string and replace it with a string of a different length. For example, the following command extracts the string “Rhode Island” from foo and replaces it with the string “Texas”, with no padding.

   SET foo\="Deep in the heart of Rhode Island"
   SET $EXTRACT(foo,22,33)\="Texas"
   WRITE foo      ; "Deep in the heart of Texas"

 

You can extract a string and set it to the null string, removing the extracted characters from the string:

   SET alph\="ABCzzzzzD"
   SET $EXTRACT(alph,4,8)\=""
   WRITE alph        ; "ABCD"

 

If you specify from larger than to, no replacement occurs:

   SET alph\="ABCD"
   SET $EXTRACT(alph,4,3)\="X"
   WRITE alph        ; "ABCD"

 

In the following example, assume that variable x does not exist.

   KILL x
   SET $EXTRACT(x,1,4)\="ABCD"
   WRITE x         ; "ABCD"

 

The SET command creates variable x and assigns it the value “ABCD”.

SET $EXTRACT performs leading padding with blank spaces as required, but does not perform trailing padding. The following example inserts the value “F” in the sixth position past the end of the string, but inserts no additional characters in positions 7 and 8:

   SET alph\="ABCD"
   SET $EXTRACT(alph,6,8)\="F"
   WRITE alph        ; "ABCD F"

 

The following example inserts the value “F” in the sixth position and adds characters past the specified range:

   SET alph\="ABCD"
   SET $EXTRACT(alph,6,8)\="FGHIJ"
   WRITE alph        ; "ABCD FGHIJ"

 

The following example shortens a character string by extracting a from,to range larger than the number of values in the replacement string.

   SET x\="ABCDEFGH"
   SET $EXTRACT(x,3,6)\="Z"
   WRITE x

 

inserts the value “Z” in the third position and removes positions 4, 5 and 6. Variable x now contains the value “ABZGH” and has a length of 5.

Notes

$EXTRACT and Unicode

The $EXTRACT function operates on characters, not bytes. Therefore, Unicode strings are handled the same as ASCII strings, as shown in the following example using the Unicode character for “pi” ($CHAR(960)):

   IF $SYSTEM.Version.IsUnicode()  {
     SET a\="QT PIE"
     SET b\="QT "\_$CHAR(960)
     SET a1\=$EXTRACT(a,\-33,4)
     SET a2\=$EXTRACT(a,4,4)
     SET a3\=$EXTRACT(a,4,99)
     SET b1\=$EXTRACT(b,\-33,4)
     SET b2\=$EXTRACT(b,4,4)
     SET b3\=$EXTRACT(b,4,99)
     WRITE !,"ASCII form returns ",!,a1,!,a2,!,a3
     WRITE !,"Unicode form returns ",!,b1,!,b2,!,b3
   }
   ELSE {WRITE "This example requires a Unicode installation of Caché"}

 

Surrogate Pairs

$EXTRACT does not recognize surrogate pairs. A surrogate pair is a pair of 16-bit Unicode characters that together encode a single ideographic character. Surrogate pairs are used to represent some Chinese characters and to support the Japanese JIS2004 standard. You can use the $WISWIDE function to determine if a string contains a surrogate pair. The $WEXTRACT function recognizes and correctly parses surrogate pairs. $EXTRACT and $WEXTRACT are otherwise identical. However, because $EXTRACT is generally faster than $WEXTRACT, $EXTRACT is preferable for all cases where a surrogate pair is not likely to be encountered.

$EXTRACT in DTM Modes

In the DTM and DTM-J modes, $EXTRACT supports two additional arguments, as follows:

$EXTRACT(string,from,to,replace,pad)

The optional replace argument replaces the substring specified by from and to with the replace substring, and returns the result. The original string is not changed.

The optional pad argument specifies a padding character. This is used when the from argument specifies a position beyond the end of string. The returned string is padded to the location specified by from followed by the replace substring. The pad value may be any single character; a nonnumeric character must be enclosed in quotes. To specify a quote character as the pad character literal, double it.

You can use the [LanguageMode()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process#LanguageMode) method of the [%SYSTEM.Process](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process) class to set DTM mode (2) or DTM-J mode (7).

The following example shows the four-argument replace syntax:

   SET x="ABCDEFGH"
   DO ##class(%SYSTEM.Process).LanguageMode(2)
   WRITE $EXTRACT(x,3,6,"##")
     /\* returns "AB##GH"  \*/

The following example use the four-argument syntax to append the replace string:

   SET x="ABCDEFGH"
   DO ##class(%SYSTEM.Process).LanguageMode(2)
   WRITE $EXTRACT(x,1,0,"##")
     /\* returns "##ABCDEFGH"  \*/

The following example shows the five-argument pad and replace syntax:

   SET x="ABCDEFGH"
   DO ##class(%SYSTEM.Process).LanguageMode(2)
   WRITE $EXTRACT(x,12,16,"##","\*")
     /\* returns "ABCDEFGH\*\*\*##"  \*/

Note:

When using four-argument or five-argument syntax, the $EXTRACT from and to arguments do not support asterisk syntax.

SET $EXTRACT cannot be used with four-argument or five-argument syntax.

$EXTRACT Compared with $PIECE and $LIST

$EXTRACT determines a substring by counting characters from the beginning of a string. $EXTRACT takes as input any ordinary character string. $PIECE and $LIST both work on specially prepared strings.

[$PIECE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fpiece) determines a substring by counting user-defined delimiter characters within the string.

[$LIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flist) determines an element from an encoded list by counting elements (not characters) from the beginning of the list. $LIST cannot be used on ordinary strings, and $EXTRACT cannot be used on encoded lists.

See Also

*   [SET](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cset) command
    
*   [$FIND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ffind) function
    
*   [$LENGTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flength) function
    
*   [$PIECE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fpiece) function
    
*   [$REVERSE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_freverse) function
    
*   [$WEXTRACT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwextract) function
    
*   [$WFIND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwfind) function
    
*   [$WISWIDE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwiswide) function
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fdouble "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ffactor "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fextract.xml**