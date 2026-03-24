# $WREVERSE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fwreverse

---

 

Caché ObjectScript Reference

$WREVERSE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwlength "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fxecute "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$WREVERSE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwreverse "$WREVERSE") \]

Go to: Description Example See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Returns the characters in a string in reverse order, recognizing surrogate pairs.

Synopsis

$WREVERSE(string)
$WRE(string)

Parameter

string

A string or expression that evaluates to a string.

Description

$WREVERSE returns the characters in string in reverse order. $WREVERSE is functionally identical to $REVERSE, except that $WREVERSE recognizes surrogate pairs.

A surrogate pair is a pair of 16-bit Unicode characters that together encode a single ideographic character. Surrogate pairs are used to represent certain ideographs which are used in Chinese, Japanese kanji, and Korean hanja. (Most commonly-used Chinese, kanji, and hanja characters are represented by standard 16-bit Unicode encodings.) Surrogate pairs provide Caché support for the Japanese JIS X0213:2004 (JIS2004) encoding standard and the Chinese GB18030 encoding standard.

A surrogate pair consists of high-order Unicode character in the hexadecimal range D800 through DBFF, and a low-order Unicode character in the hexadecimal range DC00 through DFFF. You can use the [$WISWIDE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwiswide) function to determine if a string contains a surrogate pair.

The $WREVERSE function counts a surrogate pair as a single character. The $REVERSE function treats a surrogate pair as two characters. In all other aspects, $WREVERSE and $REVERSE are functionally identical. However, because $REVERSE is generally faster than $WREVERSE, $REVERSE is preferable for all cases where a surrogate pair is not likely to be encountered.

For further details on reversing strings, refer to the [$REVERSE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_freverse) function.

Example

The following example shows how $WREVERSE treats a surrogate pair as a single character:

  IF $SYSTEM.Version.IsUnicode()  {
  SET spair\=$CHAR($ZHEX("D806"),$ZHEX("DC06"))
  SET str\="AB"\_spair\_"CD"
    WRITE !,"String before reversing:"
    ZZDUMP str
  SET wrev\=$WREVERSE(str)
    WRITE !,"$WREVERSE did not reverse surrogate pair:"
    ZZDUMP wrev
  SET rev\=$REVERSE(str)
    WRITE !,"$REVERSE reversed surrogate pair:"
    ZZDUMP rev
  }
  ELSE {WRITE "This example requires a Unicode installation of Caché"}

 

See Also

*   [$REVERSE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_freverse) function
    
*   [$WASCII](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwascii) function
    
*   [$WCHAR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwchar) function
    
*   [$WEXTRACT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwextract) function
    
*   [$WFIND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwfind) function
    
*   [$WISWIDE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwiswide) function
    
*   [$WLENGTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwlength) function
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwlength "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fxecute "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fwreverse.xml**