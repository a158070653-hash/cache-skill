# DATE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_date

---

 

Caché SQL Reference

DATE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datalength "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dateadd "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [DATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_date "DATE") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A function that takes a timestamp and returns a date.

Synopsis

DATE(timestamp)

Arguments

timestamp

An expression that specifies a timestamp or other date or date and time representation.

Description

DATE takes a timestamp expression and returns a date. The return value is of data type DATE. This is functionally the same as CAST(timestamp AS DATE). It accepts timestamp values with any of the following data types: %Library.TimeStamp, %Library.Date, and %Library.Integer or %Library.Numeric (for implicit logical dates, such as +$HOROLOG). It can also accept %Library.String values that are in a format compatible with %Library.TimeStamp (a valid ODBC date).

An invalid ODBC date string is evaluated as zero, which corresponds to the date December 31, 1840. A timestamp may contain just an ODBC format date or an ODBC format date and time. Although only the date portion of the ODBC timestamp is converted, the entire string is validated. An ODBC timestamp fails validation if the date portion is incomplete, if either the date or time portion contain an out-of-range value (including leap year calculations), or if timestamp contains any invalid format characters or trailing characters.

An empty string ('') argument returns 0 (December 31, 1840). A NULL argument returns NULL.

This function can also be invoked from Caché ObjectScript using the [DATE()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#DATE) method call:

  WRITE $SYSTEM.SQL.DATE("2016-02-23 12:37:45")

 

$HOROLOG and $ZTIMESTAMP

$HOROLOG and $ZTIMESTAMP return character string values. When a character string is cast to a numeric type, it always returns a numeric value of zero (0). The Caché DATE data type value for 0 is December 31, 1840.

Therefore, in order to interpret $HOROLOG or $ZTIMESTAMP as the current date, you must prefix it was a plus (+) sign, which forces numeric interpretation. This is shown in the following examples:

SELECT DATE($HOROLOG),DATE($ZTIMESTAMP)  // both return 12/31/1840

SELECT DATE(+$HOROLOG),DATE(+$ZTIMESTAMP)  // both return the current date

ODBC Date Strings

The DATE function and the $SYSTEM.SQL.DATE() method can both take an ODBC date format string. They validate the input string. If it passes validation, they return the corresponding date. If it fails validation, they return 0. Validation is performed as following:

*   The string must correspond to ODBC format: yyyy-mm-dd hh:mm:ss.xx. The entire string is parsed for correct format, not just the date portion of the string.
    
*   The string must contain (at least) a full date: yyyy-mm-dd. Leading zeros may be omitted or included. The time portion is optional, and any part of the time portion may be included: yyyy-mm-dd hh:.
    
*   Each numeric element of the string (both date portion and time portion) must contain a valid value. For example, month values must be in the range of 1 through 12 (inclusive). Day values cannot exceed the number of days for the specified month. Leap year days are calculated.
    
*   Dates must be within the Caché date range. The minimum date is 1840-12-31, the maximum date is 9999-12-31.
    

Examples

The following examples take a value of data type %Library.TimeStamp:

  SET myquery \= "SELECT {fn NOW} AS NowCol,DATE({fn NOW}) AS DateCol"
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET qStatus \= tStatement.%Prepare(myquery)
   IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
  SET rset \= tStatement.%Execute()
  DO rset.%Display()

 

  SET myquery \= "SELECT CURRENT\_TIMESTAMP AS TSCol,DATE(CURRENT\_TIMESTAMP) AS DateCol"
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET qStatus \= tStatement.%Prepare(myquery)
   IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
  SET rset \= tStatement.%Execute()
  DO rset.%Display()

 

  SET myquery \= "SELECT GETDATE() AS GetDateCol,DATE(GETDATE()) AS DateCol"
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET qStatus \= tStatement.%Prepare(myquery)
   IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
  SET rset \= tStatement.%Execute()
  DO rset.%Display()

 

The following example takes a string value in %Library.TimeStamp format:

  SET myquery \= "SELECT '1995-09-10 13:14:23' AS DateStrCol,DATE('1995-09-10 13:14:23') AS DateCol"
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET qStatus \= tStatement.%Prepare(myquery)
   IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
  SET rset \= tStatement.%Execute()
  DO rset.%Display()

 

The following examples take string values that represent dates in Caché logical format. In order to properly convert these values to %Library.Date data type, the value must be prefixed with a plus sign (+) to force numeric evaluation:

  SET myquery \= "SELECT $H AS HoroCol,DATE(+$H) AS DateCol"
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET qStatus \= tStatement.%Prepare(myquery)
   IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
  SET rset \= tStatement.%Execute()
  DO rset.%Display()

 

  SET myquery \= "SELECT $ZTIMESTAMP AS TSCol,DATE(+$ZTIMESTAMP) AS DateCol"
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET qStatus \= tStatement.%Prepare(myquery)
   IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
  SET rset \= tStatement.%Execute()
  DO rset.%Display()

 

See Also

*   [CAST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_cast) function
    
*   [CURDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_curdate) and [CURRENT\_DATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_currentdate) functions
    
*   [CURRENT\_TIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_currenttimestamp) function
    
*   [GETUTCDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_getutcdate) function
    
*   [NOW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_now) function
    
*   [TO\_TIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_totimestamp) function
    
*   [$HOROLOG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog) special variable
    
*   [$ZTIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztimestamp) special variable
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datalength "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dateadd "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_date.xml**