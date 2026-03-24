# $ZISWIDE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fziswide

---

 

Caché ObjectScript Reference

$ZISWIDE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzf-6 "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzlascii "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [System and Other Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_SYSTEMFUNCTIONS "System and Other Functions") \]  >  \[ [$ZISWIDE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fziswide "$ZISWIDE") \]

Go to: Description Example See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Checks whether a string contains any 16-bit wide characters.

Synopsis

$ZISWIDE(string)

Parameter

string

A string of one or more characters, enclosed in quotation marks.

Description

$ZISWIDE is a boolean function used to check whether a string contains any 16-bit wide character values. It returns one of the following values:

0

All characters have ASCII values 255 or less (8-bit characters). A null string ("") also returns 0.

1

One or more characters have an ASCII value greater than 255 (wide characters).

Note that in a Unicode version of Caché, all characters are 16 bits in length. $ZISWIDE checks the character values to determine if they are in the ASCII range (0-255), and thus could be represented by 8 bits, or in the wide character range (256-65535) and thus use all 16 bits of the Unicode character.

Example

In the following example, the first two commands test strings that contain all narrow (8-bit) character values and return 0. The third command tests a string containing a wide character value (the second character), and therefore, returns 1:

   WRITE $ZISWIDE("abcd"),","
   WRITE $ZISWIDE($CHAR(71,83,77)),","
   WRITE $ZISWIDE($CHAR(71,300,77))

 

Note that this example returns 0,0,1 only on Caché instances that were installed with Unicode support. Caché installed with 8-bit support returns 0,0,0.

See Also

*   [$ZPOSITION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzposition) function
    
*   [$ZWASCII](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzwascii) function
    
*   [$ZWCHAR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzwchar) function
    
*   [$ZWIDTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzwidth) function
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzf-6 "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzlascii "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fziswide.xml**