# TO TIMESTAMP

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_totimestamp

---

 

Caché SQL Reference

TO\_TIMESTAMP

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_tonumber "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_translate "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [TO\_TIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_totimestamp "TO_TIMESTAMP") \]

Go to: Description Date and Time String Format Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A date function that converts a formatted string to a timestamp.

Synopsis

TO\_TIMESTAMP(date\_string\[,format\])

Arguments

date\_string

A string expression to be converted to a timestamp. This expression may contain a date value, a time value, or a date and time value.

format

Optional — A date and time format string corresponding to date\_string. If omitted, defaults to DD MON YYYY HH:MI:SS.

Description

The TO\_TIMESTAMP function converts date and time strings in various formats to a standard timestamp, with data type TIMESTAMP. TO\_TIMESTAMP returns a timestamp with the following format:

yyyy-mm-dd hh:mm:ss

with leading zeroes always included. Time is specified using a 24–hour clock. By default, a returned timestamp does not include fractional seconds.

If date\_string omits components of the timestamp, TO\_TIMESTAMP supplies the missing components. If both date\_string and format omit the year, yyyy defaults to the current year; if only date\_string omits the year, it defaults to 00, which is expanded to a four-digit year according to the year format element. If a day or month value is omitted, dd defaults to 01; mm-dd defaults to 01-01. A missing time component defaults to 00. Fractional seconds are supported, but must be explicitly specified; no fractional seconds are provided by default.

TO\_TIMESTAMP supports conversion of two-digit years to four digits. TO\_TIMESTAMP supports conversion of 12-hour clock time to 24-hour clock time. It provides range validation of date and time element values, including leap year validation. Range validation violations generate an SQLCODE -400 error.

This function can also be invoked from Caché ObjectScript using the [TOTIMESTAMP()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#TOTIMESTAMP) method call:

$SYSTEM.SQL.TOTIMESTAMP(date\_string,format)

The TO\_TIMESTAMP function can be used in data definition when supplying a default value to a field. For example:

CREATE TABLE mytest
(ID NUMBER(12,0) NOT NULL,
End\_Year DATE DEFAULT TO\_TIMESTAMP('31-12-2007','DD-MM-YYYY') NOT NULL)

TO\_TIMESTAMP can be used with the CREATE TABLE or ALTER TABLE ADD COLUMN statements. Only a literal value for date\_string can be used in this context. For further details, refer to the [CREATE TABLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable) command.

Related SQL Functions

*   TO\_TIMESTAMP converts a formatted date and time string to a standard timestamp.
    
*   [TO\_CHAR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_tochar) performs the reverse operation; it converts a standard timestamp to a formatted date and time string.
    
*   [TO\_DATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_todate) converts a formatted date string to a date integer.
    
*   [CAST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_cast) and [CONVERT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_convert) perform TIMESTAMP data type conversion.
    

Date and Time String

The date\_string argument specifies a date and time string literal. If you supply a date string with no time component, TO\_TIMESTAMP supplies the time value 00:00:00. If you supply a time string with no date component, TO\_TIMESTAMP supplies the date of 01–01 (January 1) of the current year.

You can supply a date and time string of any kind for the input date\_string. Each date\_string character must correspond to the format string, with the following exceptions:

*   Leading zeros may be included or omitted (with the exception a date\_string without separator characters).
    
*   Years may be specified with two digits or four digits.
    
*   Month abbreviations (with format MON) must match the month abbreviation for that locale. For some locales, a month abbreviation may not be the initial sequential characters of the month name. Month abbreviations are not case sensitive.
    
*   Month names (with format MONTH) should be specified as full month names. However, TO\_TIMESTAMP does not require full month names with format MONTH; it accepts the initial character(s) of the full month name and selects the first month in the month list that corresponds to that initial letter sequence. Therefore, in English, “J” = “January”, “Ju” = “June”, “Jul” = “July”. All characters specified must match the sequential characters of the full month name; characters beyond the full month name are not checked. For example, “Fe”, “Febru”, and “FebruaryLeap” are all valid values; “Febs” is not a valid value. Month names are not case sensitive.
    
*   Time values can be input with the time separator characters defined for the locale. The output timestamp always represents the time value with the ODBC standard time separator characters: colon (:) and period (.)). An omitted time element defaults to zeroes.
    

Format

A format is a string of one or more format elements specified according to the following rules:

*   Format elements are not case-sensitive.
    
*   Almost any sequence or number of format elements is permitted.
    
*   Format strings separate their elements with non-alphanumeric separator characters (for example, a space, slash, or hyphen) that match the separator characters in the date\_string. These separator characters do not appear in the output string, which uses standard timestamp separators: hyphens for date values, colons for time values, and a period (when required) for fractional seconds. This use of separator characters does not depend on the DateSeparator defined for your NLS locale.
    
*   The following date format strings do not require separator characters: MMDDYYYY, DDMMYYYY, YYYYMMDDHHMISS, YYYYMMDDHHMI, YYYYMMDDHH, YYYYMMDD, YYYYDDMM, HHMISS, and HHMI. The incomplete date format YYYYMM is also supported, and assume a DD value of 01. Note that in these cases leading zeros must be provided for all elements (such as MM and DD), with the exception of the final element.
    
*   Characters in format that are not valid format elements are ignored.
    

Format Elements

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

Last two digits of the year. The first 2 digits of a YY 2-digit year default to 19.

RR / RRRR

Two-digit year to four-digit year conversion. (See below.)

DDD

Day of the year. The number of days since January 1. (See below.)

HH

Hour, specified as either 01–12 or 00–23, depending on whether a meridian indicator (AM or PM) is specified. Can be specified as HH12 or HH24.

MI

Minute, specified as 00–59.

SS

Second, specified as 00–59.

FF

Fractions of a second. FF indicates that one or more fractional digits are provided; date\_string can specify any number of fractional digits. TO\_TIMESTAMP returns exactly the fractional value explicitly supplied in date\_string; trailing zeroes are neither padded nor truncated. It provides a decimal separator format character, when necessary. By default, a Caché timestamp does not include fractional seconds.

AM / PM

Meridian indicator, specifies a 12–hour clock. (See below.)

A.M. / P.M.

Meridian indicator (with periods), specifies a 12–hour clock. (See below.)

A TO\_TIMESTAMP format can also include a D (day of week number), DY (day of week abbreviation), or DAY (day of week name) element to match the input date\_string. However, these format elements are not validated or used to determine the return value. For further details on these format elements, refer to [TO\_CHAR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_tochar).

Two-Digit Year Conversion (RR and RRRR formats)

The RR format provides two-digit to four-digit year conversion. This conversion is based on the current year. If the current year is in the first half of a century (for example, 2000 through 2050), two-digit years from 00 through 49 are expanded to a four-digit year in the current century, and two-digit years from 50 through 99 are expanded to a four-digit year in the previous century. If the current year is in the second half of a century (for example, 2050 through 2099), all two-digit years are expanded to a four-digit year in the current century. This expansion of two-digit years to four-digit years is shown in the following example:

SELECT TO\_TIMESTAMP('29 September 00','DD MONTH RR'),
       TO\_TIMESTAMP('29 September 08','DD MONTH RR'),
       TO\_TIMESTAMP('29 September 49','DD MONTH RR'),
       TO\_TIMESTAMP('29 September 50','DD MONTH RR'),
       TO\_TIMESTAMP('29 September 77','DD MONTH RR')

 

The RRRR format permits you to input a mix of two–digit and four-digit years. Four-digit years are passed through unchanged (the same as YYYY). Two-digit years are converted to four-digit years, using the RR format algorithm. This is shown in the following example:

SELECT TO\_TIMESTAMP('29 September 2008','DD MONTH RRRR')AS FourDigit,
       TO\_TIMESTAMP('29 September 08','DD MONTH RRRR') AS TwoDigit,
       TO\_TIMESTAMP('29 September 1949','DD MONTH RRRR') AS FourDigit,
       TO\_TIMESTAMP('29 September 49','DD MONTH RRRR') AS TwoDigit,
       TO\_TIMESTAMP('29 September 1950','DD MONTH RRRR') AS FourDigit,
       TO\_TIMESTAMP('29 September 50','DD MONTH RRRR') AS TwoDigit

 

Day of the Year (DDD format)

You can use DDD to convert the day of the year (number of days elapsed since January 1) to an actual date. The format string DDD YYYY must be paired with a corresponding date\_string consisting of an integer number of days and a four-digit year. (Two-digit years must be specified as RR (not YY) when used with DDD.) The format string DDD defaults to the current year. The number of elapsed days must be a positive integer in the range 1 through 365 (366 if YYYY is a leap year). The four-digit year must be within the standard Caché date range: 1841 through 9999. (If you omit the year, it defaults to the current year.) The DDD and year (YYYY, RRRR, or RR) format elements can be specified in any order; a separator character between them is mandatory; this separator can be a blank space. The following example shows this use of Day of the Year:

SELECT TO\_TIMESTAMP('2008:160','YYYY:DDD')

 

If a format string contains both a DD and a DDD element, the DDD element is dominant. This is shown in the following example, which returns 2008-02-29 00:00:00 (not 2008-12-31 00:00:00):

SELECT TO\_TIMESTAMP('2008-12-31-60','YYYY-MM-DD-DDD')

 

TO\_TIMESTAMP permits you to return a date expression corresponding to a day of the year. TO\_CHAR permits you to return the day of the year corresponding to a date expression.

Dates Before 1841

TO\_TIMESTAMP cannot represent a date before December 31, 1840. Attempted to input such a date results in an SQLCODE -400 error. The TO\_DATE function provides a Julian date format for this purpose. Julian date conversion converts a seven-digit internal positive integer value (a Julian day count) to a display-format or ODBC-format date. Time values are not supported for Julian dates.

12-Hour Clock Time

A TIMESTAMP always represents time using a 24-hour clock. A date\_string may represent time using a 12-hour clock or a 24-hour clock. TO\_TIMESTAMP assumes a 24-hour clock, unless one of the following applies:

*   The date\_string time value is followed by 'am' or 'pm' (with no periods). These meridian indicators are not case sensitive, and may be appended to the time value, or be separated from it by one or more spaces.
    
*   The format follows the time format with an 'a.m.' or 'p.m.' element (either one), separated from the time format by one or more spaces. For example: DD MON YYYY HH:MI:SS.FF P.M. This format supports 12-hour clock date\_string values such as 2:23pm, 2:23:54.6pm, 2:23:54 pm, 2:23:54 p.m., and 2:23:54 (assumed to be AM). Meridian indicators are not case sensitive. When using a meridian indicator with periods, it must be separated from the time value by one or more spaces.
    

Examples

The following embedded SQL example specifies date strings in various formats. The first one uses the default format, the others specify a format. All of these convert date\_string to the timestamp value of 2007–06–29 00:00:00:

  &sql(SELECT
       TO\_TIMESTAMP('29 JUN 2007'),
       TO\_TIMESTAMP('2007 Jun 29','YYYY MON DD'),
       TO\_TIMESTAMP('JUNE 29, 2007','month dd, YYYY'),
       TO\_TIMESTAMP('2007\*\*\*06\*\*\*29','YYYY\*\*\*MM\*\*\*DD'),
       TO\_TIMESTAMP('06/29/2007','MM/DD/YYYY'),
       TO\_TIMESTAMP('29/6/2007','DD/MM/YYYY')
    INTO :a,:b,:c,:d,:e,:f)
  IF SQLCODE\=0 { WRITE !,a,!,b,!,c,!,d,!,e,!,f }
  ELSE { WRITE "SQLCODE error:",SQLCODE }

 

The following example specifies the YYYYMM date format. It does not require element separators. TO\_TIMESTAMP supplies the missing day and time values:

 SELECT TO\_TIMESTAMP('200706','YYYYMM')

 

This example returns the timestamp 2007–06–01 00:00:00.

The following example specifies just the HH:MI:SS.FF time format. TO\_TIMESTAMP supplies the missing date value. In each case, this example returns the date of 2009–01–01 (where 2009 is the current year):

SELECT TO\_TIMESTAMP('11:34','HH:MI:SS.FF'),
       TO\_TIMESTAMP('11:34:22','HH:MI:SS.FF'),
       TO\_TIMESTAMP('11:34:22.00','HH:MI:SS.FF'),
       TO\_TIMESTAMP('11:34:22.7','HH:MI:SS.FF'),
       TO\_TIMESTAMP('11:34:22.7000000','HH:MI:SS.FF')

 

Note that fractional seconds are passed through exactly as specified, with no padding or truncation.

See Also

*   SQL commands: [CREATE TABLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable) [ALTER TABLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_altertable)
    
*   SQL functions: [CAST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_cast) [CONVERT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_convert) [TO\_CHAR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_tochar) [TO\_DATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_todate) [TO\_NUMBER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_tonumber)
    
*   Caché ObjectScript functions: [$ZDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdate) [$ZDATEH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdateh)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_tonumber "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_translate "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:38**

Source: **RSQL\_totimestamp.xml**