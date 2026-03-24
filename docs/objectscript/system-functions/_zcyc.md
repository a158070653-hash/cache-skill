# $ZCYC

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fzcyc

---

 

Caché ObjectScript Reference

$ZCYC

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzcrc "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdascii "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [System and Other Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_SYSTEMFUNCTIONS "System and Other Functions") \]  >  \[ [$ZCYC](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzcyc "$ZCYC") \]

Go to: Description Parameter Example See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Cyclical-redundancy check for data integrity.

Synopsis

$ZCYC(string)
$ZC(string)

Parameter

string

A string.

Description

$ZCYC(string) computes and returns the cyclical-redundancy check value for the string. It allows two intercommunicating programs to check for data integrity.

The sending program transmits a piece of data along with a matching check value that it calculates using $ZCYC. The receiving program verifies the transmitted data by using $ZCYC to calculate its check value. If the two check values match, the received data is the same as the sent data.

$ZCYC calculates the check value by performing an exclusive OR (XOR) on the binary representations of all the characters in the string.

Use caution when transmitting data between 8-bit and Unicode (16-bit) implementations of Caché; if a data string does not contain any wide characters, the cyclical-redundancy check values should match.

Note that the $ZCYC value of an 8-bit string is identical to the $ZCRC mode 1 value.

Parameter

string

A string. Can be specified as a value, a variable, or an expression. String values are enclosed in quotation marks.

Example

In this example, the first $ZCYC returns 65; the second returns 3; and the third returns 64.

   SET x\= $ZCYC("A") 
      ; 1000001 (only one character; no XOR )
   SET y\= $ZCYC("AB") 
      ; 1000001 XOR 1000010 -> 0000011
   SET z\= $ZCYC("ABC") 
      ; 1000001 XOR 1000010 -> 0000011 | 1000011 -> 100000
   WRITE !,"x=",x," y=",y," z=",z

 

See Also

*   [$ZCRC](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzcrc) function
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzcrc "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdascii "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fzcyc.xml**