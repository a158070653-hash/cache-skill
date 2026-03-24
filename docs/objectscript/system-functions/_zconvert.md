# $ZCONVERT

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fzconvert

---

 

Caché ObjectScript Reference

$ZCONVERT

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzboolean "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzcrc "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [System and Other Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_SYSTEMFUNCTIONS "System and Other Functions") \]  >  \[ [$ZCONVERT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzconvert "$ZCONVERT") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

String conversion function.

Synopsis

$ZCONVERT(string,mode,trantable,handle)
$ZCVT(string,mode,trantable,handle)

Parameters

string

The string to convert, specified as a quoted string. This string can be specified as a value, a variable, or an expression.

mode

A letter code specifying the conversion mode, either the type of case conversion or input/output encoding. Specify mode as a quoted string.

trantable

Optional — The [translation table to use](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzconvert#RCOS_fzconvert_io), specified as either an integer or a quoted string,

handle

Optional — An unsubscripted local variable that holds a string value. Used for multiple invocations of $ZCONVERT. The [handle parameter](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzconvert#RCOS_fzconvert_handle) contains the remaining portion of string that could not be converted at the end of $ZCONVERT, and supplies this remaining portion to the next invocation of $ZCONVERT.

Description

$ZCONVERT converts a string from one form to another. The nature of the conversion depends on the parameters you use.

$ZCONVERT Returns a Converted String

$ZCONVERT(string, mode) returns string with the characters converted as specified by mode. The conversions are of two types:

*   Case conversion
    
*   Encoding translation
    

Case conversion changes the case of each letter character in the string. You can change all letter characters in a string to their lowercase, uppercase, or titlecase form. Characters that are already in the specified case, and characters with no case (usually any nonalphabetic character) in the string are passed through unchanged. To output a literal quote character (“) within a string, input two quote characters (““). For further case conversion options, including non-ASCII and customized case conversion, refer to the section on “[System Classes for National Language Support](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSTU_customize#GSTU_customize_nls)” in Caché Specialized System Tools and Utilities.

Encoding translation translates string between the internal encoding style used on your system and another encoding style. You can perform input translation; that is, translate string from an external encoding style to the encoding style of your system. You can also perform output translation; that is, translate string from the encoding style of your system to an external encoding style. For further I/O translation options, including non-ASCII and customized translation, refer to the section on “[System Classes for National Language Support](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSTU_customize#GSTU_customize_nls)” in Caché Specialized System Tools and Utilities.

The values you can use for mode are as follows:

Mode Code

Meaning

U or u

Uppercase translation: Convert all characters in string to uppercase.

L or l

Lowercase translation: Convert all characters in string to lowercase.

T or t

Titlecase translation: Convert all characters in string to titlecase. Titlecase is only meaningful for those alphabets (principally Eastern European) that have three forms for a letter: uppercase, lowercase, and titlecase. For all other letters, titlecase translation is the same as uppercase translation.

W or w

Word translation: Convert the first character of each word in string to uppercase. Any character preceded by a blank space, a quotation mark ("), an apostrophe ('), or an open parenthesis (() is considered the first character of a word. Word translation converts all other characters to lowercase. Word translation is locale specific; the above syntax rules for English may differ for other language locales.

S or s

Sentence translation: Convert the first character of each sentence in string to uppercase. The first non-blank character of string, and any character preceded by a period (.), question mark (?), or exclamation mark (!) is considered the first character of a sentence. (Blank spaces between the preceding punctuation character and the letter are ignored.) If this character is a letter, it is converted to uppercase. Sentence translation converts all other letter characters to lowercase. Sentence translation is locale specific; the above syntax rules for English may differ for other language locales.

I or i

Perform input encoding translation on a specified string. For the two-argument form, the translation is performed using the current process I/O translation handle. If a current process I/O translation handle has not been defined, Caché performs translation based on the default process I/O translation table name.

O or o

Perform output encoding translation on a specified string. For the two-argument form, the translation is performed using the current process I/O translation handle. If a current process I/O translation handle has not been defined, Caché performs translation based on the default process I/O translation table name.

If mode is a null string or any value other than the valid characters, you receive a <FUNCTION> error.

Letter Case Translation

You can convert letters in strings to all uppercase letters or all lowercase letters. Conversion works on Unicode letters as well as ASCII letters. The following example converts the Greek alphabet from lowercase to uppercase:

   IF $SYSTEM.Version.IsUnicode() {
     FOR i\=945:1:969 {WRITE $ZCONVERT($CHAR(i),"U")}
   }
   ELSE {WRITE "This example requires a Unicode installation of Caché"}

 

However, a small number of letters only have a lowercase letter form. For example, the German eszett ($CHAR(223)) is only defined as a lowercase letter. Attempting to convert it to an uppercase letter results in the same lowercase letter:

  IF $ZCONVERT($CHAR(223),"U")\=$ZCONVERT($CHAR(223),"L") {
      WRITE "uppercase and lowercase letter are the same" }
  ELSE {WRITE "uppercase and lowercase are different" }

 

For this reason, when converting alphanumeric strings to a single letter case it is always preferable to convert to lowercase.

You can perform similar letter case translations using the $TRANSLATE function, as shown in the following example:

  WRITE $TRANSLATE(text,"ABCDEFGHIJKLMNOPQRSTUVWXYZ","abcdefghijklmnopqrstuvwxyz")

Word and Sentence Translation

“W” and “S” modes determine whether a non-blank character is the first character of a word or the first character of a sentence, and if that character is a letter, translate it to uppercase. All other letters are translated to lowercase. Case translation works on letters in any alphabet, as shown in the following example which converts Greek letters ($CHAR(945) is lowercase alpha; $CHAR(913) is uppercase alpha):

   IF $SYSTEM.Version.IsUnicode() {
   SET greek\=$CHAR(945,946,947,913,914,915)
   WRITE $ZCONVERT(greek,"W")
   }
   ELSE {WRITE "This example requires a Unicode installation of Caché"}

 

However the rules determining what constitutes a word or sentence are locale dependent. For example, the following example uses the Spanish inverted exclamation point $CHAR(161). The default (English) locale does not recognize this character as beginning a sentence or word. In this example, all letters in spanish are translated to lowercase:

   SET spanish\=$CHAR(161)\_"ola MuNdO! "\_$CHAR(161)\_"olA!"
   SET english\="hElLo wOrLd! heLLo!"
   WRITE !,$ZCONVERT(english,"S")
   WRITE !,$ZCONVERT(spanish,"S")

 

Titlecase Translation

Titlecase (“T”) mode converts every letter in the string to its titlecase form. Titlecase does not selectively uppercase letters based on their position in a word or string. Titlecase is the case that a letter is represented in when it is the first character of a word in a title. For standard Latin letters, the titlecase form is the same as the uppercase form.

Some languages (for example, Croatian) represent particular letters by two letter glyphs. For example, “lj” is a single letter in the Croatian alphabet. This letter has three forms: lowercase “lj”, uppercase “LJ”, and titlecase “Lj”. $ZCONVERT titlecase translation is used for this type of letter conversion.

Three-Parameter Form: Encoding Translation

$ZCONVERT(string, mode, trantable) performs either an input encoding translation or an output encoding translation on string. In the three-argument form, the mode values you can use are either "I" or "O". You must define the mode value. For “I” translations, the string may be a hexadecimal string, such as %4B (the letter “K”); hexadecimal strings are not case-sensitive.

If the translated value is a non-printing character, Caché displays it as a null string. If the target device cannot represent a translated character, Caché substitutes a question mark (?) character for the non-displayable character.

The trantable value can be a numeric character or a string that specifies the translation table or translation handle to use. The trantable value can be:

*   An integer value specifying a process I/O translation object. Available values are 0 through 3 (0 represents the current process I/O translation object).
    
*   An uppercase string value identifying a Caché-supplied I/O translation table. Available translation tables include:
    
    *   “RAW” which performs no translation for 8-bit characters or 16-bit Latin-1 characters (Unicode characters in which the high-order byte has the value 00). RAW translation should not be used for Unicode systems using non-Latin-1 locales, such as rusw.
        
    *   “SAME” which performs no translation on 8-bit systems, and translates 8-bit characters to the corresponding Unicode character on Unicode systems.
        
    *   “HTML” which adds (output mode) or removes (input mode) HTML escape characters to a string.
        
    *   “JS” (or “JSML”) which uses a supplied JavaScript translation table to convert to the format for Zen component pages. For output translations see the table below. For input translations, “\\0”, “\\000”, “\\x00”, and “\\u0000” are all valid escape sequences for NULL.
        
    *   “JSON” (or “JSONML”) which uses a supplied translation table to convert to JSON format. For output translations see the table below. For input translations, “\\0”, “\\000”, “\\x00”, and “\\u0000” are all valid escape sequences for NULL.
        
    *   “URL” which adds (output mode) or removes (input mode) URL parameter escape characters to a string. Characters higher than $CHAR(255) are represented in Unicode hexadecimal notation: $CHAR(256) = %u0100.
        
    *   “UTF8” which converts (output mode) 16-bit Unicode characters to a series of 8-bit characters. Thus, the characters $CHAR(0) through $CHAR(127) are the same in RAW and UTF8 mode; characters $CHAR(128) and above are converted.
        
    *   “XML” which adds (output mode) or removes (input mode) XML escape characters to a string.
        
*   A string value specifying an I/O translation table defined by an NLS locale. For example, Latin2 or CP1252. For a list of locale translation tables, refer to the [XLTTables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYS.NLS.Locale#XLTTables) property of [%SYS.NLS.Locale](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYS.NLS.Locale), as shown in the following example:
    
      SET nlsoref\=##class(%SYS.NLS.Locale).%New()
      WRITE $LISTTOSTRING($PROPERTY(nlsoref,"XLTTables"),"^")
    
     
    
*   A string value specifying a user-defined I/O translation table. A named table can be defined in a locale and points to one or two translation tables. Use a named table to define a specific system-to/from-device encoding.
    
*   An empty string ("") specifying the use of the default process I/O translation table. (For equivalent functionality, see the $$GetPDefIO^%NLS() function of the %NLS utility.)
    

The following is a table of Output mode escape characters:

 

HTML

JS

JSON

URL

XML

null $CHAR(0)

 

\\x00

\\u0000

%00

 

$CHAR(1) through $CHAR(7)

 

\\x01 through \\x07

\\u0001 through \\u0007

%01 through %07

 

backspace $CHAR(8)

 

\\b

\\b

%08

 

horizontal tab $CHAR(9)

 

\\t

\\t

%09

 

line feed $CHAR(10)

 

\\n

\\n

%0A

 

vertical tab $CHAR(11)

 

\\v

\\u000B

%0B

 

form feed $CHAR(12)

 

\\f

\\f

%0C

 

carriage return $CHAR(13)

 

\\r

\\r

%0D

 

$CHAR(14) through $CHAR(31)

 

 

\\u000E through \\u001F

%0E through %1F

 

$CHAR(32)

 

 

 

%20

 

" (doubled)

&quot;

\\"

\\”

%22

&quot;

#

 

 

 

%23

 

%

 

 

 

%25

 

&

&amp;

 

 

%26

&amp;

'

&#39;

\\'

 

 

&apos;

+

 

 

 

%2B

 

,

 

 

 

%2C

 

/

 

\\/

 

 

 

:

 

 

 

%3A

 

;

 

 

 

%3B

 

<

&lt;

 

 

%3C

&lt;

\=

 

 

 

%3D

 

\>

&gt;

 

 

%3E

&gt;

?

 

 

 

%3F

 

@

 

 

 

%40

 

\[

 

 

 

%5B

 

\\

 

\\\\

\\\\

%5C

 

\]

 

 

 

%5D

 

^

 

 

 

%5E

 

‘

 

 

 

%60

 

{

 

 

 

%7B

 

|

 

 

 

%7C

 

}

 

 

 

%7D

 

~

 

 

 

%7E

 

$CHAR(127) through $CHAR(159)

 

 

 

%7F through %9F

 

$CHAR(160)

&nbsp;

 

 

%A0

 

$CHAR(161) through $CHAR(255)

 

 

 

%A1 through %FF

 

URL and URI Conversions

A URL or URI can only contain certain 8-bit ASCII characters. All other characters must be represented by an escape sequence beginning with %. If you wish to convert a string containing UNICODE characters to a URL or URI, you must first convert your local representation to an 8-bit intermediate representation, using UTF8 encoding. You then convert the UTF8 results to URL encoding. To convert a URL back to its original UNICODE string, you perform the reverse operation. This is shown in the following example:

  IF $SYSTEM.Version.IsUnicode() {
  SET ustring\="US$ to "\_$CHAR(8364)\_" échange"
  WRITE "initial string is: ",ustring,!
ConvertUnicodeToURL
  SET utfo \= $ZCONVERT(ustring,"O","UTF8")
  SET urlo \= $ZCONVERT(utfo,"O","URL")
  WRITE "UNICODE to URL conversion: ",urlo,!
ConvertURLtoUnicode
  SET urli \= $ZCONVERT(urlo,"I","URL")
  SET utfi \= $ZCONVERT(urli,"I","UTF8")
  WRITE "URL to UNICODE conversion: ",utfi
   }
   ELSE {WRITE "This example requires a Unicode installation of Caché"}

 

Four-Parameter Form: Input/Output String

The handle parameter is a local variable that $ZCONVERT reads at the beginning of execution and writes when it completes execution. It is used to hold information between consecutive invocations of the $ZCONVERT function. It can be used for two purposes: concatenating a string to the beginning of string, and converting extremely long strings.

To concatenate a string to the beginning of string, set handle before invoking $ZCONVERT:

   SET handle\="the "
   WRITE $ZCVT("quick brown fox","O","URL",handle),!
   /\*  the%20quick%20brown%20fox  \*/
   WRITE $ZCVT("quick brown fox","O","URL",handle),!
   /\*  quick%20brown%20fox  \*/

 

Note that $ZCONVERT resets handle when it completes execution. In the previous example, it resets handle to the empty string.

This handle parameter may be used for input conversions. Specifying a handle is useful when dealing with multibyte character sequences when working with partial sets of characters, such as a stream read. In these cases, $ZCONVERT uses the handle parameter to hold a partial character sequence that may be the leading bytes of a multibyte sequence. If there are input characters left in the buffer at the end of a $ZCONVERT which do not make a complete translation unit, these leftover characters are returned in the handle. At the beginning of next $ZCONVERT, if the handle contains data, these leftover characters are prepended to the normal input data. This is particularly valuable for use in UTF-8 conversions, as shown in the following example:

  SET handle\=""
  WHILE 'stream.AtEnd() {
     WRITE $ZCONVERT(stream.Read(20000),"I","UTF8",handle)
  } 

To convert an extremely long string, it may be necessary to perform more than one string conversions by invoking $ZCONVERT multiple times. $ZCONVERT provides the optional handle parameter to hold the remaining unconverted portion of string. If you specify a handle parameter, it is updated by each invocation of $ZCONVERT. When the string conversion completes, $ZCONVERT sets handle to the empty string.

   SET handle\=""
   SET out \= $ZCVT(hugestring,"O","HTML",handle)
   IF handle '= "" {
     SET out2 \= $ZCVT(handle,"O","HTML",handle)
     WRITE "Converted string is: ",out,out2  }
   ELSE {
     WRITE "Converted string is: ",out }

Examples

The following example returns "HELLO":

   WRITE $ZCONVERT("Hello","U")

 

The following example returns "hello":

   WRITE $ZCVT("Hello","L")

 

The following example returns "HELLO":

   WRITE $ZCVT("Hello","T")

 

The following example uses the concatenate operator (\_) to append and case-convert an accented character:

   WRITE "CACH"\_$CHAR(201),!, $ZCVT("CACH"\_$CHAR(201),"L")

 

returns:

CACHÉ

caché

The following example converts the angle brackets in the string to HTML escape characters for output, returning “&lt;TAG&gt;”

   WRITE $ZCVT("<TAG>","O","HTML")

 

Note that how these angle brackets display depends on the output device; try running this program here and then running it from the Terminal prompt.

The following example shows how $ZCONVERT substitutes a ? character for a translated character it cannot display. Both the UTF8 and the current process I/O translation object (trantable 0) conversions in this example display $CHAR(63), which is the actual ? character. UTF8 cannot display translated characters above $CHAR(127). Translation table 0 cannot display translated characters above $CHAR(255):

  FOR i\=1:1:300 {IF $ZCONVERT($CHAR(i),"I","UTF8") '= "?"
                   { CONTINUE }
                  ELSE {WRITE "UTF8 ",i,"=",$ZCONVERT($CHAR(i),"I","UTF8")}
                  IF $ZCONVERT($CHAR(i),"I",0)\="?"
                   {WRITE " trantable 0 ",i,"=",$ZCONVERT($CHAR(i),"I",0),!}
                 ELSE {WRITE !}
                 }

 

See Also

*   [$ASCII](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fascii) function
    
*   [$CHAR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fchar) function
    
*   [$ZSTRIP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzstrip) function
    
*   [Pattern Matching](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_operators) operators in Using Caché ObjectScript
    
*   More information on locales in the section on “[System Classes for National Language Support](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSTU_customize#GSTU_customize_nls)” in Caché Specialized System Tools and Utilities.
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzboolean "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzcrc "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fzconvert.xml**