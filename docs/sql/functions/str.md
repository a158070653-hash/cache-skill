# STR

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_str

---

 

Caché SQL Reference

STR

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_square "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_string "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [STR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_str "STR") \]

Go to: Description Example See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A function that converts a numeric to a string.

Synopsis

STR(number\[,length\[,decimals\]\])

Arguments

number

An expression that resolves to a numeric. It can be a field name, a numeric, or the result of another function. If a field name is specified, the logical value is used.

length

Optional — An integer specifying the total length of the desired output string, including all characters (digits, decimal point, sign, blank spaces). The default is 10.

decimals

Optional — An integer specifying the number of places to the right of the decimal point to include. The default is 0.

Description

STR converts a numeric to the STRING format, truncating the numeric based on the values of length and decimals. The length argument must be large enough to include the entire integer portion of the number, and, if decimals is specified, that number of decimal digits plus 1 (for the decimal point). If length is not large enough, STR returns a string of asterisks (\*) equal to length.

STR converts numerics to their [canonical form](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_numcanonical) before string conversion. It therefore performs arithmetic operations, removes leading and trailing zeros and leading plus signs from numbers.

If the number argument is [NULL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_langelements#GSQL_langelements_null), STR returns NULL. If the number argument is the empty string (''), STR returns the empty string. STRING retains whitespace.

Example

In the following Embedded SQL example, STR converts numerics into a string:

   &sql(SELECT STR(123),
               STR(123,4),
               STR(+00123.45,3),
               STR(+00123.45,3,1),
               STR(+00123.45,5,1)
        INTO :v,:w,:x,:y,:z)
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"Resulting STR:",v," string"
     WRITE !,"Resulting STR:",w," string"
     WRITE !,"Resulting STR:",x," string"
     WRITE !,"Resulting STR:",y," string"
     WRITE !,"Resulting STR:",z," string" }

 

The first STR function returns a string consisting of 7 leading blanks and the number 123; the seven leading blanks are because the default string length is 10. The second STR function returns the string “ 123”; note the leading blank needed to return a string of length 4. The third STR function returns the string “123”; the numeric is put into canonical form, and decimals defaults to 0. The fourth STR function returns “\*\*\*” because the string length is not long enough to encompass the entire number as specified; the number of asterisks indicates the string length. The fifth STR function returns “123.4”; note that the length must be 5 to include the decimal digit.

See Also

[STRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_string) [%STRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_string_pct) [%SQLUPPER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_sqlupper) [%SQLSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_sqlstring)

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_square "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_string "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_str.xml**