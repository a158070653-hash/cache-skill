# $ZCRC

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fzcrc

---

 

Caché ObjectScript Reference

$ZCRC

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzconvert "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzcyc "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [System and Other Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_SYSTEMFUNCTIONS "System and Other Functions") \]  >  \[ [$ZCRC](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzcrc "$ZCRC") \]

Go to: Description Parameters Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Checksum function.

Synopsis

$ZCRC(string,mode,expression)

Parameters

string

A string on which a checksum operation is performed.

mode

An integer code specifying the checksum mode to use.

expression

Optional — The initial "seed" value, specified as an integer. If omitted, defaults to zero (0).

Description

$ZCRC performs a cyclic redundancy check on string and returns an integer checksum value. The value returned by $ZCRC depends on the parameters you use.

*   $ZCRC(string,mode) computes and returns a checksum on string. The value of mode determines the type of checksum $ZCRC computes.
    
*   $ZCRC(string,mode,expression) computes and returns a checksum on string using the mode specified by mode. expression supplies an initial "seed" value when checking multiple strings. It allows you to run $ZCRC calculations sequentially on multiple strings and obtain the same checksum values as if you had concatenated those strings and then run $ZCRC on the resulting string.
    

Parameters

string

A byte string. Can be specified as a value, a variable, or an expression. Only use a byte string or you will receive a <FUNCTION> error.

mode

The checksum algorithm to use. All checksum modes can be used with 8-bit (ASCII) or 16-bit Unicode (wide) characters. Legal values for mode are:

Mode

Computes

0

An 8-bit byte sum. Simply sums the ASCII values of the characters in the string. Thus $ZCRC(2,0)=50, $ZCRC(22,0)=100, $ZCRC(23,0)=101, and $ZCRC(32,0)=101.

1

An 8-bit XOR of the bytes

2

A 16-bit DataTree CRC-CCITT

3

A 16-bit DataTree CRC-16

4

A 16-bit CRC for XMODEM protocols

5

A correct 16-bit CRC-CCITT

6

A correct 16-bit CRC-16

7

A correct 32-bit CRC-32. This corresponds to the cksum utility algorithm 3 on OS X, and the CRC32 class in the Java utilities package.

Caché in [MSM language mode](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_mcompat#GCOS_mc_msm) supports mode values 0 and 1. All other mode values result in a <FUNCT> error.

expression

An argument that is an initial "seed" value. $ZCRC adds expression to the checksum generated for string. This allows you to run $ZCRC calculations on multiple strings sequentially and obtain the save checksum value as if you had concatenated those strings and run $ZCRC on the resulting string.

Examples

The following example uses mode\=0 on strings containing the letters A, B, and C and in each case returns the checksum 198:

  WRITE $ZCRC("ABC",0),!
  WRITE $ZCRC("CAB",0),!
  WRITE $ZCRC("BCA",0),!

 

The checksum is derived as follows:

  WRITE $ASCII("A")+$ASCII("B")+$ASCII("C")  /\* 65+66+67 = 198 \*/

 

The following example shows the values returned by each mode for the string “ABC”:

  FOR i\=0:1:7 {
     WRITE !,"mode ",i,"=",$ZCRC("ABC",i)
  }

 

See Also

*   [$ZCYC](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzcyc) function
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzconvert "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzcyc "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fzcrc.xml**