# $ISVALIDNUM

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fisvalidnum

---

 

Caché ObjectScript Reference

$ISVALIDNUM

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fisvaliddouble "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fjustify "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$ISVALIDNUM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fisvalidnum "$ISVALIDNUM") \]

Go to: Description Parameters Examples Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Validates a numeric value and returns a boolean; optionally provides range checking.

Synopsis

$ISVALIDNUM(num,scale,min,max)

Parameters

num

The numeric value to be validated. It can be a numeric or string value, a variable name, or any valid Caché ObjectScript expression.

scale

Optional — The number of significant fractional digits for min and max range comparisons.

min

Optional — The minimum permitted numeric value.

max

Optional — The maximum permitted numeric value.

Description

The $ISVALIDNUM function validates num and returns a boolean value. It optionally performs a range check using min and max values. The scale parameter is used during range checking to specify how many fractional digits to compare. A boolean value of 1 means that num is a properly formed number and passes the range check, if one is specified.

$ISVALIDNUM validates American format numbers, which use a period (.) as the decimal separator. It does not validate European format numbers, which use a comma (,) as the decimal separator. $ISVALIDNUM does not consider valid a number that contains numeric group separators; it returns 0 (invalid) for any number containing a comma or a blank space, regardless of the current locale.

Parameters

num

The number to be validated may be an integer, a real number, or a scientific notation number (with the letter “E” or “e”). It may be a string, expression, or variable that resolves to a number. It may be signed or unsigned, and may contain leading or trailing zeros. Validation fails ($ISVALIDNUM returns 0) if:

*   num is the empty string ("").
    
*   num contains any characters other than the digits 0–9, a leading + or – sign, a decimal point (.), and a letter “E” or “e”.
    
*   num contains more than one + or – sign, decimal point, or letter “E” or “e”.
    
*   The optional + or – sign is not the first character of num.
    
*   The letter “E” or “e” indicating a base-10 exponent is not followed by an integer in a numeric string.
    

With the exception of $ISVALIDNUM, specifying a base-10 number with a non-integer exponent in any expression results in a <SYNTAX> error. For example, WRITE 7E3.5.

If a base-10 exponent is greater than 308 (308 – (number of integers - 1)), a <MAXNUMBER> error results. For example, $ISVALIDNUM('1E309') and $ISVALIDNUM('111E307') both generate a <MAXNUMBER> error.

The scale parameter value causes evaluation using rounded or truncated versions of the num value. The actual value of the num variable is not changed by $ISVALIDNUM processing.

If num is the INF, –INF, or NAN value returned by [$DOUBLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fdouble), $ISVALIDNUM returns 1.

scale

The scale parameter is used during range checking to specify how many fractional digits to compare. Specify an integer value for scale; fractional digits in the scale value are ignored. You can specify a scale value larger than the number of fractional digits specified in the other parameters. You can specify a scale value of –1; all other negative scale values result in a <FUNCTION> error.

A nonnegative scale value causes num to be rounded to that number of fractional digits before performing min and max range checking. A scale value of 0 causes num to be rounded to an integer value (3.9 = 4) before performing range checking. A scale value of –1 causes num to be truncated to an integer value (3.9 = 3) before performing range checking. To compare all specified digits without rounding or truncating, omit the scale parameter. A scale value that is nonnumeric or the null string is equivalent to a scale value of 0.

Rounding is performed for all scale values except –1. A value of 5 or greater is always rounded up.

If you omit the scale parameter, retain the comma as a place holder.

When rounding numbers, be aware that IEEE floating point numbers and standard Caché fractional numbers differ in precision. $DOUBLE IEEE floating point numbers are encoded using binary notation. They have a precision of 53 binary bits, which corresponds to 15.95 decimal digits of precision. (Note that the binary representation does not correspond exactly to a decimal fraction.) Because most decimal fractions cannot be exactly represented in this binary notation, an IEEE floating point number may differ slightly from the corresponding standard Caché floating point number. Standard Caché fractional numbers have a precision of 18 decimal digits on all supported Caché system platforms. When an IEEE floating point number is displayed as a fractional number, the binary bits are often converted to a fractional number with far more than 18 decimal digits. This does not mean that IEEE floating point numbers are more precise than standard Caché fractional numbers.

min and max

You can specify a minimum allowed value, a maximum allowed value, neither, or both. If specified, the num value (after the scale operation) must be greater than or equal to the min value, and less than or equal to the max value. A null string as a min or max value is equal to zero. If a value does not meet these criteria, $ISVALIDNUM returns 0.

If you omit a parameter, retain the comma as a place holder. For example, when omitting scale and specifying min or max, or when omitting min and specifying max. Trailing commas are ignored.

If the num, min, or max value is a $DOUBLE number, then all three of these numbers are treated as a $DOUBLE number for this range check. This prevents unexpected range errors caused by the small generated fractional part of a $DOUBLE number.

Examples

In the following example, each invocation of $ISVALIDNUM returns 1 (valid number):

   WRITE !,$ISVALIDNUM(0)        ; All integers OK
   WRITE !,$ISVALIDNUM(4.567)    ; Real numbers OK
   WRITE !,$ISVALIDNUM("4.567")  ; Numeric strings OK
   WRITE !,$ISVALIDNUM(\-.0)      ; Signed numbers OK
   WRITE !,$ISVALIDNUM(+004.500) ; Leading/trailing zeroes OK
   WRITE !,$ISVALIDNUM(4E2)      ; Scientific notation OK   

 

In the following example, each invocation of $ISVALIDNUM returns 0 (invalid number):

   WRITE !,$ISVALIDNUM("")      ; Null string is invalid
   WRITE !,$ISVALIDNUM("4,567") ; Commas are not permitted
   WRITE !,$ISVALIDNUM("4A")    ; Invalid character

 

In the following example, each invocation of $ISVALIDNUM returns 1 (valid number), even though INF (infinity) and NAN (Not A Number) are, strictly speaking, not numbers:

   DO ##class(%SYSTEM.Process).IEEEError(0)
   WRITE !,$ISVALIDNUM($DOUBLE($ZPI))  ; DOUBLE numbers OK
   WRITE !,$ISVALIDNUM($DOUBLE("INF")) ; DOUBLE INF OK
   WRITE !,$ISVALIDNUM($DOUBLE("NAN")) ; DOUBLE NAN OK
   WRITE !,$ISVALIDNUM($DOUBLE(1)/0)   ; generated INF OK

 

The following example shows the use of the min and max parameters. All of the following return 1 (number is valid and also passes the range check):

   WRITE !,$ISVALIDNUM(4,,3,5)    ; scale can be omitted
   WRITE !,$ISVALIDNUM(4,2,3,5)   ; scale can be larger than 
                                  ; number of fractional digits
   WRITE !,$ISVALIDNUM(4,0,,5)    ; min or max can be omitted
   WRITE !,$ISVALIDNUM(4,0,4,4)   ; min and max are inclusive
   WRITE !,$ISVALIDNUM(\-4,0,\-5,5) ; negative numbers
   WRITE !,$ISVALIDNUM(4.00,2,04,05) ; leading/trailing zeros
   WRITE !,$ISVALIDNUM(.4E3,0,3E2,400) ; base-10 exponents expanded

 

The following example shows the use of the scale parameter with min and max. All of the following return 1 (number is valid and also passes the range check):

   WRITE !,$ISVALIDNUM(4.55,,4.54,4.551)
     ; When scale is omitted, all digits of num are checked.
   WRITE !,$ISVALIDNUM(4.1,0,4,4.01)
     ; When scale=0, num is rounded to an integer value
     ; (0 fractional digits) before min & max check.
   WRITE !,$ISVALIDNUM(3.85,1,3.9,5)
     ; num is rounded to 1 fractional digit, 
     ; (with values of 5 or greater rounded up) 
     ; before min check.
   WRITE !,$ISVALIDNUM(4.01,17,3,5) 
     ; scale can be larger than number of fractional digits.
   WRITE !,$ISVALIDNUM(3.9,\-1,2,3)
     ; When scale=-1, num is truncated to an integer value

 

Notes

$ISVALIDNUM and $ISVALIDDOUBLE Compared

The $ISVALIDNUM and [$ISVALIDDOUBLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fisvaliddouble) functions both validate numbers and return a boolean value (0 or 1).

*   Both functions accept as valid numbers the INF, –INF, and NAN values returned by $DOUBLE. $ISVALIDDOUBLE also accepts as valid numbers the not case-sensitive strings “NAN” and “INF”, as well as the variants “Infinity” and “sNAN”, and any of these strings beginning with a single plus or minus sign. $ISVALIDNUM rejects all of these strings as invalid, and returns 0.
    
       WRITE !,$ISVALIDNUM($DOUBLE("NAN"))    ; returns 1
       WRITE !,$ISVALIDDOUBLE($DOUBLE("NAN")) ; returns 1
       WRITE !,$ISVALIDNUM("NAN")             ; returns 0
       WRITE !,$ISVALIDDOUBLE("NAN")          ; returns 1
    
     
    
*   Both functions parse signed and unsigned integers (including –0), scientific notation numbers (with “E” or “e”), real numbers (123.45) and numeric strings (“123.45”).
    
*   Neither function recognizes the European DecimalSeparator character (comma (,)) or the NumericGroupSeparator character (American format: comma (,); European format: period (.)). For example, both reject the string “123,456” as an invalid number, regardless of the current locale setting.
    
*   Both functions parse multiple leading signs (+ and –) for numbers. Neither accepts multiple leading signs in a quoted numeric string.
    

If a numeric string is too big to be represented by a Caché floating point number, the default is to automatically convert it to an IEEE double-precision number. However, such large numbers fail the $ISVALIDNUM test, as shown in the following example:

  WRITE !,"E127 no IEEE conversion required"
  WRITE !,$ISVALIDNUM("9223372036854775807E127")
  WRITE !,$ISVALIDDOUBLE("9223372036854775807E127")
  WRITE !,"E128 automatic IEEE conversion"
  WRITE !,$ISVALIDNUM("9223372036854775807E128")
  WRITE !,$ISVALIDDOUBLE("9223372036854775807E128") 

 

$ISVALIDNUM, $NORMALIZE, and $NUMBER Compared

The $ISVALIDNUM, $NORMALIZE, and $NUMBER functions all validate numbers. $ISVALIDNUM returns a boolean value (0 or 1). $NORMALIZE and $NUMBER return a validated version of the specified number.

These three functions offer different validation criteria. Select the one that best meets your needs.

*   American format numbers are validated by all three functions. European format numbers are only validated by the $NUMBER function.
    
*   All three functions parse signed and unsigned integers (including –0), scientific notation numbers (with “E” or “e”), and numbers with a fractional part. However, $NUMBER can be set (using the “I” format) to reject numbers with a fractional part (including scientific notation with a negative base-10 exponent). All three functions parse both numbers (123.45) and numeric strings (“123.45”).
    
*   Leading and trailing zeroes are stripped out by all three functions. The decimal character is stripped out unless followed by a nonzero value.
    
*   DecimalSeparator: $NUMBER validates the decimal character (American format: period (.) or European format: comma (,)) based on its format parameter (or the default for the current locale). The other functions only validate American format decimal numbers, regardless of the current locale setting.
    
*   NumericGroupSeparator: $NUMBER accepts NumericGroupSeparator characters (in American format: comma (,) or blank space; in European format: period (.) or blank space). It accepts and strips out any number of NumericGroupSeparator characters, regardless of position. For example, in American format it validate “12 3,,4,56.9,9” as the number 123456.99. $NORMALIZE does not recognize NumericGroupSeparator characters. It validates character-by-character until it encounters a nonnumeric character; for example, it validates “123,456.99” as the number 123. $ISVALIDNUM rejects the string “123,456” as an invalid number.
    
*   Multiple leading signs (+ and –) are interpreted by all three functions for numbers. However, only $NORMALIZE accepts multiple leading signs in a quoted numeric string.
    
*   Trailing + and – signs: All of the three functions reject trailing signs in numbers. However, in a quoted numeric string $NUMBER parses one (and only one) trailing sign, $NORMALIZE parses multiple trailing signs, and $ISVALIDNUM rejects any string containing a trailing sign as an invalid number.
    
*   Parentheses: $NUMBER parses parentheses surrounding an unsigned number in a quoted string as indicating a negative number. $NORMALIZE and $ISVALIDNUM reject parentheses.
    
*   Numeric strings containing multiple decimal characters: $NORMALIZE validates character-by-character until it encounters the second decimal character. For example, in American format it validates “123.4.56” as the number 123.4. $NUMBER and $ISVALIDNUM reject any string containing more than one decimal character as an invalid number.
    
    Numeric strings containing other nonnumeric characters: $NORMALIZE validates character-by-character until it encounters an alphabetic character. It validates “123A456” as the number 123. $NUMBER and $ISVALIDNUM validate the entire string, they reject “123A456” as an invalid number.
    
*   The null string: $NORMALIZE parses the null string as zero (0). $NUMBER and $ISVALIDNUM reject the null string.
    

The $ISVALIDNUM and $NUMBER functions provide optional min/max range checking.

$ISVALIDNUM, $NORMALIZE, and $NUMBER all provide rounding of numbers to a specified number of fractional digits. $ISVALIDNUM and $NORMALIZE can round fractional digits, and round or truncate a number with a fractional part to return an integer. For example, $NORMALIZE can round 488.65 to 488.7 or 489, or truncate it to 488. $NUMBER can round both fractional digits and integer digits. For example, $NUMBER can round 488.65 to 488.7, 489, 490 or 500.

See Also

*   [$DOUBLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fdouble) function
    
*   [$FNUMBER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ffnumber) function
    
*   [$INUMBER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_finumber) function
    
*   [$ISVALIDDOUBLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fisvaliddouble) function
    
*   [$NORMALIZE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fnormalize) function
    
*   [$NUMBER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fnumber) function
    
*   More information on locales in the section on “[System Classes for National Language Support](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSTU_customize#GSTU_customize_nls)” in Caché Specialized System Tools and Utilities.
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fisvaliddouble "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fjustify "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fisvalidnum.xml**