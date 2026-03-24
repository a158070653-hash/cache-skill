# $ZDATETIMEH

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fzdatetimeh

---

 

Caché ObjectScript Reference

$ZDATETIMEH

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetime "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzexp "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Additional Math and Time Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_MATHFUNCTIONS "Additional Math and Time Functions") \]  >  \[ [$ZDATETIMEH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetimeh "$ZDATETIMEH") \]

Go to: Description Parameter Values Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Validates a date and time and converts from display format to Caché internal format.

Synopsis

$ZDATETIMEH(datetime,dformat,tformat,monthlist,yearopt,startwin,endwin,mindate,maxdate,erropt,localeopt)
$ZDTH(datetime,dformat,tformat,monthlist,yearopt,startwin,endwin,mindate,maxdate,erropt,localeopt)

Parameters

datetime

The date and time input value. A date/time string specified in display format. $ZDATETIMEH converts this date/time string to $HOROLOG format. The datetime value can be either an explicit date and time (specified in various formats), an explicit date (specified in various formats) with the time value defaulting to 0, or the string “T” or “t”, representing the current date, with the time value either specified or defaulting to 0. The “T” or “t” string can optionally include a signed integer offset. See [datetime](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetimeh#RCOS_fzdatetimeh_datetime) below.

dformat

Optional — An integer code specifying the date format for the date portion of datetime. If datetime is “T”, dformat must be 5, 6, 7, 8, 9, or 15. See [dformat](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetimeh#RCOS_fzdatetimeh_dformat) below.

tformat

Optional — An integer code specifying the time format for the time portion of datetime. See [tformat](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetimeh#RCOS_fzdatetimeh_tformat) below.

monthlist

Optional — A string or the name of a variable that specifies a set of month names. This string must begin with a delimiter character, and its 12 entries must be separated by this delimiter character. See [monthlist](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetimeh#RCOS_fzdatetimeh_monthlist) below.

yearopt

Optional — An integer code that specifies whether to represent years as two- or four-digit values. See [yearopt](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetimeh#RCOS_fzdatetimeh_yearopt) below.

startwin

Optional — The start of the sliding window during which dates must be represented with two-digit years. See [startwin](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetimeh#RCOS_fzdatetimeh_startwin) below.

endwin

Optional — The end of the sliding window during which dates are represented with two-digit years. See [endwin](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetimeh#RCOS_fzdatetimeh_endwin) below.

mindate

Optional — The lower limit of the range of valid dates. Specified as a $HOROLOG integer date count, with 0 representing December 31, 1840. Can be specified as a positive or negative integer. See [mindate](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetimeh#RCOS_fzdatetimeh_mindate) below.

maxdate

Optional — The upper limit of the range of valid dates. Specified as a $HOROLOG integer date count. See [maxdate](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetimeh#RCOS_fzdatetimeh_maxdate) below.

erropt

Optional — An expression to return when datetime is invalid. Specifying a value for this parameter suppresses error codes associated with invalid or out of range datetime values. Instead of issuing an error message, $ZDATETIMEH returns erropt. See [erropt](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetimeh#RCOS_fzdatetimeh_erropt) below.

localeopt

Optional — A boolean flag that specifies which locale to use for the dformat, tformat, monthlist, yearopt, mindate and maxdate default values, and other date and time characteristics, such as the date separator character:

localeopt\=0: the current locale property settings determine these parameter defaults.

localeopt\=1: the ODBC standard locale determines these parameter defaults.

localeopt not specified: the dformat value determines these parameter defaults. If dformat\=3, ODBC defaults are used. Japanese and Islamic date dformatvalues use their own defaults. For all other dformat values, current locale property settings are used as defaults. See [localeopt](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetimeh#RCOS_fzdatetimeh_localeopt) below.

Optional — A boolean flag that specifies which locale to use. When 0, the current locale determines the date separator, time separator, and the other characters, strings, and options used to format dates and times. When 1, the ODBC locale determines these characters, strings, and options. The default is 0, unless dformat\=3, in which case the default is 1. See below.

Omitted parameters between specified parameter values are indicated by placeholder commas. Trailing placeholder commas are not required, but are permitted. Blank spaces are permitted between the commas that indicate an omitted parameter.

Description

$ZDATETIMEH validates a specified date and time value and converts it from display format to internal format. The corresponding $ZDATETIME function converts a date and time from internal format to display format. Internal format is the format used by the $HOROLOG or $ZTIMESTAMP. It represents a date and time value as a string of two numeric values separated by a comma.

The exact value returned depends on the parameters you use.

$ZDATETIMEH(datetime) converts a date and time value in the format "MM/DD/\[YY\]YY hh:mm:ss\[.ffff\]” to $HOROLOG format.

Syntax

Meaning

MM

A two-digit month.

DD

A two-digit day.

\[YY\]YY

Two or four digits for years from 1900 to 1999. Four digits for years before 1900 or after 1999.

hh

The hour in a 24-hour clock format.

mm

Minutes.

ss

Seconds.

ffff

Fractional seconds (zero to nine digits).

$ZDATETIMEH(datetime,dformat,tformat,monthlist,yearopt,startwin,endwin,mindate,maxdate,erropt) converts a date and time value that was originally specified (through $ZDATETIME) in date and time to $HOROLOG or $ZTIMESTAMP format. The dformat, tformat, yearopt, startwin and endwin values are identical to the values used by $ZDATETIME.

When you use a dformat of 5, 6, 7, 8, or 9 $ZDATETIMEH recognizes and converts a date in any of the external American date formats corresponding to dformat codes 1, 2, 3, 5, 6, 7, 8, 9. For a complete list of valid American date formats, refer to [$ZDATEH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdateh). When you use a dformat of 15 $ZDATETIMEH recognizes and converts a date in any unambiguous European date format. For a complete list of valid European date formats, refer to [$ZDATEH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdateh).

The dformat values of 5, 6, 7, 8, 9, or 15 also accept the current date specified by the letter “T” or “t”, optionally followed by a plus (+) or a minus (-), and the number of days after or before the current date.

$ZDATETIMEH recognizes and converts a time in any of eight time formats, regardless of which time format you specify in the function call. In addition, $ZDATETIMEH recognizes the suffixes “AM, PM, NOON, and MIDNIGHT.” You can express these suffixes in uppercase, lowercase, or mixed case. You can also abbreviate these suffixes to any number of letters.

The recognized forms include:

*   The default date format, MM/DD/\[YY\]YY
    
*   The format DDMmm\[YY\]YY
    
*   The ODBC format \[YY\]YY\-MM\-DD
    
*   The format DD/MM/\[YY\]YY
    
*   The format Mmm D, YYYY
    
*   The format Mmm D YYYY
    
*   The format Mmm DD YY
    
*   The format YYYYMMDD (numeric format)
    

Parameter Values

datetime

The date and time string that you want to convert to $HOROLOG format. You can specify any of the following:

*   An expression that evaluates to a single string with the date first, followed by a single blank space, followed by the time.
    
*   An expression that evaluates to a string specifying the date only. The time value defaults to midnight (0), unless tformat is 7 or 8, in which case the time defaults to the local time zone offset from midnight (0).
    
*   The letter code “T” or “t”, which specifies the current date. This letter can optionally be followed by a plus (+) or a minus (-) sign and an integer specifying an offset, in days, from the current date. You can either follow this date expression by a single blank space and a time expression, or allow the time to default to midnight (0). (If tformat is 7 or 8, the time defaults to the local time zone offset from midnight (0).) If you use this current date option, you must specify a dformat of 5, 6, 7, 8, 9, or 15.
    

Valid date and time values depend on the DateFormat and TimeFormat properties of the current locale and the values specified for the dformat and tformat parameters. For details on specifying dates, refer to [$ZDATEH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdateh).

By default, the earliest valid datetime date is December 31, 1840 (0 in internal $HOROLOG representation). Dates are limited to positive integers by default because the DateMinimum property defaults to 0. You can specify earlier dates as negative integers, provided the DateMinimum property of the current locale is set to a greater or equal negative integer. The lowest valid DateMinimum value is -672045, which corresponds to January 1, 0001. Caché uses the proleptic Gregorian calendar, which projects the Gregorian calendar back to “Year 1”, in conformance with the ISO 8601 standard. This is, in part, because the Gregorian calendar was adopted at different times in different countries. For example, much of continental Europe adopted it in 1582; Great Britain and the United States adopted it in 1752. Thus Caché dates prior to your local adoption of the Gregorian calendar may not correspond to historical dates that were recorded based on the local calendar then in effect. For further details on dates prior to 1840, refer to the mindate parameter.

dformat

Format for the date. Valid values are:

Value

Meaning

1

MM/DD/\[YY\]YY (07/01/97 or 03/27/2002) — [American numeric format](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_dates). You must specify the correct dateseparator character (/ or .) for the current locale.

2

DD Mmm \[YY\]YY (01 Jul 97 or 27 Mar 2002)

3

\[YY\]YY-MM-DD (1997-07-01 or 2002-03-27) - ODBC format

4

DD/MM/\[YY\]YY (01/07/97 or 27/03/2002) — [European numeric format](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_dates). You must specify the correct dateseparator character (/ or .) for the current locale.

5

Mmm D, YYYY (Jul 1, 1997 or Mar 27, 2002)

6

Mmm D YYYY (Jul 1 1997 or Mar 27 2002)

7

Mmm DD \[YY\]YY (Jul 01 1997 or Mar 27 2002)

8

YYYYMMDD (19930701 or 20020327) - Numeric format

9

Mmmmm D, YYYY (July 1, 1997 or March 27, 2002)

15

DD/MM/\[YY\]YY or YYYY-MM-DD or any unambiguous European date format with any dateseparator character, or YYYYMMDD with no date separators. The dateseparator character may be any non-alphanumeric character, including blank spaces, regardless of the dateseparator character specified in the current locale. Also accepts monthlist names and “T”. For a complete list of valid European date formats, refer to [$ZDATEH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdateh).

16

YYYYc\[M\]Mc\[D\]Dc — Japanese date format. Year, month, and day numbers are the same as other date formats; leading zeros are omitted. The Japanese characters for “year”, “month”, and “day” (shown here as c) are inserted after the year, month, and day numbers. These characters are Year=$CHAR(24180), Month=$CHAR(26376), and Day=$CHAR(26085).

17

YYYYc \[M\]Mc \[D\]Dc — Japanese date format. Same as dformat 16, except that a blank space is inserted after the “year” and “month” Japanese characters.

18

\[D\]D Mmmmm YYYY — Tabular Hijri (Islamic) date format with full month name. Day leading zeros are omitted; year leading zeros are included. Caché date -445031 (07/19/0622 C.E.) = 1 Muharram 0001.

19

\[D\]D \[M\]M YYYY — Tabular Hijri (Islamic) date format with month number. Day and month leading zeros are omitted; year leading zeros are included. Caché date -445031 (07/19/0622 C.E.) = 1 1 0001.

20

\[D\]D Mmmmm YYYY — Observed Hijri (Islamic) date format with full month name. Defaults to Tabular Hijri (dformat 18). To override tabular calculation, use the class [%Calendar.Hijri](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Calendar.Hijri) to add observations of new moon crescents.

21

\[D\]D \[M\]M YYYY — Observed Hijri (Islamic) date format with month number. Defaults to Tabular Hijri (dformat 19). To override tabular calculation, use the class [%Calendar.Hijri](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Calendar.Hijri) to add observations of new moon crescents.

\-1

Get effective dformat value from the DateFormat property of the [current locale](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_dates). This is the default behavior if you do not specify dformat.

\-2

$ZDATETIMEH takes an integer specifying the count of seconds from a platform-specific origin date/time, and returns the corresponding [$HOROLOG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog) value. The input value is the value returned by the time() library function, as defined in the ISO C Programming Language Standard. For example, on POSIX-compliant systems this value is the count of seconds from January 1, 1970 00:00:00 UTC. The tformat, monthlist, yearopt, startwin, and endwin parameters are ignored. The following platform-specific formats are supported: OpenVMS: unsigned 32-bit integer; 32-bit Linux: signed 32-bit integer; 64-bit Linux: signed 64-bit integer; Windows: unsigned 64-bit integer. Refer to the MultiValue Basic [SYSTEM(99)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RVBS_fsystem) function in Caché MultiValue Basic Reference. (Currently this date conversion potentially has the “local time variant boundary day” time conversion anomaly described for tformat values 7 and 8.)

\-3

$ZDATETIMEH takes a datetime value specified in [$ZTIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztimestamp) internal format, converts that value from UTC Universal time to local time, and returns the resulting value in the same internal format. The tformat, monthlist, yearopt, startwin, and endwin parameters are ignored. $ZDATETIME performs the inverse operation. (Currently, this date conversion has the time conversion anomalies described for tformat values 7 and 8. These potentially affect dates prior to 1970, dates after 2038, and local time variant boundary days, such as the beginning date or end date for Daylight Saving Time.)

Where:

Syntax

Meaning

YYYY

YYYY is a four-digit year. \[YY\]YY is a two-digit year if datetime falls within the active window for two-digit dates; otherwise it is a four-digit number.

MM

Two-digit month.

D

One-digit day if the day number <10. Otherwise, two digits.

DD

Two-digit day.

Mmm

Mmm is a month abbreviation extracted from the MonthAbbr property of the current locale. The default values are:

“Jan Feb Mar Apr May Jun Jul Aug Sep Oct Nov Dec”

Or an alternate month abbreviation (or name of any length) extracted from an optional list specified as the monthlist parameter to $ZDATETIMEH.

Mmmmm

Full name of the month as specified by the MonthName property of the current locale. The default values are: “January February March ... November December”

Or an alternate month name extracted from an optional list specified as the monthlist parameter to $ZDATETIMEH.

dformat Default

If you omit dformat or set it to -1, the dformat default depends on the localeopt parameter and the NLS DateFormat property:

*   If localeopt\=1 the dformat default is ODBC format. The tformat, monthlist, yearopt, mindate and maxdate parameter defaults are also set to ODBC format. This is the same as setting dformat\=3.
    
*   If localeopt\=0 or is unspecified, the dformat default is taken from the NLS DateFormat property. If DateFormat\=3, the dformat default is ODBC format. However, DateFormat=3 does not affect the tformat, monthlist, yearopt, mindate and maxdate parameter defaults, which are as specified in the current NLS locale definition.
    

To determine the default date properties for your locale, invoke the [GetFormatItem()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYS.NLS.Format#GetFormatItem) NLS class method:

  WRITE ##class(%SYS.NLS.Format).GetFormatItem("DateFormat"),!
  WRITE ##class(%SYS.NLS.Format).GetFormatItem("DateSeparator")

 

$ZDATETIMEH will use the value of the [DateSeparator property of the current locale](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_dates) (either / or .) as the delimiter between months, days, and the year when dformat\=1 or 4.

European date format (dformat\=4, DD/MM/YYYY order) is the default for many (but not all) European languages, including British English, French, German, Italian, Spanish, and Portuguese (which use a “/” DateSeparator character), as well as Czech (csyw), Russian (rusw), Slovak (skyw), Slovenian (svnw), and Ukrainian (ukrw) (which use a “.” DateSeparator character). For further details on default date formats for supported locales, refer to [Dates](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_dates) in Using Caché ObjectScript.

dformat Settings

If dformat is 3 (ODBC format date), ODBC format defaults are also used for the tformat, monthlist, yearopt, mindate and maxdate parameter defaults. Current locale defaults are ignored.

If dformat is 16 or 17 (Japanese date formats), the date format is independent of the locale setting. To use Japanese-format dates you must have a Unicode Caché instance. On an 8-bit Caché instance specifying dformat is 16 or 17 causes a <ERWIDECHAR> error.

If dformat is 18, 19, 20, or 21 (Islamic date formats) and localeopt is unspecified, parameters default to Islamic defaults, rather than current locale defaults. The monthlist parameter defaults to Arabic month names transliterated with Latin characters. The tformat, yearopt, mindate and maxdate parameters default to ODBC defaults. The date separator defaults to the Islamic default (a space), not the ODBC default or the current locale DateSeparator property value. If localeopt\=0 current locale property defaults are used for these parameters. If localeopt\=1 ODBC defaults are used for these parameters.

tformat

A numeric value that specifies the format in which the time value is input. Supported values are as follows:

Value

Meaning

\-1

Get the effective tformat value from the TimeFormat property of the current locale, which defaults to a value of 1. This is the default behavior if you do not specify tformat for all dformat values except 3.

1

Specify time in the form "hh:mm:ss" (24-hour clock). This is the default when dformat\=3.

2

Specify time in the form “hh:mm” (24-hour clock)

3

Specify time in the form “hh:mm:ss\[AM/PM\] (12-hour clock)

4

Specify time in the form “hh:mm\[AM/PM\] (12-hour clock)

5

Specify time in the form "hh:mm:ss+/-hh:mm" (24-hour clock). The time is specified as local time. The following optional suffix may be supplied, but is ignored: a plus (+) or minus (–) suffix followed by the offset of local time from Coordinated Universal Time (UTC). A minus sign (-hh:mm) indicates that the local time is earlier (westward) of the Greenwich meridian by the returned offset number of hours and minutes. A plus sign (+hh:mm) indicates that the local time is later (eastward) of the Greenwich meridian by the returned offset number of hours and minutes.

6

Specify time in the form “hh:mm+/-hh:mm” (24-hour clock). The time is specified as local time. The following optional suffix may be supplied, but is ignored: a plus (+) or minus (–) suffix followed by the offset of local time from Coordinated Universal Time (UTC). A minus sign (-hh:mm) indicates that the local time is earlier (westward) of the Greenwich meridian by the returned offset number of hours and minutes. A plus sign (+hh:mm) indicates that the local time is later (eastward) of the Greenwich meridian by the returned offset number of hours and minutes.

7

Specify time in the form "hh:mm:ssZ" (24-hour clock). The time must be specified as Coordinated Universal Time (UTC). The optional “Z” suffix may be supplied or omitted, but is ignored. This suffix merely indicates that the time is assumed to be Coordinated Universal Time (UTC), rather than local time.

8

Specify time in the form “hh:mmZ” (24-hour clock). The time must be specified as Coordinated Universal Time (UTC). The optional “Z” suffix may be supplied or omitted, but is ignored. This suffix merely indicates that the time is assumed to be Coordinated Universal Time (UTC), rather than local time.

If the datetime string contains both a date part and a time part, the time part is separated from the date part by either a single space or the capital letter "T". If a time part is present:

*   Time formats 1 through 6 assume datetime specifies local time using the same time zone as the result.
    
*   Time formats 7 and 8 assume datetime specifies UTC time; these formats convert both the date and time to system local time.
    

For time formats 5 through 8 the datetime time value may be followed by a suffix consisting of either the capital letter "Z" or a UTC offset that starts with a "+" or "-". The presence of a suffix does not affect time zone conversion.

Note:

Conversions involving dformat values -2 and -3 and tformat values 7 and 8 and the UTC offsets generated by tformat values 5 and 6 have the following platform-dependent anomalies:

*   Local time variant boundary behavior may differ on different operating system platforms. When a local time variant change occurs and the local clock shifts backwards (“Fall back” at the end of [Daylight Saving Time](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog#RCOS_vhorolog_dst)) the local time hour is repeated. Within this two-hour period, a Caché time conversion operation cannot determine whether it is being applied to the first occurrence of that local time hour, or the second occurrence of the same hour. $ZDATETIME uses whichever assumption is used by the platform-specific runtime library. Therefore, within this temporal window, different operating system platforms may give different time conversion results.
    
*   Caché performs conversions between local time and UTC time using the standard time offset for all dates that are supported by the operating system platform.
    
    If a specified date is earlier than the earliest date supported by the platform, Caché uses the standard time offset for 1902–01–01 (if this date is supported by the platform). If the date 1902–01–01 is not supported by the platform, Caché uses the standard time offset for 1970–01–01. Any local time variant offset (such as Daylight Saving Time) is ignored.
    
    If a specified date is later than the latest date supported by the platform, Caché calculates a corresponding date within the range 2010–01–01 to 2037–12–31 and uses the standard time offset for that corresponding date. This algorithm should provide accurate time offsets for dates up to 2100–02–28, provided there are no future changes to the laws governing date/time observances.
    

To determine the default time format for your locale, invoke the [GetFormatItem()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYS.NLS.Format#GetFormatItem) NLS class method:

  WRITE ##class(%SYS.NLS.Format).GetFormatItem("TimeFormat")

 

12–hour Clock (tformat 3 and 4)

In 12-hour clock formats, morning and evening are specified with time suffixes, here shown as AM and PM. To determine the default time suffixes for your locale, invoke the [GetFormatItem()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYS.NLS.Format#GetFormatItem) NLS class method, as follows:

  WRITE ##class(%SYS.NLS.Format).GetFormatItem("AM"),!
  WRITE ##class(%SYS.NLS.Format).GetFormatItem("PM"),!

 

For all dformat values except 3, 18, 19, 20, and 21 the AM and PM properties default to the current locale definition. For dformat\=3 (ODBC date format) and dformat\=18, 19, 20, or 21 (Islamic date formats) the time suffixes are always “AM” and “PM”, regardless of the current locale property values. The AM and PM property defaults are “AM” and “PM” for all locales except the Japanese locale jpww.

By default, Midnight and Noon are represented as “MIDNIGHT” and “NOON” for all locales except Japanese locales (jpnw, jpuw, jpww, zdsw, zdtw, zduw), Portuguese (ptbw), Russian (rusw), and Ukrainian (ukrw). However, when dformat\=3, $ZDATETIMEH always uses the ODBC standard values, regardless of the default settings for your locale.

monthlist

An expression that resolves to a string of month names or month name abbreviations, separated by a delimiter character. The names in monthlist replace the default month abbreviation values from the MonthAbbr property or the month name values from the MonthName property of the current locale.

monthlist is valid only if dformat is 2, 5, 6, 7, 9, 15, 18, or 20. If dformat is any other value $ZDATETIMEH ignores monthlist.

The monthlist string has the following format:

*   The first character of the string is a delimiter character (usually a space). The same delimiter must appear before the first month name and between each month name in monthlist. You can specify any single-character delimiter; this delimiter must be specified between the month, day, and year portions of the specified datetime value, which is why a space is usually the preferred character.
    
*   The month names string should contain twelve delimited values, corresponding to January through December. It is possible to specify more or less than twelve month names, but if there is no month name corresponding to the month in datetime an <ILLEGAL VALUE> error is generated.
    

If you omit monthlist or specify a monthlist value of -1, $ZDATETIMEH uses the list of month names defined in the MonthAbbr or MonthName property of the current locale, unless one of the following is true: If localeopt\=1, the monthlist default is the ODBC month list (in English). If localeopt is unspecified and dformat is 18 or 20 (Islamic date formats) the monthlist default is the Islamic month list (Arabic names expressed using Latin letters), ignoring the MonthAbbr or MonthName property value.

To determine the default month names and month abbreviations for your locale, invoke the [GetFormatItem()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYS.NLS.Format#GetFormatItem) NLS class method:

  WRITE ##class(%SYS.NLS.Format).GetFormatItem("MonthName"),!
  WRITE ##class(%SYS.NLS.Format).GetFormatItem("MonthAbbr"),!

 

yearopt

With dformat values 0, 1, 2, 4, or 7, a numeric code that specifies the time window in which to display the year as a two-digit value. yearopt can be:

Value

Meaning

\-1

Get effective yearopt value from YearOption property of current locale which defaults to 0. This is the default behavior if you do not specify yearopt .

0

Represent 20th century dates (1900 through 1999) with two-digit years, unless a process-specific sliding window (established via the %DATE utility) is in effect. If such a window is in effect, represent only those dates falling within the sliding window by two-digit years. Represent all dates falling outside the 20th century or outside the process-specific sliding window by four-digit years.

1

Represent 20th century dates with two-digit years and all other dates with four-digit years.

2

Represent all dates with two-digit years.

3

Represent with two-digit years those dates falling within the sliding temporal window defined by startwin and (optionally) endwin. Represent all other dates with four-digit years. When yearopt =3, startwin and endwin are absolute dates in $HOROLOG format.

4

Represent all dates with four-digit years.

5

Represent with two-digit years all dates falling within the sliding window defined by startwin and (optionally) endwin. Represent all other dates with four-digit years. When yearopt=5, startwin and endwin are relative years.

6

Represent all dates in the current century with two-digit years and all other dates with four-digit years.

If you omit yearopt or specify a yearopt value of -1, $ZDATETIMEH uses the YearOption property of the current locale, unless one of the following is true: If localeopt\=1, the yearopt default is the ODBC year option. If localeopt\=0 or is unspecified and dformat is 18, 19, 20, or 21 (Islamic date formats) the yearopt default is the ODBC year option (4-digit years); the YearOption property value is ignored for Islamic dates.

startwin

A numeric value that specifies the start of the sliding window during which dates must be represented with two-digit years. You must supply startwin when you use a yearopt of 3 or 5. startwin is not valid with any other yearopt values.

When yearopt = 3, startwin is an absolute date in $HOROLOG date format that indicates the start date of the sliding window.

When yearopt = 5, startwin is a numeric value that indicates the start year of the sliding window expressed in the number of years before the current year. The sliding window always begins on the first day of the year (January 1) specified in startwin.

endwin

A numeric value that specifies the end of the sliding window during which dates are represented with two-digit years. You may optionally supply endwin when yearopt is 3 or 5. endwin is not valid with any other yearopt values.

When yearopt =3, endwin is an absolute date in $HOROLOG date format that indicates the end date of the sliding window.

When yearopt =5, endwin is a numeric value that indicates the end year of the sliding window expressed as the number of years past the current year. The sliding window always ends on December 31st of the year specified in endwin. If endwin is not specified, it defaults to December 31st of the year 100 years after startwin.

If endwin is omitted (or specified as -1) the effective sliding window will be 100 years long. The endwin value of -1 is a special case that always returns a date value, even when higher and lower endwin values return erropt. For this reason, it is preferable to omit endwin when specifying a 100-year window, and to avoid the use of negative endwin values.

If you supply both startwin and endwin , the sliding window they specify must not have a duration of more than 100 years.

mindate

An expression that specifies the lower limit of the range of valid dates (inclusive). Can be specified as a $HOROLOG integer date count (for example, 1/1/2013 is represented as 62823) or a $HOROLOG string value. You can include or omit the time portion of a $HOROLOG date string (for example “62823,43200”), but only the date portion of mindate is parsed. Specifying an datetime value earlier than mindate generates a <VALUE OUT OF RANGE> error.

The following are supported mindate values:

*   Positive integer: Most commonly mindate is specified as a positive integer to establish the earliest allowed date as some date after December 31, 1840. For example, a mindate of 21550 would establish the earliest allowed date as January 1, 1900. The highest valid value is 2980013 (December 31, 9999).
    
*   0: specifies the minimum date as December 31, 1840. This is the DateMinimum property default.
    
*   Negative integer -2 or larger: specifies a minimum date counting backwards from December 31, 1840. For example, a mindate of -14974 would establish the earliest allowed date as January 1, 1800. Negative mindate values are only meaningful if the DateMinimum property of the current locale has been set to an equal or greater negative number. The lowest valid value is -672045.
    
*   If omitted (or specified as -1), mindate defaults to the DateMinimum property value for the current locale, unless one of the following is true: If localeopt\=1, the mindate default is 0. If localeopt is unspecified and dformat\=3, the mindate default is 0. If localeopt is unspecified and dformat is 18, 19, 20, or 21 (Islamic date formats) the mindate default is 0.
    

You can get and set the DateMinimum property as follows:

  SET min\=##class(%SYS.NLS.Format).GetFormatItem("DateMinimum")
  WRITE "initial DateMinimum value is ",min,!
Permit18thCenturyDates
  SET x\=##class(%SYS.NLS.Format).SetFormatItem("DateMinimum",\-51498)
  SET newmin\=##class(%SYS.NLS.Format).GetFormatItem("DateMinimum")
  WRITE "set DateMinimum value is ",newmin,!!
RestrictTo19thCenturyDates
  WRITE $ZDATETIMEH("05/29/1805 12:00:00",1,,,,,,\-14974),!!
ResetDateMinimumToDefault
  SET oldmin\=##class(%SYS.NLS.Format).SetFormatItem("DateMinimum",min)
  WRITE "reset DateMinimum value from ",oldmin," to ",min 

 

You may specify mindate with or without maxdate. Specifying a mindate larger than maxdate generates an <ILLEGAL VALUE> error.

maxdate

An expression that specifies the upper limit of the range of valid dates (inclusive). Can be specified as a $HOROLOG integer date count (for example, 1/1/2100 is represented as 94599) or a $HOROLOG string value. You can include or omit the time portion of the $HOROLOG date (for example “94599,43200”), but only the date portion of maxdate is parsed.

If maxdate is omitted or if specified as -1, the maximum date limit is obtained from the DateMaximum property of the current locale, which defaults to the maximum permissible value for the date portion of $HOROLOG: 2980013 (corresponding to December 31, 9999). However, the application of the DateMaximum property is governed by the localeopt setting. When localeopt\=1 (which is the default for dformat\=3) the date maximum default is the ODBC value (2980013), regardless of the current locale setting. Islamic date formats also take the ODBC default.

Specifying a datetime larger than maxdate generates a <VALUE OUT OF RANGE> error.

Specifying a maxdate larger than 2980013 generates an <ILLEGAL VALUE> error.

You may specify maxdate with or without mindate. Specifying a maxdate smaller than mindate generates an <ILLEGAL VALUE> error.

erropt

Specifying a value for this parameter suppresses errors associated with invalid or out of range datetime values. Instead of generating <ILLEGAL VALUE> or <VALUE OUT OF RANGE> errors, the $ZDATETIMEH function returns the erropt value.

Caché performs standard numeric evaluation on datetime, which must evaluate to an integer date within the mindate/maxdate range. Thus, 7, "7", +7, 0007, 7.0, "7 dwarves", and --7 all evaluate to the same date value: 01/07/1841. By default, values greater than 2980013 or less than 0 generate a <VALUE OUT OF RANGE> error. Fractional values generate an <ILLEGAL VALUE> error. Non-numeric strings (including the null string) evaluate to 0, and thus return the $HOROLOG initial date: 12/31/1840.

The erropt parameter only suppresses errors generated due to invalid or out of range values of datetime. Errors generated due to invalid or out of range values of other parameters will always generate errors whether or not erropt has been supplied. For example, an <ILLEGAL VALUE> error is always generated when $ZDATETIMEH specifies a sliding window where endwin is earlier than startwin. Similarly, an <ILLEGAL VALUE> error is generated when maxdate is less than mindate.

localeopt

This Boolean parameter specifies either the user’s current locale definition or the ODBC locale definition as the source for defaults for the locale-specified parameters dformat, tformat, monthlist, yearopt, mindate and maxdate:

*   If localeopt\=0, all of these parameters take the current locale definition defaults.
    
*   If localeopt\=1, all of these parameters take the ODBC defaults.
    
*   If localeopt is not specified, the dformat parameter determine the default for these parameters. If dformat\=3, the ODBC defaults are used. If dformat is 18, 19, 20, or 21 the Islamic date and time format defaults are used, regardless of the current locale definition. For all other dformat values, the current locale definition defaults are used. Refer to the [dformat](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetimeh#RCOS_fzdatetimeh_dformat) description for further details.
    

The ODBC standard locale cannot be changed; it is used to format date and time strings that are portable between Caché processes that have made different National Language Support (NLS) choices. The ODBC locale date and time definitions are as follows:

*   Date format defaults to 3. Therefore, if dformat is undefined or -1, date format 3 is used.
    
*   Date separator defaults to "/". However, date format defaults to 3, which always uses "-" as the date separator.
    
*   Year option defaults to 4 digits.
    
*   Date minimum and maximum are 0 and 2980013 ($HOROLOG date count).
    
*   English month names, month abbreviations, weekday names, weekday abbreviations, and the words "Noon" and "Midnight" are used.
    
*   Time format defaults to 1. Time separator is ":". Time precision is 0 (no fractional seconds). AM and PM indicators are "AM" and "PM".
    

Notes

$ZDATETIMEH and Fractional Seconds

Unlike $ZDATETIME, $ZDATETIMEH does not allow you to specify a precision for the time. Any fractional seconds in the original $ZDATETIME\-formatted time are retained in the value $ZDATETIMEH returns.

Note that $HOROLOG does not return fractional seconds.

Invalid Values with $ZDATETIMEH

You receive a <FUNCTION> error in the following conditions:

*   If you specify an invalid dformat code (an invalid integer value, or a non-integer value.)
    
*   If you specify an invalid value for tformat (an integer value less than -1 or greater than 8, a zero, or a non-integer value.)
    
*   If you do not specify a startwin value when yearopt is 3 or 5.
    

You receive an <ILLEGAL VALUE> error under the following conditions:

*   If you specify an invalid value for any date/time unit. If specified, the erropt value is returned rather than issuing an <ILLEGAL VALUE>.
    
*   If you specify excess leading zeros for any date/time unit in an ODBC date. For example, you can represent the February 3, 2007 as “2007–2–3” or “2007–02–03”, but will receive an <ILLEGAL VALUE> for “2007–002–03”. If specified, the erropt value is returned rather than issuing an <ILLEGAL VALUE>.
    
*   If the given month number is greater than the number of month values in monthlist.
    
*   If maxdate is less than mindate.
    
*   If endwin is less than startwin.
    
*   If startwin and endwin specify a sliding temporal window whose duration is greater than 100 years.
    

You receive a <VALUE OUT OF RANGE> error under the following conditions:

*   If you specify a date (or an offset to “T”) which is earlier than Dec. 31, 1840 or later than Dec. 31, 9999, and do not supply an erropt value.
    
*   If you specify an otherwise valid date (or an offset to “T”) which is outside the range of mindate and maxdate and do not supply an erropt value.
    

The Current Date

The following examples shows how you can use the “T” or “t” letter code to specify the current date. Note that dformat must be 5, 6, 7, 8, 9, or 15.

The current date with the time defaulting to 0:

   WRITE $ZDATETIMEH("T",5)

 

Three days before the current date, with the time defaulting to 0:

   WRITE $ZDATETIMEH("T-3",5)

 

Two days after the current date, with a specified time:

   WRITE $ZDATETIMEH("T+2 11:45:00",5)

 

$ZDATETIMEH Compared to $ZDATEH

$ZDATETIMEH is similar to $ZDATEH except it converts both a date and a time value to the internal $HOROLOG format (even if no time value is specified.) $ZDATEH only converts a date value to $HOROLOG format. For example:

   WRITE $ZDATEH("Nov 25, 2002",5)

 

returns 59133.

   WRITE $ZDATETIMEH("Nov 25, 2002 10:08:09.539",5)

 

returns 59133,36489.539.

Specifying $ZDATETIMEH with no time value:

   WRITE $ZDATETIMEH("Nov 25, 2002",5)

 

returns 59133,0.

Specifying $ZDATETIMEH with no time value, and a tformat of 7 or 8:

   WRITE $ZDATETIMEH("Nov 25, 2002",5,7)

 

returns a value such as: 59133,68400, where the time value is the local time zone offset from midnight. In this case, U.S. Eastern Standard Time is 5 hours offset from UTC, so the time value here represents 19:00 (5 hours offset from midnight).

See Also

*   [JOB](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cjob) command
    
*   [$ZDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdate) function
    
*   [$ZDATEH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdateh) function
    
*   [$ZDATETIME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetime) function
    
*   [$ZTIME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fztime) function
    
*   [$ZTIMEH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fztimeh) function
    
*   [$HOROLOG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog) special variable
    
*   [$ZTIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztimestamp) special variable
    
*   %DATE utility, which is documented in the “[Legacy Documentation](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GDOC_legacy)” chapter in Using InterSystems Documentation
    
*   More information on locales in the section on “[System Classes for National Language Support](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSTU_customize#GSTU_customize_nls)” in Caché Specialized System Tools and Utilities.
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetime "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzexp "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fzdatetimeh.xml**