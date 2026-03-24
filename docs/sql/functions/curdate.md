# CURDATE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_curdate

---

 

Caché SQL Reference

CURDATE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_cot "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_currentdate "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [CURDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_curdate "CURDATE") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A scalar date/time function that returns the current local date.

Synopsis

{fn CURDATE()}
{fn CURDATE}

Description

CURDATE takes no arguments and returns the date as type DATE. Note that the argument parentheses are optional. CURDATE returns the current local date for this timezone; it adjusts for local time variants, such as Daylight Saving Time.

CURDATE in Logical mode returns the current local date in [$HOROLOG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog) format; for example, 62970. CURDATE in Display mode returns the current local date in the default format for the locale. For example, in an American locale 05/28/2013, in a European locale 28/05/2013, in a Russian locale 28.05.2013.

To specify a different date format, use the [TO\_DATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_todate) function. To change the default date format, use the [SET OPTION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_setoption) command with the DATE\_FORMAT, YEAR\_OPTION, or DATE\_SEPARATOR options.

To return just the current date, use CURDATE or [CURRENT\_DATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_currentdate). These functions return their values in DATE data type. The [CURRENT\_TIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_currenttimestamp), [GETDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_getdate) and [NOW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_now) functions can also be used to return the current date and time as a TIMESTAMP data type.

Note that all Caché SQL time and date functions except [GETUTCDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_getutcdate) are specific to the local time zone setting. To get a current timestamp that is universal (independent of time zone) you can use GETUTCDATE or the Caché ObjectScript [$ZTIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztimestamp) special variable.

These data types perform differently when using embedded SQL. The DATE data type stores values as integers in [$HOROLOG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog) format; when displayed in SQL they are converted to date display format; when returned from embedded SQL they are returned as integers. A TIMESTAMP data type stores and displays its value in the same format. You can use the [CONVERT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_convert) function to change the data type of dates and times.

Examples

The following examples both return the current date:

SELECT {fn CURDATE()} AS Today

 

SELECT {fn CURDATE} AS Today

 

The following Embedded SQL example returns the current date. Because this date is stored in $HOROLOG format, it is returned as an integer:

  &sql(SELECT {fn CURDATE()} INTO :a)
  WRITE !,"Current date is: ",a

 

The following example shows how CURDATE can be used in a SELECT statement to return all records that have a shipment date that is the same or later than today's date:

SELECT \* FROM Orders 
     WHERE ShipDate \>= {fn CURDATE()}

See Also

*   SQL functions: [CURRENT\_DATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_currentdate) [CURRENT\_TIME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_currenttime) [CURRENT\_TIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_currenttimestamp) [CURTIME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_curtime) [GETDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_getdate) [GETUTCDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_getutcdate) [NOW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_now)
    
*   Caché ObjectScript function: [$ZDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdate)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_cot "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_currentdate "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_curdate.xml**