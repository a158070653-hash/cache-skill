# GETDATE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_getdate

---

 

Caché SQL Reference

GETDATE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_floor "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_getutcdate "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [GETDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_getdate "GETDATE") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A timestamp function that returns the current local date and time.

Synopsis

GETDATE(\[precision\])

Arguments

precision

Optional — Specifies the time precision as the number of digits of fractional seconds. Valid values are the integers 0 through 9. The default is 0 (no fractional seconds); this default is configurable. A precision value is optional, the parentheses are mandatory.

Description

GETDATE can be issued with no arguments, or with the optional precision argument. It returns the current local date and time in %TimeStamp format. Its ODBC type is TIMESTAMP, LENGTH is 16, and PRECISION is 19. GETDATE returns the current local date and time for this timezone; it adjusts for local time variants, such as Daylight Saving Time.

The datetime string returned is of the format:

yyyy-mm-dd hh:mm:ss.ffff

Where “f” represents fractional seconds of precision. GETDATE values are displayed in this format in Display mode, ODBC mode, or Logical mode.

To change the default datetime string format, use the [SET OPTION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_setoption) command with the various date and time options.

GETDATE can be used in a SELECT statement select list or in the WHERE clause of a query. In designing a report, GETDATE can be used to print the current date and time each time the report is produced. GETDATE is also useful for tracking activity, such as logging the time that a transaction occurred.

GETDATE can be used in CREATE TABLE to specify a field’s default value. GETDATE is a synonym for [CURRENT\_TIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_currenttimestamp) and is provided for compatibility with Sybase and Microsoft SQL Server.

The [CURRENT\_TIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_currenttimestamp) and [NOW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_now) functions can also be used to return the current date and time as data type TIMESTAMP. CURRENT\_TIMESTAMP supports precision, NOW does not support precision.

To return just the current date, use [CURDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_curdate) or [CURRENT\_DATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_currentdate). To return just the current time, use [CURRENT\_TIME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_currenttime) or [CURTIME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_curtime). These functions use the DATE or TIME data type. None of these functions support precision.

A TIMESTAMP data type stores and displays its value in the same format. The TIME and DATE data types store their values as integers in [$HOROLOG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog) format. They can be displayed in either Display format or Logical (storage) format. You can use the [CAST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_cast) or [CONVERT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_convert) function to change the data type of dates and times.

Universal Time (UTC)

GETDATE returns the current local date and time as a TIMESTAMP. All Caché SQL timestamp, date, and time functions except [GETUTCDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_getutcdate) are specific to the local time zone setting. [GETUTCDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_getutcdate) returns the current UTC (universal) date and time as a TIMESTAMP. You can also use the Caché ObjectScript [$ZTIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztimestamp) special variable to get a current timestamp that is universal (independent of time zone).

Fractional Seconds Precision

GETDATE can return up to nine digits of precision. The number of digits of precision returned is set using the precision argument. The default for the precision argument can be configured using the following:

*   [SET OPTION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_setoption) with the TIME\_PRECISION option.
    
*   The [$SYSTEM.SQL.SetDefaultTimePrecision()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#SetDefaultTimePrecision) method call.
    
*   Go to the Management Portal, select Configuration, SQL and Object Settings, General SQL Settings (\[Home\] > \[Configuration\] > \[General SQL Settings\]). View and edit the current setting of Default time precision for GETDATE(), CURRENT\_TIME, and CURRENT\_TIMESTAMP.
    

Specify an integer 0 through 9 (inclusive) for the default number of decimal digits of precision to return. The default is 0. The actual precision returned is platform dependent; precision digits in excess of the precision available on your system are returned as zeroes.

Examples

The following example returns the current date and time:

SELECT GETDATE() AS DateTime

 

The following example returns the current date and time with two digits of precision:

SELECT GETDATE(2) AS DateTime

 

The following embedded SQL example compares local (time zone specific) and universal (time zone independent) time stamps:

  SET b\=$ZDATETIME($ZTIMESTAMP,3)
  &sql(SELECT GETDATE()
  INTO :a)
  IF SQLCODE'=0 {
    WRITE !,"Error code ",SQLCODE }
  ELSE {
    WRITE !,"GetDate is:    ",a
    WRITE !,"ZTimestamp is: ",b }

 

The following example:

UPDATE Orders SET LastUpdate \= GETDATE()
  WHERE Orders.OrderNumber\=:ord

sets the LastUpdate field in the selected row of the Orders table to the current system date and time.

In the following example, the CREATE TABLE statement uses GETDATE to set a default value for the StartDate field:

CREATE TABLE Employees(
     EmpId       INT NOT NULL,
     LastName    CHAR(40) NOT NULL,
     FirstName   CHAR(20) NOT NULL,
     StartDate   TIMESTAMP DEFAULT GETDATE())

See Also

*   SQL concepts: [Data Type](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datatype), [Date and Time Constructs](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dateconstruct)
    
*   SQL timestamp functions: [CAST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_cast), [CONVERT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_convert), [CURRENT\_TIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_currenttimestamp), [GETUTCDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_getutcdate), [NOW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_now), [TIMESTAMPADD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_timestampadd), [TIMESTAMPDIFF](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_timestampdiff), [TO\_TIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_totimestamp)
    
*   SQL current date and time functions: [CURDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_curdate), [CURRENT\_DATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_currentdate), [CURRENT\_TIME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_currenttime), [CURTIME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_curtime)
    
*   Caché ObjectScript: [$ZDATETIME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetime) function, [$HOROLOG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog) special variable, [$ZTIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztimestamp) special variable
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_floor "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_getutcdate "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_getdate.xml**