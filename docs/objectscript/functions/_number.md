# $NUMBER

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fnumber

---

 

Caché ObjectScript Reference

$NUMBER

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fnow "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_forder "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$NUMBER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fnumber "$NUMBER") \]

Go to: Description Parameters Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Validates and returns a numeric value; optionally provides rounding and range checking.

Synopsis

$NUMBER(num,format,min,max)
$NUM(num,format,min,max)

Parameters

num

The numeric value to be validated and then converted to Caché canonical form. It can be a numeric or string value, a variable name, or any valid Caché ObjectScript expression.

format

Optional — Specifies which processing options to apply to num. These processing options dictate primarily how to recognize and handle numbers containing decimal points.

min

Optional — The minimum acceptable numeric value.

max

Optional — The maximum acceptable numeric value.

Description

The $NUMBER function converts and validates the num numeric value using the specified format. It accepts numbers supplied with a variety of punctuation formats and returns numbers in Caché canonical form. You can use format to test whether a number is an integer. If min or max are specified, the number must fall within that range of values.

$NUMBER can be used for American format numbers, European format numbers, and Russian/Czech format numbers.

Using $NUMBER on the [$DOUBLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fdouble) values INF, –INF, or NAN always returns the empty string.

Parameters

format

The possible format codes are as follows. These format codes may be specified in any order. A nonnumeric format must be specified as a quoted string. Any or all of the following format codes may be omitted. If format is invalid, $NUMBER generates a <SYNTAX> error.

*   Decimal character: either “.” or “,” indicating whether to use the American (“.”) or European (“,”) convention for validating the decimal point. You can specify either of these characters, or no decimal character. If you omit the decimal character, the number takes the DecimalSeparator of the current locale.
    
*   Rounding factor: an integer indicating how many digits to round to. This integer can be preceded by an optional + or - sign. If the rounding factor is positive (or unsigned) the number is rounded to the specified number of fractional digits. If the rounding factor is 0, the number is rounded to an integer. If the rounding factor is a negative integer, the number is rounded the indicated number of places to the left of the decimal separator. For example, a rounding factor of -2 rounds 234.45 to 200. The number “5” is always rounded up; thus a rounding factor of 1 rounds 123.45 to 123.5.
    
*   Integer indicator: the letter “I” (uppercase or lowercase) which specifies that the number must resolve to an integer. For example, –07.00 resolves to an integer, but –07.01 does not. If the number does not resolve to an integer, $NUMBER returns the null string. In the following example, only the first three $NUMBER functions returns an integer. The other three return the null string:
    
      WRITE $NUMBER(\-07.00,"I"),"  non-canonical integer numeric",!
      WRITE $NUMBER(+"-07.00","I")," string forced as integer numeric",!
      WRITE $NUMBER("-7","I")," canonical integer string numeric",!
      WRITE $NUMBER("-07.00","I")," non-canonical integer string numeric",!
      WRITE $NUMBER(\-07.01,"I")," fractional numeric",!
      WRITE $NUMBER("-07.01","I")," fractional string numeric",!
    
     
    

min and max

You can specify a minimum allowed value, a maximum allowed value, neither, or both. If specified, the num value (after rounding) must be greater than or equal to the min value, and less than or equal to the max value. A null string as a min or max value is equal to zero. If a value does not meet these criteria, $NUMBER returns the null string.

Thus in the following examples, the first is valid because num (4.0) equals max (4). The second is valid because num (4.003) still equals max (4) within the format range (two fractional digits). However, the third is not valid because $NUMBER rounds num up to a value (4.01) greater than max within the format range. It returns a null string.

   WRITE !,$NUMBER(4.0,2,0,4)
   WRITE !,$NUMBER(4.003,2,0,4)
   WRITE !,$NUMBER(4.006,2,0,4)  

 

You can omit parameters, retaining the commas as place holders. The first line of the following example sets a max value, but no format or min value. The second line sets no format value, but sets a min value of the null string, which is equivalent to zero. Thus the first line returns –7, and the second line fails the min criteria and returns the null string.

   SET max\=10
   WRITE !,$NUMBER(\-7,,,max)
   WRITE !,$NUMBER(\-7,,"",max)

 

You cannot specify trailing commas. The following results in a <SYNTAX> error:

   WRITE $NUMBER(mynum,,min,)

Notes

Order of Operations

$NUMBER performs the following series of conversions and validations. If the number fails any validation step, $NUMBER returns a null string (""). If the number passes all validation steps, $NUMBER returns the resulting converted Caché canonical form number.

1.  $NUMBER uses the decimal character format to determine which character is the group separator and strips out all group separator characters (regardless of their location in the number). It uses the following rule: If the decimal character specified in format is a period (.), then the group separator is a comma (,) or blank space. If the decimal character specified in format is a comma (,), then the group separator is a period (.) or blank space. If no decimal character is specified in format, the group separator is the NumericGroupSeparator property of the current locale. (The Russian (rusw), Ukrainian (ukrw), and Czech (csyw) locales use a blank space as the numeric group separator.)
    
2.  $NUMBER validates that the number is well-formed. A well-formed number can contain any of the following:
    
    *   Numbers
        
    *   An optional decimal indicator character, as defined above (one or none, but not more than one).
        
    *   An optional plus (+) or minus (-) sign character (leading or trailing, but not more than one).
        
    *   Optional parentheses enclosing the number to indicate a negative value (debit). The number within the parentheses cannot have a sign character.
        
    *   An optional base-10 exponent, indicated by an “E” (uppercase or lowercase) followed by an integer. If “E” is specified, an exponent integer must be present. The exponent integer may be preceded by a sign character.
        
3.  If the integer indicator is present in format, $NUMBER checks for integers. An integer cannot contain a decimal indicator character. Numeric strings (“123.45”) and numbers (123.45) are parsed differently. Numeric strings fail this integer test even if there are no digits following the decimal indicator character, or if expansion of scientific notation or rounding would eliminate the fractional digits. Numbers pass these validation tests. If a number fails the integer indicator check, $NUMBER returns the null string ("").
    
4.  $NUMBER converts the number to a Caché canonical form number. It expands scientific notation, replaces enclosing parentheses with a negative sign character, strips off leading and trailing zeros, and deletes a decimal indicator character if it is not followed by any nonzero digits.
    
5.  $NUMBER uses the rounding factor (if present) to round the number the specified number of digits. It then strips off any leading or trailing zeros and the decimal indicator character if it is not followed by any digits.
    
6.  $NUMBER validates the number against the minimum value, if specified.
    
7.  $NUMBER validates the number against the maximum value, if specified.
    
8.  $NUMBER returns the resulting number.
    

European and American Decimal Separators

$NUMBER returns a number in canonical form, removing all numeric group separators and includes at most one decimal separator character. You can use the format values “,” or “.” to identify the decimal separator used in num; by specifying the decimal separator, you are also implicitly specifying the numeric group separator.

To determine the DecimalSeparator character for your locale, invoke the following method:

  WRITE ##class(%SYS.NLS.Format).GetFormatItem("DecimalSeparator")

 

To determine the NumericGroupSeparator character and NumericGroupSize number for your locale, invoke the following methods:

  WRITE ##class(%SYS.NLS.Format).GetFormatItem("NumericGroupSeparator"),!
  WRITE ##class(%SYS.NLS.Format).GetFormatItem("NumericGroupSize")

 

In following examples, a comma is specified as the decimal separator:

  SET num\="123,456"
  WRITE !,$NUMBER(num,",")  
     // converts to the fractional number "123.456"
     // (comma is identified as decimal separator)
  SET num\="123,45,6"
  WRITE !,$NUMBER(num,",")  
    // returns the null string 
    // (invalid number, too many decimal separators)
  SET num\="123.456"
  WRITE !,$NUMBER(num,",")
    // converts to the integer "123456"
    // removing group separator 
    // (if comma is decimal, then period is group separator)
  SET num\="123.4.56"
  WRITE !,$NUMBER(num,",")  
    // converts to the integer "123456"
    // removing group separators 
    // (number and placement of group separators ignored)

 

Rounding and Precision

When rounding numbers, be aware that IEEE floating point numbers and standard Caché fractional numbers differ in precision. $DOUBLE IEEE floating point numbers are encoded using binary notation. They have a precision of 53 binary bits, which corresponds to 15.95 decimal digits of precision. (Note that the binary representation does not correspond exactly to a decimal fraction.) Because most decimal fractions cannot be exactly represented in this binary notation, an IEEE floating point number may differ slightly from the corresponding standard Caché floating point number. Standard Caché fractional numbers have a precision of 18 decimal digits on all supported Caché system platforms. When an IEEE floating point number is displayed as a fractional number, the binary bits are often converted to a fractional number with far more than 18 decimal digits. This does not mean that IEEE floating point numbers are more precise than standard Caché fractional numbers.

See Also

*   [$DOUBLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fdouble) function
    
*   [$FNUMBER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ffnumber) function
    
*   [$INUMBER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_finumber) function
    
*   [$ISVALIDNUM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fisvalidnum) function
    
*   [$NORMALIZE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fnormalize) function
    
*   More information on locales in the section on “[System Classes for National Language Support](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSTU_customize#GSTU_customize_nls)” in Caché Specialized System Tools and Utilities.
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fnow "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_forder "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fnumber.xml**