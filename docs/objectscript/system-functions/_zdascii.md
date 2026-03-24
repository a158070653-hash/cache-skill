# $ZDASCII

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fzdascii

---

 

Caché ObjectScript Reference

$ZDASCII

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzcyc "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdchar "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [System and Other Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_SYSTEMFUNCTIONS "System and Other Functions") \]  >  \[ [$ZDASCII](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdascii "$ZDASCII") \]

Go to: Description Example Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Converts an eight-byte string to a $DOUBLE floating point number.

Synopsis

$ZDASCII(string,position)
$ZDA(string,position)

Parameters

string

A string. It can be a value, a variable, or an expression. It must be a minimum of eight bytes in length.

position

Optional — A starting position in the string, expressed as a positive integer. The default is 1. Position is counted in single bytes, not eight-byte strings. The position cannot be the last byte in the string, or beyond the end of the string. A numeric position value is parsed as an integer by truncating decimal digits, removing leading zeros and plus signs, etc.

Description

The value that $ZDASCII returns depends on the parameters you use.

*   $ZDASCII(string) returns a $DOUBLE (IEEE floating point) numeric interpretation of an eight-byte string starting at the first character position of string.
    
*   $ZDASCII(string,position) returns a $DOUBLE (IEEE floating point) numeric interpretation of an eight-byte string beginning at the starting byte position specified by position.
    

$ZDASCII can return either a positive or a negative number.

$ZDASCII issues a <FUNCTION> error if string is of an invalid length, or position is an invalid value.

Example

The following example determines the numeric interpretation of the character string "abcdefgh":

  WRITE $ZDASCII("12345678")

It returns: .0000000000000000000000000000000000000682132005170133

The following examples also return the same value:

  WRITE !,$ZDASCII("12345678",1)
  WRITE !,$ZDASCII("12345678xx",1)
  WRITE !,$ZDASCII("xx12345678xx",3)

Notes

$ZDASCII and Other $ASCII Functions

$ZDASCII converts a eight byte (64-bit) character string to an IEEE floating point number. $ZQASCII converts a eight byte (64-bit) character string to an integer. To convert an 8-bit byte string to an integer use $ASCII. To convert a 16-bit (wide) character string to an integer use $ZWASCII. To convert a 32-bit (long) character string to an integer use $ZLASCII.

See Also

*   [$ZDCHAR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdchar) function
    
*   [$ASCII](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fascii) function
    
*   [$ZLCHAR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzlchar) function
    
*   [$ZWASCII](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzwascii) function
    
*   [$ZQASCII](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzqascii) function
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzcyc "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdchar "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fzdascii.xml**