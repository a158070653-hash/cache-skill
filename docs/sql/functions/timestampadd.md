# TIMESTAMPADD

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_timestampadd

---

 

Caché SQL Reference

TIMESTAMPADD

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_tan "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_timestampdiff "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [TIMESTAMPADD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_timestampadd "TIMESTAMPADD") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A scalar date/time function that returns a new timestamp calculated by adding a number of intervals of a specified type to a specified timestamp.

Synopsis

{fn TIMESTAMPADD(interval-type,integer-exp,timestamp-exp)}

Arguments

interval-type

The type of time/date interval that integer-exp represents, specified as a keyword.

integer-exp

An integer value expression that is to be added to timestamp-exp.

timestamp-exp

A TIMESTAMP value expression, which will be increased by the value of integer-exp.

Description

The TIMESTAMPADD function modifies a date/time expression by incrementing the specified date part by the specified number of units. For example, if interval-type is SQL\_TSI\_MONTH and integer-exp is 5, TIMESTAMPADD increments timestamp-exp by five months. You can also decrement a date part by specifying a negative integer for integer-exp. The calculated date is returned as a TIMESTAMP. You can increment or decrement by fractional seconds, counted in thousandths of a second (.001).

Note that TIMESTAMPADD can only be used as an ODBC scalar function (with the curly brace syntax).

Similar time/date modification operations can be performed on a timestamp using the [DATEADD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dateadd) general function.

Interval Types

The interval-type argument can be one of the following timestamp intervals:

*   SQL\_TSI\_FRAC\_SECOND
    
*   SQL\_TSI\_SECOND
    
*   SQL\_TSI\_MINUTE
    
*   SQL\_TSI\_HOUR
    
*   SQL\_TSI\_DAY
    
*   SQL\_TSI\_WEEK
    
*   SQL\_TSI\_MONTH
    
*   SQL\_TSI\_QUARTER
    
*   SQL\_TSI\_YEAR
    

These timestamp intervals may be specified with or without enclosing double quotation marks: SQL\_TSI\_WEEK or "SQL\_TSI\_WEEK".

Incrementing or decrementing a timestamp interval causes other intervals to be modified appropriately. For example, incrementing the hour past midnight automatically increments the day, which may in turn increment the month, and so forth. TIMESTAMPADD always returns a valid date, taking into account the number of days in a month, and calculating for leap year. For example, incrementing January 31 by one month returns February 28 (the highest valid date in the month), unless the specified year is a leap year, in which case it returns February 29.

DATEADD and TIMESTAMPADD handle quarters (3-month intervals); DATEDIFF and TIMESTAMPDIFF do not handle quarters.

Timestamp Format

The timestamp-exp argument value has the same logical format and external format: a string of the form:

yyyy-mm-dd hh:mm:ss

For this argument:

*   If timestamp-exp specifies only a date value, the date portion of timestamp-exp is set to '1900–01–01' before calculating the resulting timestamp.
    
*   If timestamp-exp specifies only a date value, the time portion of timestamp-exp is set to '00:00:00' before calculating the resulting timestamp.
    
*   You can include or omit fractional seconds.
    

Range and Value Checking

TIMESTAMPADD performs the following checks on input values. If a value fails a check, the null string is returned.

*   A date string must be complete and properly formatted with the appropriate number of elements and digits for each element, and the appropriate separator character. Years must be specified as four digits.
    
*   Date values must be within a valid range. Years: 1841 through 9999. Months: 1 through 12. Days: 1 through 31. Hours: 0 through 23. Minutes: 0 through 59. Seconds: 0 through 59.
    
*   The incremented year value returned must be within the range 1841 through 9999. Incrementing beyond this range returns <null>.
    
*   The number of days in a month must match the month and year. For example, the date '02–29' is only valid if the specified year is a leap year.
    
*   Date values less than 10 may include or omit a leading zero. Other non-canonical integer values are not permitted. Therefore, a Day value of '07' or '7' is valid, but '007', '7.0' or '7a' are not valid.
    

Examples

The following example adds 1 week to the original timestamp:

SELECT {fn TIMESTAMPADD(SQL\_TSI\_WEEK,1,'2003-12-20 12:00:00')}

 

it returns 2003-12-27 12:00:00, because adding 1 week adds 7 days.

The following example adds 5 months to the original timestamp:

SELECT {fn TIMESTAMPADD(SQL\_TSI\_MONTH,5,'1999-12-20 12:00:00')}

 

returns 2000-05-20 12:00:00 because in this case adding 5 months also increments the year.

The following example also adds 5 months to the original timestamp:

SELECT {fn TIMESTAMPADD(SQL\_TSI\_MONTH,5,'1999-01-31 12:00:00')}

 

it returns 1999-06-30 12:00:00. Here TIMESTAMPADD modified the day value as well as the month, because simply incrementing the month would result in June 31, which is an invalid date.

The following example increments the original timestamp by 45 minutes:

SELECT {fn TIMESTAMPADD(SQL\_TSI\_MINUTE,45,'1999-12-20 00:00:00')}

 

returns 1999-12-20 00:45:00.

The following example decrements the original timestamp by 45 minutes:

SELECT {fn TIMESTAMPADD(SQL\_TSI\_MINUTE,\-45,'1999-12-20 00:00:00')}

 

it returns 1999-12-19 23:15:00. Note that in this case decrementing the time also decremented the day.

See Also

[TIMESTAMPDIFF](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_timestampdiff) [DATEADD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dateadd) [DATENAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datename) [DATEPART](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datepart) [TO\_TIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_totimestamp)

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_tan "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_timestampdiff "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:38**

Source: **RSQL\_timestampadd.xml**