# DATEPART

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_datepart

---

 

Caché SQL Reference

DATEPART

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datename "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_day "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [DATEPART](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datepart "DATEPART") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A date/time function that returns an integer representing the value of the specified part of a date/time expression.

Synopsis

DATEPART(datepart,date-expression)

Arguments

datepart

The type of date/time information to return. The name (or abbreviation) of a date or time part. This name can be specified in uppercase or lowercase, with or without enclosing quotes. The datepart can be specified as a literal or a host variable.

date-expression

A date/time expression from which the specified datepart is to be returned. date-expression must contain a value of type datepart.

Description

The DATEPART function returns the datepart information about a specified date/time expression as an integer. To return this information as a character string, use [DATENAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datename). If the datepart value is sqltimestamp (or sts), the DATEPART can return either data type TIMESTAMP or INTEGER, as described below.

DATEPART returns the value of only one element of date-expression; to return a string containing multiple date parts, use [TO\_DATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_todate).

This function can also be invoked from Caché ObjectScript using the [DATEPART()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#DATEPART) method call:

$SYSTEM.SQL.DATEPART(datepart,date-expression)

DATEPART is provided for Sybase and Microsoft SQL Server compatibility.

Datepart Argument

The datepart argument can be one of the following date/time components, either the full name (the Date Part column) or its abbreviation (the Abbreviation column). These datepart component names and abbreviations are not case-sensitive.

Date Part

Abbreviations

Return Values

year

yyyy, yy

1840-9999

quarter

qq, q

1-4

month

mm, m

1-12

week

wk, ww

1-53

weekday

dw

1-7 (Sunday,...,Saturday)

dayofyear

dy, y

1-366

day

dd, d

1-31

hour

hh

0-23

minute

mi, n

0-59

second

ss, s

0-59

millisecond

ms

0-999 (with precision of 3).

sqltimestamp

sts

SQL\_TIMESTAMP: yyyy-mm-dd hh:mm:ss

The preceding table shows the default return values for the various date parts. You can modify the returned values for several of these date parts by using the [SET OPTION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_setoption) command with various time and date options.

week: Caché can be configured to determine the week of the year for a given date using either the Caché default algorithm or the ISO 8601 standard algorithm. For further details, refer to the [WEEK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_week) function.

weekday: The Caché default for weekday is to designate Sunday as first day of the week (weekday=1). However, you can configure the first day of the week to another value, or you can apply the ISO 8601 standard which designates Monday as first day of the week. For further details, refer to the [DAYOFWEEK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dayofweek) function.

millisecond: Caché returns three fractional digits of precision, with trailing zeroes removed. If the date-expression has more than three fractional digits of precision, Caché truncates it to three digits.

sqltimestamp: Caché converts the input data to timestamp format and supplies zero values for the time elements, if necessary. Can return either data type TIMESTAMP or data type INTEGER (see below). The sqltimestamp (abbreviated sts) datepart value is for use only with DATEPART. Do not attempt to use this value in other contexts.

A datepart can be specified as a quoted string, without quotes, or with parentheses around a quoted string. No literal substitution is performed on datepart, regardless of how specified; literal substitution is performed on date-expression. All datepart values return a data type INTEGER value, except sqltimestamp (or sts), which returns its value as a character string of data type TIMESTAMP.

Date Input Formats

The date-expression argument can be in any of the following formats:

*   A Caché %Date logical value (+$H)
    
*   A Caché %TimeStamp logical value (YYYY-MM-DD HH:MM:SS)
    
*   A Caché %String (or compatible) value
    

The Caché %String (or compatible) value can be in any of the following formats:

*   99999,99999 ($H format)
    
*   Sybase/SQL-Server-date Sybase/SQL-Server-time
    
*   Sybase/SQL-Server-time Sybase/SQL-Server-date
    
*   Sybase/SQL-Server-date (default time is 00:00:00)
    
*   Sybase/SQL-Server-time (default date is 01/01/1900)
    

Sybase/SQL-Server-date is one of these five formats:

mmdelimiterdddelimiter\[yy\]yy
dd Mmm\[mm\]\[,\]\[yy\]yy
dd \[yy\]yy Mmm\[mm\]
yyyy Mmm\[mm\] dd
yyyy \[dd\] Mmm\[mm\]

where delimiter is a slash (/), hyphen (-), or period (.).

If the year is given as two digits, Caché checks the sliding window to interpret the date. The system default for the sliding window can be set via the %DATE utility, which is documented only in the “[Legacy Documentation](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GDOC_legacy)” chapter in Using InterSystems Documentation. For information on setting the sliding window for the current process, see the documentation for the Caché ObjectScript [$ZDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdate), [$ZDATEH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdateh), [$ZDATETIME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetime) and [$ZDATETIMEH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetimeh) functions.

Sybase/SQL-Server-time represents one of these three formats:

HH:MM\[:SS:SSS\]\[{AM|PM}\]
HH:MM\[:SS.S\]
HH\[''\]{AM|PM}

For sqltimestamp, time is returned as a 24-hour clock. Range validation is not performed for time components. Fractional seconds are truncated.

Invalid Argument Error Codes

If you specify an invalid datepart option, DATEPART generates an SQLCODE -8 error code, and the following %msg: 'badopt' is not a recognized DATEPART option.

If you specify an invalid date-expression value (for example, an alphabetic text string), DATEPART generates an SQLCODE -400 error code, and the following %msg: Invalid input to DATEPART() function: DATEPART('year','badval'). If you specify a date-expression that fails validation (as described below), DATEPART generates an SQLCODE -400 error code, and the following %msg: Unexpected error occurred: <ILLEGAL VALUE>datepart.

Range and Value Checking

DATEPART performs the following checks on date-expression values. If a value fails a check, the null string is returned.

*   A valid date-expression may consist of a date string (yyyy-mm-dd), a time string (hh:mm:ss), or a date and time string (yyyy-mm-dd hh:mm:ss). If both date and time are specified, both must be valid. For example, you can return a Year value if no time string is specified, but you cannot return a Year value if an invalid time string is specified.
    
*   A date string must be complete and properly formatted with the appropriate number of elements and digits for each element, and the appropriate separator character. For example, you cannot return a Year value if the Day value is omitted. Years must be specified as four digits.
    
*   A time string must be properly formatted with the appropriate separator character. Because a time value can be zero, you can omit one or more time elements (either retaining or omitting the separator characters) and these elements will be returned with a value of zero. Thus, 'hh:mm:ss', 'hh:mm:', 'hh:mm', 'hh::ss', 'hh::', 'hh', and ':::' are all valid. To omit the Hour element, date-expression must not have a date portion of the string, and you must retain at least one separator character (:).
    
*   Date and time values must be within a valid range. Years: 1841 through 9999. Months: 1 through 12. Days: 1 through 31. Hours: 0 through 23. Minutes: 0 through 59. Seconds: 0 through 59.
    
*   The number of days in a month must match the month and year. For example, the date '02–29' is only valid if the specified year is a leap year.
    
*   Most date and time values less than 10 may include or omit a leading zero. However, an Hour value of less than 10 must include the leading zero if it is part of a datetime string. Other non-canonical integer values are not permitted. Therefore, a Day value of '07' or '7' is valid, but '007', '7.0' or '7a' are not valid.
    

Examples

In the following example, each DATEPART returns the year portion of the datetime string (in this case, 2004) as an integer. Note that date-expression can be in various formats, and datepart can be specified as either the datepart name or datepart abbreviation, quoted or unquoted:

SELECT DATEPART('yy','2004-12-20 12:00:00') AS YearDTS,
       DATEPART('year','2004-02-25') AS YearDS,
       DATEPART(YYYY,'02/25/2004') AS YearD,
       DATEPART(YEAR,59590) AS YearHD,
       DATEPART('Year','59590,23456') AS YearHDT

 

The following example returns the current year and quarter, based on the $HOROLOG value:

SELECT DATEPART('yyyy',$HOROLOG) AS Year,DATEPART('q',$HOROLOG) AS Quarter

 

The following Embedded SQL example uses host variables to supply the DATEPART argument values:

  SET x\="year"
  SET datein\="2004-02-25"
  &sql(SELECT DATEPART(:x,:datein)
       INTO :partout)
  WRITE "the ",x," is ",partout

 

The following example returns the birth day-of-week for the Sample.Person table, ordered by day of week:

SELECT Name,DOB,DATEPART('weekday',DOB) AS bday
FROM Sample.Person
ORDER BY bday,DOB

 

In the following example, each DATEPART returns 20 as the minutes portion of the date-expression string:

SELECT DATEPART('mi','1999-1-20 12:20:07') AS Minutes,
       DATEPART('n','1999-02-20 10:20:') AS Minutes,
       DATEPART(MINUTE,'1999-02-20 10:20') AS Minutes

 

In the following example, each DATEPART returns 0 as the seconds portion of the date-expression string:

SELECT DATEPART('ss','1999-02-20 03:20:') AS Seconds,
       DATEPART('S','1999-02-20 03:20') AS Seconds,
       DATEPART('Second','1999-02-20') AS Seconds

 

The following example returns the full SQL timestamp as a TIMESTAMP data type. DATEPART fills in the missing time information to return a timestamp of '2004-02-25 00:00:00':

SELECT DATEPART(sqltimestamp,'2/25/2004') AS DTStamp

 

The following example returns the full SQL timestamp as an INTEGER data type. DATEPART fills in the missing time information to return a timestamp of '2004-02-25 00:00:00':

SELECT DATEPART('sqltimestamp','2/25/2004') AS DTStampAsInt

 

The following example supplies a date and time in $HOROLOG format, and returns a timestamp of '2004-02-25 06:30:56':

SELECT DATEPART(sqltimestamp,'59590,23456') AS DTStamp

 

The following example uses a subquery with DATEPART to return those people whose birthday is leap year day (February 29th):

SELECT Name,DOB
FROM (SELECT Name,DOB,DATEPART('dd',DOB) AS DayNum,DATEPART('mm',DOB) AS Month FROM Sample.Person)
WHERE Month\=2 AND DayNum\=29 

 

See Also

*   [DATEDIFF](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datediff) function
    
*   [DATENAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datename) function
    
*   [TIMESTAMPADD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_timestampadd) function
    
*   [TIMESTAMPDIFF](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_timestampdiff) function
    
*   [TO\_DATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_todate) function
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datename "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_day "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_datepart.xml**