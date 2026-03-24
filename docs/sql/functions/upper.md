# UPPER

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_upper

---

 

Caché SQL Reference

UPPER

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_ucase "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_upper_pct "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [UPPER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_upper "UPPER") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A case-transformation function that converts all lowercase letters in a string expression to uppercase letters.

Synopsis

UPPER(expression)
UPPER expression

Arguments

expression

A string expression, which can be the name of a column, a string literal, or the result of another function, where the underlying data type can be represented as any character type (such as CHAR or VARCHAR2).

Description

The UPPER function converts all alphabetic characters to uppercase letters. This is the inverse of the LOWER function. UPPER leaves unchanged numbers, punctuation, and leading or trailing blank spaces.

UPPER does not force a numeric to be interpreted as a string. Caché SQL removes leading and trailing zeros from numerics. A numeric specified as a string retains leading and trailing zeros.

This function can also be invoked from Caché ObjectScript using the [UPPER()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#UPPER) method call:

$SYSTEM.SQL.UPPER(expression)

UPPER is a standard function for alphabetic case conversion. This should not be confused with the %UPPER collation function or UPPER collation, which are deprecated and should not be used for new development. For uppercase collation use [%SQLUPPER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_sqlupper), which provides superior collation of numerics, NULL values and empty strings.

Examples

The following example returns all names, selecting those where the uppercase form of the name starts with “JO”:

SELECT Name
FROM Sample.Person
WHERE UPPER(Name) %STARTSWITH UPPER('JO')

 

The following example returns all names in uppercase, selecting those where the name starts with “JO”:

SELECT UPPER(Name) AS CapName
FROM Sample.Person
WHERE Name %STARTSWITH UPPER('JO')

 

The following Embedded SQL example converts the lowercase Greek letter Delta to uppercase. This example uses the UPPER syntax that uses a space, rather than parentheses, to separate keyword from argument:

  IF $SYSTEM.Version.IsUnicode() {
    &sql(SELECT UPPER {fn CHAR(948)},{fn CHAR(948)}
    INTO :a,:b
    FROM Sample.Person)
    IF SQLCODE'=0 {WRITE !,"Error code ",SQLCODE }
    ELSE {WRITE !,a,!,b }
  }
  ELSE {WRITE "This example requires a Unicode installation of Caché"}

 

See Also

[%SQLUPPER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_sqlupper) [%UPPER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_upper_pct) [%STARTSWITH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_startswith)

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_ucase "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_upper_pct "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:38**

Source: **RSQL\_upper.xml**