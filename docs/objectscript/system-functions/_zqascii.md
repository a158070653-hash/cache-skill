# $ZQASCII

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fzqascii

---

 

Caché ObjectScript Reference

$ZQASCII

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzposition "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzqchar "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [System and Other Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_SYSTEMFUNCTIONS "System and Other Functions") \]  >  \[ [$ZQASCII](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzqascii "$ZQASCII") \]

Go to: Description Example Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Converts an eight-byte string to a number.

Synopsis

$ZQASCII(string,position)
$ZQA(string,position)

Parameters

string

A string. It can be a value, a variable, or an expression. It must be a minimum of eight bytes in length.

position

Optional — A starting position in the string, expressed as a positive integer. The default is 1. Position is counted in single bytes, not eight-byte strings. The position cannot be the last byte in the string, or beyond the end of the string. A numeric position value is parsed as an integer by truncating decimal digits, removing leading zeros and plus signs, etc.

Description

The value that $ZQASCII returns depends on the parameters you use.

*   $ZQASCII(string) returns a numeric interpretation of an eight-byte string starting at the first character position of string.
    
*   $ZQASCII(string,position) returns a numeric interpretation of an eight-byte string beginning at the starting byte position specified by position.
    

$ZQASCII can return either a positive or a negative integer.

$ZQASCII issues a <FUNCTION> error if string is of an invalid length, or position is an invalid value.

Example

The following example determines the numeric interpretation of the character string "abcdefgh":

  WRITE $ZQASCII("abcdefgh")

 

It returns 7523094288207667809.

The following examples also return 7523094288207667809:

  WRITE !,$ZQASCII("abcdefgh",1)
  WRITE !,$ZQASCII("abcdefghxx",1)
  WRITE !,$ZQASCII("xxabcdefghxx",3)

 

Notes

$ZQASCII and $ASCII

$ZQASCII is similar to $ASCII except that it operates on eight byte (64-bit) words instead of single 8-bit bytes. For 16-bit words use $ZWASCII; for 32-bit words, use $ZLASCII.

$ZQASCII and $ZQCHAR

The $ZQCHAR function is the logical inverse of $ZQASCII. For example:

   WRITE $ZQASCII("abcdefgh")

 

returns: 7523094288207667809.

   WRITE $ZQCHAR(7523094288207667809)

 

returns “abcdefgh”.

See Also

*   [$ASCII](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fascii) function
    
*   [$ZQCHAR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzqchar) function
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzposition "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzqchar "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fzqascii.xml**