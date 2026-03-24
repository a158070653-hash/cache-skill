# $NORMALIZE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fnormalize

---

 

Caché ObjectScript Reference

$NORMALIZE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fnext "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fnow "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$NORMALIZE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fnormalize "$NORMALIZE") \]

Go to: Description Parameters Examples Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Validates and returns a numeric value; rounds to a specified precision.

Synopsis

$NORMALIZE(num,scale)

Parameters

num

The numeric value to be validated. It can be a numeric or string value, a variable name, or any valid Caché ObjectScript expression.

scale

The number of significant digits to round num to as the returned value. This number can be larger or smaller than the actual number of fractional digits in num. Permitted values are 0 (round to integer), –1 (truncate to integer), and positive integers (round to specified number of fractional digits). There is no maximum scale value. However, the functional maximum cannot exceed the numeric precision. For standard Caché fractional numbers the functional scale maximum is 18 (minus the number of integer digits – 1).

Description

The $NORMALIZE function validates num and returns the normalized form of num. It performs rounding (or truncation) of fractional digits using the scale parameter. You can use the scale parameter to round a real number to a specified number of fractional digits, to round a real number to an integer, or to truncate a real number to an integer.

After rounding, $NORMALIZE removes trailing zeros from the return value. For this reason, the number of fractional digits returned may be less than the number specified in scale, as shown in the following example:

  WRITE $NORMALIZE($ZPI,11),!
  WRITE $NORMALIZE($ZPI,12),!   /\* trailing zero removed \*/
  WRITE $NORMALIZE($ZPI,13),!
  WRITE $NORMALIZE($ZPI,14)

 

Parameters

num

The number to be validated may be an integer, a real number, or a scientific notation number (with the letter “E” or “e”). It may be a string, expression, or variable that resolves to a number. It may be signed or unsigned, and may contain leading or trailing zeros. $NORMALIZE validates character-by-character. It stops validation and returns the validated portion of the string if:

*   num contains any characters other than the digits 0–9, + or – signs, a decimal point (.), and a letter “E” or “e”.
    
*   num contains more than decimal point, or letter “E” or “e”.
    
*   If a + or – sign is found after a numeric in num it is considered a trailing sign, and no further numerics are parsed.
    
*   The letter “E” or “e” is not followed by an integer.
    

The scale parameter value causes the returned value to be a rounded or truncated version of the num value. The actual value of the num variable is not changed by $NORMALIZE processing.

scale

The mandatory scale parameter is used to specify how many fractional digits to round to. Depending on the value specified, scale can have no effect on fractional digits, round to a specified number of fractional digits, round to an integer, or truncate to an integer.

A nonnegative scale value causes num to be rounded to that number of fractional digits. When rounding, a value of 5 or greater is always rounded up. To avoid rounding a number, make scale larger than the number of possible fractional digits in num. A scale value of 0 causes num to be rounded to an integer value (3.9 = 4). A scale value of –1 causes num to be truncated to an integer value (3.9 = 3). A scale value which is nonnumeric or the null string is equivalent to a scale value of 0.

Specify an integer value for scale; decimal digits in the scale value are ignored. You can specify a scale value larger than the number of decimal digits specified in num. You can specify a scale value of –1; all other negative scale values result in a <FUNCTION> error.

Examples

In the following example, each invocation of $NORMALIZE returns the normalized version of num with the specified rounding (or integer truncation):

   WRITE !,$NORMALIZE(0,0)        ; All integers OK
   WRITE !,$NORMALIZE("",0)       ; Null string is parsed as 0
   WRITE !,$NORMALIZE(4.567,2)    ; Real numbers OK
   WRITE !,$NORMALIZE("4.567",2)  ; Numeric strings OK
   WRITE !,$NORMALIZE(\-+.0,99)    ; Leading/trailing signs OK
   WRITE !,$NORMALIZE(+004.500,1) ; Leading/trailing 0's OK
   WRITE !,$NORMALIZE(4E2,\-1)     ; Scientific notation OK   

 

In the following example, each invocation of $NORMALIZE returns a numeric subset of num:

   WRITE !,$NORMALIZE("4,567",0)
     ; NumericGroupSeparators (commas) are not recognized
     ; here validation halts at the comma, and 4 is returned.
   WRITE !,$NORMALIZE("4A",0)
     ; Invalid (nonnumeric) character halts validation
     ; here 4 is returned.

 

The following example shows the use of the scale parameter to round (or truncate) the return value:

   WRITE !,$NORMALIZE(4.55,2)
     ; When scale is equal to the fractional digits of num,
     ; all digits of num are returned without rounding.
   WRITE !,$NORMALIZE(3.85,1)
     ; num is rounded to 1 fractional digit, 
     ; (with values of 5 or greater rounded up) 
     ; here 3.9 is returned.
   WRITE !,$NORMALIZE(4.01,17) 
     ; scale can be larger than number of fractional digits,
     ; and no rounding is peformed; here 4.01 is returned.
   WRITE !,$NORMALIZE(3.85,0)
     ; When scale=0, num is rounded to an integer value.
     ; here 4 is returned.
   WRITE !,$NORMALIZE(3.85,\-1)
     ; When scale=-1, num is truncated to an integer value.
     ; here 3 is returned.

 

Notes

$DOUBLE Numbers

$DOUBLE IEEE floating point numbers are encoded using binary notation. Most decimal fractions cannot be exactly represented in this binary notation. When a $DOUBLE value is input to $NORMALIZE with a scale value, the return value frequently contains more fractional digits than specified in scale because the fractional decimal result is not representable in binary, so the return value must be rounded to the nearest representable $DOUBLE value, as shown in the following example:

  SET x\=1234.1234
  SET y\=$DOUBLE(1234.1234)
  WRITE "Decimal: ",$NORMALIZE(x,2),!
  WRITE "Double: ",$NORMALIZE(y,2),!
  WRITE "Dec/Dub: ",$NORMALIZE($DECIMAL(y),2)

 

If you are normalizing a $DOUBLE value for decimal formatting, you should convert the $DOUBLE value to decimal representation before normalizing the result, as shown in the above example.

$NORMALIZE handles $DOUBLE("INF") and $DOUBLE("NAN") values, and returns INF and NAN.

DecimalSeparator Value Ignored

$NORMALIZE is intended to operate on numbers, not strings. It converts num to a Caché canonical number prior to performing normalization. This is the same as appending a + sign to a string to force its interpretation as a number. This conversion to a canonical number does not use the DecimalSeparator property value for the current locale.

For example, of you specify for num the string "00123.4500", $NORMALIZE treats this as the canonical number 123.45, regardless of the current DecimalSeparator value. If you specify the string "00123,4500", $NORMALIZE treats this as the canonical number 123 (truncating at the first non-numeric character), regardless of the current DecimalSeparator value.

If you want a function that takes a string as input, use [$INUMBER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_finumber). If you want a function that produces a string result use [$FNUMBER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ffnumber).

$NORMALIZE, and $NUMBER Compared

The $NORMALIZE, and $NUMBER functions both validate numbers and return a validated version of the specified number.

These two functions offer different validation criteria. Select the one that best meets your needs.

*   Both functions parse signed and unsigned integers (including –0), scientific notation numbers (with “E” or “e”), and real numbers. However, $NUMBER can be set (using the “I” format) to reject numbers with a fractional part (including scientific notation with a negative base-10 exponent). Both functions parse both numbers (123.45) and numeric strings (“123.45”).
    
*   Both functions strip out leading and trailing zeroes. The decimal character is stripped out unless followed by a nonzero value.
    
*   Numeric strings containing a NumericGroupSeparator: $NUMBER parses NumericGroupSeparator characters (American format: comma (,); European format: period (.) or apostrophe (')) and the decimal character (American format: period (.) or European format: comma (,)) based on its format parameter (or the default for the current locale). It accepts and strips out any number of NumericGroupSeparator characters. For example, in American format it validate “123,,4,56.99” as the number 123456.99. $NORMALIZE does not recognize NumericGroupSeparator characters. It validates character-by-character until it encounters a nonnumeric character; for example, it validates “123,456.99” as the number 123.
    
*   Multiple leading signs (+ and –) are interpreted by both functions for numbers. Only $NORMALIZE accepts multiple leading signs in a quoted numeric string.
    
*   Trailing + and – signs: Both functions reject trailing signs in numbers. In a quoted numeric string $NUMBER parses one (and only one) trailing sign. $NORMALIZE parses multiple trailing signs.
    
*   Parentheses: $NUMBER parses parentheses surrounding an unsigned number in a quoted string as indicating a negative number. $NORMALIZE treats parentheses as nonnumeric characters.
    
*   Numeric strings containing multiple decimal characters: $NORMALIZE validates character-by-character until it encounters the second decimal character. For example, in American format it validates “123.4.56” as the number 123.4. $NUMBER rejects any string containing more than one decimal character as an invalid number.
    
    Numeric strings containing other nonnumeric characters: $NORMALIZE validates character-by-character until it encounters an alphabetic character. It validates “123A456” as the number 123. $NUMBER validates or rejects the entire string; it reject “123A456” as an invalid number.
    
*   The null string: $NORMALIZE parses the null string as zero (0). $NUMBER rejects the null string.
    

The $NUMBER function provide optional min/max range checking. This is also available using the $ISVALIDNUM function.

$NORMALIZE, $NUMBER, and $ISVALIDNUM all provide rounding of numbers to a specified number of fractional digits. $NORMALIZE can round fractional digits, and round or truncate a real number to return an integer. For example, $NORMALIZE can round 488.65 to 488.7 or 489, or truncate it to 488. $NUMBER can round real numbers or integers. For example, $NUMBER can round 488.65 to 488.7, 489, 490 or 500.

See Also

*   [$DOUBLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fdouble) function
    
*   [$FNUMBER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ffnumber) function
    
*   [$INUMBER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_finumber) function
    
*   [$ISVALIDNUM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fisvalidnum) function
    
*   [$NUMBER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fnumber) function
    
*   More information on locales in the section on “[System Classes for National Language Support](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSTU_customize#GSTU_customize_nls)” in Caché Specialized System Tools and Utilities.
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fnext "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fnow "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fnormalize.xml**