# $ZPOSITION

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fzposition

---

 

Caché ObjectScript Reference

$ZPOSITION

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzname "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzqascii "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [System and Other Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_SYSTEMFUNCTIONS "System and Other Functions") \]  >  \[ [$ZPOSITION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzposition "$ZPOSITION") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Returns the number of characters in an expression that can fit within a specified field width.

Synopsis

$ZPOSITION(expression,field,pitch)

Parameters

expression

A string expression.

field

An integer expression that specifies field width.

pitch

Optional — A numeric expression that specifies the pitch value to use for full-width characters. The default is 2. Other permissible values are 1, 1.25, and 1.5.

Description

$ZPOSITION is a DSM-J function available in Caché ObjectScript. $ZPOSITION returns the number of characters in expression that can fit within the field value. The pitch value determines the width to use for full-width characters. All other characters receive a default width of 1 and are considered to be half-width. Because half-width characters count as 1, field also expresses the number of half-width characters that fit in field.

$ZPOSITION adds the widths of the characters in the expression one at a time until the cumulative width equals the value of field or until there are no more characters in expression. The result is thus the number of characters that will fit within the specified field value including any fractional part of a character that would not completely fit.

Note:

$ZPOSITION can be abbreviated as $ZP in DSM-J mode. This abbreviation cannot be used in Caché mode.

Examples

In the following example, assume that the variable string contains two half-width characters followed by a full-width character.

   WRITE $ZPOSITION(string,3,1.5)

returns 2.6666666666666666667.

In the above example, the first two characters in string fit in the specified field width with one left over. The third character in string, a full-width character with a width of 1.5 (determined by the pitch argument), would not completely fit, although two thirds (1/1.5) of the character would fit. The fractional part of the result indicates that fact.

In the following example, string is now a string that contains a full-width character followed by two half-width characters. The result returned is 2.5:

   WRITE $ZPOSITION(string,3,1.5)

The results are now different. This is because the first two characters, which have a combined width of 2.5, would completely fit with .5 left over. Even so, only half of the third character (.5/1) would fit.

Finally, if string is a string that contains three half-width characters then all three characters would completely (and exactly) fit, and the result would be 3:

   WRITE $ZPOSITION(string,3,1.5)

Note:

Full-width characters are determined by examining the pattern-match table loaded for your Caché process. Any character with the full-width attribute is considered to be a full-width character. The special ZFWCHARZ patcode can be used to check for this attribute (for example, char?1ZFWCHARZ). For more information about the full-width attribute, see the description of the $X/$Y Tab in the section on “[System Classes for National Language Support](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSTU_customize#GSTU_customize_nls)” in Caché Specialized System Tools and Utilities.

See Also

*   [$ZWIDTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzwidth) function
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzname "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzqascii "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fzposition.xml**