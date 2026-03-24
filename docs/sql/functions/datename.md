# DATENAME

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_datename

---

 

Caché SQL Reference

DATENAME

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datediff "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datepart "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [DATENAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datename "DATENAME") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A date/time function that returns a string representing the value of the specified part of a date/time expression.

Synopsis

DATENAME(datepart,date-expression)

Arguments

datepart

The type of date/time information to return. The name (or abbreviation) of a date or time part. This name can be specified in uppercase or lowercase, with or without enclosing quotes. The datepart can be specified as a literal or a host variable.

date-expression

A date/time expression from which the date part is to be returned. date-expression must contain a value of type datepart.

Description

The DATENAME function returns the name of the specified part (such as the month "June") of a date/time value. The result is returned as a CHARACTER STRING. If the result is numeric (such as "23" for the day), it is still returned as a CHARACTER STRING. To return this information as an integer, use [DATEPART](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datepart). To return a string containing multiple date parts, use [TO\_DATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_todate).

Note that DATENAME is provided for Sybase and Microsoft SQL Server compatibility.

This function can also be invoked from Caché ObjectScript using the [DATENAME()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#DATENAME) method call:

$SYSTEM.SQL.DATENAME(datepart,date-expression)

Datepart Argument

The datepart argument can be a string containing one (and only one) of the following date/time components, either the full name (the Date Part column) or its abbreviation (the Abbreviation column). These datepart component names and abbreviations are not case-sensitive.

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

mm

January,...December

week

wk, ww

1-53

weekday

dw

Sunday, Monday, Tuesday, Wednesday, Thursday, Friday, Saturday

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

0-999 (with precision of 3)

If you specify an invalid datepart value as a literal, an SQLCODE -8 error code is issued. However, if you supply an invalid datepart value as a host variable, no SQLCODE error is issued and the DATENAME function returns a value of NULL.

The preceding table shows the default return values for the various date parts. You can modify the returned values for several of these date parts by using the [SET OPTION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_setoption) command with various time and date options.

week: Caché can be configured to determine the week of the year for a given date using either the Caché default algorithm or the ISO 8601 standard algorithm. For further details, refer to the [WEEK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_week) function.

weekday: The Caché default for weekday is to designate Sunday as first day of the week (weekday=1). However, you can configure the first day of the week to another value, or you can apply the ISO 8601 standard which designates Monday as first day of the week. For further details, refer to the [DAYOFWEEK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dayofweek) function.

millisecond: Caché returns three fractional digits of precision, with trailing zeroes removed. If the date-expression has more than three fractional digits of precision, Caché truncates it to three digits.

A datepart can be specified as a quoted string or without quotes. These syntax variants perform slightly different operations:

*   Quotes: DATENAME('month','2004-02-25'): the datepart is treated as a literal when creating cached queries. Caché SQL performs [literal substitution](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_cachedqueries#GSQLOPT_cachedqueries_litsub). This produces a more generally reusable cached query.
    
*   No quotes: DATENAME(month,'2004-02-25'): the datepart is treated as a keyword when creating cached queries. No literal substitution. This produces a more specific cached query.
    

Date Expression Formats

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

Range validation is not performed for time components. Fractional seconds are truncated.

Range and Value Checking

DATENAME performs the following checks on input values. If a value fails a check, the null string is returned.

*   A valid date-expression may consist of a date string (yyyy-mm-dd), a time string (hh:mm:ss), or a date and time string (yyyy-mm-dd hh:mm:ss). If both date and time are specified, both must be valid. For example, you can return a Year value if no time string is specified, but you cannot return a Year value if an invalid time string is specified.
    
*   A date string must be complete and properly formatted with the appropriate number of elements and digits for each element, and the appropriate separator character. For example, you cannot return a Year value if the Day value is omitted. Years must be specified as four digits.
    
*   A time string must be properly formatted with the appropriate separator character. Because a time value can be zero, you can omit one or more time elements (either retaining or omitting the separator characters) and these elements will be returned with a value of zero. Thus, 'hh:mm:ss', 'hh:mm:', 'hh:mm', 'hh::ss', 'hh::', 'hh', and ':::' are all valid. To omit the Hour element, date-expression must not have a date portion of the string, and you must retain at least one separator character (:).
    
*   Date and time values must be within a valid range. Years: 1841 through 9999. Months: 1 through 12. Days: 1 through 31. Hours: 0 through 23. Minutes: 0 through 59. Seconds: 0 through 59.
    
*   The number of days in a month must match the month and year. For example, the date '02–29' is only valid if the specified year is a leap year.
    
*   Most date and time values less than 10 may include or omit a leading zero. However, an Hour value of less than 10 must include the leading zero if it is part of a datetime string. Other non-canonical integer values are not permitted. Therefore, a Day value of '07' or '7' is valid, but '007', '7.0' or '7a' are not valid.
    

Examples

In the following example, each DATENAME returns 'Wednesday' because that is the day of week ('dw') of the specified date:

SELECT DATENAME('dw','2004-02-25') AS DayName,
       DATENAME(dw,'02/25/2004') AS DayName,
       DATENAME('DW',59590) AS DayName

 

The following example returns 'December' because that is the month name ('mm') of the specified date:

SELECT DATENAME('mm','1999-12-20 12:00:00') AS MonthName

 

The following example returns '1999' (as a string) because that is the year ('yy') of the specified date:

SELECT DATENAME('yy','1999-12-20 12:00:00') AS Year

 

Note that the above examples use the abbreviations of the date parts. However, you can specify the full name, as in this example:

SELECT DATENAME('year','1999-12-20 12:00:00') AS Year

 

The following example returns the current quarter, week-of-year, and day-of-year. Each value is returned as a string:

SELECT DATENAME('Q',$HOROLOG) AS Q,
       DATENAME('WK',$HOROLOG) AS WkCnt,
       DATENAME('DY',$HOROLOG) AS DayCnt

 

The following Embedded SQL example passes in the datepart and date-expression as a host variables:

  SET a\="year"
  SET b\=$HOROLOG
  &sql(SELECT DATENAME(:a,:b) INTO :c)
  WRITE "this year is: ",c

 

The following example uses a subquery to returns records from Sample.Person whose day of birth was a Wednesday:

SELECT Name AS WednesdaysChild,DOB
FROM (SELECT Name,DOB,DATENAME('dw',DOB) AS Wkday FROM Sample.Person)
WHERE Wkday\='Wednesday'
ORDER BY DOB

 

See Also

*   SQL functions: [DATEADD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dateadd), [DATEDIFF](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datediff), [DATEPART](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datepart), [TO\_DATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_todate), [TIMESTAMPADD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_timestampadd), [TIMESTAMPDIFF](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_timestampdiff)
    
*   Caché ObjectScript function: [$ZDATETIME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetime)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datediff "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datepart "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_datename.xml**