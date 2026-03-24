# YEAR

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_year

---

 

Caché SQL Reference

YEAR

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_xmlforest "Go back to the previous section.") 

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [YEAR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_year "YEAR") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A date function that returns the year for a date expression.

Synopsis

YEAR(date-expression)
{fn YEAR(date-expression)}

Arguments

date-expression

An expression that evaluates to either a Caché date integer, an ODBC date, or a timestamp. This expression can be the name of a column, the result of another scalar function, or a date or timestamp literal.

Description

YEAR takes as input a Caché date integer, an ODBC format date string, or a timestamp string with the format:

yyyy-mm-dd hh:mm:ss

The date-expression can also be specified as data type %Library.FilemanDate, %Library.FilemanTimestamp, or %MV.Date.

The year (yyyy) portion should be a four-digit integer in the range 1840 through 9999. There is, however, no validation or range checking for user-supplied dates. YEAR returns the year portion of invalid dates (such as 2005–02–31) and out-of-range dates (such as 1830–02–20). Year values outside the range 1840 through 9999, negative numbers, and fractions are returned as specified. Two digit years are not expanded to four digits.

YEAR returns the corresponding year as a four-digit integer.

Note:

For compatibility with Caché internal representation of dates, it is strongly recommended that all year values be expressed as four-digit integers within the range of 1840 through 9999.

The TO\_DATE and TO\_CHAR SQL functions support “Julian dates,” which can be used to represent years before 1840. Caché ObjectScript provides method calls that support such Julian dates.

YEAR returns zero when the year portion is a string of one or more zeroes (for example '0' or '0000'), or a nonnumeric value. YEAR interprets the initial numeric string encountered as the year value, so omitting the year portion of the date string ('-mm-dd hh:mm:ss'), or omitting the date portion ('hh:mm:ss') results in the first number encountered ('-mm' or 'hh') being treated as the year value. Thus, some placeholder should be supplied for an unknown year value; for compatibility with Caché, 9999 is generally the preferred value.

The year format default is four-digit years. To change this year display default, use the [SET OPTION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_setoption) command with the YEAR\_OPTION option.

The elements of a datetime string can be returned using the following SQL scalar functions: YEAR, MONTH, DAY, DAYOFMONTH, HOUR, MINUTE, SECOND. The same elements can be returned by using the [DATEPART](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datepart) or [DATENAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datename) function. DATEPART and DATENAME perform value and range checking on year values.

This function can also be invoked from Caché ObjectScript using the [YEAR()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#YEAR) method call:

$SYSTEM.SQL.YEAR(date-expression)

Examples

The following examples return the integer 2004. No validation is performed:

SELECT YEAR('2004-02-16 12:45:37') AS Year\_Given

 

SELECT {fn YEAR(59590)} AS Year\_Given

 

The following examples return the year as 0 because the year field contains a nonnumeric placeholder. The separator character (–) must be preceded by a some character(s); otherwise the month is returned as the year value:

Asterisk as year placeholder:

SELECT {fn YEAR('\*-02-16')} AS Year\_Given

 

Space character as year placeholder:

SELECT YEAR(' -02-16') AS Year\_Given

 

The following example returns the current year:

SELECT YEAR(GETDATE()) AS Year\_Now

 

The following Embedded SQL example returns the current year from two functions. The CURRENT\_DATE function returns data type DATE; the NOW function returns data type TIMESTAMP. YEAR returns a four-digit year integer for both input data types:

  &sql(SELECT {fn YEAR(CURRENT\_DATE)},
              {fn YEAR({fn NOW()})} INTO :a,:b)
  IF SQLCODE'=0 {
    WRITE !,"Error code ",SQLCODE }
  ELSE {
    WRITE !,"CURRENT\_DATE year is: ",a
    WRITE !,"NOW year is: ",b }

 

The following Embedded SQL example shows that YEAR returns the year portion of an invalid date (the year 2001 was not a leap year) or an out-of-range date:

   SET testdate1\="2001-02-29"
   SET testdate2\="1835-02-19"
   &sql(SELECT YEAR(:testdate1),
               YEAR(:testdate2) INTO :a,:b)
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"invalid date returns: ",a
     WRITE !,"out-of-range date returns: ",b }
   QUIT

 

See Also

*   SQL functions: [DATENAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datename) [DATEPART](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datepart) [DAYOFYEAR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dayofyear) [QUARTER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_quarter) [WEEK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_week) [TO\_DATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_todate)
    
*   Caché ObjectScript function: [$ZDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdate)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_xmlforest "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:38**

Source: **RSQL\_year.xml**