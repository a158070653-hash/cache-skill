# WEEK

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_week

---

 

Caché SQL Reference

WEEK

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_userfunction "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_xmlconcat "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [WEEK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_week "WEEK") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A date function that returns the week of the year as an integer for a date expression.

Synopsis

{fn WEEK(date-expression)}

Arguments

date-expression

An expression that is the name of a column, the result of another scalar function, or a date or timestamp literal.

Description

WEEK takes a date-expression, and returns the number of weeks from the beginning of the year for that date.

By default, weeks are calculated using the [$HOROLOG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog) date (number of days since Dec. 31, 1840). Therefore, weeks are counted from year to year, such that Week 1 is the days that complete the seven-day period begun by the last week of the previous year. A week always begins with a Sunday; therefore, the first Sunday of the calendar year marks the changing from Week 1 to Week 2. If the first Sunday of the year is January 1, then that Sunday is in Week 1; if the first Sunday of the year is later than January 1, then that Sunday is the first day of Week 2. For this reason, Week 1 is commonly less than seven days in length. You can determine the day of the week by using the [DAYOFWEEK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dayofweek) function. The total number of weeks in a year is commonly 53, and can be 54 in leap years.

Caché also supports the ISO 8601 standard for determining the week of the year. This standard is principally used in European countries. When Caché is configured for ISO 8601, WEEK begins counting a week with Monday, and assigns the week to the year that contains that week’s Thursday. For example, Week 1 of 2004 ran from Monday 29 December 2003 to Sunday 4 January 2004, because this week’s Thursday was 1 January 2004, which was the first Thursday of 2004. Week 1 of 2005 ran from Monday 3 January 2005 to Sunday 9 January 2005, because its Thursday was 6 January 2005, which was the first Thursday of 2005. The total number of weeks in a year is commonly 52, but can occasionally be 53. To activate ISO 8601 counting, SET ^%SYS("sql","sys","week ISO8601")=1.

The date-expression can be a Caché date integer, a [$HOROLOG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog) or [$ZTIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztimestamp) value, an ODBC format date string, or a timestamp string with the format:

yyyy-mm-dd hh:mm:ss

The time portion of the timestamp is not evaluated and can be omitted. The date-expression can also be specified as data type %Library.FilemanDate, %Library.FilemanTimestamp, or %MV.Date.

The same week information can be returned by using the [DATEPART](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datepart) or [DATENAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datename) function.

This function can also be invoked from Caché ObjectScript using the [WEEK()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#WEEK) method call:

$SYSTEM.SQL.WEEK(date-expression)

Date Validation

WEEK performs the following checks on input values. If a value fails a check, the null string is returned.

*   A date string must be complete and properly formatted with the appropriate number of elements and digits for each element, and the appropriate separator character. Years must be specified as four digits.
    
*   Date values must be within a valid range. Years: 1841 through 9999. Months: 1 through 12. Days: 1 through 31.
    
*   The number of days in a month must match the month and year. For example, the date '02–29' is only valid if the specified year is a leap year.
    
*   Date values less than 10 may include or omit a leading zero. Other non-canonical integer values are not permitted. Therefore, a Day value of '07' or '7' is valid, but '007', '7.0' or '7a' are not valid.
    

Examples

The following Embedded SQL example returns the day of week and week of year for January 2, 2005 (which is a Sunday) and January 1, 2006 (which is a Sunday).

  SET x\="2005-1-2"
  SET y\="2006-1-1"
  &sql(SELECT {fn DAYOFWEEK(:x)},{fn WEEK(:x)},
       {fn DAYOFWEEK(:y)},{fn WEEK(:y)}
  INTO :a,:b,:c,:d)
  IF SQLCODE'=0 {
    WRITE !,"Error code ",SQLCODE }
  ELSE {
    WRITE !,"2005 Day of Week is: ",a," (Sunday=1)"
    WRITE " Week of Year is: ",b
    WRITE !,"2006 Day of Week is: ",c," (Sunday=1)"
    WRITE " Week of Year is: ",d }

 

The following examples return the number 9 because the date is the ninth week of the year 2004:

SELECT {fn WEEK('2004-02-25')} AS Wk\_Date,
       {fn WEEK('2004-02-25 08:35:22')} AS Wk\_Tstamp,
       {fn WEEK(59590)} AS Wk\_DInt

 

The following example returns the number 54 because this particular date is in a leap year that began with Week 2 starting on the second day, as demonstrated by the example immediately following it:

SELECT {fn WEEK('2000-12-31')} AS Week

 

SELECT {fn WEEK('2000-01-01')}||{fn DAYNAME('2000-01-01')} AS WeekofDay1,
       {fn WEEK('2000-01-02')}||{fn DAYNAME('2000-01-02')} AS WeekofDay2

 

The following examples all return the current week:

SELECT {fn WEEK({fn NOW()})} AS Wk\_Now,
       {fn WEEK(CURRENT\_DATE)} AS Wk\_CurrD,
       {fn WEEK(CURRENT\_TIMESTAMP)} AS Wk\_CurrTS,
       {fn WEEK($HOROLOG)} AS Wk\_Horolog,
       {fn WEEK($ZTIMESTAMP)} AS Wk\_ZTS

 

The following Embedded SQL example shows the Caché default week of the year and the week of the year with the ISO 8601 standard applied:

TestISO
  SET def\=$DATA(^%SYS("sql","sys","week ISO8601"))
  IF def\=0 {SET ^%SYS("sql","sys","week ISO8601")\=0}
  ELSE {SET isoval\=^%SYS("sql","sys","week ISO8601")}
     IF isoval\=1 {GOTO UnsetISO }
     ELSE {SET isoval\=0 GOTO WeekOfYear }
UnsetISO
  SET ^%SYS("sql","sys","week ISO8601")\=0
WeekOfYear
  &sql(SELECT {fn WEEK($HOROLOG)} INTO :a)
  WRITE "For Today:",!
  WRITE "default week of year is ",a,!
  SET ^%SYS("sql","sys","week ISO8601")\=1
  &sql(SELECT {fn WEEK($HOROLOG)} INTO :b)
  WRITE "ISO8601 week of year is ",b,!
ResetISO
  SET ^%SYS("sql","sys","week ISO8601")\=isoval

 

See Also

*   SQL functions: [DATENAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datename) [DATEPART](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datepart) [DAYOFWEEK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dayofweek) [MONTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_month) [QUARTER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_quarter) [TO\_DATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_todate) [YEAR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_year)
    
*   Caché ObjectScript special variables: [$HOROLOG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog) [$ZTIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztimestamp)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_userfunction "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_xmlconcat "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:38**

Source: **RSQL\_week.xml**