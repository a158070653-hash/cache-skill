# MINUTE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_minute

---

 

Caché SQL Reference

MINUTE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_minus_pct "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_mod "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [MINUTE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_minute "MINUTE") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A time function that returns the minute for a datetime expression.

Synopsis

{fn MINUTE(time-expression)}

Arguments

time-expression

An expression that is the name of a column, the result of another scalar function, or a string or numeric literal. It must resolve either to a datetime string or a time integer, where the underlying data type can be represented as TIME or TIMESTAMP.

Description

MINUTE returns an integer specifying the minutes for a given time or datetime value. Minutes are calculated for a [$HOROLOG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog) or [$ZTIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztimestamp) value, an ODBC format date string, or a timestamp string with the format:

yyyy-mm-dd hh:mm:ss

To change this default time format, use the [SET OPTION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_setoption) command.

Note that you can supply a time integer (number of elapsed seconds), but not a time string (hh:mm:ss). You must supply a datetime string (yyyy-mm-dd hh:mm:ss). You can omit the seconds (:ss) portion of a datetime string and still return the minutes portion. The time-expression can also be specified as data type %Library.FilemanDate, %Library.FilemanTimestamp, or %MV.Date.

The minutes (mm) portion should be an integer in the range from 0 through 59. There is, however, no range checking for user-supplied values. Numbers greater than 59, negative numbers and fractions are returned as specified. Leading zeros are optional on input; leading zeros are suppressed on output.

MINUTE returns zero minutes when the minutes portion is '0', '00', or a nonnumeric value. Zero minutes is also returned if no time expression is supplied, or the minutes portion of the time expression is omitted entirely ('hh', 'hh:', 'hh::', or 'hh::ss'), or if the time expression format is invalid.

The same time information can be returned using [DATEPART](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datepart) or [DATENAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datename).

This function can also be invoked from Caché ObjectScript using the [MINUTE()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#MINUTE) method call:

$SYSTEM.SQL.MINUTE(time-expression)

Examples

The following examples both return the number 45 because it is the forty-fifth minute of the time expression in the datetime string:

SELECT {fn MINUTE('2000-02-16 18:45:38')} AS Minutes\_Given

 

SELECT {fn MINUTE(67538)} AS Minutes\_Given

 

The following example also returns 45. As shown here, the seconds portion of the time value can be omitted:

SELECT {fn MINUTE('2000-02-16 18:45')} AS Minutes\_Given

 

The following example returns 0 minutes because the time expression has been omitted from the datetime string:

SELECT {fn MINUTE('2000-02-16')} AS Minutes\_Given

 

The following examples all return the minutes portion of the current time:

SELECT {fn MINUTE(CURRENT\_TIME)} AS Min\_CurrentT,
       {fn MINUTE({fn CURTIME()})} AS Min\_CurT,
       {fn MINUTE({fn NOW()})} AS Min\_Now,
       {fn MINUTE($HOROLOG)} AS Min\_Horolog,
       {fn MINUTE($ZTIMESTAMP)} AS Min\_ZTS

 

The following example shows that leading zeros are suppressed. The first MINUTE function returns a length 2, the others return a length of 1. An omitted time is considered to be 0 minutes, which has a length of 1:

SELECT LENGTH({fn MINUTE('2004-02-05 11:45:00')}),
       LENGTH({fn MINUTE('2004-02-15 03:05:00')}),
       LENGTH({fn MINUTE('2004-02-15 3:5:0')}),
       LENGTH({fn MINUTE('2004-02-15')})

 

The following Embedded SQL example shows that the MINUTE function recognizes the TimeSeparator character specified for the locale:

  DO ##class(%SYS.NLS.Format).SetFormatItem("TimeSeparator",".")
  &sql(SELECT {fn MINUTE('2000-02-16 18.45.38')}
  INTO :a)
  WRITE "minutes=",a

 

See Also

*   SQL concepts: [Data Type](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datatype), [Date and Time Constructs](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dateconstruct)
    
*   SQL functions: [HOUR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_hour), [SECOND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_second), [CURRENT\_TIME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_currenttime), [CURTIME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_curtime), [NOW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_now), [DATEPART](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datepart), [DATENAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datename)
    
*   Caché ObjectScript function: [$ZTIME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fztime)
    
*   Caché ObjectScript special variables: [$HOROLOG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog), [$ZTIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztimestamp)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_minus_pct "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_mod "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_minute.xml**