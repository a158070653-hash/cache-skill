# LENGTH

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_length

---

 

Caché SQL Reference

LENGTH

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_len "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_length "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [LENGTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_length "LENGTH") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A string function that returns the number of characters in a string expression.

Synopsis

LENGTH(string-expression)

{fn LENGTH(string-expression)}

Arguments

string-expression

A string expression, which can be the name of a column, a string literal, or the result of another scalar function, where the underlying data type can be represented as any character type (such as CHAR or VARCHAR).

Description

LENGTH returns an integer that denotes the number of characters, not the number of bytes, of the given string expression. The string-expression can be a string (from which trailing blanks are removed), or a number (which Caché converts to canonical form).

*   LENGTH returns the length of the internal (data storage) value, not the display value.
    
*   LENGTH excludes trailing blanks and the string-termination character. (The CHARACTER\_LENGTH, CHAR\_LENGTH, and DATALENGTH functions do not exclude trailing blanks and terminators.)
    
    LENGTH does not exclude leading blanks from strings. You can remove leading blanks from a string using the [LTRIM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_ltrim) function.
    
*   LENGTH converts numbers to [canonical form](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_numcanonical), removing leading and trailing zeros and a trailing decimal separator character. It does not convert numeric strings to canonical form.
    
*   LENGTH returns NULL if passed a NULL value, and 0 if passed an empty string.
    
*   LENGTH returns a value of [data type](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datatype) INTEGER (%Library.Integer).
    
*   LENGTH does not support data stream fields. Specifying a stream field for string-expression results in an SQLCODE -37. (The CHARACTER\_LENGTH, CHAR\_LENGTH, and DATALENGTH functions do support data stream fields.
    

Note that LENGTH can be used as an ODBC scalar function (with the curly brace syntax) or as an SQL general function.

Examples

In the following example, Caché first converts each number to canonical form (removing leading and trailing zeros, resolving leading signs, and removing a trailing decimal separator character). Each LENGTH returns a length of 1:

SELECT {fn LENGTH(7.00)} AS CharCount,
       {fn LENGTH(+007)} AS CharCount,
       {fn LENGTH(007.)} AS CharCount,
       {fn LENGTH(00000.00)} AS CharCount,
       {fn LENGTH(\-0)} AS CharCount

 

In the following example, the first LENGTH removes the leading zero, returning a length value of 2; the second LENGTH treats the numeric value as a string, and does not remove the leading zero, returning a length value of 3:

SELECT LENGTH(0.7) AS CharCount,
       LENGTH('0.7') AS CharCount

 

The following example returns the value 12:

SELECT LENGTH('INTERSYSTEMS') AS CharCount

 

The following example shows how LENGTH handles leading and trailing blanks. The first LENGTH returns 15, because LENGTH excludes trailing blanks, but not leading blanks. The second LENGTH returns 12, because LTRIM excludes the leading blanks:

SELECT LENGTH('   INTERSYSTEMS      ') AS CharCount,
       LENGTH(LTRIM('   INTERSYSTEMS      ')) AS CharCount

 

The following example returns the number of characters in each Name value in the Sample.Person table:

SELECT Name,{fn LENGTH(Name)} AS CharCount
FROM Sample.Person
ORDER BY CharCount

 

The following example returns the number of characters in the DOB (date of birth) field. Note that the length returned (by LENGTH, CHAR\_LENGTH, and CHARACTER\_LENGTH) is the internal ($HOROLOG) format of the date, not the display format. The display length of DOB is ten characters; all three length functions return the internal length of 5:

SELECT DOB,{fn LENGTH(DOB)} AS LenCount,
CHAR\_LENGTH(DOB) AS CCount,
CHARACTER\_LENGTH(DOB) AS CtrCount
FROM Sample.Person

 

The following Embedded SQL example gives the length of a string of Unicode characters. The length returned is the number of characters (7), not the number of bytes.

  IF $SYSTEM.Version.IsUnicode() {
    SET a\=$CHAR(920,913,923,913,931,931,913)
    &sql(SELECT LENGTH(:a) INTO :b )
    IF SQLCODE'=0 {
       WRITE !,"Error code ",SQLCODE }
    ELSE {
       WRITE !,"The Greek Sea: ",a,!,$LENGTH(a),!,b }
  }
  ELSE {WRITE "This example requires a Unicode installation of Caché"}

 

(Note that the above example requires a Unicode installation of Caché.)

See Also

*   SQL functions: [CHAR\_LENGTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_charlength), [CHARACTER\_LENGTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_characterlength), [DATALENGTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datalength), [LEN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_len), [$LENGTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_length)
    
*   Caché ObjectScript function: [$LENGTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flength)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_len "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_length "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_length.xml**