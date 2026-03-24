# $ZQCHAR

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fzqchar

---

 

Caché ObjectScript Reference

$ZQCHAR

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzqascii "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzsearch "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [System and Other Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_SYSTEMFUNCTIONS "System and Other Functions") \]  >  \[ [$ZQCHAR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzqchar "$ZQCHAR") \]

Go to: Description Example Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Converts a number to an eight-byte string.

Synopsis

$ZQCHAR(n)
$ZQC(n)

Parameter

n

An integer in the range -9223372036854775808 through 9223372036854775807. It can be specified as a value, a variable, or an expression.

Description

$ZQCHAR returns an eight-byte (quad) character string corresponding to the binary representation of n. The bytes of the character string are presented in little-endian byte order, with the least significant byte first.

$ZQCHAR issues a <FUNCTION> error if n is invalid.

Example

The following example returns the eight-byte string for the integer 7523094288207667809:

   WRITE $ZQCHAR(7523094288207667809)

 

returns: “abcdefgh”

Notes

$ZQCHAR and $CHAR

$ZQCHAR is similar to $CHAR except that it operates on eight byte (64-bit) words instead of single 8-bit bytes. For 16-bit words use $ZWCHAR; for 32-bit words, use $ZLCHAR.

$ZQCHAR and $ZQASCII

$ZQASCII is the logical inverse of the $ZQCHAR function. For example:

   WRITE $ZQCHAR(7523094288207667809)

 

returns: abcdefgh

   WRITE $ZQASCII("abcdefgh")

 

returns: 7523094288207667809

See Also

*   [$ZQASCII](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzqascii) function
    
*   [$CHAR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fchar) function
    
*   [$ZLCHAR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzlchar) function
    
*   [$ZWCHAR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzwchar) function
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzqascii "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzsearch "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fzqchar.xml**