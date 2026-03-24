# DAYOFYEAR

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_dayofyear

---

 

Caché SQL Reference

DAYOFYEAR

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dayofweek "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_decode "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [DAYOFYEAR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dayofyear "DAYOFYEAR") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A date function that returns the day of the year as an integer for a date expression.

Synopsis

{fn DAYOFYEAR(date-expression)}

Arguments

date-expression

A date expression that is the name of a column, the result of another scalar function, or a date or timestamp literal.

Description

DAYOFYEAR returns an integer from 1 to 366 that corresponds to the day of the year for a given date expression. DAYOFYEAR calculates leap year dates.

The day of year is calculated for a Caché date integer, a [$HOROLOG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog) or [$ZTIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztimestamp) value, an ODBC format date string, or a timestamp string with the format:

yyyy-mm-dd hh:mm:ss

The time portion of the timestamp is not evaluated and can be omitted. The date-expression can also be specified as data type %Library.FilemanDate, %Library.FilemanTimestamp, or %MV.Date.

When calculating day of the month for a $HOROLOG value, DAYOFYEAR calculates leap years differences, including century day adjustments: 2000 is a leap year, 1900 and 2100 are not leap years.

DAYOFYEAR can process date-expression values prior to December 31, 1840 as negative integers. This is shown in the following example:

SELECT {fn DAYOFYEAR(\-306)} AS LastDayFeb,  /\* February 29, 1840 \*/
       {fn DAYOFYEAR(\-305)} AS FirstDayMar  /\* March 1, 1840     \*/

 

The same day count can be returned by using the [DATEPART](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datepart) or [DATENAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datename) function. DATEPART and DATENAME performs value and range checking on date expressions.

This function can also be invoked from Caché ObjectScript using the [DAYOFYEAR()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#DAYOFYEAR) method call:

$SYSTEM.SQL.DAYOFYEAR(date-expression)

Examples

The following examples both return the number 64 because the day in the date expression (March 4, 2004) is the 64th day of the year (the leap year day is automatically counted):

SELECT {fn DAYOFYEAR('2004-03-04 12:45:37')} AS DayCount

 

SELECT {fn DAYOFYEAR(59598)} AS DayCount

 

The following examples all return the count for the current day:

SELECT {fn DAYOFYEAR({fn NOW()})} AS DNumNow,
       {fn DAYOFYEAR(CURRENT\_DATE)} AS DNumCurrD,
       {fn DAYOFYEAR(CURRENT\_TIMESTAMP)} AS DNumCurrTS,
       {fn DAYOFYEAR($HOROLOG)} AS DNumHorolog,
       {fn DAYOFYEAR($ZTIMESTAMP)} AS DNumZTS

 

Note that $ZTIMESTAMP returns Coordinated Universal Time (UTC). The other time-expression values return the local time. This may affect the DAYOFYEAR value.

The following example uses a subquery to return Employee records ordered by the day of year of each person’s birthday:

SELECT Name,DOB
FROM (SELECT Name,DOB,{fn DAYOFYEAR(DOB)} AS BDay FROM Sample.Employee)
ORDER BY BDay

 

See Also

*   SQL functions: [DATENAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datename), [DATEPART](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datepart), [DAYNAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dayname), [DAYOFMONTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dayofmonth), [DAYOFWEEK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dayofweek)
    
*   Caché ObjectScript special variables: [$HOROLOG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog), [$ZTIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztimestamp)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dayofweek "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_decode "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_dayofyear.xml**