# $ZHEX

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fzhex

---

 

Caché ObjectScript Reference

$ZHEX

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzexp "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzln "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Additional Math and Time Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_MATHFUNCTIONS "Additional Math and Time Functions") \]  >  \[ [$ZHEX](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzhex "$ZHEX") \]

Go to: Description Parameter Examples Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Converts a hexadecimal string to a decimal number and vice versa.

Synopsis

$ZHEX(num)
$ZH(num)

Parameter

num

An expression that evaluates to a numeric value be converted, either a quoted string or an integer (signed or unsigned).

Description

$ZHEX converts a hexadecimal string to a decimal integer, or a decimal integer to a hexadecimal string.

If num is a string value, $ZHEX interprets it as the hexadecimal representation of a number, and returns that number in decimal. Be sure to place the string value within quotation marks.

If num is a numeric value, $ZHEX converts it to a string representation of the number in hexadecimal format. If either the initial or the final numeric value cannot be represented as an 8-byte signed integer, $ZHEX issues a <FUNCTION> error.

You can perform the same hexadecimal/decimal conversions using the [HexToDecimal()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Util#HexToDecimal) and [DecimalToHex()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Util#DecimalToHex) methods of the [%SYSTEM.Util](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Util) class:

  WRITE $SYSTEM.Util.DecimalToHex("27")

 

  WRITE $SYSTEM.Util.HexToDecimal("27"),!
  WRITE $SYSTEM.Util.HexToDecimal("1B")

 

Parameter

num

A string value or a numeric value, a variable that contains a string value or a numeric value, or an expression that evaluates to a string value or a numeric value.

A string value is read as a hexadecimal number and converted to a positive decimal integer. $ZHEX recognizes both uppercase and lowercase letters “A” through “F” as hexadecimal digits. It truncates leading zeros. It does not recognize plus and minus signs or decimal points. It stops evaluation of a string when it encounters a non-hexadecimal character. Therefore, the strings “F”, “f”, “00000F”, “F.7”, and “FRED” all evaluate to decimal 15. If the first character encountered in a string is not a hexadecimal character, $ZHEX evaluates the string as zero. Therefore, the strings “0”, “0.9”, “+F”, “-F”, and “H” all evaluate to zero. The null string ("") is an invalid value and issues a <FUNCTION> error.

An integer value is read as a decimal number and converted to hexadecimal. An integer can be positive or negative. $ZHEX recognizes leading plus and minus signs. It truncates leading zeros. It evaluates nested arithmetic operations. However, it does not recognize decimal points. It issues a <FUNCTION> error if it encounters a decimal point character. Therefore, the integers 217, 0000217, +217, -+-217 all evaluate to hexadecimal D9. -217, -0000217, and -+217 all evaluate to FFFFFFFFFFFFFF27 (the twos complement). Other values, such as floating point numbers, trailing signs, and nonnumeric characters result in a <FUNCTION> or <SYNTAX> error.

Examples

   WRITE $ZHEX("F")

 

returns 15.

   WRITE $ZHEX(15)

 

returns F.

   WRITE $ZHEX("1AB8")

 

returns 6840.

   WRITE $ZHEX(6840)

 

returns 1AB8.

   WRITE $ZHEX("XXX")

 

returns 0.

   WRITE $ZHEX(\-1)

 

returns FFFFFFFFFFFFFFFF.

   WRITE $ZHEX((3+(107\*2)))

 

returns D9.

Notes

Forcing a Hexadecimal Interpretation

To force an integer value to be interpreted as hexadecimal, concatenate any non-hexadecimal character to the end of your num parameter. For example:

   WRITE $ZHEX(16\_"H")

 

returns 22.

See Also

*   [ZZDUMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czzdump) command
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzexp "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzln "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fzhex.xml**