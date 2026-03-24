# MONTHNAME

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_monthname

---

 

Caché SQL Reference

MONTHNAME

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_month "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_now "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [MONTHNAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_monthname "MONTHNAME") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A date function that returns the name of the month for a date expression.

Synopsis

{fn MONTHNAME(date-expression)}

Arguments

date-expression

An expression that evaluates to either a Caché date integer, an ODBC date, or a timestamp. This expression can be the name of a column, the result of another scalar function, or a date or timestamp literal.

Description

MONTHNAME takes as input a Caché date integer, a [$HOROLOG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog) or [$ZTIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztimestamp) value, an ODBC format date string, or a timestamp string with the format:

yyyy-mm-dd hh:mm:ss

The time portion of the timestamp is not evaluated and can be omitted. The date-expression can also be specified as data type %Library.FilemanDate, %Library.FilemanTimestamp, or %MV.Date.

MONTHNAME returns the name of the corresponding calendar month, January through December. The returned value is a character string with a maximum length of 15.

MONTHNAME checks that the date supplied is a valid date. The year must be between 1841 and 9999 (inclusive), the month 01 through 12, and the day appropriate for that month (for example, 02/29 is only valid on leap years). If the date is not valid, MONTHNAME issues an SQLCODE -400 error (Fatal error occurred).

The names of months default to the full-length American English month names. To change these month name values, use the [SET OPTION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_setoption) command with the MONTH\_NAME option.

The same month name information can be returned by using the [DATENAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datename) function. You can use [TO\_DATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_todate) to retrieve a month name or a month name abbreviation with other date elements. To return an integer corresponding to the month, use [MONTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_month) [DATEPART](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datepart) or [TO\_DATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_todate).

This function can also be invoked from Caché ObjectScript using the [MONTHNAME()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#MONTHNAME) method call:

$SYSTEM.SQL.MONTHNAME(date-expression)

Examples

The following examples both return the character string "February" because it is the month of the date expression (February 16, 2000):

SELECT {fn MONTHNAME('2000-02-16')} AS NameOfMonth

 

SELECT {fn MONTHNAME(59589)} AS NameOfMonth

 

The following examples all return the current month:

SELECT {fn MONTHNAME({fn NOW()})} AS MnameNow,
       {fn MONTHNAME(CURRENT\_DATE)} AS MNameCurrDate,
       {fn MONTHNAME(CURRENT\_TIMESTAMP)} AS MNameCurrTS,
       {fn MONTHNAME($HOROLOG)} AS MNameHorolog,
       {fn MONTHNAME($ZTIMESTAMP)} AS MNameZTS

 

The following Embedded SQL example shows how MONTHNAME responds to an invalid date (the year 2001 was not a leap year):

   SET testdate\="2001-02-29"
   &sql(SELECT {fn MONTHNAME(:testdate)} INTO :a)
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE,!
     WRITE %msg,! }
   ELSE {
     WRITE !,"returns: ",a }
   QUIT

 

The SQLCODE -400 error code is issued with the %msg indicating <ILLEGAL VALUE>. This validity test is performed before testing if the date is in the allowed range of years (after 1840).

The following embedded SQL example shows how MONTHNAME responds to a valid, but out-of-range date (the year 1835 is too early for Caché SQL):

   SET testdate\="1835-02-19"
   &sql(SELECT {fn MONTHNAME(:testdate)} INTO :a)
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE,!
     WRITE %msg,! }
   ELSE {
     WRITE !,"returns: ",a }
   QUIT

 

The SQLCODE -400 error code is issued with the %msg indicating <VALUE OUT OF RANGE>.

See Also

*   SQL functions: [DATEPART](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datepart) [DATENAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datename) [DAYOFMONTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dayofmonth) [MONTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_month) [TO\_DATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_todate)
    
*   Caché ObjectScript function: [$ZDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdate)
    
*   Caché ObjectScript special variables: [$HOROLOG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog) [$ZTIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztimestamp)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_month "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_now "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_monthname.xml**