# $ZWASCII

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fzwascii

---

 

Caché ObjectScript Reference

$ZWASCII

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzstrip "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzwchar "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [System and Other Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_SYSTEMFUNCTIONS "System and Other Functions") \]  >  \[ [$ZWASCII](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzwascii "$ZWASCII") \]

Go to: Description Example Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Converts a two-byte string to a number.

Synopsis

$ZWASCII(string,position)
$ZWA(string,position)

Parameters

string

A string. It can be a value, a variable, or an expression. It must be a minimum of two bytes in length.

position

Optional — A starting position in the string, expressed as a positive integer. The default is 1. Position is counted in single bytes, not two-byte strings. The position cannot be the last byte in the string, or beyond the end of the string. A numeric position value is parsed as an integer by truncating decimal digits, removing leading zeros and plus signs, etc.

Description

The value that $ZWASCII returns depends on the parameters you use.

*   $ZWASCII(string) returns a numeric interpretation of a two-byte string starting at the first character position of string.
    
*   $ZWASCII(string,position) returns a numeric interpretation of a two-byte string beginning at the starting byte position specified by position.
    

Upon successful completion, $ZWASCII always returns a positive integer. $ZWASCII returns -1 if string is of an invalid length, or position is an invalid value.

Example

The following example determines the numeric interpretation of the character string "ab":

  WRITE $ZWASCII("ab")

 

It returns 25185.

The following examples also return 25185:

  WRITE !,$ZWASCII("ab",1)
  WRITE !,$ZWASCII("abxx",1)
  WRITE !,$ZWASCII("xxabxx",3)

 

In the following examples, string or position are invalid. The $ZWASCII function returns –1 in each case:

  WRITE !,$ZWASCII("a")
  WRITE !,$ZWASCII("aba",3)
  WRITE !,$ZWASCII("ababab",99)
  WRITE !,$ZWASCII("ababab",0)
  WRITE !,$ZWASCII("ababab",\-1)

 

Notes

$ZWASCII and $ASCII

$ZWASCII is similar to $ASCII except that it operates on two byte (16-bit) words instead of single 8-bit bytes. For four byte (32-bit) words, use $ZLASCII; For eight byte (64-bit) words, use $ZQASCII.

$ZWASCII(string,position) is the functional equivalent of:

$ASCII(string,position+1)\*256+$ASCII(string,position)

$ZWASCII and $ZWCHAR

The $ZWCHAR function is the logical inverse of $ZWASCII. For example:

   WRITE $ZWASCII("ab")

 

returns: 25185.

   WRITE $ZWCHAR(25185)

 

returns “ab”.

See Also

*   [$ASCII](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fascii) function
    
*   [$ZWCHAR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzwchar) function
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzstrip "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzwchar "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fzwascii.xml**