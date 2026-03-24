# MONTH

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_month

---

 

Caché SQL Reference

MONTH

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_mod "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_monthname "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [MONTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_month "MONTH") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A date function that returns the month as an integer for a date expression.

Synopsis

MONTH(date-expression)
{fn MONTH(date-expression)}

Arguments

date-expression

An expression that is the name of a column, the result of another scalar function, or a date or timestamp literal.

Description

MONTH returns an integer specifying the month. The month integer is calculated for a Caché date integer, a [$HOROLOG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog) or [$ZTIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztimestamp) value, an ODBC format date string, or a timestamp string with the format:

yyyy-mm-dd hh:mm:ss

The time portion of the timestamp is not evaluated and can be omitted. The date-expression can also be specified as data type %Library.FilemanDate, %Library.FilemanTimestamp, or %MV.Date.

The month (mm) portion of a date string should be an integer in the range from 1 through 12. There is, however, no range checking for user-supplied values. Numbers greater than 12, zero, and fractions are returned as specified. Because (–) is used as a separator character, negative numbers are not supported. Leading zeros are optional on input. Leading and trailing zeros are suppressed on output.

MONTH returns zero when the month portion is '0', '00', or a nonnumeric value. Zero is also returned if the month portion of the date string is omitted entirely ('yyyy––dd'), or if no date expression is supplied.

MONTH interprets the second numeric string encountered in a date string as the month value, so omitting the year portion of the date string ('mm-dd hh:mm:ss'), results in the second number encountered ('dd') being treated as the month value. Thus, a leading hyphen or some placeholder should be supplied for an unknown year value; for compatibility with Caché, 9999 is generally the preferred default year value.

Note that MONTH can be invoked as an ODBC scalar function (with the curly brace syntax) or as an SQL general function.

This function can also be invoked from Caché ObjectScript using the [MONTH()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#MONTH) method call:

$SYSTEM.SQL.MONTH(date-expression)

The elements of a datetime string can be returned using the following SQL functions: YEAR, MONTH, DAY (or DAYOFMONTH), HOUR, MINUTE, and SECOND. The same elements can be returned by using the [DATEPART](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datepart) or [DATENAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datename) function. Date elements can be returned using [TO\_DATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_todate). DATEPART and DATENAME performs value and range checking on month values.

The [LAST\_DAY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_lastday) function returns the date of the last day of the specified month.

Examples

The following examples both return the number 2 because February is the second month of the year:

SELECT MONTH('2000-02-16') AS Month\_Given

 

SELECT {fn MONTH(59589)} AS Month\_Given

 

The following example sorts records in birthday order by month and day, ignoring the year component of the DOB:

SELECT Name,DOB AS Birthdays
FROM Sample.Person
ORDER BY MONTH(DOB),DAY(DOB),Name

 

The following examples returns zero because the month is omitted:

SELECT MONTH('2000--16') AS Month\_Given

 

SELECT {fn MONTH('12:34:55')} AS Month\_Given

 

SELECT MONTH('2000 12:34:55') AS Month\_Given

 

The following example returns the number 2 because a placeholder character (-) has been supplied for the omitted year:

SELECT {fn MONTH('-02-16')} AS Month\_Given

 

The following examples all return the current month:

SELECT {fn MONTH({fn NOW()})} AS MNow,
       MONTH(CURRENT\_DATE) AS MCurrD,
       {fn MONTH(CURRENT\_TIMESTAMP)} AS MCurrTS,
       MONTH($HOROLOG) AS MHorolog,
       {fn MONTH($ZTIMESTAMP)} AS MZTS

 

See Also

*   SQL functions: [DATEPART](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datepart) [DATENAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datename) [DAYOFMONTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dayofmonth) [LAST\_DAY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_lastday) [MONTHNAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_monthname) [TO\_DATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_todate)
    
*   Caché ObjectScript function: [$ZDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdate)
    
*   Caché ObjectScript special variables: [$HOROLOG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog) [$ZTIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztimestamp)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_mod "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_monthname "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_month.xml**