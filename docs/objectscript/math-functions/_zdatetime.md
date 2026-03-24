# $ZDATETIME

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fzdatetime

---

 

Caché ObjectScript Reference

$ZDATETIME

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdateh "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetimeh "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Additional Math and Time Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_MATHFUNCTIONS "Additional Math and Time Functions") \]  >  \[ [$ZDATETIME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetime "$ZDATETIME") \]

Go to: Description Parameters Examples Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Validates a date and time and converts it from internal format to the specified display format.

Synopsis

$ZDATETIME(hdatetime,dformat,tformat,precision,monthlist,yearopt,startwin,endwin,mindate,maxdate,erropt,localeopt)
$ZDT(hdatetime,dformat,tformat,precision,monthlist,yearopt,startwin,endwin,mindate,maxdate,erropt,localeopt)

Parameters

hdatetime

The date and time value, specified in internal date and time format. See [hdatetime](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetime#RCOS_fzdatetime_hdatetime) below.

dformat

Optional — An integer code specifying the format for the returned date value. See [dformat](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetime#RCOS_fzdatetime_dformat) below.

tformat

Optional — An integer code specifying the format for the returned time value. See [tformat](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetime#RCOS_fzdatetime_tformat) below.

precision

Optional — An integer specifying the number of decimal places of precision (fractional seconds) for the returned time value. See [precision](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetime#RCOS_fzdatetime_precision) below.

monthlist

Optional — A string or the name of a variable that specifies a set of month names. This string must begin with a delimiter character, and its 12 entries must be separated by this delimiter character. See [monthlist](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetime#RCOS_fzdatetime_monthlist) below.

yearopt

Optional — An integer code that specifies whether to represent years as two- or four-digit values. See [yearopt](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetime#RCOS_fzdatetime_yearopt) below.

startwin

Optional — The start of the sliding window during which dates are represented with two-digit years. See [startwin](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetime#RCOS_fzdatetime_startwin) below.

endwin

Optional — The end of the sliding window during which dates are represented with two-digit years. See [endwin](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetime#RCOS_fzdatetime_endwin) below.

mindate

Optional — The lower limit of the range of valid dates. Specified as a $HOROLOG integer date count, with 0 representing December 31, 1840. Can be specified as a positive or negative integer. See [mindate](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetime#RCOS_fzdatetime_mindate) below.

maxdate

Optional — The upper limit of the range of valid dates, specified as an integer $HOROLOG date count. See [maxdate](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetime#RCOS_fzdatetime_maxdate) below.

erropt

Optional — An expression to return when hdatetime is invalid. Specifying a value for this parameter suppresses error codes associated with invalid or out of range hdatetime values. Instead of issuing an error message, $ZDATETIME returns erropt. See [erropt](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetime#RCOS_fzdatetime_erropt) below.

localeopt

Optional — A boolean flag that specifies which locale to use for the dformat, tformat, monthlist, yearopt, mindate and maxdate default values, and other date and time characteristics:

localeopt\=0: the current locale property settings determine these parameter defaults.

localeopt\=1: the ODBC standard locale determines these parameter defaults.

localeopt not specified: the dformat value determines these parameter defaults. If dformat\=3, ODBC defaults are used; otherwise current locale property settings are used. See [localeopt](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetime#RCOS_fzdatetime_localeopt) below.

Omitted parameters between specified parameter values are indicated by placeholder commas. Trailing placeholder commas are not required, but are permitted. Blank spaces are permitted between the commas that indicate an omitted parameter.

Description

$ZDATETIME validates a specified date and time and converts them from $HOROLOG or $ZTIMESTAMP internal format to one of several alternative date and time display formats. The exact value returned depends on the parameters you specify.

*   $ZDATETIME(hdatetime) returns the date and time in the default display format for the current locale.
    
*   $ZDATETIME(hdatetime,dformat,tformat,precision,monthlist,yearopt,startwin,endwin,mindate,maxdate) returns the date and time in the display format specified by dformat and tformat, further defined by the other parameters you specify. The range of valid dates may be restricted by the mindate and maxdate parameters.
    

Parameters

hdatetime

The date and time, specified as an internal format value. Caché internal format represents dates as a count of days from an arbitrary starting point (Dec. 31, 1840), and represents times as a count of seconds in the current day. The hdatetime value must be a string in one of the following formats:

*   [$HOROLOG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog): two unsigned integers separated by comma. The first is an integer specifying the date (in days), the second is an integer specifying the time (in seconds).
    
*   [$ZTIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztimestamp): two unsigned numbers separated by comma: the first is an integer specifying the date (in days), the second is a number specifying the time (in seconds and fractions of a second). The time value can have up to nine digits of precision (fractional seconds) to the right of the decimal point.
    

You can specify hdatetime as a string value, a variable, or an expression.

By default, the earliest valid hdatetime date is 0 (December 31, 1840). Dates are limited to positive integers by default because the DateMinimum property defaults to 0. You can specify earlier dates as negative integers, provided the DateMinimum property of the current locale is set to a greater or equal negative integer. The lowest valid DateMinimum value is -672045, which corresponds to January 1, 0001. Caché uses the proleptic Gregorian calendar, which projects the Gregorian calendar back to “Year 1”, in conformance with the ISO 8601 standard. This is, in part, because the Gregorian calendar was adopted at different times in different countries. For example, much of continental Europe adopted it in 1582; Great Britain and the United States adopted it in 1752. Thus Caché dates prior to your local adoption of the Gregorian calendar may not correspond to historical dates that were recorded based on the local calendar then in effect. For further details on dates prior to 1840, refer to the [mindate](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetime#RCOS_fzdatetime_mindate) parameter.

dformat

Format for the returned date. Valid values are:

Value

Meaning

1

MM/DD/\[YY\]YY (07/01/97 or 03/27/2002) — [American numeric format](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_dates). The dateseparator character (/ or .) is taken from the current locale setting.

2

DD Mmm \[YY\]YY (01 Jul 97)

3

YYYY-MM-DD (1997-07-01) — ODBC format. By default this format is independent of your current locale settings, thus specifying dates and times in an ODBC standard interchange format. (The ODBC time format default is described in the tformat section, below.) To use your current date and time locale settings with this format, set localeopt to 0.

4

DD/MM/\[YY\]YY (01/07/97 or 27/03/2002) — [European numeric format](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_dates). The dateseparator character (/ or .) is taken from the current locale setting.

5

Mmm \[D\]D, YYYY (Jul 1, 1997)

6

Mmm \[D\]D YYYY (Jul 1 1997)

7

Mmm DD \[YY\]YY (Jul 01 1997)

8

YYYYMMDD (19970701) — Numeric format

9

Mmmmm \[D\]D, YYYY (July 1, 1997)

10

W (2) — Day number for the week

11

Www (Tue) — Abbreviated day name

12

Wwwwww (Tuesday) — Full day name

13

\[D\]D/\[M\]M/YYYY (1/7/2549 or 27/11/2549) — Thai date format. Day and month are identical to European usage, except no leading zeros. The year is the Buddhist Era year, calculated by adding 543 years to the Gregorian year.

14

nnn (354) — Day number for the year

15

DD/MM/\[YY\]YY (01/07/97 or 27/03/2002) — European format (same as dformat\=4). The dateseparator character (/ or .) is taken from the current locale setting.

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

Get effective dformat value from the [user’s locale](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_dates), fmt.DateFormat, where fmt is an instance of ##class(%SYS.NLS.Format) associated with the current process. This is the default behavior if you do not specify dformat. See “Customizable Date and Time Defaults” for further details.

\-2

$ZDATETIME returns an integer specifying the count of seconds from a platform-specific origin date/time. This is the value returned by the time() library function, as defined in the ISO C Programming Language Standard. For example, on POSIX-compliant systems this value is the count of seconds from January 1, 1970 00:00:00 UTC. Fractional seconds in the input value are permitted, but ignored. The tformat, precision, monthlist, yearopt, startwin, and endwin parameters are ignored. The following platform-specific formats are supported: OpenVMS: unsigned 32-bit integer; 32-bit Linux: signed 32-bit integer; 64-bit Linux: signed 64-bit integer; Windows: unsigned 64-bit integer. Refer to the MultiValue Basic [SYSTEM(99)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RVBS_fsystem) function in Caché MultiValue Basic Reference. (Currently, this date conversion potentially has the “local time variant boundary day” time conversion anomaly described for tformat values 5, 6, 7, and 8.)

\-3

$ZDATETIME takes a datetime value specified in [$HOROLOG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog) internal format, converts that value from local time to UTC Universal time, and returns the resulting value in the same internal format. The tformat, monthlist, yearopt, startwin, and endwin parameters are ignored. $ZDATETIMEH performs the inverse operation. (Currently, this date conversion has the time conversion anomalies described for tformat values 5, 6, 7, and 8. These potentially affect dates prior to 1970, dates after 2038, and local time variant boundary days, such as the beginning date or end date for Daylight Saving Time.)

Where:

Syntax

Meaning

YYYY

YYYY is a four-digit year. \[YY\]YY is a two-digit year if hdatetime falls within the active window for two-digit years; otherwise it is a four-digit year.

MM

Two-digit month: 01 through 12. \[M\]M indicates that the leading zero is omitted for months 1 through 9.

DD

Two-digit day: 01 through 31. \[D\]D indicates that the leading zero is omitted for days 1 through 9.

Mmm

Month abbreviation extracted from the MonthAbbr property of the current locale. An alternate month abbreviation (or name of any length) can be extracted from an optional list specified as the monthlist parameter to $ZDATETIME. The MonthAbbr default values are: “Jan Feb Mar Apr May Jun Jul Aug Sep Oct Nov Dec”

Mmmmm

Full name of the month as specified by the MonthName property of the current locale. The default values are: “January February March ... November December”

W

Number 0-6 indicating the day of the week: Sunday=0, Monday=1, Tuesday=2, etc.

Www

Weekday name abbreviation as specified by the WeekdayAbbr property of the current locale. The default values are: “Sun Mon Tue Wed Thu Fri Sat”

Wwwwww

Weekday full name as specified by the WeekdayName property of the current locale. The default values are: “Sunday Monday Tuesday Wednesday Thursday Friday Saturday”

nnn

Day number for the specified year, always three digits, with leading zeros if necessary. Values are 001 through 365 (or 366 on leap years).

dformat Default

If you omit dformat or set it to -1, the dformat default depends on the localeopt parameter and the NLS DateFormat property:

*   If localeopt\=1 the dformat default is ODBC format. The tformat, monthlist, yearopt, mindate and maxdate parameters are also set to ODBC format. This is the same as setting dformat\=3.
    
*   If localeopt\=0 or is unspecified, the dformat default is taken from NLS DateFormat property. If DateFormat\=3, the dformat default is ODBC format. However, DateFormat=3 does not affect the tformat, monthlist, yearopt, mindate and maxdate parameter defaults, which are as specified in the current NLS locale definition.
    

To determine the default date format for your locale, invoke the [GetFormatItem()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYS.NLS.Format#GetFormatItem) NLS class method:

  WRITE ##class(%SYS.NLS.Format).GetFormatItem("DateFormat")

 

European date format (dformat\=4, DD/MM/YYYY order) is the default for many (but not all) European languages, including British English, French, German, Italian, Spanish, and Portuguese (which use a “/” DateSeparator character), as well as Czech (csyw), Russian (rusw), Slovak (skyw), Slovenian (svnw), and Ukrainian (ukrw) (which use a “.” DateSeparator character). For further details on default date formats for supported locales, refer to [Dates](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_dates) in Using Caché ObjectScript.

dformat Settings

If dformat is 3 (ODBC date format), ODBC format defaults are also used for the monthlist, yearopt, mindate and maxdate parameter defaults. Current locale defaults are ignored.

If dformat is -1, 1, 4, 13, or 15 (numeric date formats), $ZDATETIME uses the value of the DateSeparator property of the current locale as the delimiter between months, days, and the year. When dformat is 3 the ODBC date separator ( “-”) is used. For all other dformat values, a space is used as the date separator. The default value of DateSeparator in English is “/” and all documentation uses this delimiter.

If dformat is 11 or 12 (day names) and localeopt\=0 or is unspecified the day name values come from the current locale properties. If localeopt\=1, day names come from the ODBC locale. To determine the default weekday names and weekday abbreviations for your locale, invoke the following NLS class methods:

  WRITE ##class(%SYS.NLS.Format).GetFormatItem("WeekdayName"),!
  WRITE ##class(%SYS.NLS.Format).GetFormatItem("WeekdayAbbr"),!

 

If dformat is 16 or 17 (Japanese date formats), the returned date format is independent of the locale setting. Japanese-format dates can be returned from any Unicode Caché instance. On an 8-bit Caché instance specifying dformat is 16 or 17 causes a <ERWIDECHAR> error.

If dformat is 18, 19, 20, or 21 (Islamic date formats) and localeopt is unspecified, parameters default to Islamic defaults, rather than current locale defaults. The monthlist parameter defaults to Arabic month names transliterated with Latin characters. The tformat, yearopt, mindate and maxdate parameters default to ODBC defaults. The date separator defaults to the Islamic default (a space), not the ODBC default or the current locale DateSeparator property value. If localeopt\=0 current locale property defaults are used for these parameters. If localeopt\=1 ODBC defaults are used for these parameters.

tformat

A numeric value that specifies the format in which you want to express the time value. Supported values are:

Value

Meaning

\-1

Get the effective tformat value from the TimeFormat property of the current locale, which defaults to a value of 1. This is the default behavior if you do not specify tformat for all dformat values except 3.

1

Express time in the form "hh:mm:ss" (24-hour clock).

2

Express time in the form “hh:mm” (24-hour clock)

3

Express time in the form “hh:mm:ss\[AM/PM\]” (12-hour clock)

4

Express time in the form “hh:mm\[AM/PM\]” (12-hour clock)

5

Express time in the form "hh:mm:ss+/-hh:mm" (24-hour clock). The time is expressed as local time. The plus (+) or minus (–) suffix shows the system-defined offset of local time from Coordinated Universal Time (UTC). A minus sign (-hh:mm) indicates that the local time is earlier (westward) of the Greenwich meridian by the returned offset number of hours and minutes. A plus sign (+hh:mm) indicates that the local time is later (eastward) of the Greenwich meridian by the returned offset number of hours and minutes. Further details are described below.

6

Express time in the form “hh:mm+/-hh:mm” (24-hour clock). The time is expressed as local time. The plus (+) or minus (–) suffix shows the system-defined offset of local time from Coordinated Universal Time (UTC). A minus sign (-hh:mm) indicates that the local time is earlier (westward) of the Greenwich meridian by the returned offset number of hours and minutes. A plus sign (+hh:mm) indicates that the local time is later (eastward) of the Greenwich meridian by the returned offset number of hours and minutes. Further details are described below.

7

Express time in the form "hh:mm:ssZ" (24-hour clock). The “Z” suffix indicates that the time is expressed as Coordinated Universal Time (UTC), rather than as local time.

8

Express time in the form “hh:mmZ” (24-hour clock). The “Z” suffix indicates that the time is expressed as Coordinated Universal Time (UTC), rather than as local time.

9

For MultiValue support. Same as tformat 1 ("hh:mm:ss" 24-hour clock) for numeric values in the range 0 through 86399. Also accepts negative time values and time values greater than 86399, as described below.

10

For MultiValue support. Same as tformat 2 ("hh:mm" 24-hour clock) for numeric values in the range 0 through 86399. Also accepts negative time values and time values greater than 86399, as described below.

If you omit tformat or set it to -1, the tformat default depends on the localeopt parameter and the NLS TimeFormat property:

For all dformat values except 3, 18, 19, 20, and 21 all time formats default to the current locale definition TimeSeparator and DecimalSeparator property values. For dformat\=3 (ODBC date format) and dformat\=18, 19, 20, or 21 (Islamic date formats) the time separator is a colon (:) and the DecimalSeparator is a period (.) regardless of the current locale property values. These defaults can be overridden by setting localeopt.

To determine the default time properties for your locale, invoke the [GetFormatItem()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYS.NLS.Format#GetFormatItem) NLS class method:

  WRITE ##class(%SYS.NLS.Format).GetFormatItem("TimeFormat"),!
  WRITE ##class(%SYS.NLS.Format).GetFormatItem("TimeSeparator"),!
  WRITE ##class(%SYS.NLS.Format).GetFormatItem("DecimalSeparator")

 

For tformat values 1 through 4 (which return local time), the date is separated from the time by a space. For tformat values 5 through 8 the date is separated from the time by the letter “T”.

12–hour Clock (tformat 3 and 4)

In 12-hour clock formats, morning and evening are represented by time suffixes, here shown as AM and PM. To determine the default time suffixes for your locale, invoke the [GetFormatItem()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYS.NLS.Format#GetFormatItem) NLS class method, as follows:

  WRITE ##class(%SYS.NLS.Format).GetFormatItem("AM"),!
  WRITE ##class(%SYS.NLS.Format).GetFormatItem("PM"),!

 

For all dformat values except 3, 18, 19, 20, and 21 the AM and PM properties default to the current locale definition. For dformat\=3 (ODBC date format) and dformat\=18, 19, 20, or 21 (Islamic date formats) the time suffixes are always “AM” and “PM”, regardless of the current locale property values. The AM and PM property defaults are “AM” and “PM” for all locales except the Japanese locale jpww.

By default, Midnight and Noon are represented as “MIDNIGHT” and “NOON” for all locales except Japanese locales (jpnw, jpuw, jpww, zdsw, zdtw, zduw), Portuguese (ptbw), Russian (rusw), and Ukrainian (ukrw).

However, when dformat\=3, $ZDATETIME always uses the ODBC standard values, regardless of the default settings for your locale.

Local Time (tformat 5, 6, 7, and 8)

When tformat is set to 5 or 6, the hdatetime input value is assumed to be local date and time, and is displayed as local date and time. If hdatetime is the current local date and time ($HOROLOG), changing [$ZTIMEZONE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztimezone) will change this current date and time for the current process.

The offset suffix specifies the local time variant setting as a positive or negative offset in hours and minutes from the Greenwich meridian. Note that this local time variant is not necessarily the time zone offset. For example, the Eastern United States time zone is 5 hours west of Greenwich (\-5:00), but the local time variant (Daylight Saving Time) offsets the time zone time by one hour to \-04:00. Setting $ZTIMEZONE changes the current process date and time returned by $ZDATETIME($HOROLOG,1,5), but does not change the system local time variation setting.

Note:

tformat 5 or 6 return the local time variation offset from UTC time. This is neither the local time zone offset, nor is it a comparison of your local time with local time at Greenwich England. The term Greenwich Mean Time (GMT) may be confusing; local time at Greenwich England is the same as UTC during the winter; during the summer it differs from UTC by one hour. This is because a local time variant, known as British Summer Time, is applied.

The following example shows how the tformat 5 value is affected by the operating system’s local time variation setting and by changing the time zone for the current process. (Note that this example checks whether a local time variation boundary occurs during program execution):

LocalDatetimeOffset
  SET dst\=$SYSTEM.Util.IsDST()
  SET local\=$ZDATETIME($HOROLOG,1,5)
  WRITE local," is the local date/time and offset",!!
  SET off\=$PIECE(local,"+",2)
  IF off\="" {SET off\=$PIECE(local,"-",2)
             WRITE "-",off," is local offset",!}
  ELSE {WRITE "+",off," is local offset",!}
  SET tz\=$ZTIMEZONE
  WRITE tz/60," is the local timezone offset, in hours",!!
  IF dst\=1 {WRITE " DST in effect, ",off," offset is not ",tz/60," time zone offset",!}
    ELSEIF dst\=0 {WRITE " DST not in effect, offset ",off,"=timezone ",tz/60,!}
    ELSE {WRITE " DST setting cannot be determined",!}
ChangeTimezoneForCurrentProcess
  SET $ZTIMEZONE\=tz+180
  WRITE !,"changed the process time zone westward 3 hours",!
  WRITE $ZDATETIME($HOROLOG,1,5)," is new local date/time and offset",!
  WRITE "note that time has changed, but offset has not changed"
  SET $ZTIMEZONE\=tz
ConfirmNoDSTBoundary
  SET dst2\=$SYSTEM.Util.IsDST()
  GOTO:dst'=dst2 LocalDatetimeOffset

 

When tformat is set to 7 or 8, the hdatetime input value is assumed to be local date and time. The time is changed to correspond to UTC time (calculated using the local timezone setting). The date returned may also be changed (if necessary) to correspond to this UTC time value. Thus the returned date may differ from the local date.

Note:

Conversions involving dformat values -2 and -3 and tformat values 7 and 8 and the UTC offsets generated by tformat values 5 and 6 have the following platform-dependent anomalies:

*   Conversions from local time to UTC time depend on local time variant boundary behavior, which may differ on different operating system platforms:
    
    When the local clock shifts forwards (“Spring ahead” at the start of [Daylight Saving Time](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog#RCOS_vhorolog_dst)) the local time loses an hour. This “lost” hour is an illegal local time value. Caché $HOROLOG should never return an illegal local time value. However, if a user manually enters this illegal local time value (for example, by setting [$HOROLOG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog)), $ZDATETIME conversion results are undefined and highly platform-dependent.
    
    When the local clock shifts backwards (“Fall back” at the end of Daylight Saving Time) the local time hour is repeated. Within this two-hour period, a Caché time conversion operation cannot determine whether it is being applied to the first occurrence of that local time hour, or the second occurrence of the same hour. $ZDATETIME uses whichever assumption is used by the platform-specific runtime library. Therefore, within this temporal window, different operating system platforms may give different time conversion results.
    
*   Conversions between local time and UTC time must use the local time variant rules in force for the specified year and location. Because these rules are established by local laws, may have changed in the past, and are subject to change in the future, $ZDATETIME conversions depend upon the completeness and accuracy of the rules as encoded by the operating system platform. Predictions for future years must necessarily use the current rules, and these rules may change.
    
*   Conversions between local time and UTC time depend on the date range supported by the operating system platform:
    
    If a date is earlier than the earliest date supported by the platform, Caché uses the standard time offset for 1902–01–01 (if this date is supported by the platform). If the date 1902–01–01 is not supported by the platform, Caché uses the standard time offset for 1970–01–01. Any local time variant offset (such as Daylight Saving Time) is ignored.
    
    If a date is later than the latest date supported by the platform, Caché calculates a corresponding date within the range 2010–01–01 to 2037–12–31 and uses the standard time offset for that corresponding date. This algorithm should provide accurate time offsets for dates up to 2100–02–28, provided there are no future changes to the laws governing date/time observances.
    

Note:

$ZDATETIME has no way to determine if an hdatetime input value is in UTC time or local time. Therefore, do not use tformat values 5, 6, 7, or 8 with an hdatetime that is already in UTC, such as a $ZTIMESTAMP value. If you use the output from a tformat 7 or 8 conversion in an operation that converts the time back to local time be aware that the date may have been changed in the local-to-UTC conversion.

MultiValue Time (tformat 9 and 10)

Time formats 9 and 10 are provided for MultiValue support. They are identical to tformats 1 and 2 for all time values within the allowed range for Caché ObjectScript: 0 through 86399. Within this range they handle fractional seconds in the same manner as other time formats. Like other time formats, -0 is treated as 0.

Time formats 9 and 10 accept negative numeric values and returns a corresponding negative time value. tformat 9 returns a negative time value as "-hh:mm:ss" (24-hour clock). tformat 10 returns a negative time value as "-hh:mm" (24-hour clock). For time format 10, “–59.9” returns “–00:00” and “–60” returns “–00:01”. Negative fractional seconds are always truncated. Therefore, –4.9 returns “–00:00:04”; “–0” and “–0.9” both return the positive value “00:00:00”.

Time formats 9 and 10 accept numeric values greater than 86399, and return the corresponding time. Thus 86400 returns “24:00:00”, and 400000 returns “111:06:40”. Fractional seconds are returned for values in the range 0 through 86399. Fractional seconds are truncated for values of 86400 and greater.

If precision is specified, and a time value outside the Caché range is specified, any specified fractional seconds are truncated and the fractional precision portion is returns as zeros.

precision

An integer value that specifies the number of decimal places of precision in which you want to express the time. That is, if you enter a value of 3 as precision, $ZDATETIME displays the seconds carried out to three decimal places. If you enter a value of 9 as precision, $ZDATETIME displays the seconds carried out to nine decimal places.

Supported values are as follows:

\-1: Get the precision value from the TimePrecision property of the current locale, which defaults to a value of 0. This is the default behavior if you do not specify precision.

A value of n that is greater than or equal to zero (0) results in the expression of time to n decimal places.

Precision is only applicable if the hdatetime format can include a fractional time value ($ZTIMESTAMP format), and if the tformat option selected includes seconds. Trailing zeros are not suppressed. If the precision specified exceeds the precision available on your system, the excess digits of precision are returned as trailing zeros.

To determine the default time precision for your locale, invoke the [GetFormatItem()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYS.NLS.Format#GetFormatItem) NLS class method:

  WRITE ##class(%SYS.NLS.Format).GetFormatItem("TimePrecision")

 

monthlist

An expression that resolves to a string of month names or month name abbreviations, separated by a delimiter character. The names in monthlist replace the default month abbreviation values from the MonthAbbr property or the month name values from the MonthName property of the current locale.

monthlist is valid only if dformat is 2, 5, 6, 7, 9, 18, or 20. If dformat is any other value $ZDATETIME ignores monthlist.

The monthlist string has the following format:

*   The first character of the string is a delimiter character (usually a space). The same delimiter must appear before the first month name and between each month name in monthlist. You can specify any single-character delimiter; this delimiter appears between the month, day, and year portions of the returned date value, which is why a space is usually the preferred character.
    
*   The month names string should contain twelve delimited values, corresponding to January through December. It is possible to specify more or less than twelve month names, but if there is no month name corresponding to the month in hdatetime an <ILLEGAL VALUE> error is generated.
    

If you omit monthlist or specify a monthlist value of -1, $ZDATETIME uses the list of month names defined in the MonthAbbr or MonthName property of the current locale, unless one of the following is true: If localeopt\=1, the monthlist default is the ODBC month list (in English). If localeopt is unspecified and dformat is 18 or 20 (Islamic date formats) the monthlist default is the Islamic month list (Arabic names expressed using Latin letters), ignoring the MonthAbbr or MonthName property value.

To determine the default month names and month abbreviations for your locale, invoke the [GetFormatItem()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYS.NLS.Format#GetFormatItem) NLS class method:

  WRITE ##class(%SYS.NLS.Format).GetFormatItem("MonthName"),!
  WRITE ##class(%SYS.NLS.Format).GetFormatItem("MonthAbbr"),!

 

yearopt

With dformat values 0, 1, 2, 4, 7, or 15, a numeric code that specifies the temporal window in which to display the year as a two-digit value. yearopt can be:

Value

Meaning

\-1

Get effective yearopt value from YearOption property of current locale which defaults to a value of 0. This is the default behavior if you do not specify yearopt.

0

Represent 20th century dates (1900 through 1999) with two-digit years, unless a process-specific sliding window (established via the %DATE utility) is in effect. If such a window is in effect, represent only those dates falling within the sliding window by two-digit years. Represent all dates falling outside the 20th century or outside the process-specific sliding window by four-digit years.

1

Represent 20th century dates with two-digit years and all other dates with four-digit years.

2

Represent all dates with two-digit years.

3

Represent with two-digit years those dates falling within the sliding temporal window defined by startwin and (optionally) endwin. Represent all other dates with four-digit years. When yearopt\=3, startwin and endwin are absolute dates in $HOROLOG format.

4

Represent all dates with four-digit years.

5

Represent with two-digit years all dates falling within the sliding window defined by startwin and (optionally) endwin. Represent all other dates with four-digit years. When yearopt=5, startwin and endwin are relative years.

6

Represent all dates in the current century with two-digit years and all other dates with four-digit years.

To determine the default year option for your locale, invoke the [GetFormatItem()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYS.NLS.Format#GetFormatItem) NLS class method:

  WRITE ##class(%SYS.NLS.Format).GetFormatItem("YearOption")

 

If you omit yearopt or specify a yearopt value of -1, $ZDATETIME uses the YearOption property of the current locale, unless one of the following is true: If localeopt\=1, the yearopt default is the ODBC year option. If localeopt\=0 or is unspecified and dformat is 18, 19, 20, or 21 (Islamic date formats) the yearopt default is the ODBC year option (4-digit years); the YearOption property value is ignored for Islamic dates.

startwin

A numeric value that specifies the start of the sliding window during which dates must be represented with two-digit years. You must supply startwin when you use a yearopt of 3 or 5. startwin is not valid with any other yearopt values.

When yearopt\=3, startwin is an absolute date in $HOROLOG date format that indicates the start date of the sliding window.

When yearopt\=5, startwin is a numeric value that indicates the start year of the sliding window expressed in the number of years before the current year.

endwin

A numeric value that specifies the end of the sliding window during which dates are represented with two-digit years. You may optionally supply endwin when yearopt is 3 or 5. endwin is not valid with any other yearopt values.

When yearopt\=3, endwin is an absolute date in $HOROLOG date format that indicates the end date of the sliding window.

When yearopt\=5, endwin is a numeric value that indicates the end year of the sliding window expressed as the number of years past the current year.

When yearopt\=5, the sliding window always begins on January 1st of the year specified in startwin and ends on December 31st of the year specified in endwin, or of the implied end year (if you omit endwin).

If endwin is omitted (or specified as -1) the effective sliding window will be 100 years long. The endwin value of -1 is a special case that always returns a date value, even when higher and lower endwin values return erropt. For this reason, it is preferable to omit endwin when specifying a 100-year window, and to avoid the use of negative endwin values.

If you supply both startwin and endwin , the sliding window they specify must not have a duration of more than 100 years.

mindate

An expression that specifies the lower limit of the range of valid dates (inclusive). Can be specified as a $HOROLOG integer date count (for example, 1/1/2013 is represented as 62823) or a $HOROLOG string value. You can include or omit the time portion of the $HOROLOG date (for example “62823,43200”), but only the date portion of mindate is parsed. Specifying an hdatetime value earlier than mindate generates a <VALUE OUT OF RANGE> error.

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
  WRITE $ZDATETIME(\-13000,1,,,,,,,\-14974),!!
ResetDateMinimumToDefault
  SET oldmin\=##class(%SYS.NLS.Format).SetFormatItem("DateMinimum",min)
  WRITE "reset DateMinimum value from ",oldmin," to ",min 

 

You may specify mindate with or without maxdate. Specifying a mindate larger than maxdate generates an <ILLEGAL VALUE> error.

ODBC Date Format (dformat 3)

The application of the DateMinimum property is governed by the localeopt setting. When localeopt\=1 (which is the default for dformat\=3) the date minimum is 0, regardless of the current locale setting. Therefore, in ODBC format (dformat\=3) the following can be used to specify a date prior to December 31, 1840:

*   Specify a mindate earlier than the specified date:
    
      WRITE $ZDATETIME(\-30,3,,,,,,,\-365)
    
     
    
*   Specify a DateMinimum property value earlier than the specified date and set localeopt\=0:
    
      DO ##class(%SYS.NLS.Format).SetFormatItem("DateMinimum",\-365)
      WRITE $ZDATETIME(\-30,3,,,,,,,,,,0)
    
     
    

maxdate

An expression that specifies the upper limit of the range of valid dates (inclusive). Can be specified as a $HOROLOG integer date count (for example, 1/1/2100 is represented as 94599) or a $HOROLOG string value. You can include or omit the time portion of the $HOROLOG date (for example “94599,43200”), but only the date portion of maxdate is parsed.

If maxdate is omitted or if specified as -1, the maximum date limit is obtained from the DateMaximum property of the current locale, which defaults to the maximum permissible value for the date portion of $HOROLOG: 2980013 (corresponding to December 31, 9999). However, the application of the DateMaximum property is governed by the localeopt setting. When localeopt\=1 (which is the default for dformat\=3) the date maximum default is the ODBC value (2980013), regardless of the current locale setting. Islamic date formats also take the ODBC default.

Specifying a hdatetime date larger than maxdate generates a <VALUE OUT OF RANGE> error.

Specifying a maxdate larger than 2980013 generates an <ILLEGAL VALUE> error.

You may specify maxdate with or without mindate. Specifying a maxdate smaller than mindate generates an <ILLEGAL VALUE> error.

erropt

Specifying a value for this parameter suppresses errors associated with invalid or out of range hdatetime values. Instead of generating <ILLEGAL VALUE> or <VALUE OUT OF RANGE> errors, the $ZDATETIME function returns the erropt value.

Caché performs standard numeric evaluation on hdatetime, the date part of which must evaluate to an integer within the mindate/maxdate range. By default, date values greater than 2980013 or less than 0 generate a <VALUE OUT OF RANGE> error. Fractional values generate an <ILLEGAL VALUE> error. Non-numeric strings (including the null string) evaluate to 0, and thus return the $HOROLOG initial date: 12/31/1840.

The erropt parameter only suppresses errors generated due to invalid or out of range values of hdatetime. Errors generated due to invalid or out of range values of other parameters will always generate errors whether or not erropt has been supplied. For example, an <ILLEGAL VALUE> error is always generated when $ZDATETIME specifies a sliding window where endwin is earlier than startwin. Similarly, an <ILLEGAL VALUE> error is generated when maxdate is less than mindate.

localeopt

This Boolean parameter specifies either the user’s current locale definition or the ODBC locale definition as the source for defaults for the locale-specified parameters dformat, tformat, monthlist, yearopt, mindate and maxdate:

*   If localeopt\=0, all of these parameters take the current locale definition defaults.
    
*   If localeopt\=1, all of these parameters take the ODBC defaults.
    
*   If localeopt is not specified, the dformat parameter determine the default for these parameters. If dformat\=3, the ODBC defaults are used. If dformat is 18, 19, 20, or 21 the Islamic date and time format defaults are used, regardless of the current locale definition. For all other dformat values, the current locale definition defaults are used. Refer to the [dformat](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetime#RCOS_fzdatetime_dformat) description for further details.
    

The ODBC standard locale cannot be changed; it is used to format date and time strings that are portable between Caché processes that have made different National Language Support (NLS) choices. The ODBC locale date and time definitions are as follows:

*   Date format defaults to 3. Therefore, if dformat is undefined or -1, date format 3 is used.
    
*   Date separator defaults to "/". However, date format defaults to 3, which always uses "-" as the date separator.
    
*   Year option defaults to 4 digits.
    
*   Date minimum and maximum are 0 and 2980013 ($HOROLOG date count).
    
*   English month names, month abbreviations, weekday names, weekday abbreviations, and the words "Noon" and "Midnight" are used.
    
*   Time format defaults to 1. Time separator is ":". Time precision is 0 (no fractional seconds). AM and PM indicators are "AM" and "PM".
    

Examples

The following example displays the current local date and time. It takes the default date and time format for the locale:

  WRITE $ZDATETIME($HOROLOG)

 

The following example displays the current date and time. $ZTIMESTAMP contains the current date and time value as Coordinated Universal Time (UTC) date and time. The dformat parameter specifies ODBC date format, the tformat parameter specifies a 24-hour clock, and the precision parameter specifies 6 digits of fractional second precision:

  WRITE $ZDATETIME($ZTIMESTAMP,3,1,6)

 

This returns the current time stamp date and time, formatted like: 2005-11-25 18:45:16.960000.

The following example shows how a local time can be converted to UTC time, and how the date may also change as a result of this conversion. In most time zones, the time conversion in one of the following $ZDATETIME operations also changes the date:

  SET local \= $ZDATETIME("60219,82824",3,1)
  SET utcwest \= $ZDATETIME("60219,82824",3,7)
  SET utceast \= $ZDATETIME("60219,00024",3,7)
  WRITE !,local,!,utcwest,!,utceast

 

Notes

Invalid Values with $ZDATETIME

You receive a <FUNCTION> error in the following conditions:

*   If you specify an invalid dformat code (an integer value less than -3 or greater than 17, a zero, or a noninteger value)
    
*   If you specify a invalid value for tformat (an integer value less than -1 or greater than 10, a zero, or a noninteger value)
    
*   If you do not specify a startwin value when yearopt is 3 or 5
    

You receive a <ILLEGAL VALUE> error under the following conditions:

*   If you specify an invalid value for a date or time and do not supply an erropt value
    
*   If the given month number is greater than the number of month values in monthlist
    
*   If maxdate is less than mindate
    
*   If endwin is less than startwin
    
*   If startwin and endwin specify a sliding temporal window whose duration is greater than 100 years
    

You receive a <VALUE OUT OF RANGE> error under the following conditions:

*   If you specify an otherwise valid date which is outside the range defined by the values assumed for maxdate and mindate and do not supply an erropt value
    

Customizable Date and Time Defaults

Upon Caché startup, the default date and time formats are initialized to the American date and time formats (for example, MM/DD/\[YY\]YY). To set this and other default formats to the values for your current locale, set the following global variable: SET ^SYS("NLS","Config","LocaleFormat")=1. This sets all format defaults for all processes to your current locale values. These defaults persist until this global is changed.

Note:

This section describes the user locale definitions applied when localeopt is undefined or set to 0. When localeopt\=1, $ZDATETIME uses a predefined ODBC locale.

In the following example, the first $ZDATETIME returns a date and time in the default format for the locale. The input parameters are the $ZTIMESTAMP special variable, with the dformat and tformat taking defaults, and precision set to 2 decimal digits. In most locales, the first $ZDATETIME will return dformat\=1 or the American date and time format with a slash date separator and a dot decimal separator for fractional seconds.

In the ChangeVals section, the first [SetFormatItem()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYS.NLS.Format#SetFormatItem) method changes the locale date format default to dformat\=4, or the European date format (DD/MM/\[YY\]YY), as is shown by the second $ZDATETIME. The second SetFormatItem() method changes the locale default for the date separator character (which affects the dformat –1, 1, 4, and 15). In this example, the date separator character is set to a dot (“.”), as shown by the third $ZDATETIME. The third SetFormatItem() method changes the decimal separator character for this locale to the European standard (“,”), as shown by the final $ZDATETIME. This program then restores the initial date format values:

InitalizeLocaleFormat
   SET ^SYS("NLS","Config","LocaleFormat")\=1
InitialVals
   SET fmt\=##class(%SYS.NLS.Format).GetFormatItem("DateFormat")
   SET sep\=##class(%SYS.NLS.Format).GetFormatItem("DateSeparator")
   SET dml\=##class(%SYS.NLS.Format).GetFormatItem("DecimalSeparator")
   WRITE !,$ZDATETIME($ZTIMESTAMP,,,2)
ChangeVals
  SET x\=##class(%SYS.NLS.Format).SetFormatItem("DateFormat",4)
  WRITE !,$ZDATETIME($ZTIMESTAMP,,,2)
  SET y\=##class(%SYS.NLS.Format).SetFormatItem("DateSeparator",".")
  WRITE !,$ZDATETIME($ZTIMESTAMP,,,2)
  SET z\=##class(%SYS.NLS.Format).SetFormatItem("DecimalSeparator",",")
  WRITE !,$ZDATETIME($ZTIMESTAMP,,,2)
RestoreVals
   SET x\=##class(%SYS.NLS.Format).SetFormatItem("DateFormat",fmt)
   SET y\=##class(%SYS.NLS.Format).SetFormatItem("DateSeparator",sep)
   SET z\=##class(%SYS.NLS.Format).SetFormatItem("DecimalSeparator",dml)
   WRITE !,$ZDATETIME($ZTIMESTAMP,,,2)

 

$ZDATETIME Compared to $ZDATE

$ZDATETIME is similar to $ZDATE except it converts a combined date and time value. $ZDATE only converts a date value. For example:

  WRITE $ZDATE($HOROLOG)

 

returns the current date, formatted like: 03/25/2011.

  WRITE $ZDATETIME($HOROLOG)

 

returns the current date and time, formatted like: 03/25/2011 13:53:57.

$ZDATE does not support tformat values 5 through 8.

See Also

*   [JOB](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cjob) command
    
*   [$ZDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdate) function
    
*   [$ZDATEH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdateh) function
    
*   [$ZDATETIMEH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetimeh) function
    
*   [$ZTIME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fztime) function
    
*   [$HOROLOG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog) special variable
    
*   [$ZTIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztimestamp) special variable
    
*   %DATE utility, which is documented in the “[Legacy Documentation](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GDOC_legacy)” chapter in Using InterSystems Documentation
    
*   More information on locales in the section on “[System Classes for National Language Support](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSTU_customize#GSTU_customize_nls)” in Caché Specialized System Tools and Utilities.
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdateh "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetimeh "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fzdatetime.xml**