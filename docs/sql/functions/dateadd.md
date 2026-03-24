# DATEADD

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_dateadd

---

 

Caché SQL Reference

DATEADD

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_date "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datediff "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [DATEADD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dateadd "DATEADD") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A date/time function that returns a date calculated by adding an integer number of date part units (such as hours or days) to a specified timestamp.

Synopsis

DATEADD(datepart,integer-exp,date-exp)

Arguments

datepart

The name (or abbreviation) of a date or time part. This name can be specified in uppercase or lowercase, with or without enclosing quotes. The datepart can be specified as a literal or a host variable.

integer-exp

A numeric expression of any number type. The value is truncated to an integer (positive or negative). The value indicates the number of datepart units that will be added to (or subtracted from) date-exp.

date-exp

The date/time expression to be modified. This can be a date string or a timestamp string, or a function such as CURRENT\_DATE. The value returned is always a timestamp.

Description

The DATEADD function modifies a date/time expression by incrementing the specified date part by the specified number of units. For example, if datepart is 'month' and integer-exp is 5, DATEADD increments date-exp by five months. You can also decrement a date part by specifying a negative integer for integer-exp.

The calculated date is returned as a TIMESTAMP. DATEADD always returns a complete date/time expression in the format:

yyyy-mm-dd hh:mm:ss

DATEADD values are displayed in this format in Display mode, ODBC mode, or Logical mode.

DATEADD always returns a valid date, taking into account the number of days in a month, and calculating for leap year. For example, incrementing January 31 by one month returns February 28 (the highest valid date in the month), unless the specified year is a leap year, in which case it returns February 29. Incrementing a leap year date of February 29 by one year returns February 28. Incrementing a leap year date of February 29 by four years returns February 29.

If you specify a date-exp that includes fractional seconds, the returned value also includes fractional seconds. If you omit the time portion of date-exp, DATEADD returns a default time of 00:00:00. If you omit the date portion of date-exp, DATEADD returns a default date of 1900–01–01.

DATEADD and TIMESTAMPADD handle quarters (3–month intervals); DATEDIFF and TIMESTAMPDIFF do not handle quarters.

Similar time/date modification operations can be performed using the [TIMESTAMPADD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_timestampadd) ODBC scalar function.

This function can also be invoked from Caché ObjectScript using the [DATEADD()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#DATEADD) method call:

$SYSTEM.SQL.DATEADD(datepart,integer-exp,date-exp)

Datepart Argument

The datepart argument can be one of the following date/time components, either the full name (the Date Part column) or its abbreviation (the Abbreviation column). These datepart component names and abbreviations are not case-sensitive.

Date Part

Abbreviations

integer-exp = 1

year

yyyy, yy

Increments year by 1.

quarter

qq, q

Increments month by 3.

month

mm, m

Increments month by 1.

week

wk, ww

Increments day by 7.

weekday

dw

Increments day by 1.

day

dd, d

Increments day by 1.

dayofyear

dy, y

Increments day by 1.

hour

hh

Increments hour by 1.

minute

mi, n

Increments minute by 1.

second

ss, s

Increments second by 1.

millisecond

ms

Increments by .001 of a second.

Incrementing or decrementing a date part causes other date parts to be modified appropriately. For example, incrementing the hour past midnight automatically increments the day, which may in turn increment the month, and so forth.

A datepart can be specified as a quoted string or without quotes. These syntax variants perform slightly different operations:

*   Quotes: DATEADD('month',12,$HOROLOG): the datepart is treated as a literal when creating cached queries. Caché SQL performs [literal substitution](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_cachedqueries#GSQLOPT_cachedqueries_litsub). This produces a more generally reusable cached query.
    
*   No quotes: DATEADD(month,12,$HOROLOG): the datepart is treated as a keyword when creating cached queries. No literal substitution. This produces a more specific cached query.
    

If you specify an invalid datepart value as a literal, an SQLCODE -8 error code is issued. However, if you supply an invalid datepart value as a host variable, no SQLCODE error is issued and the DATEPART function returns a value of NULL.

Date Expression Formats

The date-exp argument can be in any of the following formats, and may include or omit fractional seconds:

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

Sybase/SQL-Server-time represents one of these three formats:

HH:MM\[:SS:SSS\]\[{AM|PM}\]
HH:MM\[:SS.S\]
HH\[''\]{AM|PM}

If the year is given as two digits, Caché checks the sliding window to interpret the date. The system default for the sliding window can be set via the %DATE utility, which is documented only in the “[Legacy Documentation](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GDOC_legacy)” chapter in Using InterSystems Documentation. For information on setting the sliding window for the current process, see the documentation for the Caché ObjectScript [$ZDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdate), [$ZDATEH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdateh), [$ZDATETIME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetime) and [$ZDATETIMEH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetimeh) functions.

Note that DATEADD is provided for Sybase and Microsoft SQL Server compatibility.

Range and Value Checking

DATEADD performs the following checks on input values. If a value fails a check, the null string is returned.

*   A date string must be complete and properly formatted with the appropriate number of elements and digits for each element, and the appropriate separator character. Years must be specified as four digits.
    
*   Date values must be within a valid range. Years: 1841 through 9999. Months: 1 through 12. Days: 1 through 31. Hours: 0 through 23. Minutes: 0 through 59. Seconds: 0 through 59.
    
*   The incremented year value returned must be within the range 1841 through 9999. Incrementing beyond this range returns <null>.
    
*   The number of days in a month must match the month and year. For example, the date '02–29' is only valid if the specified year is a leap year.
    
*   Date values less than 10 may include or omit a leading zero. Other non-canonical integer values are not permitted. Therefore, a Day value of '07' or '7' is valid, but '007', '7.0' or '7a' are not valid.
    

Examples

The following example adds 1 week to the specified date:

SELECT DATEADD('week',1,'1999-12-20') AS NewDate

 

it returns 1999-12-27 00:00:00, because adding 1 week adds 7 days. Note that DATEADD supplies the omitted time portion.

The following example adds 5 months to the original timestamp:

SELECT DATEADD(MM,5,'1999-12-20 12:00:00') AS NewDate

 

it returns 2000-05-20 12:00:00 because adding 5 months also increments the year.

The following example also adds 5 months to the original timestamp:

SELECT DATEADD('mm',5,'1999-01-31 12:00:00') AS NewDate

 

it returns 1999-06-30 12:00:00. Here DATEADD modified the day value as well as the month, because simply incrementing the month would result in June 31, which is an invalid date.

The following example adds 45 minutes to the original timestamp:

SELECT DATEADD(MI,45,'1999-12-20 12:00:00') AS NewTime

 

it returns 1999-12-20 12:45:00.

The following example also adds 45 minutes to the original timestamp, but in this case the addition increments the date:

SELECT DATEADD('mi',45,'1999-12-20 23:30:00') AS NewTime

 

it returns 1999-12-21 00:15:00.

The following example decrements the original timestamp by 45 minutes:

SELECT DATEADD(N,\-45,'1999-12-20 12:00:00') AS NewTime

 

it returns 1999-12-20 11:15:00.

The following example adds 60 days to the current date, adjusting for the varying lengths of months:

SELECT DATEADD(D,60,CURRENT\_DATE) AS NewDate

 

In the following example, the first DATEADD adds 92 days to the specified date, the second DATEADD adds 1 quarter to the specified date:

SELECT DATEADD('dd',92,'1999-12-20') AS NewDateD,
       DATEADD('qq',1,'1999-12-20') AS NewDateQ

 

The first returns 2000-03-21 00:00:00; the second returns 2000-03-20 00:00:00. Incrementing by a quarter increments the month field by 3, and, when needed, increments the year field. It also corrects for the maximum number of days for a given month.

The above examples all use date part abbreviations. However, you can also specify the date part by its full name, as in this example:

SELECT DATEADD('day',92,'1999-12-20') AS NewDate

 

it returns 2000-03-21 00:00:00.

The following Embedded SQL example uses host variables to perform the same DATEADD operation as the previous example:

  SET x\="day"
  SET datein\="1999-12-20"
  &sql(SELECT DATEADD(:x,92,:datein)
       INTO :dateout)
  WRITE "in:  ",datein,!,"out: ",dateout

 

See Also

*   [DATEDIFF](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datediff) function
    
*   [DATENAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datename) function
    
*   [DATEPART](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datepart) function
    
*   [TIMESTAMPADD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_timestampadd) function
    
*   [TIMESTAMPDIFF](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_timestampdiff) function
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_date "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datediff "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_dateadd.xml**