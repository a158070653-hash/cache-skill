# $FNUMBER

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_ffnumber

---

 

Caché ObjectScript Reference

$FNUMBER

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ffind "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fget "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$FNUMBER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ffnumber "$FNUMBER") \]

Go to: Description Parameters Examples Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Formats a numeric value with a specified format; optionally rounds to a specified precision.

Synopsis

$FNUMBER(inumber,format,decimal)
$FN(inumber,format,decimal)

Parameters

inumber

The number that is to be formatted. It can be a numeric value, the name of a numeric variable, or any valid Caché ObjectScript expression that evaluates to a numeric value.

format

A format specification of how the number is to be formatted. Specified as a quoted string consisting of zero or more format codes, in any order. Format codes are described below. Note that some format codes are incompatible and result in an error. For default formatting, with or without the decimal parameter, you can specify the empty string ("").

decimal

Optional — The number of fractional decimal digits to be included in the returned number.

Description

$FNUMBER returns the number specified by inumber in the specified format.

Parameters

inumber

An expression that resolves to a number. If inumber is a string, Caché first converts it to a number, truncating at the first non-numeric character. (Caché converts a non-numeric string to 0.) Before $FNUMBER formats this number, Caché converts it to [canonical form](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_numcanonical).

format

The possible format codes are as follows. You can specify them singly or in combination.

Code

Description

""

Empty string. Returns inumber in [canonical number format](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_numcanonical). This format is the same as "L" format.

+

Returns a nonnegative number prefixed by the PlusSign property of the current locale (“+” by default). If the number is negative, it returns the number prefixed by the MinusSign property of the current locale (“-” by default).

\-

Returns a negative number without prefixing it by the MinusSign property of the current locale. Otherwise, it returns the number without any leading sign.

,

Returns the number with the value of the [NumericGroupSeparator](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ffnumber#RCOS_ffnumber_numericgroup) property of the current locale inserted every [NumericGroupSize](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ffnumber#RCOS_ffnumber_numericgroup) numerals to the left of the decimal point. Combining “,” with either “.” or “N” formats results in a <FUNCTION> error.

.

Returns the number using standard European formatting, regardless of the current locale settings. Sets DecimalSeparator to comma (,), NumericGroupSeparator to period (.), NumericGroupSize to 3, PlusSign to plus (+), MinusSign to minus (-). Combining “.” with either “,” or “O” formats results in a <FUNCTION> error.

D

$DOUBLE special formatting. This code has two effects:

“D” specifies that $DOUBLE(-0) should return -0; otherwise, $DOUBLE(-0) returns 0. However, “-D” overrides the negative sign and returns 0.

You can specify “D” or “d” for this code; a returned INF or NAN will be expressed in the corresponding uppercase or lowercase letters. The default is upper case.

E

E-notation (scientific notation). Returns the number in scientific notation. If you omit the decimal number of fractional digits, 6 is used as the default. You can specify “E” or “e” for this code; the returned value will contain the corresponding uppercase or lowercase symbol. The exponent portion of the returned value is two digits in length with a leading sign, unless three exponent digits are required. “E” and “G” are incompatible and result in a <FUNCTION> error.

G

E-notation or fixed decimal notation. If the number of fractional digits that would result from conversion to scientific notation is larger than the decimal value (or the default of 6 decimal digits), the number is returned in scientific notation. For example, $FNUMBER(1234.99,"G",2) returns 1.23E+03. If the number of fractional digits that would result from conversion to scientific notation is equal to or smaller than the decimal value (or the default of 6 decimal digits), the number is returned in fixed decimal (standard) notation. For example, $FNUMBER(1234.99,"G",3) returns 1235. You can specify “G” or “g” for this code; the returned scientific notation value will contain the corresponding uppercase “E” or lowercase “e”. “E” and “G” are incompatible and result in a <FUNCTION> error.

L

Leading sign. Sign, if present, must precede the numerical portion of inumber. Parentheses are not permitted. This code cannot be used with the “P” or “T” format codes; attempting to do so results in a <SYNTAX> or <FUNCTION> error. Leading sign is the default format.

N

No NumericGroupSeparator. Does not allow the use of a numeric group separator. This format code is incompatible with the comma (,) format code.

O

ODBC locale. Overrides the current locale, and instead uses the standard ODBC locale with the following values: PlusSign=+; MinusSign=-; DecimalSeparator=.; NumericGroupSeparator=,; NumericGroupSize=3. This format code is incompatible with the dot (.) format code.

P

Parentheses sign. Returns a negative number in parentheses and without a leading MinusSign locale property value. Otherwise, it returns the number without parentheses, but with a leading and trailing space character. This code cannot be used with the “+”, “-”, “L”, or “T” format codes; attempting to do so results in a <SYNTAX> error.

T

Trailing sign. Returns the number with a trailing sign if a prefix sign would otherwise have been generated. However, it does not force a trailing sign. To produce a trailing sign for a nonnegative number (positive or zero), you must also specify the “+” format code. To produce a trailing sign for a negative number, you must not specify the “-” format code. The trailing sign used is determined by the PlusSign and MinusSign properties of the current locale respectively. A trailing space character, but no sign, is inserted in the case of a nonnegative number with “+” omitted or in the case of a negative number with “-” specified. Parentheses are not permitted. This code cannot be used with the “L” or “P” format codes; attempting to do so results in a <SYNTAX> or <FUNCTION> error.

In Caché, fractional numbers less than 1 are represented in canonical form without a zero integer: 0.66 becomes .66. In most cases, $FNUMBER returns fractional numbers less than 1 with a leading zero integer: .66 becomes 0.66. However, two-parameter $FNUMBER with a format of "" (empty string), "L" (which is functionally identical to empty string), and "D" return fractional numbers less than 1 in canonical form: .66. All other two-parameter $FNUMBER format options, and all three-parameter $FNUMBER format options, return fractional numbers less than 1 with a leading zero integer: .66 becomes 0.66.

The $DOUBLE function can return the values INF (infinite) and NAN (not a number). INF can take a negative sign; format codes represent INF as if it were a number. For example: +INF, INF-, (INF). NAN does not take a sign; the only format code that affects NAN is “d”, which returns it in lowercase letters. The “E” and “G” codes have no effect on INF and NAN values.

decimal

Rounding is performed before the number is truncated or zero filled. It can be specified as a positive integer value, the name of an integer variable, or any valid Caché ObjectScript expression that evaluates to a positive integer. If the decimal value is 0, the number is returned as an integer, with no decimal point. If the decimal value is greater than the number of decimal digits in the number, the remaining positions are zero filled. If inumber itself is less than 1, $FNUMBER inserts a leading zero before the decimal point.

You can specify the decimal parameter to control the number of fractional digits returned, after rounding is performed. The decimal parameter must evaluate to an integer. For example, assume that variable c contains the number 6.25198.

   SET c\="6.25198"
   SET x\=$FNUMBER(c,"+",3)
   SET y\=$FNUMBER(c,"+",8)
   WRITE !,x,!,y

 

The first $FNUMBER returns +6.252 and the second returns +6.25198000. Zero fill is performed if the decimal parameter exceeds the number of fractional digits in the number.

Examples

The following examples show how the different formatting designations can affect the behavior of $FNUMBER. These examples assume that the current locale is the default locale.

The following example shows the effects of sign codes on a positive number:

   SET a\=1234
   WRITE $FNUMBER(a,""),!   ; returns 1234
   WRITE $FNUMBER(a,"+"),!  ; returns +1234
   WRITE $FNUMBER(a,"-"),!  ; returns 1234
   WRITE $FNUMBER(a,"L"),!  ; returns 1234
   WRITE $FNUMBER(a,"T"),!  ; returns 1234 (with a trailing space)
   WRITE $FNUMBER(a,"T+"),! ; returns 1234+

 

The following example shows the effects of sign codes on a negative number:

   SET b\=\-1234
   WRITE $FNUMBER(b,""),!  ; returns -1234
   WRITE $FNUMBER(b,"+"),! ; returns -1234
   WRITE $FNUMBER(b,"-"),! ; returns 1234
   WRITE $FNUMBER(b,"L"),! ; returns -1234
   WRITE $FNUMBER(b,"T"),! ; returns 1234-

 

The following example shows the effects of the “P” format code on positive and negative numbers. This example writes asterisks before and after the number to show that a positive number is returned with a leading and a trailing blank:

  WRITE "\*",$FNUMBER(\-123,"P"),"\*",!  ; returns \*(123)\*
  WRITE "\*",$FNUMBER(123,"P"),"\*",!   ; returns \* 123 \*

 

The following example returns 1,234,567.81; $FNUMBER inserts a comma every 3 numerals to the left of the decimal point of x.

   SET x\=1234567.81
   WRITE $FNUMBER(x,",")

 

The following example returns 1.234.567,81; $FNUMBER inserts European formatting to x.

   SET x\=1234567.81
   WRITE $FNUMBER(x,".")

 

The following example returns 124,329.00; $FNUMBER inserts a comma and adds a decimal point and two zeros to the value of x.

    SET x\=124329
    WRITE $FNUMBER(x,",",2)

 

Notes

Decimal Separator

$FNUMBER uses the DecimalSeparator property value for the current locale (“.” by default) as the delimiter character between the integer part and the fractional part of the returned number. When the “.” format code is specified, this delimiter is a “,” regardless of the current locale setting.

To determine the DecimalSeparator character for your locale, invoke the [GetFormatItem()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYS.NLS.Format#GetFormatItem) method:

  WRITE ##class(%SYS.NLS.Format).GetFormatItem("DecimalSeparator")

 

Numeric Group Separator and Size

When the format string includes “,” $FNUMBER uses the NumericGroupSeparator property value from the current locale as the delimiter between groups of digits in the integer part of the returned number. The size of these groups is determined by the NumericGroupSize property of the current locale.

The English language locale defaults to a comma (“,”) as the NumericGroupSeparator and 3 as the NumericGroupSize. Many European locales use a period (“.”) as the NumericGroupSeparator. The Russian (rusw), Ukrainian (ukrw), and Czech (csyw) locales use a blank space as the NumericGroupSeparator. The NumericGroupSize defaults to 3 for all locales, including Japanese. (Users of Japanese may wish to group integer digits in units of either 3 or 4, depending upon context.)

When the format string includes “.” (and does not include “N”) $FNUMBER uses NumericGroupSeparator=”.” and NumericGroupSize=3 to format the return value, regardless of your current locale settings.

To determine the NumericGroupSeparator character and NumericGroupSize number for your locale, invoke the [GetFormatItem()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYS.NLS.Format#GetFormatItem) method:

  WRITE ##class(%SYS.NLS.Format).GetFormatItem("NumericGroupSeparator"),!
  WRITE ##class(%SYS.NLS.Format).GetFormatItem("NumericGroupSize")

 

Plus Sign and Minus Sign

$FNUMBER uses the PlusSign and MinusSign property values for the current locale (“+” and “-” by default). When the “.” format code is specified, these signs are set to “+” and “-”, regardless of the current locale.

To determine the PlusSign and MinusSign characters for your locale, invoke the [GetFormatItem()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYS.NLS.Format#GetFormatItem) method:

  WRITE ##class(%SYS.NLS.Format).GetFormatItem("PlusSign"),!
  WRITE ##class(%SYS.NLS.Format).GetFormatItem("MinusSign")

 

Differences between $FNUMBER and $INUMBER

Most format codes have similar meanings in the $FNUMBER and $INUMBER functions, but the exact behavior triggered by each code differs by function because of the nature of the validations and conversions being performed.

In particular, the “-” and “+” format codes do not have quite the same meaning for $FNUMBER as they do for $INUMBER. With $FNUMBER, “-” and “+” are not mutually exclusive, and “-” only affects the MinusSign (by suppressing it), and “+” only affects the PlusSign (by inserting it). With $INUMBER, “-” and “+” are mutually exclusive. “-” means no sign is permitted, and “+” means there must be a sign.

See Also

*   [$DOUBLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fdouble) function
    
*   [$JUSTIFY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fjustify) function
    
*   [$INUMBER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_finumber) function
    
*   [$ISVALIDNUM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fisvalidnum) function
    
*   [$NORMALIZE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fnormalize) function
    
*   [$NUMBER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fnumber) function
    
*   More information on locales in the section on “[System Classes for National Language Support](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSTU_customize#GSTU_customize_nls)” in Caché Specialized System Tools and Utilities.
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ffind "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fget "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_ffnumber.xml**