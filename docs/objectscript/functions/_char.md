# $CHAR

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fchar

---

 

Caché ObjectScript Reference

$CHAR

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fcase "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fclassmethod "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$CHAR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fchar "$CHAR") \]

Go to: Description Parameter Examples Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Converts the integer value of an expression to the corresponding ASCII or Unicode character.

Synopsis

$CHAR(expression,...)
$C(expression,...)

Parameter

expression

The integer value to be converted.

Description

$CHAR returns the character that corresponds to the decimal (base-10) integer value specified by expression. This character can be an 8-bit (ASCII) character, or a 16-bit (Unicode) character. For 8-bit characters, the value in expression must evaluate to a positive integer in the range 0 to 255. For 16-bit characters, specify integers in the range 256 through 65535 (hex FFFF). Values larger than 65535 return an empty string. Values from 65536 (hex 10000) through 1114111 (hex 10FFFF) are used to represent Unicode surrogate pairs; these characters can be returned using $WCHAR. On an 8-bit Caché installation, $CHAR returns the empty string for expression values greater than 255.

You can specify expression as a comma-separated list, in which case $CHAR returns the corresponding character for each expression in the list.

Parameter

expression

The expression can be an integer value, the name of a variable that contains an integer value, or any valid Caché ObjectScript expression that evaluates to an integer value. To return characters for multiple integer values, specify a comma-separated list of expressions.

Examples

The following example uses $CHAR in a FOR loop to output the characters for all ASCII codes in the range 65 to 90. These are the uppercase alphabetic characters.

  FOR i\=65:1:90 {
     WRITE !,$CHAR(i) }

 

The following two examples show the use of multiple expression values. The first returns “AB” and the second returns “AaBbCcDdEeFfGgHhIiJjKk”:

  WRITE $CHAR(65,66),!
  FOR i\=65:1:75 { 
     WRITE $CHAR(i,i+32) }

 

The following example shows the use of a multibyte character. The character corresponding to the integer 960 is the symbol for pi:

   IF $SYSTEM.Version.IsUnicode()  {
   WRITE $CHAR(960) }
   ELSE {WRITE "This example requires a Unicode version of Caché"}

 

Notes

$CHAR with the WRITE Command

When you use $CHAR to write characters with the WRITE command, the output characters reset the positions of the special variables $X and $Y. This is true even for the NULL character (ASCII 0), which is not the same as a null string (""). As a rule, you should use $CHAR with caution when writing nonprinting characters, because such characters may produce unpredictable cursor positioning and screen behavior.

Numeric Values in $CHAR Arguments

You can use signed numeric values for expression. Caché ignores negative numbers and only evaluates positive or unsigned numbers. In the following example, the $CHAR with signed integers returns only the first and third expression, ignoring the second expression, which is a negative integer.

  WRITE !,$CHAR(65,66,67)
  WRITE !,$CHAR(+65,\-66,67)

 

ABC

AC

You can use floating point numeric values for expression. Caché ignores the fractional portion of the argument and only considers the integer portion. In the following example, $CHAR ignores the fractional portion of the number and produces the character represented by character code 65, an uppercase A.

   WRITE $CHAR(65.5)

 

Unicode Support

On a Unicode installation of Caché, $CHAR supports Unicode characters when represented by decimal (base-10) integers. On an 8-bit installation of Caché, $CHAR returns the empty string for integers greater than 255.

The Unicode value for a character is usually expressed as a 4-digit number in hexadecimal notation, using the digits 0-9 and the letters A-F. However, standard functions in the Caché ObjectScript language generally identify characters according to ASCII codes, which are decimal values, not hexadecimal.

Hence, the $CHAR function supports Unicode encoding by returning a character based on the decimal Unicode value that was input, not the more standard hexadecimal value. To convert a decimal number to hexadecimal, use the [$ZHEX](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzhex) function.

Surrogate Pairs

$CHAR does not recognize surrogate pairs. A surrogate pair is a pair of 16-bit Unicode characters that together encode a single ideographic character. Surrogate pairs are used to represent some Chinese characters and to support the Japanese JIS2004 standard. You can use the $WISWIDE function to determine if a string contains a surrogate pair. The $WCHAR function recognizes and correctly parses surrogate pairs. $CHAR and $WCHAR are otherwise identical. However, because $CHAR is generally faster than $WCHAR, $CHAR is preferable for all cases where a surrogate pair is not likely to be encountered.

Note:

$WCHAR should not be confused with $ZWCHAR, which always parses characters in pairs.

Functions Related to $CHAR

The $ASCII function is the inverse of $CHAR. You can use it to convert a character to its equivalent numeric value. $ASCII converts all characters, including Unicode characters. In addition, all Caché platforms support the related functions, $ZLCHAR and $ZWCHAR. They are similar to $CHAR, but operate on a word (two bytes) or a long word (four bytes). You can use $ZISWIDE to determine if there are any multibyte (“wide”) characters in the expression of $CHAR.

See Also

*   [READ](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cread) command
    
*   [WRITE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cwrite) command
    
*   [$ASCII](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fascii) function
    
*   [$WCHAR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwchar) function
    
*   [$WISWIDE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwiswide) function
    
*   [$ZHEX](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzhex) function
    
*   [$ZLCHAR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzlchar) function
    
*   [$ZWCHAR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzwchar) function
    
*   [$X](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vx) special variable
    
*   [$Y](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vy) special variable
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fcase "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fclassmethod "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fchar.xml**