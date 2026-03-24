# $DECIMAL

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fdecimal

---

 

Caché ObjectScript Reference

$DECIMAL

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fdata "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fdouble "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$DECIMAL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fdecimal "$DECIMAL") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Returns a number converted to a Caché floating point value.

Synopsis

$DECIMAL(num,digits)

Parameters

num

The numeric value to be converted. Commonly this is an IEEE floating point number.

digits

Optional — An integer that specifies the number of significant digits to return. $DECIMAL rounds the return value to that number of digits, using the IEEE floating point rounding algorithm. Valid values are 1 through 38, and 0. If digits is greater than the number of digits the value is returned unchanged. If digits is 0, no rounding is performed on num unless it has more than 20 significant digits (see below for details on 0 value).

Description

$DECIMAL returns a floating point number converted to the Caché floating point data type. This function is used to convert a fractional number in IEEE double-precision format to the corresponding fractional number in Caché format. This is the inverse of the operation performed by the [$DOUBLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fdouble) function.

The Caché [SQL data types](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datatype) DOUBLE and DOUBLE PRECISION represent IEEE floating point numbers; the FLOAT data type represents standard Caché fractional numbers.

IEEE floating point numbers are represented internally using 53 binary bits. Because most fractional decimal numbers have no exact binary representation, a fractional number in $DOUBLE format will usually differ slightly from its $DECIMAL conversion. Standard Caché fractional numbers have a precision of 18 decimal digits on all supported Caché system platforms. When an IEEE floating point number is displayed as a fractional number the binary bits are often converted to a fractional number with far more than 18 decimal digits. This does not mean that IEEE floating point numbers are more precise than standard Caché fractional numbers.

The num value can be specified as a number or a numeric string. It is resolved to canonical form (leading and trailing zeros removed, multiple plus and minus signs resolved, etc.) before $DECIMAL conversion. If the num value is outside of the range of values that can be converted to FLOAT data type, $DECIMAL generates a <MAXNUMBER> error. Specifying a nonnumeric string to num returns 0. Specifying a mixed-numeric string (for example "7dwarves" or "7.5.4") to num truncates the input value at the first nonnumeric character then converts the numeric portion.

Default rounding is done as follows:

*   Fractional Numbers: If digits is not specified and num has more than 19 significant digits, $DECIMAL rounds the fractional portion so that the resulting number is 19 digits (or fewer, if the rounding results in trailing zeros). $DECIMAL always rounds to the nearest fractional value with the greater absolute value.
    
*   Very Large Integer Numbers: If digits is not specified and num has more than 19 significant integer digits to the left of the decimal point, $DECIMAL rounds so that the resulting integer has 19 significant digits, with the remaining integer digits represented by zeros.
    
*   Very Small Fractional Numbers less than 1: If digits is not specified and num is a fractional number with more than 19 zeroes to the right of the decimal point before the significant value, $DECIMAL preserves the zeros and then rounds so that the significant portion of the fraction is rounded to 19 significant digits.
    
*   Integers with Very Small Fractional Numbers: If digits is not specified and num is a number contains a non-zero integer portion with more than 19 zeroes to the right of the decimal point before a significant value, $DECIMAL rounds to the integer.
    

The digits argument can be used to round the return value to a specified number of digits. Trailing zeros are always deleted. If digits is a positive integer, rounding is done using the IEEE rounding standard. If num has more than 38 significant digits (and digits\=38) $DECIMAL rounds the fractional portion of the number at the 38th digit and represents all of the following num digits with zeros. If digits is greater than 38, an <ILLEGAL VALUE> error is generated.

If digits is 0, the number is cast to string collation; this is equivalent to (+num)\_"". If num is 20 digits or less, digits\=0 is the same as digits\=20.

However, if digits is 0 and num is more than 20 digits, special rounding (not IEEE rounding) is performed, as follows. Rounding is performed to return 20 digits. Special rounding is then performed on the 20th digit if it rounds to a 0 or a 5. In the case when the 20th digit would round up to a 0 or a 5, Caché rounds it down to a 9 or a 4, respectively. In the case when the 20th digit would round down to a 0 or a 5, Caché rounds it up to a 1 or a 6, respectively. Other 20th digit values are returned unchanged. This rounding algorithm is used to provide correct numeric collation and avoid rounding inconsistencies.

Integer Divide

With certain values, Caché floating point and IEEE double numbers yield a different integer divide product. For example:

  WRITE !,"Integer divide operations:"
  WRITE !,"Cache  \\: ",$DECIMAL(4.1)\\.01  // 410
  WRITE !,"Double \\: ",$DOUBLE(4.1)\\.01   // 409

 

For further details on arithmetic operations involving IEEE double numbers, see the appendix “[Numeric Computing in InterSystems Applications](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GORIENT_appx_numcompute)” in the Caché Programming Orientation Guide.

INF and NAN

If num is INF, a <MAXNUMBER> error is generated. If num is NAN, an <ILLEGAL VALUE> error is generated. These invalid values are shown in the following example:

  SET i\=$DOUBLE("INF")
  SET n\=$DOUBLE("NAN")
  WRITE $DECIMAL(i),!
  WRITE $DECIMAL(n)

Examples

The following example demonstrates that $DECIMAL has no effect when applied to a fractional number that is already in Caché format:

  SET x\=$DECIMAL($ZPI)
  SET y\=$ZPI
    IF x\=y { WRITE !,"Identical:"
             WRITE !,"Cache $DECIMAL: ",x
             WRITE !,"Native Cache:   ",y }
   ELSE { WRITE !,"Different:"
          WRITE !,"Cache $DECIMAL: ",x
          WRITE !,"Native Cache:   ",y }

 

The following example returns the value of pi as a $DOUBLE value and as a standard Caché numeric value. This example shows that equality operations should not be attempted between $DOUBLE and standard Caché numbers, and that equivalence cannot be restored by using $DECIMAL to convert IEEE back to Caché:

  SET x\=$DECIMAL($ZPI)
  SET y\=$DOUBLE($ZPI)
  SET z\=$DECIMAL(y)
  IF x\=y { WRITE !,"Cache & IEEE Same" }
  ELSEIF x\=z { WRITE !,"Cache & IEEE-to-Cache same" }
  ELSE { WRITE !,"All three different"
         WRITE !,"Cache decimal: ",x
         WRITE !,"IEEE float:    ",y
         WRITE !,"IEEE to Cache: ",z }

 

The following example returns the $DECIMAL conversion of pi as a $DOUBLE value. These conversions are rounded by different digits argument values:

  SET x\=$DOUBLE($ZPI)
  WRITE !,$DECIMAL(x)
   /\* returns 3.141592653589793116 (19 digits) \*/
  WRITE !,$DECIMAL(x,1)
   /\* returns 3 \*/
  WRITE !,$DECIMAL(x,8)
   /\* returns 3.1415927 (note rounding) \*/
  WRITE !,$DECIMAL(x,12)
   /\* returns 3.14159265359 (note rounding) \*/
  WRITE !,$DECIMAL(x,18)
   /\* returns 3.14159265358979312 \*/
  WRITE !,$DECIMAL(x,19)
   /\* returns 3.141592653589793116 (19 digits) \*/
  WRITE !,$DECIMAL(x,20)
   /\* returns 3.141592653589793116 (19 digits) \*/
  WRITE !,$DECIMAL(x,21)
   /\* returns 3.141592653589793116 (19 digits) \*/
  WRITE !,$DECIMAL(x,0)
   /\* returns 3.1415926535897931159 (20 digits) \*/

 

See Also

*   [ZZDUMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czzdump) command
    
*   [$DOUBLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fdouble) function
    
*   [$FNUMBER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ffnumber) function
    
*   [$NUMBER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fnumber) function
    
*   [Data Types](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datatype) in Caché SQL Reference
    
*   [Operators](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_operators) in Using Caché ObjectScript
    
*   [Numeric Computing in InterSystems Applications](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GORIENT_appx_numcompute) in Caché Programming Orientation Guide
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fdata "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fdouble "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fdecimal.xml**