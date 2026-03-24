# SECOND

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_second

---

 

Caché SQL Reference

SECOND

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_searchindex "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_sign "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [SECOND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_second "SECOND") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A time function that returns the second for a datetime expression.

Synopsis

{fn SECOND(time-expression)}

Arguments

time-expression

An expression that is the name of a column, the result of another scalar function, or a string or numeric literal. It must resolve either to a timestamp string or a [$HOROLOG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog) string, where the underlying data type can be represented as TIME or TIMESTAMP.

Description

SECOND returns an integer from 0 to 59, and may return fractional seconds as well. The seconds are calculated for a [$HOROLOG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog) or [$ZTIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztimestamp) value, an ODBC format date string (with no time value), or an ODBC format timestamp string with the format:

yyyy-mm-dd hh:mm:ss

To change this default time format, use the [SET OPTION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_setoption) command.

You must supply either a timestamp string (yyyy-mm-dd hh:mm:ss) or a $HOROLOG string. A $HOROLOG string may be a full datetime string (63274,37279) or only the time integer portion of $HOROLOG (37279). You cannot supply a time string (hh:mm:ss); this always returns 0, regardless of the actual number of seconds. The time-expression can also be specified as data type %Library.FilemanDate, %Library.FilemanTimestamp, or %MV.Date.

The seconds (ss) portion should be an integer in the range from 0 through 59. There is, however, no range checking for user-supplied values. Numbers greater than 59, negative numbers, and fractions are returned as specified. Leading zeros are optional on input. Leading and trailing zeros are suppressed on output.

SECOND returns 0 seconds when the seconds portion is '0', '00', or a nonnumeric value. Zero seconds is also returned if an ODBC date with no time expression is supplied, if the seconds portion of the time expression is omitted entirely ('hh', 'hh:mm', 'hh:mm:', or 'hh::'), or the time expression is invalid.

The same time information can be returned using [DATEPART](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datepart) or [DATENAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datename).

This function can also be invoked from Caché ObjectScript using the [SECOND()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#SECOND) method call:

$SYSTEM.SQL.SECOND(time-expression)

Fractional Seconds

SECOND returns fractions of a second if supplied in time-expression. Trailing zeros are truncated. If no fractional seconds are specified (for example: 38.00) the decimal separator is also truncated.

The standard Caché internal representation of time values ([$HOROLOG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog)) does not support fractional seconds. Timestamps do support fractional seconds.

The following SQL functions support fractional seconds: SECOND, CURRENT\_TIMESTAMP, DATENAME, DATEPART, and GETDATE. CURTIME, CURRENT\_TIME, and NOW do not support fractional seconds.

The SQL [SET OPTION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_setoption) statement permits you to set the default precision (number of decimal digits) for fractional seconds.

The Caché ObjectScript [$ZTIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztimestamp) special variable can be used to represent fractional seconds. The Caché ObjectScript functions $ZDATETIME, $ZDATETIMEH, $ZTIME, and $ZTIMEH support fractional seconds.

Examples

The following examples both return the number 38 because it is the thirty-eighth second of the time expression:

SELECT {fn SECOND('2000-02-16 18:45:38')} AS Seconds\_Given

 

SELECT {fn SECOND(67538)} AS Seconds\_Given

 

The following example returns .9 seconds. The leading and trailing zeros are truncated:

SELECT {fn SECOND('2000-02-16 18:45:00.9000')} AS Seconds\_Given

 

The following example returns 0 seconds because the seconds portion of the datetime string has been omitted:

SELECT {fn SECOND('2000-02-16 18:45')} AS Seconds\_Given

 

The following example returns 0 seconds because the time expression has been omitted from the datetime string:

SELECT {fn SECOND('2000-02-16')} AS Seconds\_Given

 

The following examples all return the seconds portion of the current time, in whole seconds:

SELECT {fn SECOND(CURRENT\_TIME)} AS Sec\_CurrentT,
       {fn SECOND({fn CURTIME()})} AS Sec\_CurT,
       {fn SECOND({fn NOW()})} AS Sec\_Now,
       {fn SECOND($HOROLOG)} AS Sec\_Horolog,
       {fn SECOND($ZTIMESTAMP)} AS Sec\_ZTS

 

The following example shows that leading zeros are suppressed. The first SECOND function returns a length 2, the others return a length of 1. An omitted time is considered to be 0 seconds, which has a length of 1:

SELECT LENGTH({fn SECOND('2004-02-05 11:45:22')}),
       LENGTH({fn SECOND('2004-02-15 03:05:06')}),
       LENGTH({fn SECOND('2004-02-15 3:5:6')}),
       LENGTH({fn SECOND('2004-02-15')})

 

The following Embedded SQL example shows that the SECOND function recognizes the TimeSeparator character specified for the locale:

  DO ##class(%SYS.NLS.Format).SetFormatItem("TimeSeparator",".")
  &sql(SELECT {fn SECOND('2000-02-16 18.45.38')} INTO :a)
  WRITE "seconds=",a

 

See Also

*   SQL concepts: [Data Type](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datatype), [Date and Time Constructs](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dateconstruct)
    
*   SQL functions: [HOUR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_hour), [MINUTE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_minute), [CURRENT\_TIME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_currenttime), [CURTIME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_curtime), [NOW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_now), [DATEPART](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datepart), [DATENAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datename)
    
*   Caché ObjectScript function: [$ZTIME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fztime)
    
*   Caché ObjectScript special variables: [$HOROLOG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog), [$ZTIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztimestamp)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_searchindex "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_sign "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_second.xml**