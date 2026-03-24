# $INUMBER

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_finumber

---

 

Caché ObjectScript Reference

$INUMBER

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fincrement "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fisobject "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$INUMBER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_finumber "$INUMBER") \]

Go to: Description Parameters Examples Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Validates a numeric value and converts it to internal format.

Synopsis

$INUMBER(fnumber,format,erropt)
$IN(fnumber,format,erropt)

Parameters

fnumber

The numeric value to be converted to the internal format. It can be a numeric or string value, a variable name, or any valid Caché ObjectScript expression.

format

A format specification indicating which external numeric formats are valid representations of numbers. Specified as a quoted string consisting of zero or more format codes, in any order. [Format codes](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_finumber#RCOS_finumber31) are described below. Note that some format codes are incompatible and result in an error. For default formatting, with or without the erropt parameter, you can specify the empty string ("").

erropt

Optional — The expression returned if fnumber is considered invalid based on format.

Description

The $INUMBER function validates the numeric value fnumber using the formats specified in format. It then converts it to the internal Caché format.

If fnumber does not correspond to the specified format and you have not specified erropt, Caché generates an <ILLEGAL VALUE> error. If you have specified erropt, an invalid numeric value returns the erropt string.

Parameters

format

The possible format codes are as follows. You can specify them singly or in combination to instruct $INUMBER to adhere strictly to the format rules. If no format codes are entered, $INUMBER will be as flexible as possible in validating fnumber (see [Null Format Provides Maximum Flexibility](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_finumber#RCOS_finumber55) for more information).

Code

Description

+

Mandatory sign. The fnumber value must have a explicit sign. Even the number 0 must be signed (+0 or -0). The sign can be either leading or trailing, unless restricted by either an “L” or a “T” format code. Parentheses cannot be used. The only value that does not require a sign is NAN, which can be specified with or without a sign when using code “D+”.

\-

Unsigned. No sign may be present in fnumber.

D

$DOUBLE numbers. This code converts fnumber to an IEEE floating point number. This is equivalent to $DOUBLE(fnumber). If “D” is specified, you can input the quoted strings “INF” and “NAN” as an fnumber value. INF and NAN can be specified in any combination of uppercase and lowercase letters, with or without leading or trailing signs or enclosing parentheses. (Signs are accepted, but ignored, for NAN.) The variant forms INFINITY and SNAN are also supported.

E or G

E-notation (scientific notation). This code allows you to specify fnumber as a string in scientific notation format. This code permits, but does not require, that you specify fnumber in scientific notation.

N

No NumericGroupSeparator. Does not allow the use of a numeric group separator. This format code is incompatible with the comma (,) format code.

O

ODBC locale. Overrides the current locale, and instead uses the standard ODBC locale with the following values: PlusSign=+; MinusSign=-; DecimalSeparator=.; NumericGroupSeparator=,; NumericGroupSize=3. This format code is incompatible with the dot (.) format code.

P

Negative numbers must be enclosed in parentheses. Nonnegative numbers must be unsigned, and may have or omit leading and trailing spaces.

L

Leading sign. Sign, if present, must precede the numerical portion of fnumber. Parentheses are not permitted.

T

Trailing sign. Sign, if present, must follow the numerical portion of fnumber. Parentheses are not permitted.

,

Expects fnumber to use the format specified by properties in the current locale. The NumericGroupSeparator (“,” by default) may or may not appear in fnumber, but if present, it must consistently appear every NumericGroupSize (3 by default) digits to the left of the decimal point.

.

Requires standard European formatting, regardless of the current locale settings. Requires DecimalSeparator as comma (,), NumericGroupSeparator as period (.), NumericGroupSize as 3, PlusSign as plus (+), MinusSign as minus (-). Periods are optional, but if present must consistently appear every three digits to the left of the decimal comma.

When “+”, “-” and “P” Format Codes are Absent

When format does not include any of the “+”, “-”, or “P” codes, then fnumber may contain any one of the following:

*   No sign or parentheses.
    
*   Either the PlusSign locale property (“+” by default) or the MinusSign locale property (“-” by default) but not both. The position of this sign is determined by the “L” or the “T” format code if specified.
    
*   Leading and trailing parentheses.
    

When “L”, “T” and “P” Format Codes are Absent

When format does not include any of the “L”, “T”, or “P” format codes, any sign present in fnumber may be either leading or trailing (but not both).

When “,” and “.” Format Codes are Absent

When format does not include either the “,” or “.” format codes, fnumber may optionally have NumericGroupSeparator symbols appear anywhere to the left or right of the DecimalSeparator, if any. However, each NumericGroupSeparator must have at least one digit to its immediate left and one to its immediate right. When format includes “N”, no NumericGroupSeparator symbols are permitted.

Mutually Exclusive Format Codes

Some format codes conflict with each other. Each of the following pairs of format codes are mutually exclusive and result in an error:

*   “-+” results in a <FUNCTION> error
    
*   “-P” or “+P” result in a <SYNTAX> error
    
*   “TP” or “LP” result in a <SYNTAX> error
    
*   “TL” results in a <FUNCTION> error
    
*   “,.” results in a <FUNCTION> error
    
*   “,N” results in a <FUNCTION> error
    
*   “.O” results in a <FUNCTION> error
    

A <FUNCTION> error is also generated if you specify an invalid format code character.

Null Format Provides Maximum Flexibility

You can specify format as a null string. This is called a null format. When a null format is specified, $INUMBER accepts a fnumber value with any one of the following sign conventions:

*   No sign or parentheses.
    
*   Either a leading or trailing MinusSign, but not both.
    
*   Either a leading or trailing PlusSign, but not both.
    
*   Leading and trailing parentheses.
    

When a null format is specified, fnumber may optionally have NumericGroupSeparator symbols appear anywhere to the left or right of the DecimalSeparator, if any. However, each NumericGroupSeparator must have at least one digit to its immediate left and one to its immediate right. Sign rules are flexible, and leading and trailing blanks and zeros are ignored. Thus, the following two commands:

   WRITE !,$INUMBER("+1,23,456,7.8,9,100","")
   WRITE !,$INUMBER("0012,3456,7.891+","")

 

are both valid and return the same number, formatted according to the default locale. However,

   WRITE $INUMBER("1,23,,345,7.,8,9,","")

is invalid because of the adjacent commas, the adjacent period and comma, and the trailing comma. It generates an <ILLEGAL VALUE> error.

Behavior Common to All Formats

Regardless of the specified format codes, $INUMBER always ignores leading and trailing blank spaces or zeros, but considers fnumber to be invalid if it has any of the following characteristics:

*   Both a PlusSign and a MinusSign
    
*   More than one PlusSign or MinusSign
    
*   Parentheses and a PlusSign
    
*   Parentheses and a MinusSign
    
*   More than one DecimalSeparator
    
*   Embedded Spaces
    
*   Any characters other than the following:
    
    *   Numeric digits
        
    *   “(“
        
    *   “)”
        
    *   Leading or trailing spaces
        
    *   The DecimalSeparator specified by the current locale (if format does not include “.”)
        
    *   The NumericGroupSeparator specified by the current locale (if format does not include “.”)
        
    *   The PlusSign property specified by the current locale (if format does not include “.”)
        
    *   The MinusSign property specified by the current locale (if format does not include “.”)
        
    *   “.” (if format includes “.”)
        
    *   “,” (if format includes “.”)
        
    *   “+” (if format includes “.”)
        
    *   “-” (if format includes “ .”)
        
*   The strings “INF” and “NAN” (and their variants) if format includes “D”.
    

Examples

These examples illustrate how different formats affect the behavior of $INUMBER. All of these examples assume the current locale is the default locale.

In the following example, $INUMBER accepts a leading minus sign because of the “L” format code and returns -123456789.12345678:

   WRITE $INUMBER("-123,4,56,789.1234,5678","L")

 

In the following example, $INUMBER generates an <ILLEGAL VALUE> error because the sign is leading but the “T” format code specifies that trailing signs must be used:

   WRITE $INUMBER("-123,4,56,789.1234,5678","T")

 

In the following example, the first $INUMBER succeeds and returns a negative number. The second $INUMBER generates an <ILLEGAL VALUE> error because fnumber includes a sign but the “P” format code specifies that negative numbers must be enclosed in parentheses rather than signed:

   WRITE !,$INUMBER("(123,4,56,789.1234,5678)","P")
   WRITE !,$INUMBER("-123,4,56,789.1234,5678","P")

 

In the following example, $INUMBER generates an <ILLEGAL VALUE> error because a sign is present but the “-” format code specifies that numbers must be unsigned:

   WRITE $INUMBER("-123,4,56,789.1234,5678","-")

 

In the following example, $INUMBER fails but does not generate an error due to the illegal use of a sign, but instead returns as its value the string “ERR” specified as the erropt:

   WRITE $INUMBER("-123,4,56,789.1234,5678","-","ERR")

 

The following example returns -23456789.123456789; $INUMBER accepts the specified fnumber as valid because the leading sign follows the formatting specified by “L” and the strict spacing of commas every three digits to the left of the decimal place with no commas to its right follows the strict formatting specified by the “,” code:

   WRITE $INUMBER("-23,456,789.123456789","L,")

 

In the following example, the “E” code permits conversion of a scientific notation string to a number. Note that all format codes support scientific notation as a numeric literal, but only “E” (or “G”) support scientific notation as a string. This example uses variables and concatenation to provide the scientific notation string values:

   SET num\=1.234
   SET exp\=\-14
   WRITE $INUMBER(1.234E-14,"E","E-lit-err"),!
   WRITE $INUMBER(num\_"E"\_exp,"E","E-string-err"),!
   WRITE $INUMBER(1.234E-14,"L","L-lit-err"),!
   WRITE $INUMBER(num\_"E"\_exp,"L","L-string-err"),!

 

The following example compares the values returned by “L” code and a “D” code for a fractional number and for the constant pi. The “D” code converts to an IEEE floating point ($DOUBLE) number:

   WRITE $INUMBER(1.23E-23,"L"),!
   WRITE $INUMBER(1.23E-23,"D"),!
   WRITE $INUMBER($ZPI,"L"),!
   WRITE $INUMBER($ZPI,"D"),!

 

Notes

Differences between $INUMBER and $FNUMBER

Most format codes have similar meanings in the $INUMBER and $FNUMBER functions, but the exact behavior triggered by each code differs by function because of the nature of the validations and conversions being performed.

In particular, the “-” and “+” format codes do not have quite the same meaning for $INUMBER as they do for $FNUMBER. With $FNUMBER, “-” and “+” are not mutually exclusive, and “-” only affects the MinusSign (by suppressing it), and “+” only affects the PlusSign (by inserting it). With $INUMBER, “-” and “+” are mutually exclusive. “-” means no sign is permitted, and “+” means there must be a sign.

Decimal Separator

$INUMBER uses the DecimalSeparator property value for the current locale (“.” by default) as the delimiter character between the integer part and the fractional part of fnumber. When the “.” format code is specified, this delimiter is a “,” regardless of the current locale.

To determine the DecimalSeparator character for your locale, invoke the [GetFormatItem()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYS.NLS.Format#GetFormatItem) method:

  WRITE ##class(%SYS.NLS.Format).GetFormatItem("DecimalSeparator")

 

Numeric Group Separator and Size

$INUMBER uses the NumericGroupSeparator property value from the current locale (“,” by default) as the delimiter between groups of digits in the integer part of fnumber. The size of these groups is determined by the NumericGroupSize property of the current locale (“3” by default). When the “.” format code is specified, this delimiter is a “.” and appears every three digits regardless of the current locale.

To determine the NumericGroupSeparator character and NumericGroupSize number for your locale, invoke the [GetFormatItem()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYS.NLS.Format#GetFormatItem) method:

  WRITE ##class(%SYS.NLS.Format).GetFormatItem("NumericGroupSeparator"),!
  WRITE ##class(%SYS.NLS.Format).GetFormatItem("NumericGroupSize")

 

Plus Sign and Minus Sign

$INUMBER uses the PlusSign and MinusSign property values from the current locale (“+” and “-” by default). When the “.” format code is specified, these signs are set to “+” and “-”, regardless of the current locale.

To determine the PlusSign and MinusSign characters for your locale, invoke the [GetFormatItem()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYS.NLS.Format#GetFormatItem) method:

  WRITE ##class(%SYS.NLS.Format).GetFormatItem("PlusSign"),!
  WRITE ##class(%SYS.NLS.Format).GetFormatItem("MinusSign")

 

See Also

*   [$DOUBLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fdouble) function
    
*   [$FNUMBER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ffnumber) function
    
*   [$ISVALIDNUM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fisvalidnum) function
    
*   [$NORMALIZE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fnormalize) function
    
*   [$NUMBER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fnumber) function
    
*   More information on locales in the section on “[System Classes for National Language Support](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSTU_customize#GSTU_customize_nls)” in Caché Specialized System Tools and Utilities.
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fincrement "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fisobject "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_finumber.xml**