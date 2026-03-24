# $ZWCHAR

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fzwchar

---

 

Caché ObjectScript Reference

$ZWCHAR

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzwascii "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzwidth "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [System and Other Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_SYSTEMFUNCTIONS "System and Other Functions") \]  >  \[ [$ZWCHAR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzwchar "$ZWCHAR") \]

Go to: Description Example Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Converts a number to a two-byte string.

Synopsis

$ZWCHAR(n)
$ZWC(n)

Parameter

n

A positive integer in the range 0 through 65535. It can be specified as a value, a variable, or an expression.

Description

$ZWCHAR returns a two-byte (wide) character string corresponding to the binary representation of n. The bytes of the character string are presented in little-endian byte order, with the least significant byte first. It is the functional equivalent of:

   WRITE $CHAR(n#256,n\\256)

If n is out of range or a negative number, $ZWCHAR returns the null string.

Example

The following example returns the two-byte string for the integer 25185:

   WRITE $ZWCHAR(25185)

 

returns: ab

Notes

$ZWCHAR and $CHAR

$ZWCHAR is similar to $CHAR except that it operates on two byte (16-bit) words instead of single 8-bit bytes. For four byte (32-bit) words, use $ZLCHAR; For eight byte (64-bit) words, use $ZQCHAR.

$ZWCHAR and $ZWASCII

$ZWASCII is the logical inverse of the $ZWCHAR function. For example:

   WRITE $ZWCHAR(25185)

 

returns: ab

   WRITE $ZWASCII("ab")

 

returns: 25185

See Also

*   [$ZWASCII](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzwascii) function
    
*   [$CHAR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fchar) function
    
*   [$ZLCHAR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzlchar) function
    
*   [$ZQCHAR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzqchar) function
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzwascii "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzwidth "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fzwchar.xml**