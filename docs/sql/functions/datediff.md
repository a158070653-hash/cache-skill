# DATEDIFF

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_datediff

---

 

Caché SQL Reference

DATEDIFF

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dateadd "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datename "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [DATEDIFF](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datediff "DATEDIFF") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A date/time function that returns an integer difference for a specified datepart between two dates.

Synopsis

DATEDIFF(datepart,startdate,enddate)

Arguments

datepart

The name (or abbreviation) of a date or time part. This name can be specified in uppercase or lowercase, with or without enclosing quotes. The datepart can be specified as a literal or a host variable.

startdate

The starting date/time for the interval. May be a date, a time, or a datetime in a variety of standard formats.

enddate

The ending date/time for the interval. May be a date, a time, or a datetime in a variety of standard formats. startdate is subtracted from enddate to determine how many datepart intervals are between the two dates.

Description

The DATEDIFF function returns the INTEGER number of the specified datepart difference between the two specified dates. The date range begins at startdate and ends at enddate. (If enddate is earlier than startdate, DATEDIFF returns a negative INTEGER value.)

DATEDIFF returns the total number of the specified unit between startdate and enddate. For example, the number of minutes between two datetime values evaluates the date component as well as the time component, and adds 1440 minutes for each day difference. DATEDIFF returns the count of the specified date part boundaries crossed between startdate and enddate. For example, that any two dates that specify sequential years (for example 2010-09-23 and 2011-01-01) return a year DATEDIFF of 1, regardless of whether the actual duration between the two dates is more than or less than 365 days. Similarly, the number of minutes between 12:23:59 and 12:24:05 is 1, although only 6 seconds actually separate the two values.

Note that DATEDIFF is provided for Sybase and Microsoft SQL Server compatibility. Similar time/date comparison operations can be performed using the [TIMESTAMPDIFF](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_timestampdiff) ODBC scalar function.

This function can also be invoked from Caché ObjectScript using the [DATEDIFF()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#DATEDIFF) method call:

$SYSTEM.SQL.DATEDIFF(datepart,startdate,enddate)

Specifying an invalid datepart, startdate, or enddate to the DATEDIFF() method generates a <ZDDIF> error.

Datepart Argument

The datepart argument can be one of the following date/time components, either the full name (the Date Part column) or its abbreviation (the Abbreviation column). These datepart component names and abbreviations are not case-sensitive.

Date Part

Abbreviations

year

yyyy, yy

month

mm, m

week

wk, ww

weekday

dw

day

dd, d

dayofyear

dy

hour

hh

minute

mi, n

second

ss, s

millisecond

ms

The weekday and dayofyear datepart values are functionally identical to the day datepart value.

DATEDIFF and TIMESTAMPDIFF do not handle quarters (3-month intervals).

If you specify a startdate and enddate that include fractional seconds, you can return the difference as a number of fractional seconds, expressed as thousands of a second (.001), as shown in the following example:

SELECT DATEDIFF('ms','62871,56670.10','62871,56670.27'),     /\* returns 170 \*/
       DATEDIFF('ms','62871,56670.1111','62871,56670.27222') /\* returns 161.12 \*/

 

A datepart can be specified as a quoted string or without quotes. These syntax variants perform slightly different operations:

*   Quotes: DATEDIFF('month','2004-02-25',$HOROLOG): the datepart is treated as a literal when creating cached queries. Caché SQL performs [literal substitution](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_cachedqueries#GSQLOPT_cachedqueries_litsub). This produces a more generally reusable cached query.
    
*   No quotes: DATEDIFF(month,'2004-02-25',$HOROLOG): the datepart is treated as a keyword when creating cached queries. No literal substitution. This produces a more specific cached query.
    

Date Expression Formats

The startdate and enddate arguments can be in any of the following formats:

*   A Caché %Date logical value (+$H), also known as $HOROLOG format.
    
*   A Caché %TimeStamp logical value (YYYY-MM-DD HH:MM:SS.FFF), also known as ODBC format.
    
*   A Caché %String (or compatible) value.
    

The Caché %String (or compatible) value can be in any of the following formats, and may include or omit fractional seconds:

*   99999,99999 ([$HOROLOG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog) format). The $HOROLOG special variable does not return fractional seconds. However, you can specify a value in $HOROLOG format that includes fractional seconds: 99999,99999.999
    
*   Sybase/SQL-Server-date Sybase/SQL-Server-time
    
*   Sybase/SQL-Server-time Sybase/SQL-Server-date
    
*   Sybase/SQL-Server-date (default time is 00:00:00)
    
*   Sybase/SQL-Server-time (default date is 01/01/1900)
    

Sybase/SQL-Server-date is one of these five formats:

mm/dd/\[yy\]yy
dd Mmm\[mm\]\[,\]\[yy\]yy
dd \[yy\]yy Mmm\[mm\]
yyyy Mmm\[mm\] dd
yyyy \[dd\] Mmm\[mm\]

In the first syntactic format the delimiter can be a slash (/), a hyphen (-), or a period (.).

Sybase/SQL-Server-time represents one of these three formats:

HH:MM\[:SS\[:FFF\]\]\[{AM|PM}\]
HH:MM\[:SS\[.FFF\]\]
HH\[''\]{AM|PM}

Years

If the year is given as two digits or the date is omitted entirely, Caché checks the sliding window to interpret the date. The system-wide default for the sliding window is 1900; thus by default a two-digit year is assumed to be in the 20th century. This is shown in the following example:

SELECT DATEDIFF('year','10/11/14','09/14/2015'),
       DATEDIFF('year','12:00:00','2004-07-09 12:00:00')

 

The sliding window default can be set system-wide or for the current process via the %DATE utility, which is documented only in the “[Legacy Documentation](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GDOC_legacy)” chapter in Using InterSystems Documentation. For information on establishing a sliding window for interpreting a specified date with a two-digit year, see the documentation for the Caché ObjectScript [$ZDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdate), [$ZDATEH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdateh), [$ZDATETIME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetime) and [$ZDATETIMEH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetimeh) functions.

Fractional Seconds

DATEDIFF returns fractional seconds as milliseconds (a three-digit integer) regardless of the number of fractional digits specified. Fractional digits beyond three are represented as fractional milliseconds. This is shown in the following example:

SELECT DATEDIFF('ms','12:00:00.1','12:00:00.2'),
       DATEDIFF('ms','12:00:00.10009','12:00:00.20007')

 

Some NLS locales specify the fractional separator as a comma (European usage) rather than as a period. If the current locale is one of these locales, DATEDIFF accepts either a period or a comma as the fractional seconds separator character for local date formats. You cannot use a comma as the fractional seconds separator for a date in $HOROLOG format, or a date in ODBC format. Attempting to do so generates an SQLCODE -8. Both of these formats require a period regardless of the current NLS locale.

Time Differences Independent of TimeFormat

DATEDIFF returns a time difference in seconds and milliseconds, even when the TimeFormat for the current process is set to not return seconds. This is shown in the following example:

  SET tfmt\=##class(%SYS.NLS.Format).GetFormatItem("TimeFormat")
  DO ##class(%SYS.NLS.Format).SetFormatItem("TimeFormat",1)
     WRITE "datetime values (with seconds) are: ",!,
           $ZDATETIME("62871,56670.10",1,\-1),"  ",$ZDATETIME("62871,56673.27",1,\-1),!
  &sql(SELECT DATEDIFF('ss','62871,56670.10','62871,56673.27') INTO :x)
     WRITE "DATEDIFF number of seconds is: ",x,!!
  DO ##class(%SYS.NLS.Format).SetFormatItem("TimeFormat",2)
     WRITE "datetime values (without seconds) are: ",!,
           $ZDATETIME("62871,56670.10",1,\-1),"  ",$ZDATETIME("62871,56673.27",1,\-1),!
  &sql(SELECT DATEDIFF('ss','62871,56670.10','62871,56673.27') INTO :x)
     WRITE "DATEDIFF number of seconds is: ",x,!
  DO ##class(%SYS.NLS.Format).SetFormatItem("TimeFormat",tfmt)

 

Range and Value Checking

DATEDIFF performs the following checks on input values. If a value fails a check, the null string is returned.

*   A valid startdate or enddate may consist of a date string (yyyy-mm-dd), a time string (hh:mm:ss), or a date and time string (yyyy-mm-dd hh:mm:ss). If both date and time are specified, both must be valid. For example, you can return a Year value if no time string is specified, but you cannot return a Year value if an invalid time string is specified.
    
*   A date string must be complete and properly formatted with the appropriate number of elements and digits for each element, and the appropriate separator character. For example, you cannot return a Year value if the Day value is omitted. Years must be specified as four digits. If you omit the date portion of an input value, DATEDIFF defaults to '1900–01–01'.
    
*   A time string must be properly formatted with the appropriate separator character. If you omit the time portion of an input value, DATEDIFF defaults to '00:00:00.000' (midnight). Because a time value can be zero, you can omit one or more time elements (either retaining or omitting the separator characters) and these elements will be returned with a value of zero. Thus, 'hh:mm:ss', 'hh:mm:', 'hh:mm', 'hh::ss', 'hh::', 'hh', and ':::' are all valid. To omit the Hour element, the date expression must not have a date portion of the string, and you must retain at least one separator character (:).
    
*   Date and time values must be within a valid range. Years: 1841 through 9999. Months: 1 through 12. Days: 1 through 31. Hours: 0 through 23. Minutes: 0 through 59. Seconds: 0 through 59.
    
*   The number of days in a month must match the month and year. For example, the date '02–29' is only valid if the specified year is a leap year.
    

Error Handling

*   In Embedded SQL, if you specify an invalid datepart as an input variable, an SQLCODE -8 error code is issued. If you specify an invalid datepart as a literal, a <SYNTAX> error occurs. If you specify an invalid startdate or enddate as either an input variable or a literal, an SQLCODE -8 error code is issued.
    
*   In Dynamic SQL, if you supply an invalid datepart, startdate, or enddate, the DATEDIFF function returns a value of NULL. No SQLCODE error is issued.
    

Examples

The following example returns 353 because there are 353 days (D) between the two timestamps:

SELECT DATEDIFF(D,'1999-01-01 00:00:00','1999-12-20 12:00:00')

 

In the following example, each DATEDIFF returns 1, because the year portion of the dates differs by 1. The actual duration between the dates is not considered:

SELECT DATEDIFF('yyyy','1910-08-21','1911-08-21') AS ExactYear,
       DATEDIFF('yyyy','1910-06-30','1911-01-01') AS HalfYear,
       DATEDIFF('yyyy','1910-01-01','1911-12-31') AS Nearly2Years,
       DATEDIFF('yyyy','1910-12-31 11:59:59','1911-01-01 00:00:00') AS NewYearSecond

 

Note that the above examples use an abbreviation for the date part. However, you can specify the full name, as in this example:

SELECT DATEDIFF('year','1968-09-10 13:19:00','1999-12-20 00:00:00')

 

The following Embedded SQL example uses host variables to perform the same DATEDIFF operation as the previous example:

  SET x\="year"
  SET date1\="1968-09-10 13:19:00"
  SET date2\="1999-12-20 00:00:00"
  &sql(SELECT DATEDIFF(:x,:date1,:date2)
       INTO :diff)
  WRITE diff

 

The following example uses a subquery to return those records where the person date of birth is 1500 days or less from the current date:

SELECT Name,Age,DOB
FROM (SELECT Name,Age,DOB, DATEDIFF('dy',DOB,$HOROLOG) AS DaysTo FROM Sample.Person)
WHERE DaysTo <= 1500
ORDER BY Age

 

See Also

*   [DATEADD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dateadd) function
    
*   [DATENAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datename) function
    
*   [DATEPART](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datepart) function
    
*   [TIMESTAMPADD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_timestampadd) function
    
*   [TIMESTAMPDIFF](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_timestampdiff) function
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dateadd "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datename "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_datediff.xml**