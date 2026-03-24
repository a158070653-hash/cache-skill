# $WEXTRACT

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fwextract

---

 

Caché ObjectScript Reference

$WEXTRACT

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwchar "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwfind "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$WEXTRACT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwextract "$WEXTRACT") \]

Go to: Description Returning a Substring Replacing a Substring Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Extracts a substring from a character string by position, or replaces a substring by position, recognizing surrogate pairs.

Synopsis

$WEXTRACT(string,from,to)
$WE(string,from,to)

SET $WEXTRACT(string,from,to)=value
SET $WE(string,from,to)=value

Parameters

string

The target string in which substrings are identified. Specify string as an expression that evaluates to a quoted string or a numeric value. In SET $WEXTRACT syntax, string must be a variable or a multi-dimensional property.

from

Optional — The starting position within the target string. Characters are counted from 1. A surrogate pair is counted as a single character. Permitted values are n (a positive integer specifying the start position as a character count from the beginning of string), \* (specifying the last character in string), and \*-n (offset integer count of characters backwards from end of string). SET $WEXTRACT syntax also supports \*+n (offset integer count of characters to append beyond the end of string). If not specified, the default is 1. Different values are used for the two-parameter form $WEXTRACT(string,from), and the three-parameter form $WEXTRACT(string,from,to):

Without to: Specifies a single character. To count from the beginning of string, specify an expression that evaluates to a positive integer (counting from 1); a zero (0) or negative number returns the empty string. To count from the end of string specify \*, or \*-n. If from is omitted it defaults to 1.

With to: Specifies the start of a range of characters. To count from the beginning of string, specify an expression that evaluates to a positive integer (counting from 1). A zero (0) or negative number evaluates as 1. To count from the end of string specify \*, or \*-n.

to

Optional — Specifies the end position (inclusive) for a range of characters. Must be used with from. Permitted values are n (a positive integer equal to or larger than from that specifies the end position as a character count from the beginning of string), \* (specifying the last character in string), and \*-n (offset integer count of characters backwards from end of string). A surrogate pair is counted as a single character. You can specify a to value that is beyond the end of the string.

SET $WEXTRACT syntax also supports \*+n (offset integer count of the end of a range of characters to append beyond the end of string).

Description

$WEXTRACT identifies a substring within string by position, either counting characters from the beginning of string or counting characters by offset from the end of string. A substring can be a single character or a range of characters. $WEXTRACT recognizes a surrogate pair as a single character.

$WEXTRACT can be used in two ways:

*   To [return a substring](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwextract#RCOS_fwextract_return) from string. This uses the $WEXTRACT(string,from,to) syntax.
    
*   To [replace a substring](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwextract#RCOS_fwextract_replace) within string. The replacement substring may be the same length, longer, or shorter than the original substring. This uses the SET $WEXTRACT(string,from,to)=value syntax.
    

$WEXTRACT and $EXTRACT are functionally identical, except for the handling of surrogate pairs.

Surrogate Pairs

The $WEXTRACT from and to parameters count a surrogate pair as a single character. You can use the [$WISWIDE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwiswide) function to determine if a string contains a surrogate pair.

A surrogate pair is a pair of 16-bit Unicode characters that together encode a single ideographic character. Surrogate pairs are used to represent certain ideographs which are used in Chinese, Japanese kanji, and Korean hanja. (Most commonly-used Chinese, kanji, and hanja characters are represented by standard 16-bit Unicode encodings, not surrogate pairs.) Surrogate pairs provide Caché support for the Japanese JIS X0213:2004 (JIS2004) encoding standard and the Chinese GB18030 encoding standard.

A surrogate pair consists of high-order Unicode character in the hexadecimal range D800 through DBFF, and a low-order Unicode character in the hexadecimal range DC00 through DFFF.

The $WEXTRACT function treats a surrogate pair as a single character. The $EXTRACT function treats a surrogate pair as two characters. If a string contains no surrogate pairs, either $WEXTRACT and $EXTRACT can be used and return the same value. However, because $EXTRACT is generally faster than $WEXTRACT, $EXTRACT is preferable for all cases where a surrogate pair is not likely to be encountered. For further details on extracting a substring, refer to the [$EXTRACT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fextract) function.

Returning a Substring

$WEXTRACT returns a substring by character position from string. The nature of this substring extraction depends on the parameters used:

*   $WEXTRACT(string) extracts the first character in the string.
    
*   $WEXTRACT(string,from) extracts a single character in the position specified by from. The from value can be an integer count of characters from the beginning of the string, an asterisk specifying the last character of the string, or an asterisk with a negative integer specifying a character count backwards from the end of the string.
    
    The following example extracts single letters from a string containing a surrogate pair. Note that $LENGTH counts a surrogate pair as two characters, but $WEXTRACT counts a surrogate pair as a single character:
    
      IF $SYSTEM.Version.IsUnicode()  {
      SET hipart\=$CHAR($ZHEX("D806"))
      SET lopart\=$CHAR($ZHEX("DC06"))
      SET spair\=hipart\_lopart /\* surrogate pair \*/
      WRITE "length of surrogate pair ",$LENGTH(spair),!
      SET mystr\="AB"\_spair\_"DEFG"
      WRITE !,$WEXTRACT(mystr,4)    // "D" the 4th character
      WRITE !,$WEXTRACT(mystr,\*)    // "G" the last character
      WRITE !,$WEXTRACT(mystr,\*\-5)  // "B" the offset 5 character from end
      WRITE !,$WEXTRACT(mystr,\*\-0)  // "G" the last character by 0 offset
      }
      ELSE {WRITE "This example requires a Unicode installation of Caché"}
    
     
    
*   $WEXTRACT(string,from,to) extracts the range of characters starting with the from position and ending with the to position (inclusive). The following $WEXTRACT functions both return the string “Alabama”, counting surrogate pairs as single characters:
    
      IF $SYSTEM.Version.IsUnicode()  {
      SET hipart\=$CHAR($ZHEX("D806"))
      SET lopart\=$CHAR($ZHEX("DC06"))
      SET spair\=hipart\_lopart /\* surrogate pair \*/
      SET var2\=spair\_"XXX"\_spair\_"Alabama"\_spair
       WRITE !,$WEXTRACT(var2,6,12)
       WRITE !,$WEXTRACT(var2,\*\-7,\*\-1)
      }
      ELSE {WRITE "This example requires a Unicode installation of Caché"}
    
     
    
    If the from and to positions are the same,$WEXTRACT returns a single character. If the to position is closer to the beginning of the string than the from position, $WEXTRACT returns the null string.
    

Replacing a Substring

You can use $WEXTRACT with the SET command to replace a specified character or range of characters with another value. You can also use it to append characters to the end of a string. SET $WEXTRACT counts a surrogate pair as a single character.

When $WEXTRACT is used with SET on the left hand side of the equals sign, string can be a valid variable name. If the variable does not exist, SET $WEXTRACT defines it. The string parameter can also be a [multidimensional property](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ROBJ_property_multidimensional) reference; it cannot be a non-multidimensional object property. Attempting to use SET $WEXTRACT on a non-multidimensional object property results in an <OBJECT DISPATCH> error.

You cannot use SET (a,b,c,...)=value syntax with $WEXTRACT (or $EXTRACT, $PIECE, or $LIST) on the left of the equals sign, if the function uses relative offset syntax: \* representing the end of a string and \*-n or \*+n representing relative offset from the end of the string. You must instead use SET a=value,b=value,c=value,... syntax.

For further details on replacing a substring, refer to the [$EXTRACT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fextract) function.

Examples

The following example shows the two-parameter form of $WEXTRACT returning the Unicode value for a surrogate pair:

  IF $SYSTEM.Version.IsUnicode()  {
    SET hipart\=$CHAR($ZHEX("D806"))
    SET lopart\=$CHAR($ZHEX("DC06"))
    SET spair\=hipart\_lopart /\* surrogate pair \*/
    SET x\="ABC"\_spair\_"DEFGHIJK"
    WRITE !,"$EXTRACT character "
    ZZDUMP $EXTRACT(x,4)
    WRITE !,"$WEXTRACT character "
    ZZDUMP $WEXTRACT(x,4)
  }
  ELSE {WRITE "This example requires a Unicode installation of Caché"}

 

The following example shows the three-parameter form of $WEXTRACT including a surrogate pair in a substring range:

  IF $SYSTEM.Version.IsUnicode()  {
  SET hipart\=$CHAR($ZHEX("D806"))
  SET lopart\=$CHAR($ZHEX("DC06"))
  SET spair\=hipart\_lopart /\* surrogate pair \*/
  SET x\="ABC"\_spair\_"DEFGHIJK"
   WRITE !,"$EXTRACT two characters "
   ZZDUMP $EXTRACT(x,3,4)
   WRITE !,"$WEXTRACT two characters "
   ZZDUMP $WEXTRACT(x,3,4)
  }
  ELSE {WRITE "This example requires a Unicode installation of Caché"}

 

See Also

*   [$EXTRACT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fextract) function
    
*   [$WASCII](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwascii) function
    
*   [$WCHAR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwchar) function
    
*   [$WFIND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwfind) function
    
*   [$WISWIDE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwiswide) function
    
*   [$WLENGTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwlength) function
    
*   [$WREVERSE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwreverse) function
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwchar "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwfind "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fwextract.xml**