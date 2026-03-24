# LEAST

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_least

---

 

Caché SQL Reference

LEAST

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_lcase "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_left "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [LEAST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_least "LEAST") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A function that returns the least value from a list of values.

Synopsis

LEAST(expression,expression\[,...\])

Arguments

expression

An expression that resolves to a number or a string. The values of these expressions are compared to each other and the least value returned. An expression can be a field name, a literal, an arithmetic expression, a host variable, or an object reference. You can list up to 140 comma-separated expressions.

Description

LEAST returns the smallest (least) value from a comma-separated list of values. Expressions are evaluated in left-to-right order. If only one expression is provided, LEAST returns that value. If any expression is NULL, LEAST returns NULL.

If all of the expression values resolve to canonical numbers, they are compared in numeric order. If a quoted string contains a number in canonical format, it is compared in numeric order. However, if a quoted string contains a number not in canonical format (for example, '00', '0.4', or '+4'), it is compared as a string. String comparisons are performed character-by-character in collation order. Any string value is greater than any numeric value.

The empty string is greater than any numeric value, but less than any other string value.

If the returned value is a number, LEAST returns it in canonical format (leading and trailing zeros removed, etc.). If the returned value is a string, LEAST returns it unchanged, including any leading or trailing blanks.

The inverse function of LEAST is [GREATEST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_greatest).

Data Type of Returned Value

If the data types of the expression values are different, the data type returned is the type most compatible with all of the possible return values, the data type with the highest [data type precedence](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datatype#RSQL_datatype_precedence). For example, if one expression is an integer and another expression is a fractional number, LEAST returns a value with data type NUMERIC. This is because NUMERIC is the data type with the highest precedence that is compatible with both.

Examples

In the following example, each LEAST compares three canonical numbers:

SELECT LEAST(22,2.2,\-21) AS HighNum,
       LEAST('2.2','22','-21') AS HighNumStr

 

In the following example, each LEAST compare three numeric strings. However, each LEAST contains one string that is non-canonical; these non-canonical values are compared as character strings. A character string is always greater than a number:

SELECT LEAST('22','+2.2','21'),
       LEAST('0.2','22','21')

 

In the following example, each LEAST compare three strings and returns the value with the lowest collation sequence:

SELECT LEAST('A','a',''),
       LEAST('a','aa','abc'),
       LEAST('#','0','7'),
       LEAST('##','00','77')

 

The following example compares two dates, treated as canonical numbers: the date of birth as a $HOROLOG integer, and the integer 58074 converted to a date. It returns the date of birth for each person born in the 20th century. Anyone born after December 31, 1999 is displayed with the default birth date of January 1, 2000:

SELECT Name,LEAST(DOB,TO\_DATE(58074)) AS NewMillenium
FROM Sample.Person

 

See Also

*   SQL functions: [GREATEST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_greatest) [CONVERT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_convert) [TO\_NUMBER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_tonumber)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_lcase "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_left "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_least.xml**