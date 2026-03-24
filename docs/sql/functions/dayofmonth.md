# DAYOFMONTH

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_dayofmonth

---

 

Caché SQL Reference

DAYOFMONTH

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dayname "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dayofweek "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [DAYOFMONTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dayofmonth "DAYOFMONTH") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A date function that returns the day of the month for a date expression.

Synopsis

{fn DAYOFMONTH(date-expression)}

Arguments

date-expression

An expression that is the name of a column, the result of another scalar function, or a date or timestamp literal.

Description

DAYOFMONTH returns the day of the month as an integer from 1 to 31. The date-expression can be a Caché date integer, a [$HOROLOG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog) or [$ZTIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztimestamp) value, an ODBC format date string, or a timestamp string with the format:

yyyy-mm-dd hh:mm:ss

The time portion of the timestamp or $HOROLOG string is not evaluated and can be omitted. The date-expression can also be specified as data type %Library.FilemanDate, %Library.FilemanTimestamp, or %MV.Date.

The DAYOFMONTH and DAY functions are functionally identical.

This function can also be invoked from Caché ObjectScript using the [DAYOFMONTH()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#DAYOFMONTH) method call:

  WRITE $SYSTEM.SQL.DAYOFMONTH("2004-02-25")

 

Timestamp date-expression

The day (dd) portion of a timestamp string should be an integer in the range from 1 through 31. There is, however, no range checking for user-supplied values. Numbers greater than 31 and fractions are returned as specified. Because (–) is used as a separator character, negative numbers are not supported. Leading zeros are optional on input; leading zeros are suppressed on output.

DAYOFMONTH returns NULL when the day portion is '0', '00', or a nonnumeric value. NULL is also returned if the day portion of the date string is omitted entirely ('yyyy–mm hh:mm:ss'), or if no date expression is supplied.

The elements of a datetime string can be returned using the following SQL scalar functions: YEAR, MONTH, DAYOFMONTH (or DAY), HOUR, MINUTE, SECOND. The same elements can be returned by using the [DATEPART](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datepart) or [DATENAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datename) function. DATEPART and DATENAME performs value and range checking on day values.

$HOROLOG date-expression

When calculating day of the month for a $HOROLOG value, DAYOFMONTH calculates leap years differences, including century day adjustments: 2000 is a leap year, 1900 and 2100 are not leap years.

DAYOFMONTH can process date-expression values prior to December 31, 1840 as negative integers. This is shown in the following example:

SELECT {fn DAYOFMONTH(\-306)} AS DayOfMonthFeb, /\* February 29, 1840 \*/
       {fn DAYOFMONTH(\-305)} AS DayOfMonthMar  /\* March 1, 1840     \*/

 

The [LAST\_DAY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_lastday) function returns the date (in $HOROLOG format) of the last day of the month for a specified date.

Examples

The following examples return the number 25 because the date specified is the twenty-fifth day of the month:

SELECT {fn DAYOFMONTH('2004-02-25')} AS DayNumTS,
       {fn DAYOFMONTH(59590)} AS DayNumH

 

The following example also returns the number 25 for the day of the month. The year is omitted, but the separator character (–) serves as a placeholder:

SELECT {fn DAYOFMONTH('-02-25 11:45:32')} AS DayNum

 

The following examples return <null>:

SELECT{fn DAYOFMONTH('2000-02-00 11:45:32')} AS DayNum

 

SELECT {fn DAYOFMONTH('2000-02 11:45:32')} AS DayNum

 

SELECT {fn DAYOFMONTH('11:45:32')} AS DayNum

 

The following DAYOFMONTH examples all returns the current day of the month:

SELECT {fn DAYOFMONTH({fn NOW()})} AS DoM\_Now,
       {fn DAYOFMONTH(CURRENT\_DATE)} AS DoM\_CurrD,
       {fn DAYOFMONTH(CURRENT\_TIMESTAMP)} AS DoM\_CurrTS,
       {fn DAYOFMONTH($HOROLOG)} AS DoM\_Horolog,
       {fn DAYOFMONTH($ZTIMESTAMP)} AS DoM\_ZTS

 

Note that $ZTIMESTAMP returns Coordinated Universal Time (UTC). The other time-expression values return the local time. This may affect the DAYOFMONTH value.

The following example shows that leading zeros are suppressed. It returns a length of either 1 or 2, depending on the day of the month value:

SELECT LENGTH({fn DAYOFMONTH('2004-02-05')}),
       LENGTH({fn DAYOFMONTH('2004-02-15')})

 

See Also

*   SQL functions: [DATENAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datename), [DATEPART](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datepart), [DAY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_day), [DAYNAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dayname), [DAYOFWEEK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dayofweek), [DAYOFYEAR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dayofyear), [LAST\_DAY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_lastday), [MONTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_month), [TO\_DATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_todate)
    
*   Caché ObjectScript function: [$ZDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdate)
    
*   Caché ObjectScript special variables: [$HOROLOG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog), [$ZTIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztimestamp)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dayname "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dayofweek "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_dayofmonth.xml**