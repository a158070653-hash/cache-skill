# TO DATE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_todate

---

 

Caché SQL Reference

TO\_DATE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_tochar "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_tonumber "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [TO\_DATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_todate "TO_DATE") \]

Go to: Description Date String Format Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A date function that converts a formatted string to a date.

Synopsis

TO\_DATE(date\_string\[,format\])

TODATE(date\_string\[,format\])

Arguments

date\_string

The string to be converted to a date. A string date expression where the underlying data type is CHAR or VARCHAR2.

format

Optional — A date format string corresponding to date\_string. If format is omitted, 'DD MON YYYY' is the default value; this default is configurable.

Description

The names TO\_DATE and TODATE are interchangeable and are supported for Oracle compatibility.

The TO\_DATE function converts date strings in various formats to a date integer value, with data type DATE. It is used to input dates in various string formats, storing them in a standard internal representation. TO\_DATE returns a date with the following format:

nnnnn

Where nnnnn is a positive integer between 0 (December 31, 1840) and 2980013 (December 31, 9999), inclusive. This represents a count of days. Time values are ignored.

The default earliest date is December 31, 1840. You can change the DATE data type MINVAL parameter to permit negative integers representing dates prior to December 31, 1840, as described in the [Data Types](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datatype) reference page in this manual. Dates before December 31, 1840 can also be represented using Julian dates, as described below.

This function can also be invoked from Caché ObjectScript using the [TODATE()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#TODATE) method call:

$SYSTEM.SQL.TODATE(date\_string,format)

The TO\_DATE function can be used in data definition when supplying a default value to a field. For example:

CREATE TABLE mytest
(ID NUMBER(12,0) NOT NULL,
End\_Year DATE DEFAULT TO\_DATE('31-12-2007','DD-MM-YYYY') NOT NULL)

For further details on this use of TO\_DATE, refer to the [CREATE TABLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable) command.

Related SQL Functions

*   TO\_DATE converts a formatted date string to a date integer.
    
*   [TO\_CHAR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_tochar) performs the reverse operation; it converts a date integer to a formatted date string.
    
*   [TO\_TIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_totimestamp) converts a formatted date and time string to a standard timestamp.
    
*   [CAST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_cast) and [CONVERT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_convert) perform DATE data type conversion.
    

Note:

An earlier version of TO\_DATE supported date integer-to-string conversions that now must be done using the TO\_CHAR function. Older applications may have to be modified.

Date String

The first argument specifies a date string literal. You can supply a date string of any kind for the input date\_string. Each character must correspond to the format string, with the following exceptions:

*   Leading zeros may be included or omitted (with the exception of a date\_string without separator characters).
    
*   Years may be specified with two digits or four digits.
    
*   Month names may be specified in full or as the first three letters of the name. Only the first three letters must be correct. Month names are not case sensitive.
    
*   Time values appended to a date are ignored.
    

Format

The second argument specifies a date format as a string of code characters.

Default Date Format

If you specify no format, TO\_DATE parses the date string using the default format. The supplied default format is DD MON YYYY. For example, '11 Nov 1993'.

This default format is configurable, using the Caché ObjectScript [$SYSTEM.SQL.SetToDateDefaultFormat()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#SetToDateDefaultFormat) class method. To determine the current setting, call [$SYSTEM.SQL.CurrentSettings()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#CurrentSettings), which displays the TO\_DATE() Default Format setting.

Format Elements

A format is a string of one or more format elements specified according to the following rules:

*   Format elements are not case-sensitive.
    
*   Almost any sequence or number of format elements is permitted.
    
*   Format strings separate their elements with non-alphanumeric separator characters (for example, a space, slash, or hyphen) that match the separator characters in the date\_string. This use of specified date separator characters does not depend on the DateSeparator defined for your NLS locale.
    
*   The following date format strings do not require separator characters: MMDDYYYY, DDMMYYYY, YYYYMMDD, and YYYYDDMM. The incomplete date format YYYYMM is also supported, and assume a DD value of 01. Note that in these cases leading zeros must be provided for MM and DD values.
    

The following table lists the valid date format elements for the format argument:

Element

Meaning

DD

Two-digit day of month (01-31). Leading zeros are not required, unless format contains no date separator characters.

MM

Two-digit month number (01-12; 01 = January). Leading zeros are not required, unless format contains no date separator characters.

In Japanese and Chinese, a month number consists of a numeric value followed by the ideogram for “month”.

MON

Abbreviated name of month, as specified by the MonthAbbr property in the current locale. By default, in English this is the first three letters of the month name. In other locales, month abbreviations may be more than three letters long and/or may not consist of the first letters of the month name. A period character is not permitted. Not case-sensitive.

MONTH

Full name of the month, as specified by the MonthName property in the current locale. Not case-sensitive.

YYYY

Four-digit year.

YY

Last two digits of the year. The first 2 digits of a 2-digit year default to 19.

RR / RRRR

Two-digit year to four-digit year conversion. (See below.)

DDD

Day of the year. The count of days since January 1. (See below.)

J

Julian date. (See below.)

A TO\_DATE format can also include a D (day of week number), DY (day of week abbreviation), or DAY (day of week name) element. However, these format elements are not validated or used to determine the return value. For further details on these format elements, refer to [TO\_CHAR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_tochar).

Date Formats for Single Date Elements

You can specify DD, DDD, MM, or YYYY as a complete date format. Because these format strings omit the month, year, or both the month and year, Caché interprets them as referring to the current month and year:

*   DD returns the date for the specified day in the current month of the current year.
    
*   DDD returns the date for the specified day of the year in the current year.
    
*   MM returns the date for the first day of the specified month in the current year.
    
*   YYYY - returns the date for the first day of the current month of the specified year.
    

The following Embedded SQL examples show these formats:

   NEW SQLCODE
   &sql(SELECT
        TO\_DATE('300','DDD'),
        TO\_DATE('24','DD')
      INTO :a,:b)
   IF SQLCODE\=0 {
    WRITE "DDD format: ",a," = ",$ZDATE(a,1,,4),!
    WRITE "DD format: ",b," = ",$ZDATE(b,1,,4) }
   ELSE { WRITE "error:",SQLCODE }

 

   NEW SQLCODE
   &sql(SELECT
        TO\_DATE('8','MM'),
        TO\_DATE('2004','YYYY')
      INTO :a,:b)
   IF SQLCODE\=0 {
    WRITE "MM format: ",a," = ",$ZDATE(a,1,,4),!
    WRITE "YYYY format: ",b," = ",$ZDATE(b,1,,4),!
    WRITE "done" }
   ELSE { WRITE "error:",SQLCODE }

 

Two-Digit Year Conversion (RR and RRRR formats)

The YY format converts a two-digit year value to four digits by simply appending 19. Thus 07 becomes 1907 and 93 becomes 1993.

The RR format provides more flexible two-digit to four-digit year conversion. This conversion is based on the current year. If the current year is in the first half of a century (for example, 2000 through 2050), two-digit years from 00 through 49 are expanded to a four-digit year in the current century, and two-digit years from 50 through 99 are expanded to a four-digit year in the previous century. If the current year is in the second half of a century (for example, 2050 through 2099), all two-digit years are expanded to a four-digit year in the current century. This expansion of two-digit years to four-digit years is shown in the following Embedded SQL example:

   NEW SQLCODE
   &sql(SELECT 
       TO\_DATE('29 September 00','DD MONTH RR'),
       TO\_DATE('29 September 08','DD MONTH RR'),
       TO\_DATE('29 September 49','DD MONTH RR'),
       TO\_DATE('29 September 50','DD MONTH RR'),
       TO\_DATE('29 September 77','DD MONTH RR')
      INTO :a,:b,:c,:d,:e)
   IF SQLCODE\=0 {
    WRITE a," = ",$ZDATE(a,1,,4),!
    WRITE b," = ",$ZDATE(b,1,,4),!
    WRITE c," = ",$ZDATE(c,1,,4),!
    WRITE d," = ",$ZDATE(d,1,,4),!
    WRITE e," = ",$ZDATE(e,1,,4) }
   ELSE { WRITE "error:",SQLCODE }

 

The RRRR format permits you to input a mix of two–digit and four-digit years. Four-digit years are passed through unchanged (the same as YYYY). Two-digit years are converted to four-digit years, using the RR format algorithm. This is shown in the following Embedded SQL example:

   NEW SQLCODE
   &sql(SELECT 
       TO\_DATE('29 September 2008','DD MONTH RRRR'),
       TO\_DATE('29 September 08','DD MONTH RRRR'),
       TO\_DATE('29 September 1949','DD MONTH RRRR'),
       TO\_DATE('29 September 49','DD MONTH RRRR'),
       TO\_DATE('29 September 1950','DD MONTH RRRR'),
       TO\_DATE('29 September 50','DD MONTH RRRR')
       INTO :a,:b,:c,:d,:e,:f)
   IF SQLCODE\=0 {
    WRITE a," 4-digit = ",$ZDATE(a,1,,4),!
    WRITE b," 2-digit = ",$ZDATE(b,1,,4),!
    WRITE c," 4-digit = ",$ZDATE(c,1,,4),!
    WRITE d," 2-digit = ",$ZDATE(d,1,,4),!
    WRITE e," 4-digit = ",$ZDATE(e,1,,4),!
    WRITE f," 2-digit = ",$ZDATE(f,1,,4) }
   ELSE { WRITE "error:",SQLCODE }

 

Day of the Year (DDD format)

You can use DDD to convert the day of the year (number of days elapsed since January 1) to an actual date. The format string DDD YYYY must be paired with a corresponding date\_string consisting of an integer number of days and a four-digit year. (Two-digit years must be specified as RR (not YY) when used with DDD.) The format string DDD defaults to the current year. The number of elapsed days must be a positive integer in the range 1 through 365 (366 if YYYY is a leap year). The four-digit year must be within the standard Caché date range: 1841 through 9999. The DDD and YYYY format elements can be specified in any order; a separator character between them is mandatory. The following example shows this use of Day of the Year:

   NEW SQLCODE
   &sql(SELECT TO\_DATE('2008:60','YYYY:DDD')
        INTO :a)
   IF SQLCODE\=0 {WRITE a," = ",$ZDATE(a,1,,4) }
   ELSE { WRITE "error:",SQLCODE }

 

If a format string contains both a DD and a DDD element, the DDD element is dominant. This is shown in the following example, which returns 2/29/2008 (not 12/31/2008):

   NEW SQLCODE
   &sql(SELECT TO\_DATE('2008-12-31-60','YYYY-MM-DD-DDD')
        INTO :a)
   IF SQLCODE\=0 {WRITE a," = ",$ZDATE(a,1,,4) }
   ELSE { WRITE "error:",SQLCODE }

 

TO\_DATE permits you to return a date expression corresponding to a day of the year. TO\_CHAR permits you to return the day of the year corresponding to a date expression.

Julian Dates (J format)

In Caché SQL, a Julian date can be used for any date before December 31, 1840. Because Caché represents this date internally as 0, special syntax is needed to represent earlier dates. TO\_DATE provides a format of 'J' (or 'j') for this purpose. Julian date conversion converts a seven-digit internal numeric value (a Julian day count) to a display-format or ODBC-format date. For example:

   NEW SQLCODE
   &sql(SELECT TO\_DATE(2300000,'J')
        INTO :a)
   IF SQLCODE\=0 {WRITE a }
   ELSE { WRITE "error:",SQLCODE }

 

returns the following date: 1585–01–31 (ODBC format) or 01/31/1585 (display format). Julian day count 1721424 returns January 1st of the Year 1 (1–01–01). Julian day counts such as 1709980 (battle of Actium marks beginning of Roman Empire under Augustus Caesar) return BCE (BC) dates, which are displayed with the year preceded by a minus sign.

Note:

By default, the %Date data type does not represent dates prior to [December 31, 1840](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog#RCOS_vhorolog_historical_note). However, you can redefine the MINVAL parameter for this data type to permit representation of earlier dates as negative integers, with the limit of January 1, Year 1. This representation of dates as negative integers is not compatible with the “Julian” date format described here. For further details refer to the [Data Types](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datatype) reference page in this manual.

A Julian day count is always represented internally as a seven-digit number, with leading zeros when necessary. TO\_DATE allows you to input a Julian day count without the leading zeros. The highest permitted Julian date is 5373484, it returns 12/31/9999. The lowest permitted Julian date is 0000001, it returns 01/01/-4712 (which is BCE date 01/01/-4713). Any value outside this range generates an SQLCODE -400 error, with a %msg value of “Invalid Julian Date value. Julian date must be between 1 and 5373484”.

Note:

The following consideration should not affect the interconversion of dates and Julian day counts using TO\_CHAR and TO\_DATE. It may affect some calculations made using Julian day counts.

Julian day counts prior to 1721424 (1/1/1) are compatible with other software implementations, such as Oracle. They are not identical to BCE dates in ordinary usage. In ordinary usage, there is no Year 0; dates go from 12/31/-1 to 1/1/1. In Oracle usage, the Julian dates 1721058 through 1721423 are simply invalid, and return an error. In Caché these Julian dates return the non-existent Year 0 as a place holder. Thus calculations involving BCE dates must be adjusted by one year to correspond to common usage.

Also be aware that these date counts do not take into account changes in date caused by the Gregorian calendar reform (enacted October 15, 1582, but not adopted in Britain and its colonies until 1752).

TO\_DATE permits you to return a date expression corresponding to a Julian day count. TO\_CHAR permits you to return a Julian day count corresponding to a date expression, as shown in the following example:

SELECT 
  TO\_CHAR('1776-07-04','J') AS JulianCount,
  TO\_DATE(2369916,'J') AS JulianDate

 

Examples

Default Date Format Examples

The following embedded SQL example specifies date strings that are parsed using the default date format. Both of these are converted to the DATE data type internal value of 60537:

  NEW SQLCODE
  &sql(SELECT 
       TO\_DATE('29 September 2006'),
       TO\_DATE('29 SEP 2006')
    INTO :a,:b)
  IF SQLCODE\=0 {WRITE a,!,b }
   ELSE { WRITE "error:",SQLCODE }

 

The following embedded SQL example specifies date strings with two-digit years with format default. Note that two-digit years default to 1900 through 1999. Thus, the internal DATE value is 24012:

  NEW SQLCODE
  &sql(SELECT 
       TO\_DATE('29 September 06'),
       TO\_DATE('29 SEP 06')
    INTO :a,:b)
  IF SQLCODE\=0 {WRITE a,!,b }
   ELSE { WRITE "error:",SQLCODE }

 

Specified Date Format Examples

The following embedded SQL example specifies date strings in various formats. All of these are converted to the DATE data type internal value of 60810.

  NEW SQLCODE
  &sql(SELECT 
       TO\_DATE('2007 Jun 29','YYYY MON DD'),
       TO\_DATE('JUNE 29, 2007','month dd, YYYY'),
       TO\_DATE('2007\*\*\*06\*\*\*29','YYYY\*\*\*MM\*\*\*DD'),
       TO\_DATE('06/29/2007','MM/DD/YYYY')
    INTO :a,:b,:c,:d)
  IF SQLCODE\=0 {WRITE !,a,!,b,!,c,!,d }
  ELSE { WRITE "error:",SQLCODE } 

 

The following embedded SQL example specifies date formats that do not require element separators. They return the date internal value of 60810:

  NEW SQLCODE
  &sql(SELECT 
       TO\_DATE('06292007','MMDDYYYY'),
       TO\_DATE('29062007','DDMMYYYY'),
       TO\_DATE('20072906','YYYYDDMM'),
       TO\_DATE('20070629','YYYYMMDD')
    INTO :a,:b,:c,:d)
  IF SQLCODE\=0 {WRITE !,a,!,b,!,c,!,d }
  ELSE { WRITE "error:",SQLCODE }

 

The following example specifies the YYYYMM date format. It does not require element separators. It supplies 01 for the missing day element, returning the date 60782 (June 1, 2007):

   NEW SQLCODE
   &sql(SELECT TO\_DATE('200706','YYYYMM')
        INTO :a )
   IF SQLCODE\=0 {WRITE a," = ",$ZDATE(a,1,,4) }
   ELSE { WRITE "error:",SQLCODE }

 

See Also

*   SQL functions: [CAST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_cast), [CONVERT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_convert), [TO\_CHAR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_tochar), [TO\_TIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_totimestamp)
    
*   Caché ObjectScript functions: [$ZDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdate), [$ZDATEH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdateh)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_tochar "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_tonumber "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:38**

Source: **RSQL\_todate.xml**