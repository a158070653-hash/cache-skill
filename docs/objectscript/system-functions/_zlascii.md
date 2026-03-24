# $ZLASCII

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fzlascii

---

 

Caché ObjectScript Reference

$ZLASCII

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fziswide "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzlchar "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [System and Other Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_SYSTEMFUNCTIONS "System and Other Functions") \]  >  \[ [$ZLASCII](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzlascii "$ZLASCII") \]

Go to: Description Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Converts a four-byte string to a number.

Synopsis

$ZLASCII(string,position)
$ZLA(string,position)

Parameters

string

A string that can be specified as a value, a variable, or an expression. It must be a minimum of four bytes in length.

position

Optional — A starting position in the string. The default is 1.

Description

The value $ZLASCII returns depends on the parameters you use.

*   $ZLASCII(string) returns a numeric interpretation of a four-byte string, starting with the first character position of string.
    
*   $ZLASCII(string,position) returns a numeric interpretation of a four-byte string beginning at the starting position specified by position.
    

Upon successful completion, $ZLASCII always returns a positive integer. $ZLASCII returns -1 if string is of an invalid length, or position is an invalid value.

Notes

$ZLASCII and $ASCII

$ZLASCII is similar to $ASCII except that it operates on four byte (32-bit) words instead of single 8-bit bytes. For two byte (16-bit) words use $ZWASCII; for eight byte (64-bit) words, use $ZQASCII.

$ZLASCII(string,position) is the functional equivalent of:

$ASCII(string,position+3)\*256 + $ASCII(string,position+2)\*256 + $ASCII(string,position+1)\*256 + $ASCII(string,position)

$ZLASCII and $ZLCHAR

The $ZLCHAR function is the logical inverse of the $ZLASCII function. For example:

   SET x\=$ZLASCII("abcd")
   WRITE !,x
   SET y\=$ZLCHAR(x)
   WRITE !,y

 

Given “abcd” $ZLASCII returns 1684234849. Given 1684234849 $ZLCHAR returns “abcd”.

See Also

*   [$ASCII](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fascii) function
    
*   [$ZLCHAR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzlchar) function
    
*   [$ZWASCII](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzwascii) function
    
*   [$ZQASCII](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzqascii) function
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fziswide "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzlchar "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fzlascii.xml**