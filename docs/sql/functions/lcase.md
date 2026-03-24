# LCASE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_lcase

---

 

Caché SQL Reference

LCASE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_last_identity "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_least "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [LCASE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_lcase "LCASE") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A case-transformation function that converts all uppercase letters in a string to lowercase letters.

Synopsis

LCASE(string-expression)
{fn LCASE(string-expression)}

Arguments

string-expression

The string expression whose characters are to be converted to lowercase. The expression can be the name of a column, a string literal, or the result of another scalar function, where the underlying data type can be represented as any character type (such as CHAR or VARCHAR).

Description

LCASE converts uppercase letters to lowercase. It has no effects on non-alphabetic characters. It leaves unchanged punctuation and leading and trailing blank spaces.

LCASE does not force numerics to be interpreted as a string. Caché SQL converts numerics to canonical form, removing leading and trailing zeros. Caché SQL does not convert numeric strings to canonical form.

The LOWER function can also be used convert uppercase letters to lowercase.

The %SQLUPPER function is the preferred way in SQL to convert a data value for not case-sensitive collation. Refer to [%SQLUPPER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_sqlupper) for further information on case transformation for collation.

Examples

The following example returns each person’s name in lowercase letters:

SELECT TOP 10 Name,{fn LCASE(Name)} AS LowName
     FROM Sample.Person

 

LCASE also works on Unicode (non-ASCII) alphabetic characters, as shown in the following Embedded SQL example, which converts Greek letters from uppercase to lowercase:

  IF $SYSTEM.Version.IsUnicode() {
     SET a\=$CHAR(920,913,923,913,931,931,913)
     &sql(SELECT LCASE(:a) INTO :b )
     IF SQLCODE'=0 {
       WRITE !,"Error code ",SQLCODE }
     ELSE {WRITE !,a,!,b }
  }
  ELSE {WRITE "This example requires a Unicode installation of Caché"}

 

See Also

*   SQL functions: [LOWER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_lower) [UCASE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_ucase)
    
*   Caché ObjectScript function: [$ZCONVERT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzconvert)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_last_identity "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_least "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_lcase.xml**