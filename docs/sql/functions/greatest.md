# GREATEST

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_greatest

---

 

Caché SQL Reference

GREATEST

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_getutcdate "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_hour "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [GREATEST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_greatest "GREATEST") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A function that returns the greatest value from a list of values.

Synopsis

GREATEST(expression,expression\[,...\])

Arguments

expression

An expression that resolves to a number or a string. The values of these expressions are compared to each other. An expression can be a field name, a literal, an arithmetic expression, a host variable, or an object reference. You can list up to 140 comma-separated expressions.

Description

GREATEST returns the greatest value from a comma-separated list of expression values. Expressions are evaluated in left-to-right order. If only one expression is provided, GREATEST returns that value. If any expression is NULL, GREATEST returns NULL.

If all of the expression values resolve to canonical numbers, they are compared in numeric order. If a quoted string contains a number in canonical format, it is compared in numeric order. However, if a quoted string contains a number not in canonical format (for example, '00', '0.4', or '+4'), it is compared as a string. String comparisons are performed character-by-character in collation order. Any string value is greater than any numeric value.

The empty string is greater than any numeric value, but less than any other string value.

If the returned value is a number, GREATEST returns it in canonical format (leading and trailing zeros removed, etc.). If the returned value is a string, GREATEST returns it unchanged, including any leading or trailing blanks.

The inverse function of GREATEST is [LEAST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_least).

Data Type of Returned Value

If the data types of the expression values are different, the data type returned is the type most compatible with all of the possible return values, the data type with the highest [data type precedence](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datatype#RSQL_datatype_precedence). For example, if one expression is an integer and another expression is a fractional number, GREATEST returns a value with data type NUMERIC. This is because NUMERIC is the data type with the highest precedence that is compatible with both.

Examples

In the following example, each GREATEST compares three canonical numbers:

SELECT GREATEST(22,2.2,\-21) AS HighNum,
       GREATEST('2.2','22','-21') AS HighNumStr

 

In the following example, each GREATEST compare three numeric strings. However, each GREATEST contains one string that is non-canonical; these non-canonical values are compared as character strings. A character string is always greater than a number:

SELECT GREATEST('22','+2.2','-21'),
       GREATEST('0.2','22','-21')

 

In the following example, each GREATEST compare three strings and returns the value with the highest collation sequence:

SELECT GREATEST('A','a',''),
       GREATEST('a','ab','abc'),
       GREATEST('#','0','7'),
       GREATEST('##','00','77')

 

The following example compares two dates, treated as canonical numbers: the date of birth as a $HOROLOG integer, and the integer 58073 converted to a date. It returns the date of birth for each person born in the 21st century. Anyone born before January 1, 2000 is displayed with the default birth date of December 31, 1999:

SELECT Name,GREATEST(DOB,TO\_DATE(58073)) AS NewMillenium
FROM Sample.Person

 

See Also

*   SQL functions: [LEAST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_least) [CONVERT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_convert) [TO\_NUMBER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_tonumber)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_getutcdate "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_hour "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_greatest.xml**