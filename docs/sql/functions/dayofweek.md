# DAYOFWEEK

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_dayofweek

---

 

Caché SQL Reference

DAYOFWEEK

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dayofmonth "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dayofyear "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [DAYOFWEEK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dayofweek "DAYOFWEEK") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A date function that returns the day of the week as an integer for a date expression.

Synopsis

{fn DAYOFWEEK(date-expression)}

Arguments

date-expression

A valid ODBC-format date or $HOROLOG format date, with or without the time component. An expression that is the name of a column, the result of another scalar function, or a date or timestamp literal.

Description

DAYOFWEEK takes a date-expression and returns an integer corresponding to the day of the week for that date. Days of the week are counted from the first day of the week; the Caché default is that Sunday is the first day of the week. Therefore, by default, the returned values represent these days:

*   1 — Sunday
    
*   2 — Monday
    
*   3 — Tuesday
    
*   4 — Wednesday
    
*   5 — Thursday
    
*   6 — Friday
    
*   7 — Saturday
    

The first day of the week default can be overriden system-wide or for specific namespaces, as described in “[Setting First Day of Week](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dayofweek#RSQL_dayofweek_setting)”.

The date-expression can be a Caché date integer, a [$HOROLOG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog) or [$ZTIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztimestamp) value, an ODBC format date string, or a timestamp string with the format:

yyyy-mm-dd hh:mm:ss

The time portion of the timestamp is not evaluated and can be omitted. The date-expression can also be specified as data type %Library.FilemanDate, %Library.FilemanTimestamp, or %MV.Date.

The same day of week information can be returned by using the [DATEPART](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datepart) or [TO\_DATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_todate) function. To return the name of the day of the week, use [DAYNAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dayname), [DATENAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datename), or [TO\_DATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_todate).

This function can also be invoked from Caché ObjectScript using the [DAYOFWEEK()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#DAYOFWEEK) method call:

$SYSTEM.SQL.DAYOFWEEK(date-expression)

Date Validation

DAYOFWEEK performs the following checks on input values. If a value fails a check, the null string is returned.

*   A valid date-expression may consist of a date string (yyyy-mm-dd), a date and time string (yyyy-mm-dd hh:mm:ss), a Caché date integer, or a $HOROLOG value. DAYOFWEEK evaluates only the date portion of the date-expression.
    
*   A date string must be complete and properly formatted with the appropriate number of elements and digits for each element, and the appropriate separator character. Years must be specified as four digits.
    
*   Date values must be within a valid range. Years: 1841 through 9999. Months: 1 through 12. Days: 1 through 31.
    
*   The number of days in a month must match the month and year. For example, the date '02–29' is only valid if the specified year is a leap year.
    
*   Date values less than 10 may include or omit a leading zero. Other non-canonical integer values are not permitted. Therefore, a Day value of '07' or '7' is valid, but '007', '7.0' or '7a' are not valid.
    

Setting First Day of Week

By default, the first day of the week is Sunday. You can override this default system-wide by specifying SET ^%SYS("sql","sys","day of week")=n, where n values are 1=Monday through 7=Sunday. To set Monday as the first day of the week specify SET ^%SYS("sql","sys","day of week")=1. If Monday is the first day of the week, a Wednesday date-expression returns 3, rather than the 4 that would be returned if Sunday was the first day of the week. To reset the Caché default (Sunday as first day of week), specify SET ^%SYS("sql","sys","day of week")=7.

You can set the first day of the week for a specific namespace by specifying SET ^%SYS("sql","sys","day of week",namespace)=n, where n values are 1=Monday through 7=Sunday. To set Monday as the first day of the week for the USER namespace, specify SET ^%SYS("sql","sys","day of week","USER")=1. Once the first day of the week is set at the namespace level, changing the system-wide setting by specifying SET ^%SYS("sql","sys","day of week")=n has no effect on that namespace. To restore the ability to change that namespace’s first day of week default, you must kill ^%SYS("sql","sys","day of week",namespace). See example below.

Caché also supports the ISO 8601 standard for determining the day of the week, week of the year, and other date settings. This standard is principally used in European countries. The ISO 8601 standard begins counting the days of the week with Monday. To activate ISO 8601, SET ^%SYS("sql","sys","week ISO8601")=1; to deactivate, set it to 0. If week ISO8601 is activated and Caché day of week is undefined or set to the default (7=Sunday), the ISO 8601 standard overrides the Caché default. If Caché day of week is set to any other value, it overrides week ISO8601 for DAYOFWEEK. See example below.

Examples

In the following example, both select-items return the number 6 (if Sunday is set as the first day of the week) because the specified date-expression (63876 = November 20, 2015) is a Friday:

SELECT {fn DAYOFWEEK('2015-11-20')}||' '||DATENAME('dw','2015-11-20') AS ODBCDoW,
       {fn DAYOFWEEK(63876)}||' '||DATENAME('dw','63876') AS HorologDoW

 

In the following example, all select-items return the integer corresponding to the current day of the week:

SELECT {fn DAYOFWEEK({fn NOW()})} AS DoW\_Now,
       {fn DAYOFWEEK(CURRENT\_DATE)} AS DoW\_CurrDate,
       {fn DAYOFWEEK(CURRENT\_TIMESTAMP)} AS DoW\_CurrTstamp,
       {fn DAYOFWEEK($ZTIMESTAMP)} AS DoW\_ZTstamp,
       {fn DAYOFWEEK($HOROLOG)} AS DoW\_Horolog

 

Note that $ZTIMESTAMP returns Coordinated Universal Time (UTC). The other time-expression values return the local time. This may affect the DAYOFWEEK value.

The following Embedded SQL example demonstrates changing the first day of week for a namespace. It initially sets the system-wide first day of week (to 7), then sets the first day of week for a namespace (to 3). A subsequent system-wide first day of week change (to 2) has no effect on namespace first day of week until the program kills the namespace-specific setting. Killing the namespace-specific setting immediately resets that namespace’s first day of week to the current system-wide value. Finally, the program restores the initial system-wide setting.

Note:

The following program tests if you have namespace-specific first day of week settings for SAMPLES or USER. If you do, this program aborts to prevent changing these settings.

SetUp
  SET TestNsp\="SAMPLES"
  SET ControlNsp\="USER"
InitialDoWValues
  WRITE "Systemwide default DoW initial values",!
  DO TestDayofWeek()
  IF a\=b {WRITE "No namespace-specific DoW defaults",!!}
  ELSE {WRITE "DoW initial settings are namespace-specific",!
        WRITE "Stopping this program"
        QUIT }
  SET initialDoW\=^%SYS("sql","sys","day of week")
SetSystemwideDoW
  KILL ^%SYS("sql","sys","day of week",TestNsp)
  KILL ^%SYS("sql","sys","day of week",ControlNsp)
  SET ^%SYS("sql","sys","day of week")\=7
  WRITE "Systemwide DoW set",!
  DO TestDayofWeek()
SetNamespaceDoW
  SET ^%SYS("sql","sys","day of week",TestNsp)\=3
  WRITE TestNsp," namespace DoW set",!
  &sql(SELECT {fn DAYOFWEEK($HOROLOG)} INTO :a)
  DO TestDayofWeek()
ResetSystemwideDoW
  SET ^%SYS("sql","sys","day of week")\=2
  WRITE "Systemwide DoW set with ",TestNsp," DoW set",!
  DO TestDayofWeek
KillNamespaceDoW
  KILL ^%SYS("sql","sys","day of week",TestNsp)
  WRITE "Namespace ",TestNsp," DoW killed",!
  DO TestDayofWeek
ResetSystemwideDoWDefault
  SET ^%SYS("sql","sys","day of week")\=initialDoW
  WRITE "Systemwide DoW reset after ",TestNsp," DoW killed",!
  DO TestDayofWeek
TestDayofWeek()
  ZNSPACE TestNsp
  &sql(SELECT {fn DAYOFWEEK($HOROLOG)} INTO :a)
  WRITE "Today is the ",a," day of week in ",$NAMESPACE,!
  ZNSPACE ControlNsp
  &sql(SELECT {fn DAYOFWEEK($HOROLOG)} INTO :b)
  WRITE "Today is the ",b," day of week in ",$NAMESPACE,!!
  RETURN

 

The following Embedded SQL example shows the default day of the week and the day of the week with the ISO 8601 standard applied. It assumes that the Caché day of week is undefined or set to the default:

TestISO
  SET def\=$DATA(^%SYS("sql","sys","week ISO8601"))
  IF def\=0 {SET ^%SYS("sql","sys","week ISO8601")\=0}
  ELSE {SET isoval\=^%SYS("sql","sys","week ISO8601")}
     IF isoval\=1 {GOTO UnsetISO }
     ELSE {SET isoval\=0 GOTO DayofWeek }
UnsetISO
  SET ^%SYS("sql","sys","week ISO8601")\=0
DayofWeek
  &sql(SELECT {fn DAYOFWEEK($HOROLOG)} INTO :a)
  WRITE "Today:",!
  WRITE "default day of week is ",a,!
  SET ^%SYS("sql","sys","week ISO8601")\=1
  &sql(SELECT {fn DAYOFWEEK($HOROLOG)} INTO :b)
  WRITE "ISO8601 day of week is ",b,!
ResetISO
  SET ^%SYS("sql","sys","week ISO8601")\=isoval

 

See Also

*   SQL functions: [DATENAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datename), [DATEPART](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datepart), [DAYNAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dayname), [DAYOFMONTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dayofmonth), [DAYOFYEAR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dayofyear), [TO\_DATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_todate)
    
*   Caché ObjectScript function: [$ZDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdate)
    
*   Caché ObjectScript special variables: [$HOROLOG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog), [$ZTIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztimestamp)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dayofmonth "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dayofyear "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_dayofweek.xml**