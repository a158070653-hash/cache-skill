# $ZWIDTH

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fzwidth

---

 

Caché ObjectScript Reference

$ZWIDTH

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzwchar "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzwpack "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [System and Other Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_SYSTEMFUNCTIONS "System and Other Functions") \]  >  \[ [$ZWIDTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzwidth "$ZWIDTH") \]

Go to: Description Example Note See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Returns the total width of the characters in an expression.

Synopsis

$ZWIDTH(expression,pitch)

Parameters

expression

A string expression

pitch

Optional — The numeric pitch value to use for full-width characters. The default is 2. Other permissible values are 1, 1.25, and 1.5. (These values with any number of trailing zeros are permissible.) All other pitch values result in a <FUNCTION> error.

Description

$ZWIDTH is a DSM-J function available in Caché ObjectScript. $ZWIDTH returns the total width of the characters in expression. The pitch value determines the width to use for full-width characters. All other characters are assigned a width of 1 and are considered to be half-width.

Note:

$ZWIDTH can be abbreviated as $ZW in DSM-J mode. This abbreviation cannot be used in Caché mode.

Example

Assume that the variable STR contains two half-width characters followed by a full-width character:

   WRITE $ZWIDTH(STR,1.5)

returns 3.5.

In this example, the two half-width characters total 2. Adding 1.5 (the specified pitch value) for the full-width characters produces a total of 3.5.

Note

Full-width characters are determined by examining the pattern-match table loaded for your Caché process. Any character with the full-width attribute is considered to be a full-width character. You can use the special ZFWCHARZ patcode to check for this attribute (char?1ZFWCHARZ). For more information about the full-width attribute, see the description of the $X/$Y Tab in the section on “[System Classes for National Language Support](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSTU_customize#GSTU_customize_nls)” in Caché Specialized System Tools and Utilities.

See Also

*   [$ZPOSITION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzposition) function
    
*   [$ZZENKAKU](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzzenkaku) function
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzwchar "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzwpack "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fzwidth.xml**