# DAYNAME

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_dayname

---

 

Caché SQL Reference

DAYNAME

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_day "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dayofmonth "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [DAYNAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dayname "DAYNAME") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A date function that returns the name of the day of the week for a date expression.

Synopsis

{fn DAYNAME(date-expression)}

Arguments

date-expression

An expression that evaluates to either a Caché date integer, an ODBC date, or a timestamp. This expression can be the name of a column, the result of another scalar function, or a date or timestamp literal.

Description

DAYNAME returns the name of the day that corresponds to a specified date. The returned value is a character string with a maximum length of 15. The default day names returned are: Sunday, Monday, Tuesday, Wednesday, Thursday, Friday, Saturday.

To change these default day name values, use the [SET OPTION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_setoption) command with the WEEKDAY\_NAME option.

The day name is calculated for a Caché date integer, a [$HOROLOG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog) or [$ZTIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztimestamp) value, an ODBC format date string, or a timestamp string with the format:

yyyy-mm-dd hh:mm:ss

The time portion of the timestamp is not evaluated and can be omitted. The date-expression can also be specified as data type %Library.FilemanDate, %Library.FilemanTimestamp, or %MV.Date.

DAYNAME checks that the date supplied is a valid date. The year must be between 1841 and 9999 (inclusive), the month 01 through 12, and the day appropriate for that month (for example, 02/29 is only valid on leap years). If the date is not valid, DAYNAME issues an SQLCODE -400 error (Fatal error occurred).

The same day of week information can be returned by using the [DATENAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datename) function. You can use [TO\_DATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_todate) to retrieve a day name or day name abbreviation with other date elements. To return an integer corresponding to the day of the week, use [DAYOFWEEK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dayofweek) [DATEPART](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datepart) or [TO\_DATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_todate).

This function can also be invoked from Caché ObjectScript using the [DAYNAME()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#DAYNAME) method call:

$SYSTEM.SQL.DAYNAME(date-expression)

Examples

The following examples both return the character string Wednesday because the day of the date (February 25, 2004) is a Wednesday. The first example takes a timestamp string:

SELECT {fn DAYNAME('2004-02-25 12:35:46')} AS Weekday

 

The second example takes a Caché date integer:

SELECT {fn DAYNAME(59590)} AS Weekday

 

The following examples all return the name of the current day of the week:

SELECT {fn DAYNAME({fn NOW()})} AS Wd\_Now,
       {fn DAYNAME(CURRENT\_DATE)} AS Wd\_CurrDate,
       {fn DAYNAME(CURRENT\_TIMESTAMP)} AS Wd\_CurrTstamp,
       {fn DAYNAME($ZTIMESTAMP)} AS Wd\_ZTstamp,
       {fn DAYNAME($HOROLOG)} AS Wd\_Horolog

 

Note that $ZTIMESTAMP returns Coordinated Universal Time (UTC). The other time-expression values return the local time. This may affect the DAYNAME value.

The following embedded SQL example shows how DAYNAME responds to an invalid date (the year 2001 was not a leap year):

   SET testdate\="2001-02-29"
   &sql(SELECT {fn DAYNAME(:testdate)}
   INTO :a)
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"returns: ",a }
   QUIT

 

The following embedded SQL example shows how DAYNAME responds to an invalid date (the year 1835 is too early for Caché SQL):

   SET testdate\="1835-02-19"
   &sql(SELECT {fn DAYNAME(:testdate)}
   INTO :a)
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"returns: ",a }
   QUIT

 

See Also

*   SQL functions: [DATENAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datename), [DATEPART](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datepart), [DAYOFMONTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dayofmonth), [DAYOFWEEK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dayofweek), [DAYOFYEAR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dayofyear), [TO\_DATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_todate)
    
*   Caché ObjectScript function: [$ZDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdate)
    
*   Caché ObjectScript special variables: [$HOROLOG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog), [$ZTIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztimestamp)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_day "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dayofmonth "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_dayname.xml**