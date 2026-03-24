# CURRENT TIMESTAMP

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_currenttimestamp

---

 

Caché SQL Reference

CURRENT\_TIMESTAMP

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_currenttime "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_curtime "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [CURRENT\_TIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_currenttimestamp "CURRENT_TIMESTAMP") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A date/time function that returns the current local date and time.

Synopsis

CURRENT\_TIMESTAMP\[(precision)\]

Arguments

precision

Optional — Specifies the time precision as the number of digits of fractional seconds. Valid values are the integers 0 through 9. The default is 0 (no fractional seconds); this default is configurable.

Description

CURRENT\_TIMESTAMP takes either no arguments or a precision argument, and returns the current date and time as a TIMESTAMP data type. Empty argument parentheses are not permitted. CURRENT\_TIMESTAMP returns the current local date and time for this timezone; it adjusts for local time variants, such as Daylight Saving Time.

The datetime string is stored and returned in the following format:

yyyy-mm-dd hh:mm:ss.ffff

Where “f” represents fractional seconds of precision. You can use $HOROLOG to store or return the current local date and time in internal format.

To change the default datetime string format, use the [SET OPTION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_setoption) command with the various date and time options.

You can specify CURRENT\_TIMESTAMP, with or without precision, as the field default value when defining a datetime field using CREATE TABLE or ALTER TABLE.

Fractional Seconds Precision

CURRENT\_TIMESTAMP can return up to nine digits of precision. The number of digits of precision returned is set using the precision argument. The default for the precision argument can be configured using the following:

*   [SET OPTION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_setoption) with the TIME\_PRECISION option.
    
*   The [$SYSTEM.SQL.SetDefaultTimePrecision()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#SetDefaultTimePrecision) method call.
    
*   Go to the Management Portal, select \[Home\] > \[Configuration\] > \[General SQL Settings\]. View and edit the current setting of Default time precision for GETDATE(), CURRENT\_TIME, and CURRENT\_TIMESTAMP.
    

Specify an integer 0 through 9 (inclusive) for the default number of decimal digits of precision to return. The default is 0. The actual precision returned is platform dependent; precision digits in excess of the precision available on your system are returned as zeroes.

Date and Time Functions Compared

The [GETDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_getdate) and [NOW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_now) functions can also be used to return the current local date and time as a TIMESTAMP data type. GETDATE supports precision, NOW does not support precision.

The [SYSDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_sysdate) function is identical to CURRENT\_TIMESTAMP, with the exception that SYSDATE does not support precision. CURRENT\_TIMESTAMP is the preferred Caché SQL function; SYSDATE is provided for compatibility with other vendors.

To return just the current date, use [CURDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_curdate) or [CURRENT\_DATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_currentdate). To return just the current time, use [CURRENT\_TIME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_currenttime) or [CURTIME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_curtime). These functions return their values in DATE or TIME data type. None of these functions support precision.

The TIMESTAMP data type storage format and display format are the same. The TIME and DATE data types store their values as integers in [$HOROLOG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog) format; when displayed in SQL they are converted to date or time display format. Embedded SQL returns them in logical (storage) format by default. You can change the Embedded SQL returned value format using the [#SQLCompile Select](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_macros#GCOS_macros_mpp_lbSQLCompile_Select) macro preprocessor directive, as described in the “ObjectScript Macros and the Macro Preprocessor” chapter of Using Caché ObjectScript.

You can use the [CAST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_cast) or [CONVERT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_convert) function to change the data type of dates and times.

Note that all Caché SQL time and date functions except [GETUTCDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_getutcdate) are specific to the local time zone setting. To get a current timestamp that is universal (independent of time zone) you can use GETUTCDATE or the Caché ObjectScript [$ZTIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztimestamp) special variable.

Examples

The following example returns the current local date and time three different ways: in timestamp format with full seconds, in timestamp format with a precision of two digits of fractional seconds, and in $HOROLOG internal storage format with full seconds:

SELECT 
   CURRENT\_TIMESTAMP AS FullSecStamp,
   CURRENT\_TIMESTAMP(2) AS FracSecStamp,
   $HOROLOG AS InternalFullSec

 

The following Embedded SQL example sets a locale default precision. The first CURRENT\_TIMESTAMP specifies no precision; it returns the current time with the locale default precision number of zeroes appended. The second CURRENT\_TIMESTAMP specifies precision; this overrides the locale default precision. The precision argument can be larger or smaller than the locale default:

InitialVal
  SET pre\=##class(%SYS.NLS.Format).GetFormatItem("TimePrecision")
ChangeVal
  SET x\=##class(%SYS.NLS.Format).SetFormatItem("TimePrecision",4)
  &sql(SELECT CURRENT\_TIMESTAMP,CURRENT\_TIMESTAMP(2) INTO :a,:b)
  IF SQLCODE'=0 {
    WRITE !,"Error code ",SQLCODE }
  ELSE {
    WRITE !,"Timestamp is:  ",a
    WRITE !,"Timestamp is:  ",b }
RestoreVal
  SET x\=##class(%SYS.NLS.Format).SetFormatItem("$TimePrecision",pre)

 

The following Embedded SQL example compares local (time zone specific) and universal (time zone independent) time stamps:

  SET b\=$ZDATETIME($ZTIMESTAMP,3)
  &sql(SELECT CURRENT\_TIMESTAMP INTO :a)
  IF SQLCODE'=0 {
    WRITE !,"Error code ",SQLCODE }
  ELSE {
    WRITE !,"Timestamp is:  ",a
    WRITE !,"ZTimestamp is: ",b }

 

The following example sets the LastUpdate field in the selected row of the Orders table to the current system date and time.

UPDATE Orders SET LastUpdate \= CURRENT\_TIMESTAMP
  WHERE Orders.OrderNumber\=:ord

The following example creates a table named Orders, which records product orders received:

CREATE TABLE Orders (
     OrderId     INT NOT NULL,
     ClientId    INT,
     ItemName    CHAR(40) NOT NULL,
     OrderDate   TIMESTAMP DEFAULT CURRENT\_TIMESTAMP(3),
     PRIMARY KEY (OrderId))

The OrderDate column contains the date and time that the order was received. It uses the TIMESTAMP data type and inserts the current system date and time as the default value using the CURRENT\_TIMESTAMP function with a precision of 3.

See Also

*   SQL concepts: [Data Type](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datatype) [Date and Time Constructs](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dateconstruct)
    
*   SQL timestamp functions: [CAST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_cast) [CONVERT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_convert) [GETDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_getdate) [GETUTCDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_getutcdate) [NOW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_now) [TIMESTAMPADD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_timestampadd) [TIMESTAMPDIFF](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_timestampdiff) [TO\_TIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_totimestamp)
    
*   SQL current date and time functions: [CURDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_curdate) [CURRENT\_DATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_currentdate) [CURRENT\_TIME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_currenttime) [CURTIME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_curtime) [SYSDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_sysdate)
    
*   Caché ObjectScript: [$ZDATETIME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetime) function [$HOROLOG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog) special variable [$ZTIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztimestamp) special variable
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_currenttime "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_curtime "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_currenttimestamp.xml**