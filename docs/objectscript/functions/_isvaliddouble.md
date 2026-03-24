# $ISVALIDDOUBLE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fisvaliddouble

---

 

Caché ObjectScript Reference

$ISVALIDDOUBLE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fisobject "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fisvalidnum "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$ISVALIDDOUBLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fisvaliddouble "$ISVALIDDOUBLE") \]

Go to: Description Parameters Examples Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Validates a $DOUBLE numeric value and returns a boolean; optionally provides range checking.

Synopsis

$ISVALIDDOUBLE(num,scale,min,max)

Parameters

num

The numeric value to be validated. It can be a numeric or string value, a variable name, or any valid Caché ObjectScript expression. If a valid number, num is converted to a IEEE double-precision floating point type.

scale

Optional — The number of significant decimal digits for min and max range comparisons.

min

Optional — The minimum permitted numeric value. The value you supply is converted to a IEEE double-precision floating point type. If not specified, min defaults to $DOUBLE(“-INF”).

max

Optional — The maximum permitted numeric value. The value you supply is converted to a IEEE double-precision floating point type. If not specified, max defaults to $DOUBLE(“INF”).

Description

The $ISVALIDDOUBLE function validates whether num is IEEE double-precision floating point number and returns a boolean value. It optionally performs a range check using min and max values, which are automatically converted to IEEE numbers. The scale parameter is used during range checking to specify how many fractional digits to compare. A boolean value of 1 means that num is a properly formed $DOUBLE number and passes the range check, if one is specified.

$ISVALIDDOUBLE validates American format numbers, which use a period (.) as the decimal separator. It does not validate European format numbers, which use a comma (,) as the decimal separator. $ISVALIDNUM does not consider valid a number that contains numeric group separators; it returns 0 (invalid) for any number containing a comma or a blank space, regardless of the current locale.

Parameters

num

The number to be validated may be an integer, a fractional number, a number in scientific notation (with the letter “E” or “e”). It may be a string, expression, or variable that resolves to a number. It may be signed or unsigned, and may contain leading or trailing zeros. Validation fails ($ISVALIDDOUBLE returns 0) if:

*   num contains any characters other than the digits 0–9, a leading + or – sign, a decimal point (.), and a letter “E” or “e”.
    
*   num contains more than one + or – sign, decimal point, or letter “E” or “e”.
    
*   The optional + or – sign is not the first character of num.
    
*   The letter “E” or “e” indicating a base-10 exponent is not followed by an integer in a numeric string. With a number, “E” not followed by an integer results in a <SYNTAX> error.
    
*   num is the null string.
    

If num is INF (with or without a + or – sign), it is a valid $DOUBLE number; the boolean value returned by $ISVALIDDOUBLE depends on whether num passes the specified range check.

If num is NAN $ISVALIDDOUBLE returns 1.

scale

The scale parameter is used during range checking to specify how many fractional digits to compare. Specify an integer value for scale; any fractional digits in the scale value are ignored. You can specify a scale value larger than the number of fractional digits specified in the other parameters. You can specify a scale value of –1; all other negative scale values result in a <FUNCTION> error.

A nonnegative scale value causes num to be rounded to that number of fractional digits before performing min and max range checking. A scale value of 0 causes num to be rounded to an integer value (3.9 = 4) before performing range checking. A scale value of –1 causes num to be truncated to an integer value (3.9 = 3) before performing range checking. To compare all specified digits without rounding or truncating, omit the scale parameter. A scale value that is nonnumeric or the null string is equivalent to a scale value of 0.

Rounding is performed for all scale values except –1. A value of 5 or greater is always rounded up.

The scale parameter value causes evaluation using rounded or truncated versions of the num value. The actual value of the num variable is not changed by $ISVALIDDOUBLE processing.

If you omit the scale parameter, retain the comma as a place holder.

min and max

You can specify a minimum allowed value, a maximum allowed value, neither, or both. If specified, the num value (after the scale operation) must be greater than or equal to the min value, and less than or equal to the max value. The values are converted to IEEE floating point numbers before being used for range checking. A null string as a min or max value is equal to zero. If a value does not meet these criteria, $ISVALIDDOUBLE returns 0.

The NAN value is always valid, regardless of the min or max value.

If you omit a parameter, retain the comma as a place holder. For example, when omitting scale and specifying min or max, or when omitting min and specifying max. Trailing commas are ignored.

Examples

In the following example, each invocation of $ISVALIDDOUBLE returns 1 (valid number):

   WRITE !,$ISVALIDDOUBLE(0)        ; All integers OK
   WRITE !,$ISVALIDDOUBLE(4.567)    ; Fractional numbers OK
   WRITE !,$ISVALIDDOUBLE("4.567")  ; Numeric strings OK
   WRITE !,$ISVALIDDOUBLE(\-.0)      ; Signed numbers OK
   WRITE !,$ISVALIDDOUBLE(+004.500) ; Leading/trailing zeroes OK
   WRITE !,$ISVALIDDOUBLE(4E2)      ; Scientific notation OK   

 

In the following example, each invocation of $ISVALIDDOUBLE returns 0 (invalid number):

   WRITE !,$ISVALIDDOUBLE("")      ; Null string is invalid
   WRITE !,$ISVALIDDOUBLE("4,567") ; Commas are not permitted
   WRITE !,$ISVALIDDOUBLE("4A")    ; Invalid character

 

In the following example, each invocation of $ISVALIDDOUBLE returns 1 (valid number), even though INF (infinity) and NAN (Not A Number) are, strictly speaking, not numbers:

   WRITE !,$ISVALIDDOUBLE($DOUBLE($ZPI))  ; DOUBLE numbers OK
   WRITE !,$ISVALIDDOUBLE($DOUBLE("INF")) ; DOUBLE INF OK
   WRITE !,$ISVALIDDOUBLE($DOUBLE("NAN")) ; DOUBLE NAN OK

 

In the following example, specifying a min value eliminates -INF but not INF:

   WRITE !,$ISVALIDDOUBLE($DOUBLE("-INF"),,99999999999)
   WRITE !,$ISVALIDDOUBLE($DOUBLE("INF"),,99999999999)

 

The following example shows the use of the min and max parameters. All of the following return 1 (number is valid and also passes the range check):

   WRITE !,$ISVALIDDOUBLE(4,,3,5)    ; scale can be omitted
   WRITE !,$ISVALIDDOUBLE(4,2,3,5)   ; scale can be larger than 
                                     ; number of fractional digits
   WRITE !,$ISVALIDDOUBLE(4,0,,5)    ; min or max can be omitted
   WRITE !,$ISVALIDDOUBLE(4,0,4,4)   ; min and max are inclusive
   WRITE !,$ISVALIDDOUBLE(\-4,0,\-5,5) ; negative numbers
   WRITE !,$ISVALIDDOUBLE(4.00,2,04,05) ; leading/trailing zeros
   WRITE !,$ISVALIDDOUBLE(.4E3,0,3E2,400) ; base-10 exponents expanded

 

The following example shows the use of the scale parameter with min and max. All of the following return 1 (number is valid and also passes the range check):

   WRITE !,$ISVALIDDOUBLE(4.55,,4.54,4.551)
     ; When scale is omitted, all digits of num are checked.
   WRITE !,$ISVALIDDOUBLE(4.1,0,4,4.01)
     ; When scale=0, num is rounded to an integer value
     ; (0 fractional digits) before min & max check.
   WRITE !,$ISVALIDDOUBLE(3.85,1,3.9,5)
     ; num is rounded to 1 fractional digit, 
     ; (with values of 5 or greater rounded up) 
     ; before min check.
   WRITE !,$ISVALIDDOUBLE(4.01,17,3,5) 
     ; scale can be larger than number of fractional digits.
   WRITE !,$ISVALIDDOUBLE(3.9,\-1,2,3)
     ; When scale=-1, num is truncated to an integer value

 

Notes

$ISVALIDDOUBLE and $ISVALIDNUM Compared

The $ISVALIDDOUBLE and [$ISVALIDNUM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fisvalidnum) functions both validate American format numbers and return a boolean value (0 or 1).

*   Both functions accept as valid numbers the INF, –INF, and NAN values returned by $DOUBLE. $ISVALIDDOUBLE also accepts as valid numbers the not case-sensitive strings “NAN” and “INF”, as well as the variants “Infinity” and “sNAN”, and any of these strings beginning with a single plus or minus sign. $ISVALIDNUM rejects all of these strings as invalid, and returns 0.
    
       WRITE !,$ISVALIDNUM($DOUBLE("NAN"))    ; returns 1
       WRITE !,$ISVALIDDOUBLE($DOUBLE("NAN")) ; returns 1
       WRITE !,$ISVALIDNUM("NAN")             ; returns 0
       WRITE !,$ISVALIDDOUBLE("NAN")          ; returns 1
    
     
    
*   Both functions parse signed and unsigned integers (including –0), scientific notation numbers (with “E” or “e”), real numbers (123.45) and numeric strings (“123.45”).
    
*   Neither function recognizes the European DecimalSeparator character (comma (,)) or the NumericGroupSeparator character (American format: comma (,); European format: period (.)). For example, both reject the string “123,456” as an invalid number, regardless of the current locale setting.
    
*   Both functions parse multiple leading signs (+ and –) for numbers. Neither accepts multiple leading signs in a quoted numeric string.
    

See Also

*   [$DOUBLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fdouble) function
    
*   [$FNUMBER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ffnumber) function
    
*   [$INUMBER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_finumber) function
    
*   [$ISVALIDNUM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fisvalidnum) function
    
*   [$NORMALIZE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fnormalize) function
    
*   [$NUMBER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fnumber) function
    
*   More information on locales in the section on “[System Classes for National Language Support](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSTU_customize#GSTU_customize_nls)” in Caché Specialized System Tools and Utilities.
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fisobject "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fisvalidnum "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fisvaliddouble.xml**