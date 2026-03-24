# TIMESTAMPDIFF

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_timestampdiff

---

 

Caché SQL Reference

TIMESTAMPDIFF

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_timestampadd "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_tochar "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [TIMESTAMPDIFF](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_timestampdiff "TIMESTAMPDIFF") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A scalar date/time function that returns the integer number of intervals of a specified type between two timestamps.

Synopsis

{fn TIMESTAMPDIFF(interval-type,startdate,enddate)}

Arguments

interval-type

The type of time/date interval that the returned value will represent.

startdate

A TIMESTAMP value expression.

enddate

A TIMESTAMP value expression that will be compared to startdate.

Description

The TIMESTAMPDIFF function returns the difference between two given timestamps (that is, one timestamp is subtracted from the other) for the specified interval (seconds, days, weeks, etc.). The value returned is an INTEGER that specifies the number of these intervals between the two timestamps. (If enddate is earlier than startdate, TIMESTAMPDIFF returns a negative INTEGER value.) You can return an interval of fractional seconds, counted in thousandths of a second (.001).

The interval-type argument can be one of the following timestamp intervals:

*   SQL\_TSI\_FRAC\_SECOND
    
*   SQL\_TSI\_SECOND
    
*   SQL\_TSI\_MINUTE
    
*   SQL\_TSI\_HOUR
    
*   SQL\_TSI\_DAY
    
*   SQL\_TSI\_WEEK
    
*   SQL\_TSI\_MONTH
    
*   SQL\_TSI\_YEAR
    

These timestamp intervals may be specified with or without enclosing double quotation marks: SQL\_TSI\_WEEK or "SQL\_TSI\_WEEK".

TIMESTAMPDIFF and DATEDIFF do not handle quarters (3-month intervals).

The timestamp argument values have the same logical format and external format: a string of the form 'yyyy-mm-dd hh:mm:ss'.

*   If either timestamp expression is a TIME value and interval-type specifies calendar time (days, weeks, months, or years), the date portion of the expression is set to '1900–01–01' before calculating the resulting timestamp.
    
*   If either timestamp expression is a DATE value and interval-type specifies clock time (seconds, minutes, or hours), the time portion of the expression is set to '00:00:00' before calculating the resulting timestamp.
    
*   You can include or omit fractional seconds.
    

Note that TIMESTAMPDIFF can only be used as an ODBC scalar function (with the curly brace syntax). Similar time/date comparison operations can be performed on a timestamp using the [DATEDIFF](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datediff) general function.

Range and Value Checking

TIMESTAMPDIFF performs the following checks on input values. If a value fails a check, the null string is returned.

*   A valid startdate or enddate may consist of a date string (yyyy-mm-dd), a time string (hh:mm:ss), or a date and time string (yyyy-mm-dd hh:mm:ss). If both date and time are specified, both must be valid. For example, you can return a Year value if no time string is specified, but you cannot return a Year value if an invalid time string is specified.
    
*   A date string must be complete and properly formatted with the appropriate number of elements and digits for each element, and the appropriate separator character. For example, you cannot return a Year value if the Day value is omitted. Years must be specified as four digits. If you omit the date portion of an input value, TIMESTAMPDIFF defaults to '1900–01–01'.
    
*   A time string must be properly formatted with the appropriate separator character. Because a time value can be zero, you can omit one or more time elements (either retaining or omitting the separator characters) and these elements will be returned with a value of zero. Thus, 'hh:mm:ss.nnn', 'hh:mm:ss', 'hh:mm:', 'hh:mm', 'hh::ss', 'hh::', 'hh', and ':::' are all valid. To omit the Hour element, the date expression must not have a date portion of the string, and you must retain at least one separator character (:).
    
*   Date and time values must be within a valid range. Years: 1841 through 9999. Months: 1 through 12. Days: 1 through 31. Hours: 0 through 23. Minutes: 0 through 59. Seconds: 0 through 59.
    
*   The number of days in a month must match the month and year. For example, the date '02–29' is only valid if the specified year is a leap year.
    
*   Most date and time values less than 10 may include or omit a leading zero. However, an Hour value of less than 10 must include the leading zero if it is part of a datetime string. Other non-canonical integer values are not permitted. Therefore, a Day value of '07' or '7' is valid, but '007', '7.0' or '7a' are not valid.
    

Examples

The following example returns 7 because the second timestamp (1999-12-20 12:00:00) is 7 months greater than the first one:

SELECT {fn TIMESTAMPDIFF(SQL\_TSI\_MONTH,
     '1999-5-19 00:00:00','1999-12-20 12:00:00')}

 

The following example returns 566 because the second timestamp ('12:00:00') is 566 minutes greater than the first one (02:34:12):

SELECT {fn TIMESTAMPDIFF(SQL\_TSI\_MINUTE,'02:34:12','12:00:00')}

 

See Also

[TIMESTAMPADD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_timestampadd) [DATEDIFF](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datediff) [TO\_TIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_totimestamp)

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_timestampadd "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_tochar "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:38**

Source: **RSQL\_timestampdiff.xml**