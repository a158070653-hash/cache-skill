# HOUR

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_hour

---

 

Caché SQL Reference

HOUR

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_greatest "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_ifnull "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [HOUR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_hour "HOUR") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A time function that returns the hour for a datetime expression.

Synopsis

{fn HOUR(time-expression)}

Arguments

time-expression

An expression that is the name of a column, the result of another scalar function, or a string or numeric literal. It must resolve either to a datetime string or a time integer, where the underlying data type can be represented as TIME or TIMESTAMP.

Description

HOUR returns an integer specifying the hour for a given time or datetime value. The hour is calculated for a [$HOROLOG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog) or [$ZTIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztimestamp) value, an ODBC format date string, or a timestamp string with the format:

yyyy-mm-dd hh:mm:ss

To change this default time format, use the [SET OPTION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_setoption) command.

Note that you can supply a time integer (number of elapsed seconds), but not a time string (hh:mm:ss). You must supply a datetime string (yyyy-mm-dd hh:mm:ss). You can omit the seconds (:ss) or minutes and seconds (mm:ss) portion of a datetime string and still return the hour portion. The time-expression can also be specified as data type %Library.FilemanDate, %Library.FilemanTimestamp, or %MV.Date.

Hours are expressed in 24-hour time. The hours (hh) portion should be an integer in the range from 0 through 23. There is, however, no range checking for user-supplied values. Numbers greater than 23, negative numbers, and fractions are returned as specified. Leading zeros are optional on input; leading zeros are suppressed on output.

HOUR returns a value of 0 hours when the hours portion is '0', '00', or a nonnumeric value. Zero hours is also returned if no time expression is supplied, or if the hours portion of the time expression is omitted (':mm:ss' or '::ss').

The same time information can be returned using [DATEPART](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datepart) or [DATENAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datename).

This function can also be invoked from Caché ObjectScript using the [HOUR()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#HOUR) method call:

$SYSTEM.SQL.HOUR(time-expression)

Examples

The following examples both return the number 18 because the time-expression value is 18:45:38:

SELECT {fn HOUR('2000-02-16 18:45:38')} AS Hour\_Given

 

SELECT {fn HOUR(67538)} AS Hour\_Given

 

The following example also returns 18. The seconds (or minutes and seconds) portion of the time value can be omitted.

SELECT {fn HOUR('2000-02-16 18:45')} AS Hour\_Given

 

The following example returns 0 hours, because the time portion of the datetime string has been omitted:

SELECT {fn HOUR('2000-02-16')} AS Hour\_Given

 

The following examples all return the hours portion of the current time:

SELECT {fn HOUR(CURRENT\_TIME)} AS H\_CurrentT,
       {fn HOUR({fn CURTIME()})} AS H\_CurT,
       {fn HOUR({fn NOW()})} AS H\_Now,
       {fn HOUR($HOROLOG)} AS H\_Horolog,
       {fn HOUR($ZTIMESTAMP)} AS H\_ZTS

 

Note that $ZTIMESTAMP returns Coordinated Universal Time (UTC). The other time-expression values return the local time.

The following example shows that leading zeros are suppressed. The first HOUR function returns a length 2, the others return a length of 1. An omitted time is considered to be 0 hours, which has a length of 1:

SELECT LENGTH({fn HOUR('2004-02-05 11:45')}),
       LENGTH({fn HOUR('2004-02-15 03:45')}),
       LENGTH({fn HOUR('2004-02-15 3:45')}),
       LENGTH({fn HOUR('2004-02-15')})

 

The following Embedded SQL example shows that the HOUR function recognizes the TimeSeparator character specified for the locale:

  DO ##class(%SYS.NLS.Format).SetFormatItem("TimeSeparator",".")
  &sql(SELECT {fn HOUR('2000-02-16 18.45.38')} INTO :a)
  WRITE "hour=",a

 

See Also

*   SQL concepts: [Data Type](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datatype) [Date and Time Constructs](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dateconstruct)
    
*   SQL functions: [MINUTE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_minute) [SECOND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_second) [CURRENT\_TIME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_currenttime) [CURTIME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_curtime) [NOW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_now) [DATEPART](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datepart) [DATENAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datename)
    
*   Caché ObjectScript function: [$ZTIME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fztime)
    
*   Caché ObjectScript special variables: [$HOROLOG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog) [$ZTIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztimestamp)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_greatest "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_ifnull "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_hour.xml**