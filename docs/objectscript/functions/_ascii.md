# $ASCII

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fascii

---

 

Caché ObjectScript Reference

$ASCII

 [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fbit "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$ASCII](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fascii "$ASCII") \]

Go to: Description Parameters Examples Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Converts a character to a numeric code.

Synopsis

$ASCII(expression,position)
$A(expression,position)

Parameters

expression

The character to be converted.

position

Optional — The position of a character within a character string, counting from 1. The default is 1.

Description

$ASCII returns the character code value for a single character specified in expression. This character can be an 8-bit (extended ASCII) character or a 16-bit (Unicode) character. The returned value is a positive integer.

The expression parameter may evaluate to a single character or to a string of characters. If expression evaluates to a string of characters, you can include the optional position parameter to indicate which character you want to convert.

Parameters

expression

An expression that evaluates to a quoted string of one or more characters. The expression can be specified as the name of a variable, a numeric value, a string literal, or any valid Caché ObjectScript expression. If expression yields a string of more than one character, use position to select the desired character. If you omit position for a character string, $ASCII returns the numeric code for the first character. $ASCII returns -1 if the expression evaluates to a null string.

position

The position must be specified as a nonzero positive integer. It may be signed or unsigned. You can use a noninteger numeric value in position; however, Caché ignores the fractional portion and only considers the integer portion of the numeric value. If you do not include position, $ASCII returns the numeric value of the first character in expression. $ASCII returns -1 if the integer value of position is larger than the number of characters in expression or less than 1.

Examples

The following example returns 87, the ASCII numeric value of the character W.

   WRITE $ASCII("W")

 

The following example returns 960, the numeric equivalent for the Unicode character "pi".

   WRITE $ASCII($CHAR(959+1))

 

The following example returns 84, the ASCII numeric equivalent for the first character in the variable Z.

  SET Z\="TEST"
  WRITE $ASCII(Z)

 

The following example returns 83, the ASCII numeric equivalent for the third character in the variable Z.

   SET Z\="TEST"
   WRITE $ASCII(Z,3)

 

The following example returns a -1 because the second argument specifies a position greater than the number of characters in the string.

   SET Z\="TEST"
   WRITE $ASCII(Z,5)

 

The following example uses $ASCII in a FOR loop to convert all of the characters in variable x to their ASCII numeric equivalents. The $ASCII reference includes the position parameter, which is updated for each execution of the loop. When position reaches a number that is greater than the number of characters in x, $ASCII returns a value of -1, which terminates the loop.

  SET x\="abcdefghijklmnopqrstuvwxyz"
  FOR i\=1:1 { 
    QUIT:$ASCII(x,i)\=\-1 
    WRITE !,$ASCII(x,i) 
    }
  QUIT

 

The following example generates a simple checksum for the string X. When $CHAR(CS) is concatenated with the string, the checksum of the new string is always zero. Therefore, validation is simplified.

CXSUM
  SET x\="Now is the time for all good men to come to the aid of their party"
  SET CS\=0
  FOR i\=1:1:$LENGTH(x) {
    SET CS\=CS+$ASCII(x,i)
    WRITE !,"Checksum is:",CS
    }
  SET CS\=128\-CS#128
  WRITE !,"Final checksum is:",CS

 

The following example converts a lowercase or mixed-case alphabetic string to all uppercase.

ST  
  SET String\="ThIs Is a MiXeDCAse stRiNg"
  WRITE !,"Input: ",String
  SET Len\=$LENGTH(String),Nstring\=" " 
  FOR i\=1:1:Len { DO CNVT }
  QUIT
CNVT  
  SET Char\=$EXTRACT(String,i),Asc\=$ASCII(Char)
  IF Asc\>96,Asc<123 {
    SET Char\=$CHAR(Asc\-32)
    SET Nstring\=Nstring\_Char
    }
  ELSE {
    SET Nstring\=Nstring\_Char
    }
  WRITE !,"Output: ",Nstring
  QUIT

 

Notes

Unicode Support

The $ASCII function supports both 8-bit and 16-bit characters. For 8-bit characters, it returns the numeric values 0 through 255. For 16-bit (Unicode) characters it returns numeric codes up to 65535.

The Unicode value for a character is usually expressed as a 4-digit number in hexadecimal notation, using the digits 0-9 and the letters A-F (for 10 through 15, respectively). However, standard functions in the Caché ObjectScript language generally identify characters according to ASCII numeric codes, which are base-10 decimal values, not hexadecimal.

Hence, the $ASCII function supports Unicode encoding by returning the decimal Unicode value of the inputted character, instead of the hexadecimal value that the Unicode standard recommends. This way, the function remains backward compatible, while also supporting Unicode. To convert a decimal number to hexadecimal, use the [$ZHEX](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzhex) function.

Surrogate Pairs

$ASCII does not recognize surrogate pairs. A surrogate pair is a pair of 16-bit Unicode characters that together encode a single ideographic character. Surrogate pairs are used to represent some Chinese characters and to support the Japanese JIS2004 standard. You can use the $WISWIDE function to determine if a string contains a surrogate pair. The $WASCII function recognizes and correctly parses surrogate pairs. $ASCII and $WASCII are otherwise identical. However, because $ASCII is generally faster than $WASCII, $ASCII is preferable for all cases where a surrogate pair is not likely to be encountered.

Note:

$WASCII should not be confused with $ZWASCII, which always parses characters in pairs.

Related Functions

The $CHAR function is the inverse of $ASCII. You can use it to convert an integer code to a character.

$ASCII converts a single character to an integer. To convert a 16-bit (wide) character string to an integer use $ZWASCII. To convert a 32-bit (long) character string to an integer use $ZLASCII. To convert a 64-bit (quad) character string to an integer use $ZQASCII. To convert a 64-bit character string to an IEEE floating-point number ($DOUBLE data type) use $ZDASCII.

See Also

*   [READ](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cread) command
    
*   [WRITE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cwrite) command
    
*   [$CHAR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fchar) function
    
*   [$WASCII](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwascii) function
    
*   [$WISWIDE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwiswide) function
    
*   [$ZLASCII](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzlascii) function
    
*   [$ZWASCII](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzwascii) function
    
*   [$ZQASCII](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzqascii) function
    
*   [$ZDASCII](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdascii) function
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

  [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fbit "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fascii.xml**