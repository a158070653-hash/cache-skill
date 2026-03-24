# $NCONVERT

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fnconvert

---

 

Caché ObjectScript Reference

$NCONVERT

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fname "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fnext "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$NCONVERT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fnconvert "$NCONVERT") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Converts a number to a binary value encoded in a string of 8-bit characters.

Synopsis

$NCONVERT(n,format,endian)
$NC(n,format,endian)

Parameters

n

Any number, which can be specified as a value, a variable, or an expression. Additional limitations on valid values are imposed by the format selected.

format

One of the following format codes, specified as a quoted string: S1, S2, S4, S8, U1, U2, U4, F4, or F8.

endian

Optional — A boolean value, where 0 = little-endian and 1 = big-endian. The default is 0.

Description

$NCONVERT uses the specified format to convert the number n to an encoded string of 8-bit characters. The values of these characters are in the range $CHAR(0) through $CHAR(255).

The following are the supported format codes:

S1

Signed integer encoded into a string of one 8-bit byte. The value must be in the range -128 through 127, inclusive.

S2

Signed integer encoded into a string of two 8-bit bytes. The value must be in the range -32768 through 32767, inclusive.

S4

Signed integer encoded into a string of four 8-bit bytes. The value must be in the range -2147483648 through 2147483647, inclusive.

S8

Signed integer encoded into a string of eight 8-bit bytes. The value must be in the range -9223372036854775808 through 9223372036854775807, inclusive.

U1

Unsigned integer encoded into a string of one 8-bit byte. The maximum value is 255.

U2

Unsigned integer encoded into a string of two 8-bit bytes. The maximum value is 65535.

U4

Unsigned integer encoded into a string of four 8-bit bytes. The maximum value is 4294967295.

F4

IEEE floating point number encoded into a string of four 8-bit bytes.

F8

IEEE floating point number encoded into a string of eight 8-bit bytes.

Values beyond the range of format limits result in a <VALUE OUT OF RANGE> error. Specifying a negative number for an Unsigned format results in a <VALUE OUT OF RANGE> error. If n is a non-numeric value (contains any non-numeric characters) Caché performs [conversion of a string to a numeric value](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_nonnumasnum). A string beginning with a non-numeric character is converted to 0.

Caché rounds a fractional number to an integer value for all formats except F4 and F8.

You can use the [IsBigEndian()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Version#IsBigEndian) class method to determine which bit ordering is used on your operating system platform: 1=big-endian bit order; 0=little-endian bit order.

  WRITE $SYSTEM.Version.IsBigEndian()

 

$SCONVERT provides the inverse of the $NCONVERT operation.

Examples

The following example converts a series of unsigned numbers to two-byte encoded values:

  FOR x\=250:1:260 {
    ZZDUMP $NCONVERT(x,"U2") }
  QUIT

 

The following example performs the same operation in big-endian order:

  FOR x\=250:1:260 {
    ZZDUMP $NCONVERT(x,"U2",1) }
  QUIT

 

See Also

*   [$SCONVERT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fsconvert) function
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fname "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fnext "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fnconvert.xml**