# $FIND

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_ffind

---

 

Caché ObjectScript Reference

$FIND

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ffactor "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ffnumber "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$FIND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ffind "$FIND") \]

Go to: Description Examples Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Finds a substring by value and returns an integer specifying its end position in the string.

Synopsis

$FIND(string,substring,position)
$F(string,substring,position)

Parameters

string

The target string that is to be searched. It can be a variable name, a numeric value, a string literal, or any valid Caché ObjectScript expression that resolves to a string.

substring

The substring that is to be searched for. It can be a variable name, a numeric value, a string literal, or any valid Caché ObjectScript expression that resolves to a string.

position

Optional — A position within the target string at which to start the search. It must be a positive integer.

Description

$FIND returns an integer specifying the end position of a substring within a string. $FIND searches string for substring. $FIND is case sensitive. If substring is found, $FIND returns the integer position of the first character following substring. If substring is not found, $FIND returns a value of 0.

Because $FIND returns the position of the character following the substring, when substring is a single character that matches the first character of string $FIND returns 2. When substring is the null string (""), $FIND returns 1.

You can include the position option to specify a starting position for the search. If position is greater than the number of characters in string, $FIND returns a value of 0.

Examples

For example, if variable var1 contains the string “ABCDEFG” and variable var2 contains the string “BCD,” the following $FIND returns the value 5, indicating the position of the character (“E”) that follows the var2 string:

   SET var1\="ABCDEFG",var2\="BCD"
   WRITE $FIND(var1,var2)

 

The following example returns 4, the position of the character immediately to the right of the substring “FOR”.

   SET X\="FOREST"
   WRITE $FIND(X,"FOR")

 

In the following examples, $FIND searches for a substring that is not in string, for a null substring, and for a substring that is the first character of string. The examples return 0, 1, and 2, respectively:

   WRITE !,$FIND("aardvark","z")  ; returns 0
   WRITE !,$FIND("aardvark","")   ; returns 1
   WRITE !,$FIND("aardvark","a")  ; returns 2

 

The following examples show what happens when string is a null string:

   WRITE !,$FIND("","z")  ; returns 0
   WRITE !,$FIND("","")   ; returns 1

 

The following example returns 14, the position of the character immediately to the right of the first occurrence of “R” after the seventh character in X.

   SET X\="EVERGREEN FOREST",Y\="R"
   WRITE $FIND(X,Y,7)

 

In the following example, $FIND begins its search after the last character in string. It returns zero (0):

   SET X\="EVERGREEN FOREST",Y\="R"
   WRITE $FIND(X,Y,20)

 

The following example uses $FIND with $REVERSE to perform a search operation from the end of the string. This example locates the last example of a string within a line of text. It returns the position of that string as 33:

   SET line\="THE QUICK BROWN FOX JUMPED OVER THE LAZY DOG."
   SET position\=$LENGTH(line)+2\-$FIND($REVERSE(line),$REVERSE("THE"))
   WRITE "The last THE in the line begins at ",position

 

The following example uses name indirection to return 6, the position of the character immediately to the right of the substring “THIS”:

   SET Y\="x",x\="""THIS IS A TEST"""
   WRITE $FIND(@Y,"THIS")

 

For more information, refer to [Indirection](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_operators#GCOS_operators_indirection) in Using Caché ObjectScript.

Notes

$FIND, $EXTRACT, $PIECE, and $LIST

*   $FIND locates a substring by value and returns a position.
    
*   $EXTRACT locates a substring by position and returns the substring value.
    
*   $PIECE locates a substring by a delimiter character or delimiter string, and returns the substring value.
    
*   $LIST operates on specially encoded strings. It locates a substring by substring count and returns the substring value.
    

The $FIND, $EXTRACT, $LENGTH, and $PIECE functions operate on standard character strings. The various $LIST functions operate on encoded character strings, which are incompatible with standard character strings. The sole exception is the one-argument and two-argument forms of $LIST, which take an encoded character string as input, but output a single element value as a standard character string.

Surrogate Pairs

$FIND does not recognize surrogate pairs. A surrogate pair is a pair of 16-bit Unicode characters that together encode a single ideographic character. Surrogate pairs are used to represent some Chinese characters and to support the Japanese JIS2004 standard. You can use the $WISWIDE function to determine if a string contains a surrogate pair. The $WFIND function recognizes and correctly parses surrogate pairs. $FIND and $WFIND are otherwise identical. However, because $FIND is generally faster than $WFIND, $FIND is preferable for all cases where a surrogate pair is not likely to be encountered.

See Also

*   [$EXTRACT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fextract) function
    
*   [$LENGTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flength) function
    
*   [$LIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flist) function
    
*   [$PIECE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fpiece) function
    
*   [$REVERSE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_freverse) function
    
*   [$WEXTRACT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwextract) function
    
*   [$WFIND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwfind) function
    
*   [$WISWIDE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwiswide) function
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ffactor "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ffnumber "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_ffind.xml**