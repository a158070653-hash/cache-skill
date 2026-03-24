# CHAR

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_char

---

 

Caché SQL Reference

CHAR

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_ceiling "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_characterlength "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [CHAR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_char "CHAR") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A string function that returns the character that has the ASCII code value specified in a string expression.

Synopsis

CHAR(code-value)
{fn CHAR(code-value)}

Arguments

code-value

An integer code that corresponds to a character.

Description

CHAR returns the character that corresponds to the specified integer code value. On Unicode systems, you can specify the integer code for any Unicode character, 0 through 65535. CHAR returns NULL if code-value is a integer that exceeds the permissible range of values.

CHAR returns an empty string ('') if code-value is a nonnumeric string. CHAR returns NULL if passed a NULL value.

Note that CHAR can be used as an ODBC scalar function (with the curly brace syntax) or as an SQL general function.

Examples

The following examples both return the character Z:

SELECT CHAR(90) AS CharCode

 

SELECT {fn CHAR(90)} AS CharCode

 

The following example returns the Greek letter lambda:

  IF $SYSTEM.Version.IsUnicode() {
  &sql(SELECT {fn CHAR(955)}
       INTO :greeklet)
  WRITE !,"Greek letter: ",greeklet
  }
  ELSE {WRITE "This example requires a Unicode installation of Caché"}

 

Note that the above example requires a Unicode installation of Caché.

See Also

*   SQL functions: [ASCII](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_ascii) [CHAR\_LENGTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_charlength) [CHARACTER\_LENGTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_characterlength)
    
*   Caché ObjectScript functions: [$CHAR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fchar) [$ZLCHAR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzlchar) [$ZWCHAR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzwchar)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_ceiling "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_characterlength "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_char.xml**