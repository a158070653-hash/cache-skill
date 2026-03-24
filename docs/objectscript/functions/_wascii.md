# $WASCII

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fwascii

---

 

Caché ObjectScript Reference

$WASCII

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fview "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwchar "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$WASCII](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwascii "$WASCII") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Returns the numeric code corresponding to a character, recognizing surrogate pairs.

Synopsis

$WASCII(expression,position)
$WA(expression,position)

Parameters

expression

The character to be converted.

position

Optional — The position of a character within a character string, counting from 1. The default is 1.

Description

$WASCII returns the character code value for a single character specified in expression. $WASCII recognizes a surrogate pair as a single character. The returned value is a positive integer.

The expression parameter may evaluate to a single character or to a string of characters. If expression evaluates to a string of characters, you can include the optional position parameter to indicate which character you want to convert. The position counts a surrogate pair as a single character. You can use the [$WISWIDE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwiswide) function to determine if a string contains a surrogate pair.

A surrogate pair is a pair of 16-bit Unicode characters that together encode a single ideographic character. Surrogate pairs are used to represent certain ideographs which are used in Chinese, Japanese kanji, and Korean hanja. (Most commonly-used Chinese, kanji, and hanja characters are represented by standard 16-bit Unicode encodings.) Surrogate pairs provide Caché support for the Japanese JIS X0213:2004 (JIS2004) encoding standard and the Chinese GB18030 encoding standard.

A surrogate pair consists of high-order Unicode character in the hexadecimal range D800 through DBFF, and a low-order Unicode character in the hexadecimal range DC00 through DFFF.

The $WASCII function recognizes a surrogate pair as a single character. The $ASCII function treats a surrogate pair as two characters. In all other aspects, $WASCII and $ASCII are functionally identical. However, because $ASCII is generally faster than $WASCII, $ASCII is preferable for all cases where a surrogate pair is not likely to be encountered. For further details on character to numeric code conversion, refer to the [$ASCII](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fascii) function.

Examples

The following example shows $WASCII returning the Unicode value for a surrogate pair:

  IF $SYSTEM.Version.IsUnicode()  {
  SET hipart\=$CHAR($ZHEX("D806"))
  SET lopart\=$CHAR($ZHEX("DC06"))
  WRITE !,$ASCII(hipart)," = high-order value"
  WRITE !,$ASCII(lopart)," = low-order value"
  SET spair\=hipart\_lopart /\* surrogate pair \*/
  SET xpair\=hipart\_hipart /\* NOT a surrogate pair \*/
  WRITE !,$WASCII(spair)," = surrogate pair value"
  WRITE !,$WASCII(xpair)," = Not a surrogate pair"
  }
  ELSE {WRITE "This example requires a Unicode installation of Caché"}

 

The following example compares $WASCII and $ASCII return values for a surrogate pair:

  IF $SYSTEM.Version.IsUnicode()  {
  SET hipart\=$CHAR($ZHEX("D806"))
  SET lopart\=$CHAR($ZHEX("DC06"))
  WRITE !,$ASCII(hipart)," = high-order value"
  WRITE !,$ASCII(lopart)," = low-order value"
  SET spair\=hipart\_lopart /\* surrogate pair \*/
  WRITE !,$ASCII(spair)," = $ASCII value for surrogate pair"
  WRITE !,$WASCII(spair)," = $WASCII value for surrogate pair"
  }
  ELSE {WRITE "This example requires a Unicode installation of Caché"}

 

The following example shows the effects on position counting of surrogate pairs. It returns both the $WASCII and $ASCII values for each position. $WASCII counts a surrogate pair as one position; $ASCII counts a surrogate pair as two positions:

  IF $SYSTEM.Version.IsUnicode()  {
  SET hipart\=$CHAR($ZHEX("D806"))
  SET lopart\=$CHAR($ZHEX("DC06"))
  WRITE !,$ASCII(hipart)," = high-order value"
  WRITE !,$ASCII(lopart)," = low-order value",!
  SET str\="AB"\_lopart\_hipart\_lopart\_"CD"\_hipart\_lopart\_"EF"
  FOR x\=1:1:11 {
  WRITE !,"position ",x," $WASCII ",$WASCII(str,x)," $ASCII ",$ASCII(str,x) }
  }
  ELSE {WRITE "This example requires a Unicode installation of Caché"}

 

See Also

*   [$ASCII](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fascii) function
    
*   [$WCHAR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwchar) function
    
*   [$WEXTRACT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwextract) function
    
*   [$WFIND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwfind) function
    
*   [$WISWIDE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwiswide) function
    
*   [$WLENGTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwlength) function
    
*   [$WREVERSE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwreverse) function
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fview "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwchar "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fwascii.xml**