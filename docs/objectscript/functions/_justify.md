# $JUSTIFY

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fjustify

---

 

Caché ObjectScript Reference

$JUSTIFY

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fisvalidnum "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flength "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$JUSTIFY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fjustify "$JUSTIFY") \]

Go to: Description Parameters Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Right-aligns an expression within a specified width, rounding to a specified number of fractional digits.

Synopsis

$JUSTIFY(expression,width,decimal)
$J(expression,width,decimal)

Parameters

expression

The value that is to be right-aligned. It can be a numeric value, a string literal, the name of a variable, or any valid Caché ObjectScript expression.

width

The number of characters within which expression is to be right-aligned. A positive integer or an expression that evaluates to a positive integer.

decimal

Optional — The number of fractional digits. A positive integer or an expression that evaluates to a positive integer. Caché rounds or pads the number of fractional digits in expression to this value. If you specify decimal, Caché treats expression as a numeric.

Description

$JUSTIFY returns the value specified by expression right-aligned within the specified width. You can include the decimal parameter to decimal-align numbers within width.

*   $JUSTIFY(expression,width): the 2-parameter syntax right-justifies expression within width. It does not perform any conversion of expression. The expression can be a numeric or a nonnumeric string.
    
*   $JUSTIFY(expression,width,decimal): the 3-parameter syntax converts expression to a [canonical number](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_numbers), rounds or zero pads fractional digits to decimal, then right-justifies the resulting numeric value within width. If expression is a nonnumeric string, Caché converts it to 0, pads it, then right-justifies it.
    

$JUSTIFY recognizes the DecimalSeparator character for the current locale. It adds or deletes a DecimalSeparator character as needed. The DecimalSeparator character depends upon the locale; commonly it is either a period (.) for American-format locales, or a comma (,) for European-format locales. To determine the DecimalSeparator character for your locale, invoke the following method:

  WRITE ##class(%SYS.NLS.Format).GetFormatItem("DecimalSeparator")

 

Commonly, $JUSTIFY is used to format numbers with fractional digits: every number is given the same number of fractional digits, and the numbers are right-aligned so that the DecimalSeparator characters align in a column of numbers. $JUSTIFY is especially useful for outputting formatted values using the WRITE command.

Parameters

expression

The value to be right-justified, and optionally expressed as a numeric with a specified number of fractional digits.

*   If string justification is desired, do not specify decimal. The expression can contain any characters. $JUSTIFY right-justifies expression, as described in width.
    
*   If numeric justification is desired, specify decimal. If decimal is specified, $JUSTIFY converts expression to a [canonical number](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_numbers). It resolves leading plus and minus signs and removes leading and trailing zeros. It truncates expression at the first nonnumeric character. If expression begins with a nonnumeric character (such as a currency symbol), $JUSTIFY converts the expression value to 0. For further details on how Caché converts a numeric to a canonical number, and Caché handling of a numeric string containing nonnumeric characters, refer to the [Numbers](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_numbers) section of the “Data Types and Values” chapter of Using Caché ObjectScript.
    
    After $JUSTIFY converts expression to a canonical number, it zero-pads or rounds this canonical number to decimal number of fractional digits, then right-justifies the result, as described in width. $JUSTIFY does not recognize NumericGroupSeparator characters, currency symbols, multiple DecimalSeparator characters, or trailing plus or minus signs.
    

width

The width in which to right-justify the converted expression. If width is greater than the length of expression (after numeric and fractional digit conversion), Caché right-justifies to width, left-padding as needed with blank spaces. If width is less than the length of expression (after numeric and fractional digit conversion), Caché sets width to the length of the expression value.

Specify width as a positive integer. A width value of 0, the null string (""), or a nonnumeric string is treated as a width of 0, which means that Caché sets width to the length of the expression value.

decimal

The number of fractional digits. If expression contains more fractional digits, $JUSTIFY rounds the fractional portion to this number of fractional digits. If expression contains fewer fractional digits, $JUSTIFY pads the fractional portion with zeros to this number of fractional digits, adding a Decimal Separator character, if needed. If decimal\=0, $JUSTIFY rounds expression to an integer value and deletes the Decimal Separator character.

If the expression value is less than 1, $JUSTIFY inserts a leading zero before the DecimalSeparator character.

The [$DOUBLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fdouble) values INF, -INF, and NAN are returned unchanged by $JUSTIFY, regardless of the decimal value.

$JUSTIFY and $FNUMBER

You can use [$FNUMBER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ffnumber) to format a number for display. Both $JUSTIFY and $FNUMBER can round (or zero pad) to a specified number of fractional digits. $FNUMBER can also be used to add NumericGroupSeparator characters. However, note the following:

*   $FNUMBER cannot format a number once it has been right-aligned using $JUSTIFY. ($FNUMBER interprets the leading spaces as nonnumeric characters.)
    
*   $JUSTIFY cannot perform numeric justification on a number once you have added NumericGroupSeparator characters or have appended a currency symbol. ($JUSTIFY interprets NumericGroupSeparators or currency symbols as nonnumeric characters.)
    

Therefore, to properly add NumericGroupSeparators, round fractional digits, append a currency symbol, and right-align the resulting number, you use $FNUMBER to perform rounding and inserting of NumericGroupSeparators. You then use $JUSTIFY with 2-parameter syntax to right-align the resulting string:

  SET num\=123456.789
  SET fmtnum\=$FNUMBER(num,",",2)
  SET money\="$"\_fmtnum
  SET rmoney\=$JUSTIFY(money,15)
  WRITE ">",rmoney,"<"

 

Examples

The following example performs right-justification on strings. No numeric conversion is performed:

  WRITE ">",$JUSTIFY("right",10),"<",!
  WRITE ">",$JUSTIFY("aligned",10),"<",!
  WRITE ">",$JUSTIFY("+0123.456",10),"<",!
  WRITE ">",$JUSTIFY("string longer than width",10),"<",!

 

The following example performs numeric right-justification with a specified number of fractional digits:

   SET var1 \= 250.50999
   SET var2 \= 875
   WRITE !,$JUSTIFY(var1,20,2),!,$JUSTIFY(var2,20,2)
   WRITE !,$JUSTIFY("\_\_\_\_\_\_\_\_\_",20)
   WRITE !,$JUSTIFY("TOTAL",9),$JUSTIFY(var1+var2,11,2)

 

return the following lines:

               250.51
               875.00
              \_\_\_\_\_\_\_
TOTAL         1125.51

The following example performs numeric right-justification with the $DOUBLE values INF and NAN:

  SET rtn\=##class(%SYSTEM.Process).IEEEError(0)
  SET x\=$DOUBLE(1.2e500)
  WRITE !,"Double: ",x
  WRITE !,">",$JUSTIFY(x,12,2),"<"
  SET y\=$DOUBLE(x\-x)
  WRITE !,"Double INF minus INF: ",y
  WRITE !,">",$JUSTIFY(y,12,2),"<"

 

See Also

*   [$FNUMBER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ffnumber) function
    
*   [$X](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vx) special variable
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fisvalidnum "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flength "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fjustify.xml**