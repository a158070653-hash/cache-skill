# $SCONVERT

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fsconvert

---

 

Caché ObjectScript Reference

$SCONVERT

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_freverse "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fselect "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$SCONVERT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fsconvert "$SCONVERT") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Converts a binary encoded value to a number.

Synopsis

$SCONVERT(s,format,endian,position)
$SC(s,format,endian,position)

Parameters

s

A string of 8-bit bytes which encode for a number. Limitations on valid values are imposed by the format selected.

format

One of the following format codes, specified as a quoted string: S1, S2, S4, S8, U1, U2, U4, F4, or F8.

endian

Optional — A boolean value, where 0 = little-endian and 1 = big-endian. The default is 0.

position

Optional — The character position in the string of 8-bit bytes at which to begin conversion. Character positions are counted from 1. The default value is 1. If you specify position, you must either specify endian or a placeholder comma.

Description

$SCONVERT converts s from an encoded string of 8-bit bytes to a numeric value, using the specified format.

The following are the supported format codes:

S1

Signed integer encoded into a string of one 8-bit bytes. The value must be in the range -128 through 127, inclusive.

S2

Signed integer encoded into a string of two 8-bit bytes. The value must be in the range -32768 through 32767, inclusive.

S4

Signed integer encoded into a string of four 8-bit bytes. The value must be in the range -2147483648 through 2147483647, inclusive.

S8

Signed integer encoded into a string of eight 8-bit bytes. The value must be in the range -9223372036854775808 through 9223372036854775807, inclusive.

U1

Unsigned integer encoded into a string of one 8-bit bytes. The maximum value is 256.

U2

Unsigned integer encoded into a string of two 8-bit bytes. The maximum value is 65535.

U4

Unsigned integer encoded into a string of four 8-bit bytes. The maximum value is 4294967295.

F4

IEEE floating point number encoded into a string of four 8-bit bytes.

F8

IEEE floating point number encoded into a string of eight 8-bit bytes.

String s must contain sufficient characters starting at and following the specified character position to satisfy the number of 8-bit bytes required by the format code. For example, $SCONVERT(s,"S4",0,9) requires that the length of s be at least 12 characters because the decoded result comes from the character positions 9, 10, 11 and 12. Values beyond this range result in a <VALUE OUT OF RANGE> error.

$SCONVERT is intended only for use on 8-bit byte strings. If $SCONVERT is on a Unicode instance of Caché and if any of the characters in the encoded string are in the range $CHAR(256) through $CHAR(65536) the return value is unpredictable.

If argument s is a numeric value, it is converted to a string containing its [canonical numeric form](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_numcanonical) before it is decoded.

You can use the [IsBigEndian()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Version#IsBigEndian) class method to determine which bit ordering is used on your operating system platform: 1=big-endian bit order; 0=little-endian bit order.

  WRITE $SYSTEM.Version.IsBigEndian()

 

$SCONVERT provides the inverse of the $NCONVERT operation.

Examples

In the following example, $SCONVERT converts a two-byte binary encoded value to a number:

  SET x\=$NCONVERT(258,"U2")
  ZZDUMP x 
  SET y\=$SCONVERT(x,"U2")
  WRITE !,y

 

The following example, $SCONVERT converts a two-byte binary encoded value in big-endian order to a number:

  SET x\=$NCONVERT(258,"U2",1)
  ZZDUMP x 
  SET y\=$SCONVERT(x,"U2",1)
  WRITE !,y

 

See Also

*   [$NCONVERT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fnconvert) function
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_freverse "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fselect "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fsconvert.xml**