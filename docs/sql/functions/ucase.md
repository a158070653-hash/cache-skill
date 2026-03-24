# UCASE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_ucase

---

 

Caché SQL Reference

UCASE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_truncate_pct "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_upper "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [UCASE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_ucase "UCASE") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A case-transformation function that converts all lowercase letters in a string to uppercase letters.

Synopsis

UCASE(string-expression)
{fn UCASE(string-expression)}

Arguments

string-expression

The string whose characters are to be converted to uppercase. The expression can be the name of a column, a string literal, or the result of another scalar function, where the underlying data type can be represented as any character type (such as CHAR or VARCHAR).

Description

UCASE converts lowercase letters to uppercase. It has no effects on non-alphabetic characters; it leaves unchanged numbers, punctuation, and leading or trailing blank spaces.

Note that UCASE can be used as an ODBC scalar function (with the curly brace syntax) or as an SQL general function.

UCASE does not force a numeric to be interpreted as a string. Caché SQL removes leading and trailing zeros from numerics. A numeric specified as a string retains leading and trailing zeros.

The %SQLUPPER function is the preferred way in SQL to convert a data value for not case-sensitive collation. Refer to [%SQLUPPER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_sqlupper) for further information on case transformation for collation.

This function can also be invoked from Caché ObjectScript using the [UPPER()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#UPPER) method call:

$SYSTEM.SQL.UPPER(expression)

Examples

The following example returns each person’s name in uppercase letters:

SELECT Name,{fn UCASE(Name)} AS CapName
     FROM Sample.Person

 

UCASE also works on Unicode (non-ASCII) alphabetic characters, as shown in the following Embedded SQL example, which converts Greek letters from lowercase to uppercase:

  IF $SYSTEM.Version.IsUnicode() {
    SET a\=$CHAR(950,949,965,963)
    &sql(SELECT UCASE(:a)
    INTO :b
    FROM Sample.Person)
    IF SQLCODE'=0 {WRITE !,"Error code ",SQLCODE }
    ELSE {WRITE !,a,!,b }
  }
  ELSE {WRITE "This example requires a Unicode installation of Caché"}

 

See Also

*   SQL functions: [%ALPHAUP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_alphaup) [LCASE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_lcase) [%SQLUPPER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_sqlupper) [UPPER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_upper) [%UPPER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_upper_pct)
    
*   Caché ObjectScript function: [$ZCONVERT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzconvert)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_truncate_pct "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_upper "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:38**

Source: **RSQL\_ucase.xml**