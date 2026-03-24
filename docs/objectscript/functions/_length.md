# $LENGTH

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_flength

---

 

Caché ObjectScript Reference

$LENGTH

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fjustify "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flist "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$LENGTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flength "$LENGTH") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Returns the number of characters or delimited substrings in a string.

Synopsis

$LENGTH(expression,delimiter)
$L(expression,delimiter)

Parameters

expression

The target string. It can be a numeric value, a string literal, a variable name, or any valid expression that resolves to a string.

delimiter

Optional — A string that demarcates separate substrings in the target string. It can be a variable name, a numeric value, a string literal, or any valid expression that resolves to a string.

Description

$LENGTH returns the number of characters in a specified string or the number of delimited substrings in a specified string, depending on the parameters used. Note that length counts the number of characters; an 8-bit character and a 16-bit wide character are both counted as one character.

*   $LENGTH(expression) returns the number of characters in the string. If the expression is a null string, $LENGTH returns a 0. If expression is a numeric expression, it is converted to [canonical form](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_numcanonical) before determining its length. If expression is a string numeric expression, no conversion is performed. If expression is the [$DOUBLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fdouble) values INF, -INF, or NAN, the lengths returned are 3, 4, and 3, respectively.
    
    This syntax can be used with the $EXTRACT function, which locates a substring by position and returns the substring value.
    
*   $LENGTH(expression,delimiter) returns the number of substrings within the string. $LENGTH returns the number of substrings separated from one another by the indicated delimiter. This number is always equal to the number of delimiters in the string, plus one.
    
    This syntax can be used with the $PIECE function, which locates a substring by a delimiter and returns the substring value.
    
    If the delimiter is the null string, $LENGTH returns a 0. If the delimiter is any other valid string literal and the string is a null string, $LENGTH returns a 1.
    

Encoded Strings

Caché supports strings that contain internal encoding. Because of this encoding, $LENGTH should not be used to determine the data content of a string.

*   $LENGTH should not be used for a [List structure string](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_strings#GCOS_str_list) created using $LISTBUILD or $LIST. Because a Caché List string is encoded, the length returned does not meaningfully indicate the number of characters in the list elements. The sole exception is the one-argument and two-argument forms of $LIST, which take an encoded Caché List string as input, but outputs a single List element value as a standard character string. You can use the [$LISTLENGTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistlength) function to determine the number of substrings (list elements) in an encoded list string.
    
*   $LENGTH should not be used for a [bit string](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_strings_bit). Because a Caché bit string is encoded, the length returned does not meaningfully indicate the number of bits in the bit string. You can use the [$BITCOUNT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fbitcount) function, which returns the number of bits in the string.
    
*   $LENGTH should not be used for a [JSON string](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GJSON). The value assigned by setting a variable to a JSON object or a JSON array is an object reference. Therefore the length of that variable value would be the length of the object reference, which has no connection to the length of the data encoded in the JSON string.
    

Surrogate Pairs

$LENGTH does not recognize surrogate pairs. A surrogate pair is a pair of 16-bit Unicode characters that together encode a single ideographic character. Surrogate pairs are used to represent some Chinese characters and to support the Japanese JIS2004 standard. You can use the $WISWIDE function to determine if a string contains a surrogate pair. The $WLENGTH function recognizes and correctly parses surrogate pairs. $LENGTH and $WLENGTH are otherwise identical. However, because $LENGTH is generally faster than $WLENGTH, $LENGTH is preferable for all cases where a surrogate pair is not likely to be encountered.

Examples

In the following example, both $LENGTH functions return 4, the number of characters in the string.

   IF $SYSTEM.Version.IsUnicode() {
   SET roman\="test"
   WRITE !,$LENGTH(roman)," characters in: ",roman
   SET greek\=$CHAR(964,949,963,964)
   WRITE !,$LENGTH(greek)," characters in: ",greek
   }
   ELSE {WRITE "This example requires a Unicode installation of Caché"}

 

In the following example, the first $LENGTH returns 5. This is the length of 74000, the canonical version of the specified number. The second $LENGTH returns 8, the length of the string “+007.4e4”.

   WRITE !,$LENGTH(+007.4e4)
   WRITE !,$LENGTH("+007.4e4")

 

In the following example, the first WRITE returns 11 the number of characters in var1 (including, of course, the space character). The second WRITE returns 2, the number of substrings in var1 using the space character as the substring delimiter.

   SET var1\="HELLO WORLD"
   WRITE !,$LENGTH(var1)
   WRITE !,$LENGTH(var1," ")

 

The following example returns 3, the number of substrings within the string, as delimited by the dollar sign ($) character.

   SET STR\="ABC$DEF$EFG",DELIM\="$"
   WRITE $LENGTH(STR,DELIM)

 

If the specified delimiter is not found in the string $LENGTH returns 1, because the only substring is the string itself.

The following example returns a 0 because the string tested is the null string.

   SET Nstring \= ""
   WRITE $LENGTH(Nstring)

 

The following example shows the values returned when a delimiter or its string is the null string.

   SET String \= "ABC"
   SET Nstring \= ""
   SET Delim \= "$"
   SET Ndelim \= ""
   WRITE !,$LENGTH(String,Delim)   ; returns 1
   WRITE !,$LENGTH(Nstring,Delim)  ; returns 1
   WRITE !,$LENGTH(String,Ndelim)  ; returns 0
   WRITE !,$LENGTH(Nstring,Ndelim) ; returns 0

 

See Also

*   [$EXTRACT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fextract) function
    
*   [$PIECE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fpiece) function
    
*   [$WISWIDE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwiswide) function
    
*   [$WLENGTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwlength) function
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fjustify "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flist "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_flength.xml**