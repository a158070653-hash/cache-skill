# REVERSE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_reverse

---

 

Caché SQL Reference

REVERSE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_replicate "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_right "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [REVERSE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_reverse "REVERSE") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A scalar string function that returns a character string in reverse character order.

Synopsis

REVERSE(string-expression)

Arguments

string-expression

The string expression to be reversed. The expression can be the name of a column, a string literal, a numeric, or the result of another scalar function, where the underlying data type can be represented as any character type (such as CHAR or VARCHAR).

Description

REVERSE returns string-expression with its character order reversed. For example, 'Hello World!' is returned as '!dlroW olleH'. This is a simple string-order reversal, with no additional processing.

The string returned is data type VARCHAR, regardless of the data type of the input value. Numbers are converted to canonical form, numeric strings are not converted to canonical form before reversing.

Leading and trailing blanks are unaffected by reversing.

Reversing a NULL value results in a NULL.

Note:

Because REVERSE always returns a VARCHAR string, some types of data become invalid when reversed:

*   A reversed list is no longer a valid list and cannot be converted from storage format to display format.
    
*   A reversed date is no longer a valid date, and cannot be converted from storage format to display format.
    

Examples

The following example reverses the Name field values. In this case, this results in names sorted by middle initial:

SELECT Name,REVERSE(Name) AS RevName
FROM Sample.Person
ORDER BY RevName

 

Note that because Name and RevName are just different representations of the same field, ORDER BY RevName and ORDER BY RevName,Name perform the same ordering.

The following example reverses a number and a numeric string:

SELECT REVERSE(+007.10) AS RevNum,
       REVERSE('+007.10') AS RevNumStr

 

The following Embedded SQL example reverses a $DOUBLE number:

  SET dnum\=$DOUBLE(1.1)
  &sql(SELECT REVERSE(:dnum) INTO :drevnum)
  WRITE dnum,!
  WRITE drevnum,!

 

The following example shows what happens when you reverse a list:

SELECT FavoriteColors,REVERSE(FavoriteColors) AS RevColors
FROM Sample.Person

 

The following example shows what happens when you reverse a date:

SELECT DOB,%INTERNAL(DOB) AS IntDOB,REVERSE(DOB) AS RevDOB
FROM Sample.Person

 

See Also

*   [CHAR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_char)
    
*   [STRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_string)
    
*   [SUBSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_substring)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_replicate "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_right "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_reverse.xml**