# $WFIND

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fwfind

---

 

Caché ObjectScript Reference

$WFIND

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwextract "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwiswide "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$WFIND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwfind "$WFIND") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Finds a substring by value and returns an integer specifying its end position in the string, recognizing surrogate pairs.

Synopsis

$WFIND(string,substring,position)
$WF(string,substring,position)

Parameters

string

The target string that is to be searched. It can be a variable name, a numeric value, a string literal, or any valid Caché ObjectScript expression that resolves to a string.

substring

The substring that is to be searched for. It can be a variable name, a numeric value, a string literal, or any valid Caché ObjectScript expression that resolves to a string.

position

Optional — A position within the target string at which to start the search. It must be a positive integer.

Description

$WFIND returns an integer specifying the end position of a substring within a string. In calculating position, it counts each surrogate pair as a single character. $WFIND is functionally identical to $FIND, except that $WFIND recognizes surrogate pairs. It counts a surrogate pair as a single character.

A surrogate pair is a pair of 16-bit Unicode characters that together encode a single ideographic character. Surrogate pairs are used to represent certain ideographs which are used in Chinese, Japanese kanji, and Korean hanja. (Most commonly-used Chinese, kanji, and hanja characters are represented by standard 16-bit Unicode encodings.) Surrogate pairs provide Caché support for the Japanese JIS X0213:2004 (JIS2004) encoding standard and the Chinese GB18030 encoding standard.

A surrogate pair consists of high-order Unicode character in the hexadecimal range D800 through DBFF, and a low-order Unicode character in the hexadecimal range DC00 through DFFF. You can use the [$WISWIDE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwiswide) function to determine if a string contains a surrogate pair.

The $WFIND function counts a surrogate pair as a single character. The $FIND function counts a surrogate pair as two characters. In all other aspects, $WFIND and $FIND are functionally identical. However, because $FIND is generally faster than $WFIND, $FIND is preferable for all cases where a surrogate pair is not likely to be encountered.

For further details on finding a substring, refer to the [$FIND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ffind) function.

Examples

The following example shows how $WFIND counts a surrogate pair as a single character in the return value:

  IF $SYSTEM.Version.IsUnicode()  {
  SET spair\=$CHAR($ZHEX("D806"),$ZHEX("DC06"))
  SET str\="ABC"\_spair\_"DEF"
  WRITE !,$FIND(str,"DE")," $FIND location in string"
  WRITE !,$WFIND(str,"DE")," $WFIND location in string"
  }
  ELSE {WRITE "This example requires a Unicode installation of Caché"} 

 

The following example shows how $WFIND counts a surrogate pair as a single character in the position parameter:

  IF $SYSTEM.Version.IsUnicode()  {
  SET spair\=$CHAR($ZHEX("D806"),$ZHEX("DC06"))
  SET str\="ABC"\_spair\_"DEF"
  WRITE !,$FIND(str,"DE",6)," $FIND location in string"
  WRITE !,$WFIND(str,"DE",6)," $WFIND location in string"
  }
  ELSE {WRITE "This example requires a Unicode installation of Caché"} 

 

See Also

*   [$FIND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ffind) function
    
*   [$WASCII](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwascii) function
    
*   [$WCHAR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwchar) function
    
*   [$WEXTRACT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwextract) function
    
*   [$WISWIDE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwiswide) function
    
*   [$WLENGTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwlength) function
    
*   [$WREVERSE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwreverse) function
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwextract "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwiswide "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fwfind.xml**