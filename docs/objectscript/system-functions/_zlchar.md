# $ZLCHAR

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fzlchar

---

 

Caché ObjectScript Reference

$ZLCHAR

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzlascii "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzname "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [System and Other Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_SYSTEMFUNCTIONS "System and Other Functions") \]  >  \[ [$ZLCHAR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzlchar "$ZLCHAR") \]

Go to: Description Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Converts a number to a four-byte string.

Synopsis

$ZLCHAR(n)
$ZLC(n)

Parameter

n

A positive integer in the range 0 through 4294967295. It can be specified as a value, a variable, or an expression.

Description

$ZLCHAR returns a four-byte (long) character string for n. The bytes of the character string are presented in little-endian byte order, with the least significant byte first.

If n is out of range or a negative number, $ZLCHAR returns the null string.

Notes

$ZLASCII and $ZLCHAR

The $ZLASCII function is the logical inverse of $ZLCHAR. For example:

   SET x\=$ZLASCII("abcd")
   WRITE !,x
   SET y\=$ZLCHAR(x)
   WRITE !,y

 

Given “abcd” $ZLASCII returns 1684234849. Given 1684234849 $ZLCHAR returns “abcd”.

$ZLCHAR and $CHAR

$ZLCHAR is similar to $CHAR, except that it operates on four byte (32-bit) words instead of single 8-bit bytes. For two byte (16-bit) words use $ZWASCII; for eight byte (64-bit) words, use $ZQASCII.

$ZLCHAR is the functional equivalent of the following form of $CHAR:

   SET n\=$ZLASCII("abcd")
   WRITE !,n
   WRITE !,$CHAR(n#256,n\\256#256,n\\(256\*\*2)#256,n\\(256\*\*3))

 

Given “abcd” $ZLASCII returns 1684234849. Given 1684234849, this $CHAR statement returns “abcd”.

See Also

*   [$ZLASCII](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzlascii) function
    
*   [$CHAR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fchar) function
    
*   [$ZWCHAR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzwchar) function
    
*   [$ZQCHAR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzqchar) function
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzlascii "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzname "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fzlchar.xml**