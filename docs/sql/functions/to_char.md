# TO CHAR

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_tochar

---

 

Caché SQL Reference

TO\_CHAR

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_timestampdiff "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_todate "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [TO\_CHAR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_tochar "TO_CHAR") \]

Go to: Description Date-to-String Conversion Time-to-String Conversion Timestamp to Formatted Datetime String Conversion Number-to-String Conversion See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A string function that converts a date, timestamp, or number to a formatted character string.

Synopsis

TO\_CHAR(tochar-expression\[,format\])

TOCHAR(tochar-expression\[,format\])

Arguments

tochar-expression

A logical date, timestamp, or number expression to be converted.

format

Optional — A character code that specifies a date, timestamp, or number format for the tochar-expression conversion. If omitted, TO\_CHAR returns tochar-expression as a canonical number.

Description

The names TO\_CHAR and TOCHAR are interchangeable and are supported for Oracle compatibility.

The TO\_CHAR function with format has five uses:

*   [To convert a date integer to a formatted date string.](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_tochar#RSQL_tochar_date)
    
*   [To convert a date before 1840 to a Julian date integer.](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_tochar#RSQL_tochar_julian)
    
*   [To convert a time integer to a formatted time string.](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_tochar#RSQL_tochar_time)
    
*   [To convert a date and time to a formatted datetime string.](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_tochar#RSQL_tochar_timestamp)
    
*   [To convert a number to a formatted numeric string.](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_tochar#RSQL_tochar_number)
    

This function can also be invoked from Caché ObjectScript using the [TOCHAR()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#TOCHAR) method call:

$SYSTEM.SQL.TOCHAR(tochar-expression,format)

Valid and Invalid Arguments

*   For tochar-expression to be interpreted as a timestamp, it must be of the format YYYY-MM-DD HH:MI:SS, or one of the following valid variants: Month and date values that are less than 10 may include or omit a leading zero; if the leading zero is omitted, it is also omitted in the returned date. The seconds value may be omitted, though the colon indicating its place must be specified (HH:MI:); in the returned time the seconds default to 00. The seconds value may include fractional seconds (HH:MM:SS.nnn); in the returned time these fractional seconds are truncated. A timestamp must include a time portion, even if format does not specify time formatting.
    
*   If tochar-expression is not a valid timestamp format, TO\_CHAR interprets it as an integer, ending interpretation when it encounters the first non-integer character. If format is a date or timestamp format, TO\_CHAR interprets tochar-expression as a $HOROLOG date integer. Thus 2010-03-23 12-15:23 (note erroneous hyphen in time value) is interpreted as the $HOROLOG date 2010 (1846-07-03 12:00:00 AM).
    
*   If a tochar-expression date or time is not a valid date or time value, Caché issues an SQLCODE -400 error. This can occur with a nonexistent date, such as February 30, or a date before 12/31/1840.
    
*   If you specify a format with an invalid date, time, or timestamp code element (for example, YYYYY, MIN, HH48), TO\_CHAR returns the format code literal for the invalid code element; it returns date, time, or timestamp conversion values for valid code elements, if any.
    
*   If TO\_CHAR cannot recognize any format code elements (for example, format is an empty string) or if a number format has fewer digits than the tochar-expression value, TO\_CHAR returns pound sign (#) characters. (This is true when tochar-expression begins with at least two integer digits; otherwise TO\_CHAR returns NULL.)
    
*   If you omit format, TO\_CHAR returns the numeric portion of tochar-expression as a canonical number, truncating when it encounters a nonnumeric character. If tochar-expression is nonnumeric, TO\_CHAR returns 0. If tochar-expression is null, TO\_CHAR returns null.
    

TO\_CHAR and TO\_DATE

*   TO\_CHAR converts a date integer to a formatted date string, or a time integer to a formatted time string. If you erroneously supply TO\_CHAR with a formatted date or time string, it returns erroneous data.
    
*   TO\_DATE converts a formatted date string to the corresponding date integer. If you erroneously supply TO\_DATE with a date integer, it returns this integer unmodified.
    
*   Note that for Julian dates these operations are reversed.
    

These correct and erroneous uses of TO\_DATE and TO\_CHAR are shown in the following examples.

The following Embedded SQL example uses TO\_DATE to perform a date conversion. TO\_DATE takes a date string and returns the corresponding date integer (59832). The $ZDATE function is used to display this date integer as the formatted date 10/24/2004. In this example, TO\_DATE is also erroneously supplied a date integer; it simply returns this integer.

   &sql(SELECT 
          TO\_DATE('2004-10-24','YYYY-MM-DD'), /\* correct \*/
          TO\_DATE(59832,'YYYY-MM-DD')         /\* ERROR!  \*/
        INTO :a,:b)
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,a
     WRITE !,$ZDATE(a)
     WRITE !,b
 }

 

The following Embedded SQL example shows date conversions using TO\_CHAR. The first TO\_CHAR converts a date integer to the corresponding formatted date string, as expected. However, the second TO\_CHAR gives unexpected results. Since TO\_CHAR expects a numeric input, it treats the date separators in the input as minus signs and performs the subtractions. It therefore formats a date corresponding to the date integer 1970 (2004 minus 10 minus 24): 1846–5–24. Obviously, this was not the programmer’s intent.

   &sql(SELECT 
        TO\_CHAR(59832,'YYYY-MM-DD'),     /\* correct \*/
        TO\_CHAR(2004\-10\-24,'YYYY-MM-DD') /\* ERROR!  \*/
        INTO :a,:b)
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,a
     WRITE !,b }

 

Related SQL Functions

*   TO\_CHAR converts a date integer, a timestamp, or a number to a string.
    
*   [TO\_DATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_todate) performs the reverse operation for dates; it converts a formatted date string to a date integer.
    
*   [TO\_TIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_totimestamp) performs the reverse operation for timestamps; it converts a formatted date and time string to a standard timestamp.
    
*   [TO\_NUMBER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_tonumber) performs the reverse operation for numbers; it converts a numeric string to a number.
    
*   [CAST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_cast) and [CONVERT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_convert) perform DATE, TIMESTAMP, and NUMBER data type conversions.
    

Date-to-String Conversion

[$HOROLOG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog) format is the Caché SQL Logical format for representing dates and times. It is a string containing two comma-separated integers: the first is the number of days since December 31, 1840; the second is the number of seconds since midnight of the current day.

You can use TO\_CHAR to convert a $HOROLOG date integer or a $HOROLOG string of two comma-separated integers to a formatted date string, or a formatted date and time string. The value for tochar-expression must be a valid $HOROLOG value.

The following table lists the valid date format codes for this version of TO\_CHAR.

Format Code

Meaning

D

Day of week (1-7). By default, 1 is Sunday (the first day of the week), but this designation is configurable; refer to the [DAYOFWEEK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dayofweek) function.

DD

Two-digit day of month (01-31).

DY

Abbreviated name of the day, as specified by the WeekdayAbbr property of the current locale. The defaults are: Sun Mon Tue Wed Thu Fri Sat

DAY

Name of day, as specified by the WeekdayName property in the current locale. The defaults are: Sunday Monday Tuesday Wednesday Thursday Friday Saturday

MM

Two-digit month number (01-12; 01 = JAN).

MON

Abbreviated name of month, as specified by the MonthAbbr property in the current locale. The defaults are: Jan Feb Mar Apr May Jun Jul Aug Sep Oct Nov Dec. Not case-sensitive.

MONTH

Full name of the month, as specified by the MonthName property in the current locale. The defaults are: January February March April May June July August September October November December. Not case-sensitive.

YYYY

Four-digit year.

YYY

Last 3 digits of the year.

YY

Last 2 digits of the year.

Y

Last digit of the year.

RRRR

Four-digit year.

RR

Last 2 digits of the year.

DDD

Day of the Year (see below).

J

Julian date (number of days since January 1, 4712 BC (BCE)).

Separator characters are required between the date format elements, with the exception of the following format strings: YYYYMMDD, DDMMYYYY, and YYYYMM. The last of these returns the year and month values and ignores the day of the month.

Note that locales mentioned in the format code definitions refer to the same locales described in the Caché ObjectScript $ZDATE and $ZDATEH documentation.

Date Conversion Examples

The following are all valid uses of TO\_CHAR with a $HOROLOG date integer or a full $HOROLOG string value to return a formatted date string or a date and time string:

SELECT TO\_CHAR(63012,'YYYY-MM-DD') AS DateFD,
       TO\_CHAR(63012,'YYYY-MM-DD HH24:MI:SS') AS DateFDT,
       TO\_CHAR('63012,50278','YYYY-MM-DD') AS DateTimeFD,
       TO\_CHAR('63012,50278','YYYY-MM-DD HH24:MI:SS') AS DateTimeFDT

 

In the following example each TO\_CHAR takes a date integer and returns a date string formatted according to the format string argument:

SELECT TO\_CHAR(63012,'MM/DD/YYYY'),         /\* returns 07/09/2013            \*/
       TO\_CHAR(63012,'DAY MONTH DD, YYYY')  /\* returns Tuesday July 09, 2013 \*/

 

The following example takes a date integer and returns a formatted date string. Characters that are not format characters are passed through to the output string as literals:

SELECT TO\_CHAR(63012,'The date MM/DD/YYYY should be noted')

 

returns the string: The date 07/09/2013 should be noted

Day of the Year

You can use DDD to convert a date expression to the day of the year (number of days elapsed since January 1) and the year. The format string DDD,YYYY must be paired with a date expression in $HOROLOG format. (The $HOROLOG time value, if specified, is ignored.) The DDD and YYYY (or YY) format elements can be specified in any order; a separator character between them is mandatory and is returned as a literal. The following examples show this use of Day of the Year:

SELECT TO\_CHAR('60871','YYYY:DDD')

 

SELECT TO\_CHAR('60871,12345','DDD YY')

 

TO\_CHAR permits you to return the day of the year corresponding to a date expression. TO\_DATE permits you to return a date expression corresponding to a day of the year.

Julian Date Conversion

The “Julian” date format is provided to allow for dates before the year 1841. TO\_CHAR converts a date value for data type %Date or %TimeStamp to a seven-digit Julian date integer.

Note:

By default, the %Date data type does not represent dates prior to [December 31, 1840](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog#RCOS_vhorolog_historical_note). However, you can redefine the MINVAL parameter for this data type to permit representation of earlier dates as negative integers, with the limit of January 1, Year 1. This representation of dates as negative integers is not compatible with the “Julian” date format described here. For further details refer to the [Data Types](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datatype) reference page in this manual.

If you specify a format that consists of a string containing the letter 'J', the date value returned will be a “Julian” date—that is, a count of days from January 1, 4712BCE. Only the letter 'J' may be specified in the format string; the inclusion of any other characters causes 'J' to be treated as a literal, and the date to be translated as a standard date.

The maximum tochar-expression value for Julian dates is '9999-12-31' which corresponds to Julian day count 5373484. The minimum value is '-4712-01-01' which corresponds to Julian day count 0000001. A Julian day count is always represented as a seven-digit integer, with leading zeros when necessary.

The following example returns 2369916 (signing of the Declaration of Independence of the United States) and 1709980 (battle of Actium marks beginning of Roman Empire under Augustus Caesar):

SELECT TO\_CHAR('1776-07-04','J') AS UnitedStatesStart,
       TO\_CHAR('-0031-09-02','J') AS RomanEmpireStart

 

Note:

The following consideration should not affect the interconversion of dates and Julian day counts using TO\_CHAR and TO\_DATE. It may affect some calculations made using Julian day counts.

Julian day counts prior to 1721424 (1/1/1) are compatible with other software implementations, such as Oracle. They are not identical to BCE dates in ordinary usage. In ordinary usage, there is no Year 0; dates go from 12/31/-1 to 1/1/1. In Oracle usage, the Julian dates 1721058 through 1721423 are simply invalid, and return an error. In Caché these Julian dates return the non-existent Year 0 as a place holder. Thus calculations involving BCE dates must be adjusted by one year to correspond to common usage.

Also be aware that these date counts do not take into account changes in date caused by the Gregorian calendar reform (enacted October 15, 1582, but not adopted in Britain and its colonies until 1752).

TO\_CHAR permits you to return a Julian day count corresponding to a date expression. TO\_DATE permits you to return a date expression corresponding to a Julian day count, as shown in the following example:

SELECT TO\_CHAR('1776-07-04','J') AS JulianCount,
       TO\_DATE(2369916,'J') AS JulianDate

 

For further details on using Julian dates, see the TO\_DATE function.

Time-to-String Conversion

You can use TO\_CHAR to convert the following tochar-expression time values to a formatted time string:

*   A $HOROLOG time integer (the time component of $HOROLOG). The value for tochar-expression must be a valid Logical time (an integer in the range 0 through 86399). Do not supply a full $HOROLOG value with both date and time components (such as 62556,42152); TO\_CHAR time conversion would incorrectly convert the first (date) component of $HOROLOG to a formatted time string, and ignore the second (time) component.
    
*   A Logical timestamp value. The value for tochar-expression must be of the %TimeStamp data type (not a string data type) in the format YYYY-MM-DD hh:mm:ss. The date component of the timestamp is ignored and the time component converted. For example, [SYSDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_sysdate) is a Logical timestamp.
    
*   A time value in standard ODBC time format. The value for tochar-expression must be in the format hh:mm:ss and can be a string.
    
*   A time value in local time format (using the current NLS locale settings). For example, if the NLS TimeSeparator is set to “^”, the value for tochar-expression can be in the format hh^mm^ss and can be a string.
    

In all of these cases, the value for format must be a string that contains only time format codes:

Format Code

Meaning

HH

Hour of Day (1 through 12)

HH12

Hour of Day (1 through 12)

HH24

Hour of Day (0 through 23)

MI

Minute (0 through 59)

SS

Second (0 through 59)

SSSSS

Seconds since midnight (0 through 86388)

AM / PM

Meridian Indicator (AM = before noon, PM = after noon). Converts a time value to 12–hour format with the appropriate AM or PM suffix. The returned AM or PM suffix is derived from the time value, not from which format code you specified. In the format you can use either AM or PM; they are functionally identical.

Inclusion of any other format code values causes the tochar-expression integer to be interpreted as a date.

The following example causes '60871' to be interpreted as the time value 04:54:31 PM:

SELECT TO\_CHAR('60871','HH12:MI:SS PM')

 

The following example converts the time portions of two Logical timestamps to formatted time strings. Note that format does not support fractional seconds; fractional seconds in tochar-expression are truncated.

SELECT TO\_CHAR(SYSDATE,'HH12:MI:SS PM'),
       TO\_CHAR(CURRENT\_TIMESTAMP(6),'HH12:MI:SS PM')

 

The following Embedded SQL example converts time values specified in both ODBC standard format and current NLS locale format:

  SET restore\=##class(%SYS.NLS.Format).GetFormatItem("TimeSeparator")
  WRITE "Time Separator is = ",restore,!
  DO ##class(%SYS.NLS.Format).SetFormatItem("TimeSeparator","^")
  WRITE "Time Separator is now = ",##class(%SYS.NLS.Format).GetFormatItem("TimeSeparator"),!
  &sql(SELECT TO\_CHAR('15:35:43.99','HH12:MI:SS PM'),
              TO\_CHAR('15^35^43.99','HH12:MI:SS PM') 
              INTO :standard,:local)
  WRITE "Converted standard-format time: ",standard,!
  WRITE "Converted locale-format time: ",local,!
  DO ##class(%SYS.NLS.Format).SetFormatItem("TimeSeparator",restore)
  WRITE "Time Separator is = ",##class(%SYS.NLS.Format).GetFormatItem("TimeSeparator")

 

Timestamp to Formatted Datetime String Conversion

You can use TO\_CHAR to convert a timestamp to a formatted datetime string. The value for tochar-expression must be a valid Logical timestamp value.

The date portion of the timestamp is formatted using the date-to-string conversion format codes. The following table lists additional format codes for the time portion of the timestamp.

Format Code

Meaning

HH

Hour of Day (1 through 12)

HH12

Hour of Day (1 through 12)

HH24

Hour of Day (0 through 23)

MI

Minute (0 through 59)

SS

Second (0 through 59)

SSSSS

Seconds since midnight (0 through 86388)

AM

Meridian Indicator (before noon)

PM

Meridian Indicator (after noon)

The following example returns the current system date (a timestamp), and the current system date converted for display with two different formats:

SELECT SYSDATE,
       TO\_CHAR(SYSDATE,'MM/DD/YYYY HH:MI:SS'),
       TO\_CHAR(SYSDATE,'DD MONTH YYYY at SSSSS seconds')

 

Note that any characters used in the format string which are not format codes are just returned in place in the resulting string.

Number-to-String Conversion

You can use TO\_CHAR to convert a number to a formatted numeric string. The following table lists the valid format codes for the format argument for this use of TO\_CHAR.

If you omit the format argument, the input numeric value is evaluated as an integer: leading zeros and a leading plus sign are deleted, a leading minus sign is retained, and the numeric value is truncated at the first nonnumeric character, such as a comma or period. No leading blanks or other formatting is provided.

Format Code

Example

Description

9

9999

Return value with the specified number of digits, with a leading space if positive or with a minus sign if negative. Leading zeros are blank, except for a zero value, which returns a zero for the integer part of the fixed-point number.

0

09999 

99990

Return leading zeros.

Return trailing zeros.

$

$9999

Return value with a leading dollar sign. Note that the dollar sign is preceded by a blank for positive numbers.

B

B9999

Return blanks for the integer part of a fixed-point number when the integer part is zero (regardless of 0’ in the format argument).

S

S9999 

9999S

Return negative value with a leading minus sign "-". Return positive value with a leading plus sign "+".

Return negative value with a trailing minus sign "-". Return positive value with a trailing plus sign "+".

D

99D99

Return a decimal separator character in the specified position. The DecimalSeparator used is the one defined for the locale. The default is a period ".". Only one "D" is allowed in the format argument.

G

9G999

Return a numeric group separator character in the specified position(s). The NumericGroupSeparator used is the one defined for the locale. The default is a comma ",". No numeric group separators may appear to the right of the decimal separator.

FM

FM90.9

Return a value with no leading or trailing blanks.

,

9,999

Return a comma in the specified position. No comma may appear to the right of the decimal. The format argument may not begin with a comma.

.

99.99

Return a decimal point (that is, a period “.”) in the specified position. Only one "." is allowed in the format argument.

Your format can specify the decimal separator and the numeric group separator either as a literal character, or as the current value of the locale’s DecimalSeparator and NumericGroupSeparator. You can determine the current locale values as follows:

  WRITE ##class(%SYS.NLS.Format).GetFormatItem("DecimalSeparator"),!
  WRITE ##class(%SYS.NLS.Format).GetFormatItem("NumericGroupSeparator")  

 

If the format argument contains fewer integer digits than the input numeric expression, TO\_CHAR does not return a number; instead, it returns a string of two or more pound signs (##). The number of pound signs represents the length of the current format argument, plus one.

If the format argument contains fewer decimal digits than the input numeric expression, TO\_CHAR rounds the number to the specified number of decimal digits, or to an integer, if no decimal format is provided.

If tochar-expression is null, TO\_CHAR returns null.

Number-to-String Examples

The following embedded SQL example shows basic number-to-string conversions:

   &sql(SELECT 
     TO\_CHAR(1000,'9999'),
     TO\_CHAR(10,'9999')
   INTO :numfull,:numshort)
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"Formatted number:",numfull
     WRITE !,"Formatted number:",numshort
     WRITE !,"Note leading blanks" }

 

Returns the specified number with the appropriate number of leading blanks. An unsigned positive number is always preceded by a blank character. Additional leading blanks are provided if the specified number has fewer digits than the format argument.

The following embedded SQL example shows the use of separator characters:

   &sql(SELECT 
      TO\_CHAR(1000,'9,999.99'),
      TO\_CHAR(1000,'9G999D99')
   INTO :comma,:groupsep)
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"Formatted number:",comma
     WRITE !,"Formatted number:",groupsep
     WRITE !,"Note leading blank" }

 

The first TO\_CHAR returns the string: ' 1,000.00'. The second TO\_CHAR may also return this value, but the separator characters displayed depend upon the locale setting.

The following embedded SQL example shows the use of positive and negative signs:

   &sql(SELECT 
      TO\_CHAR(10,'99.99'),
      TO\_CHAR(\-10,'99.99'),
      TO\_CHAR(10,'S99.99'),
      TO\_CHAR(\-10,'S99.99'),
      TO\_CHAR(10,'99.99S'),
      TO\_CHAR(\-10,'99.99S')
   INTO :pos,:neg,:poslead,:neglead,:postrail,:negtrail)
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"Formatted number:",pos
     WRITE !,"Formatted number:",neg
     WRITE !,"Formatted number:",poslead
     WRITE !,"Formatted number:",neglead
     WRITE !,"Formatted number:",postrail
     WRITE !,"Formatted number:",negtrail
     WRITE !,"Note use of leading blank" }

 

Note that a leading blank only appears before a positive number with no sign formatting. No leading blank appears before a negative number, or before any signed number, regardless of the placement of the sign.

The following embedded SQL example show the use of the “FM” format to override the default leading blank for unsigned positive numbers:

   &sql(SELECT 
      TO\_CHAR(12345678.90,'99,999,999.99'),
      TO\_CHAR(12345678.90,'FM99,999,999.99')
   INTO :num,:fmnum)
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"Formatted number:",num
     WRITE !,"Formatted number:",fmnum
     WRITE !,"Note leading blank" }

 

The first TO\_CHAR returns the string: ' 12,345,678.90'. The second TO\_CHAR returns the string: '12,345,678.90' (with no leading blank).

The following embedded SQL example show the use of the leading dollar sign:

   &sql(SELECT 
     TO\_CHAR(1234567890,'$9G999G999G999'),
     TO\_CHAR(1234567890,'S$9G999G999G999'),
     TO\_CHAR(12345678.90,'$99G999G999D99')
   INTO :d,:sd,:dD)
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"Formatted number:",d
     WRITE !,"Formatted number:",sd
     WRITE !,"Formatted number:",dD
     WRITE !,"Note leading blanks" }

 

The dollar sign is always preceded either by a sign or by a blank character.

The following embedded SQL example shows what happens when the format argument contain fewer integer digits than the input numeric value:

   &sql(SELECT 
     TO\_CHAR(1234567.89,'9'),
     TO\_CHAR(1234567.89,'99'),
     TO\_CHAR(1234567.89,'99D99')
   INTO :a,:b,:c)
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"Formatted number:",a
     WRITE !,"Formatted number:",b
     WRITE !,"Formatted number:",c }

 

Each TO\_CHAR returns a string of pound signs: “##”, “###”, and “######”, respectively.

The following embedded SQL example shows what happens when the format argument contains fewer decimal (fractional) digits than the input numeric expression:

   &sql(SELECT 
     TO\_CHAR(1234567.4999,'9999999.9'),
     TO\_CHAR(1234567.91,'9999999')
   INTO :a,:b)
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"Formatted number:",a
     WRITE !,"Formatted number:",b }

 

The returned numbers are rounded to “1234567.5” and “1234568”, respectively.

See Also

*   SQL functions: [CONVERT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_convert) [TO\_DATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_todate) [TO\_NUMBER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_tonumber)
    
*   Caché ObjectScript functions: [$FNUMBER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ffnumber) [$ZDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdate)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_timestampdiff "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_todate "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:38**

Source: **RSQL\_tochar.xml**