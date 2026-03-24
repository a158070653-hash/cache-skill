# $ZDATE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fzdate

---

 

Caché ObjectScript Reference

$ZDATE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzcsc "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdateh "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Additional Math and Time Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_MATHFUNCTIONS "Additional Math and Time Functions") \]  >  \[ [$ZDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdate "$ZDATE") \]

Go to: Description Parameters Examples Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Validates a date and converts it from internal format to the specified display format.

Synopsis

$ZDATE(hdate,dformat,monthlist,yearopt,startwin,endwin,mindate,maxdate,erropt,localeopt)
$ZD(hdate,dformat,monthlist,yearopt,startwin,endwin,mindate,maxdate,erropt,localeopt)

Parameters

hdate

An integer specifying an internal date format value. This integer represents the number of days elapsed since December 31, 1840. If $HOROLOG is specified for hdate, only the date portion of $HOROLOG is used. See [hdate](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdate#RCOS_fzdate_hdate) below.

dformat

Optional — An integer code specifying the format for the returned date. See [dformat](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdate#RCOS_fzdate_dformat) below.

monthlist

Optional — A string or the name of a variable that specifies a set of month names. This string must begin with a delimiter character, and its 12 entries must be separated by this delimiter character. See [monthlist](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdate#RCOS_fzdate_monthlist) below.

yearopt

Optional — An integer code that specifies whether to represent years as two- or four-digit values. See [yearopt](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdate#RCOS_fzdate_yearopt) below.

startwin

Optional — The start of the sliding window during which dates must be represented with two-digit years. See [startwin](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdate#RCOS_fzdate_startwin) below.

endwin

Optional — The end of the sliding window during which dates are represented with two-digit years. See [endwin](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdate#RCOS_fzdate_endwin) below.

mindate

Optional — The lower limit of the range of valid dates. Specified as a $HOROLOG integer date count, with 0 representing December 31, 1840. Can be specified as a positive or negative integer. See [mindate](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdate#RCOS_fzdate_mindate) below.

maxdate

Optional — The upper limit of the range of valid dates. Specified as a $HOROLOG integer date count. See [maxdate](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdate#RCOS_fzdate_maxdate) below.

erropt

Optional — An expression to return when hdate is invalid. Specifying a value for this parameter suppresses error codes associated with invalid or out of range hdate values. Instead of issuing an error message, $ZDATE returns erropt. See [erropt](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdate#RCOS_fzdate_erropt) below.

localeopt

Optional — A boolean flag that specifies which locale to use for the dformat, monthlist, yearopt, mindate and maxdate default values, and other date characteristics, such as the date separator character:

localeopt\=0: the current locale property settings determine these parameter defaults.

localeopt\=1: the ODBC standard locale determines these parameter defaults.

localeopt not specified: the dformat value determines these parameter defaults. If dformat\=3, ODBC defaults are used. Japanese and Islamic date dformatvalues use their own defaults. For all other dformat values, current locale property settings are used as defaults. See [localeopt](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdate#RCOS_fzdate_localeopt) below.

Omitted parameters between specified parameter values are indicated by placeholder commas. Trailing placeholder commas are not required, but are permitted. Blank spaces are permitted between the commas that indicate an omitted parameter.

Description

The $ZDATE function converts a specified date in internal storage format ($HOROLOG format) to one of several alternate date display formats. The value returned by $ZDATE depends on the parameters you use.

Simple $ZDATE format

$ZDATE(hdate), the most basic form of $ZDATE, returns the date in a display format that corresponds to the specified hdate. hdate is an integer count of the number of days elapsed since December 31, 1840. It can range from 0 to 2980013 (12/31/1840 to 12/31/9999).

By default, $ZDATE(hdate) represents years between 1900 and 1999 with two digits. It represents years that fall before 1900 or after 1999 with four digits. For example:

  WRITE $ZDATE(21400),!  ; returns 08/04/1899
  WRITE $ZDATE(50000),!  ; returns 11/23/77
  WRITE $ZDATE(60000),!  ; returns 04/10/2005
  WRITE $ZDATE(0),!      ; returns 12/31/1840

 

When you supply a $HOROLOG date to $ZDATE, only the date portion is used. In $HOROLOG format, date and time are presented as two integers separated by a comma. Upon encountering the comma (a non-numeric character) $ZDATE ignores the rest of the string. In the following example, $ZDATE returns 04/10/2005 and the current date using $HOROLOG format values:

   WRITE !,$ZDATE("60000,12345")
   WRITE !,$ZDATE($HOROLOG)

 

Customizable Date Default

Upon Caché startup, the default date format is initialized to dformat\=1, which is the American date format with a slash date separator (MM/DD/\[YY\]YY). To set this and other default formats to the values for your current locale, set the following global variable: SET ^SYS("NLS","Config","LocaleFormat")=1. This sets all format defaults for all processes to your current locale values. These defaults persist until this global is changed.

Note:

This section describes the user locale definitions applied when localeopt is undefined or set to 0. When localeopt\=1, $ZDATE uses a predefined ODBC locale.

You can use NLS (National Language Support) to override format defaults for the current process. You can either change the all format defaults to the values for a specified locale, or change individual format values.

*   To set all of the format defaults (including the date format default) to the properties of a specified locale, invoke the following method call: SET fmt=##class(%SYS.NLS.Format).%New("lname"), where lname is the NLS name of the desired locale. (For example, deuw=German, espw=Spanish, fraw=French, ptbw=Brazilian Portuguese, rusw=Russian, jpnw=Japanese. A complete list of locales is found in the Management Portal: \[Home\] > \[Configuration\] > \[NLS Settings\] > \[Locale Definitions\]) To set these defaults to the properties of the current locale, specify a lname of "current", or the empty string ("").
    
*   To set the default date format to a specified dformat format, invoke the [SetFormatItem()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYS.NLS.Format#SetFormatItem) method: SET rtn=##class(%SYS.NLS.Format).SetFormatItem("DateFormat",n), where n is the number of the dformat value you wish to make the default.
    

The following example demonstrates setting all format defaults to the Russian locale, returning a date from $ZDATE in the default format (Russian), then resetting the format defaults to the current locale defaults. Note that the Russian locale uses a period, rather than a slash as the date part separator:

   IF $SYSTEM.Version.IsUnicode() {
   WRITE !,$ZDATE($HOROLOG)
   SET fmt\=##class(%SYS.NLS.Format).%New("rusw")
   WRITE !,$ZDATE($HOROLOG)
   SET fmt\=##class(%SYS.NLS.Format).%New("current")
   WRITE !,$ZDATE($HOROLOG)
   }
   ELSE {WRITE "This example requires a Unicode installation of Caché"}

 

The following example demonstrates setting individual format defaults. The first $ZDATE returns a date in the default format. The first [SetFormatItem()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYS.NLS.Format#SetFormatItem) method changes the default to dformat\=4, or the European date format (DD/MM/\[YY\]YY), as is shown by the second $ZDATE. The second SetFormatItem() method changes the default for the date separator character (which affects the dformat -1, 1, 4, and 15). In this example, the date separator character is set to a dot (“.”), as is shown by the third $ZDATE. Finally, this program restores the original date format values:

InitialVals
   SET fmt\=##class(%SYS.NLS.Format).GetFormatItem("DateFormat")
   SET sep\=##class(%SYS.NLS.Format).GetFormatItem("DateSeparator")
   WRITE !,$ZDATE($HOROLOG)
ChangeVals
   SET x\=##class(%SYS.NLS.Format).SetFormatItem("DateFormat",4)
   WRITE !,$ZDATE($HOROLOG)
   SET y\=##class(%SYS.NLS.Format).SetFormatItem("DateSeparator",".")
   WRITE !,$ZDATE($HOROLOG)
RestoreVals
   SET x\=##class(%SYS.NLS.Format).SetFormatItem("DateFormat",fmt)
   SET y\=##class(%SYS.NLS.Format).SetFormatItem("DateSeparator",sep)
   WRITE !,$ZDATE($HOROLOG)

 

For further details on default date formats for supported locales, refer to [Dates](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_dates) in Using Caché ObjectScript.

Parameters

hdate

The internal date format value representing the number of days elapsed since December 31, 1840. By default, it must be an integer in the range 0 through 2980013. You can specify hdate as a numeric or string literal or as an expression.

By default, the earliest valid hdate is 0 (December 31, 1840). Dates are limited to positive integers by default because the DateMinimum property defaults to 0. You can specify earlier dates as negative integers, provided the DateMinimum property of the current locale is set to a greater or equal negative integer. The lowest valid DateMinimum value is -672045, which corresponds to January 1, 0001. Caché uses the proleptic Gregorian calendar, which projects the Gregorian calendar back to “Year 1”, in conformance with the ISO 8601 standard. This is, in part, because the Gregorian calendar was adopted at different times in different countries. For example, much of continental Europe adopted it in 1582; Great Britain and the United States adopted it in 1752. Thus Caché dates prior to your local adoption of the Gregorian calendar may not correspond to historical dates that were recorded based on the local calendar then in effect. For further details on dates prior to 1840, refer to the [mindate](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdate#RCOS_fzdate_mindate) parameter.

dformat

Format for the returned date. Valid values are:

Value

Meaning

1

MM/DD/\[YY\]YY (07/01/97 or 03/27/2002) — [American numeric format](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_dates). The dateseparator character (/ or .) is taken from the current locale setting.

2

DD Mmm \[ YY \]YY (01 Jul 97 or 27 Mar 2002)

3

YYYY-MM-DD (1997-07-01 or 2002-03-27) — ODBC format. By default this format is independent of your current locale settings (localeopt\=1), thus specifying dates in an ODBC standard interchange format. To use your current date locale settings with this format, set localeopt\=0.

4

DD/MM/\[YY\]YY (01/07/97 or 27/03/2002) — [European numeric format](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_dates). The dateseparator character (/ or .) is taken from the current locale setting.

5

Mmm \[D\]D, YYYY (Jul 1, 1997 or Mar 27, 2002)

6

Mmm \[D\]D YYYY (Jul 1 1997 or Mar 27 2002)

7

Mmm DD \[YY\]YY (Jul 01 97 or Mar 27 2002)

8

YYYYMMDD (19970701 or 20020327) — Numeric format

9

Mmmmm \[D\]D, YYYY (July 1, 1997 or March 27, 2002)

10

W (2) — Day number for the week. By default, Monday = 1.

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

Get effective dformat value either from the [user’s locale](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_dates) (if localeopt\=0 or undefined), or from the ODBC locale (which defaults dformat\=3). If dformat is taken from the user’s locale, it is the value of fmt.DateFormat, where fmt is an instance of ##class(%SYS.NLS.Format) associated with the current process. This is the default behavior if you do not specify dformat. See “[Customizable Date Default](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdate#RCOS_fzdate_default)” for further details.

where:

Syntax

Meaning

YYYY

YYYY is a four-digit year. \[YY\]YY is a two-digit year if hdate falls within the active window for two-digit years; otherwise it is a four-digit year.

MM

Two-digit month: 01 through 12. \[M\]M indicates that the leading zero is omitted for months 1 through 9.

DD

Two-digit day: 01 through 31. \[D\]D indicates that the leading zero is omitted for days 1 through 9.

Mmm

Month abbreviation extracted from the MonthAbbr property of the current locale. An alternate month abbreviation (or name of any length) can be extracted from an optional list specified as the monthlist parameter to $ZDATE. The default MonthAbbr values are:

“Jan Feb Mar Apr May Jun Jul Aug Sep Oct Nov Dec”

Mmmmm

Full name of the month as specified by the MonthName property of the current locale. The default values are:

“January February March ... November December”

W

Number 0-6 indicating the day of the week:

Sunday=0, Monday=1, Tuesday=2, etc.

Www

Weekday name abbreviation as specified by the WeekdayAbbr property of the current locale. The default values are:

“Sun Mon Tue Wed Thu Fri Sat”

Wwwwww

Weekday full name as specified by the WeekdayName property of the current locale. The default values are:

“Sunday Monday Tuesday ... Friday Saturday”

nnn

Day number for the specified year, always three digits, with leading zeros if necessary. Values are 001 through 365 (or 366 on leap years).

dformat Default

If you omit dformat or set it to -1, the dformat default depends on the localeopt parameter and the NLS DateFormat property:

*   If localeopt\=1 the dformat default is ODBC format. The monthlist, yearopt, mindate and maxdate parameter defaults are also set to ODBC format. This is the same as setting dformat\=3.
    
*   If localeopt\=0 or is unspecified, the dformat default is taken from the NLS DateFormat property. If DateFormat\=3, the dformat default is ODBC format. However, DateFormat=3 does not affect the monthlist, yearopt, mindate and maxdate parameter defaults, which are as specified in the current NLS locale definition.
    

To determine the default date format for your locale, invoke the [GetFormatItem()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYS.NLS.Format#GetFormatItem) NLS class method:

  WRITE ##class(%SYS.NLS.Format).GetFormatItem("DateFormat")

 

European date format (dformat\=4, DD/MM/YYYY order) is the default for many (but not all) European languages, including British English, French, German, Italian, Spanish, and Portuguese (which use a “/” DateSeparator character), as well as Czech (csyw), Russian (rusw), Slovak (skyw), Slovenian (svnw), and Ukrainian (ukrw) (which use a “.” DateSeparator character). For further details on default date formats for supported locales, refer to [Dates](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_dates) in Using Caché ObjectScript.

dformat Settings

If dformat is 3 (ODBC date format), ODBC format defaults are also used for the monthlist, yearopt, mindate and maxdate parameter defaults. Current locale defaults are ignored.

If dformat is -1, 1, 4, 13, or 15 (numeric date formats), $ZDATE uses the value of the DateSeparator property of the current locale as the delimiter between months, days, and the year. When dformat is 3 the ODBC date separator ( “-”) is used. For all other dformat values, a space is used as the date separator. The default value of DateSeparator in English is “/” and all documentation uses this delimiter.

If dformat is 11 or 12 (day names) and localeopt\=0 or is unspecified the day name values come from the current locale properties. If localeopt\=1, day names come from the ODBC locale. To determine the default weekday names and weekday abbreviations for your locale, invoke the following NLS class methods:

  WRITE ##class(%SYS.NLS.Format).GetFormatItem("WeekdayName"),!
  WRITE ##class(%SYS.NLS.Format).GetFormatItem("WeekdayAbbr"),!

 

If dformat is 16 or 17 (Japanese date formats), the returned date format is independent of the locale setting. Japanese-format dates can be returned from any Unicode Caché instance. On an 8-bit Caché instance specifying dformat is 16 or 17 causes a <ERWIDECHAR> error.

If dformat is 18, 19, 20, or 21 (Islamic date formats) and localeopt is unspecified, parameters default to Islamic defaults, rather than current locale defaults. The monthlist parameter defaults to Arabic month names transliterated with Latin characters. The tformat, yearopt, mindate and maxdate parameters default to ODBC defaults. The date separator defaults to the Islamic default (a space), not the ODBC default or the current locale DateSeparator property value. If localeopt\=0 current locale property defaults are used for these parameters. If localeopt\=1 ODBC defaults are used for these parameters.

monthlist

An expression that resolves to a string of month names or month name abbreviations, separated by a delimiter character. The names in monthlist replace the default month abbreviation values from the MonthAbbr property or the month name values from the MonthName property of the current locale.

monthlist is valid only if dformat is 2, 5, 6, 7, 9, 18, or 20. If dformat is any other value $ZDATE ignores monthlist.

The monthlist string has the following format:

*   The first character of the string is a delimiter character (usually a space). The same delimiter must appear before the first month name and between each month name in monthlist. You can specify any single-character delimiter; this delimiter appears between the month, day, and year portions of the returned date value, which is why a space is usually the preferred character.
    
*   The month names string should contain twelve delimited values, corresponding to January through December. It is possible to specify more or less than twelve month names, but if there is no month name corresponding to the month in hdate an <ILLEGAL VALUE> error is generated.
    

If you omit monthlist or specify a monthlist value of -1, $ZDATE uses the list of month names defined in the MonthAbbr or MonthName property of the current locale, unless one of the following is true: If localeopt\=1, the monthlist default is the ODBC month list (in English). If localeopt is unspecified and dformat is 18 or 20 (Islamic date formats) the monthlist default is the Islamic month list (Arabic names expressed using Latin letters), ignoring the MonthAbbr or MonthName property value.

To determine the default month names and month abbreviations for your locale, invoke the [GetFormatItem()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYS.NLS.Format#GetFormatItem) NLS class method:

  WRITE ##class(%SYS.NLS.Format).GetFormatItem("MonthName"),!
  WRITE ##class(%SYS.NLS.Format).GetFormatItem("MonthAbbr"),!

 

The following example lists the month names the default locale, changes the locale for this process to the Russian locale, then lists the Russian month names and displays the current date with a Russian month name. It then reverts the locale defaults to current locale and again displays the current date, this time with the default month name.

   IF $SYSTEM.Version.IsUnicode() {
   WRITE ##class(%SYS.NLS.Format).GetFormatItem("MonthName"),!
   SET fmt\=##class(%SYS.NLS.Format).%New("rusw")
   WRITE fmt.MonthName,!
   WRITE $ZDATE($HOROLOG,9),!
   SET fmt\=##class(%SYS.NLS.Format).%New()
   WRITE $ZDATE($HOROLOG,9)
   }
   ELSE {WRITE "This example requires a Unicode installation of Caché"}

 

yearopt

With dformat values 1, 2, 4, 7, or 15, an integer code that specifies the temporal window in which to display the year as a two-digit value. For all other dformat values, the yearopt is ignored. Valid yearopt values are:

Value

Meaning

\-1

Get effective yearopt value from YearOption property of current locale which defaults to a value of 0. This is the default behavior if you do not specify yearopt.

0

Represent 20th century dates (1900 through 1999) with two-digit years and all other dates with four-digit years, unless a process-specific sliding window (established via the %DATE utility) is in effect. If such a window is in effect, represent only those dates falling within the sliding window by two-digit years, and all other dates with four-digit years.

1

Represent 20th century dates with two-digit years and all other dates with four-digit years.

2

Represent all dates with two-digit years.

3

Represent with two-digit years those dates falling within the sliding temporal window defined by startwin and (optionally) endwin. Represent all other dates with four-digit years. When yearopt =3, startwin and endwin are absolute dates in $HOROLOG format.

4

Represent all dates with four-digit years. ODBC year option.

5

Represent with two-digit years all dates falling within the sliding temporal window defined by startwin and (optionally) endwin. Represent all other dates with four-digit years. When yearopt=5, startwin and endwin are relative years.

6

Represent all dates in the current century with two-digit years and all other dates with four-digit years.

To determine the default year option for your locale, invoke the [GetFormatItem()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYS.NLS.Format#GetFormatItem) NLS class method:

  WRITE ##class(%SYS.NLS.Format).GetFormatItem("YearOption")

 

If you omit yearopt or specify a yearopt value of -1, $ZDATE uses the YearOption property of the current locale, unless one of the following is true: If localeopt\=1, the yearopt default is the ODBC year option. If localeopt\=0 or is unspecified and dformat is 18, 19, 20, or 21 (Islamic date formats) the yearopt default is the ODBC year option (4-digit years); the YearOption property value is ignored for Islamic dates.

startwin

A numeric value that specifies the start of the sliding window during which dates must be represented with two-digit years. See parameter section. You must supply startwin when yearopt is 3 or 5. startwin is not valid with any other yearopt values.

When yearopt = 3, startwin is an absolute date in $HOROLOG date format that indicates the start date of the sliding window.

When yearopt = 5, startwin is a numeric value that indicates the start year of the sliding window expressed as the number of years before the current year. The sliding window always begins on January 1st of the year specified in startwin.

endwin

A numeric value that specifies the end of the sliding window during which dates are represented with two-digit years. You may optionally supply endwin when yearopt is 3 or 5. endwin is not valid with any other yearopt values.

When yearopt = 3, endwin is an absolute date in $HOROLOG date format that indicates the end date of the sliding window.

When yearopt = 5, endwin is a numeric value that indicates the end year of the sliding window expressed as the number of years past the current year. The sliding window always ends on December 31st of the year specified in endwin. If endwin is not specified, it defaults to December 31st of the year 100 years after startwin.

If endwin is omitted (or specified as -1) the effective sliding window will be 100 years long. The endwin value of -1 is a special case that always returns a date value, even when higher and lower endwin values return erropt. For this reason, it is preferable to omit endwin when specifying a 100-year window, and to avoid the use of negative endwin values.

If you supply both startwin and endwin, the sliding window they specify must not have a duration of more than 100 years.

mindate

An expression that specifies the lower limit of the range of valid dates (inclusive). Can be specified as a $HOROLOG integer date count (for example, 1/1/2013 is represented as 62823) or a $HOROLOG string value. You can include or omit the time portion of a $HOROLOG date string (for example “62823,43200”), but only the date portion of mindate is parsed. Specifying an hdate value earlier than mindate generates a <VALUE OUT OF RANGE> error.

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
  WRITE $ZDATE(\-13000,1,,,,,\-14974),!!
ResetDateMinimumToDefault
  SET oldmin\=##class(%SYS.NLS.Format).SetFormatItem("DateMinimum",min)
  WRITE "reset DateMinimum value from ",oldmin," to ",min 

 

You may specify mindate with or without maxdate. Specifying a mindate larger than maxdate generates an <ILLEGAL VALUE> error.

ODBC Date Format (dformat 3)

The application of the DateMinimum property is governed by the localeopt setting. When localeopt\=1 (which is the default for dformat\=3) the date minimum is 0, regardless of the current locale setting. Therefore, in ODBC format (dformat\=3) the following can be used to specify a date prior to December 31, 1840:

*   Specify a mindate earlier than the specified date:
    
      WRITE $ZDATE(\-30,3,,,,,\-365)
    
     
    
*   Specify a DateMinimum property value earlier than the specified date and set localeopt\=0:
    
      DO ##class(%SYS.NLS.Format).SetFormatItem("DateMinimum",\-365)
      WRITE $ZDATE(\-30,3,,,,,,,,0)
    
     
    

maxdate

An expression that specifies the upper limit of the range of valid dates (inclusive). Can be specified as a $HOROLOG integer date count (for example, 1/1/2100 is represented as 94599) or a $HOROLOG string value. You can include or omit the time portion of the $HOROLOG date (for example “94599,43200”), but only the date portion of maxdate is parsed.

If maxdate is omitted or if specified as -1, the maximum date limit is obtained from the DateMaximum property of the current locale, which defaults to the maximum permissible value for the date portion of $HOROLOG: 2980013 (corresponding to December 31, 9999). However, the application of the DateMaximum property is governed by the localeopt setting. When localeopt\=1 (which is the default for dformat\=3) the date maximum default is the ODBC value (2980013), regardless of the current locale setting. Islamic date formats also take the ODBC default.

Specifying a hdate larger than maxdate generates a <VALUE OUT OF RANGE> error.

Specifying a maxdate larger than 2980013 generates an <ILLEGAL VALUE> error.

You may specify maxdate with or without mindate. Specifying a maxdate smaller than mindate generates an <ILLEGAL VALUE> error.

erropt

Specifying a value for this parameter suppresses errors associated with invalid or out of range hdate values. Instead of generating <ILLEGAL VALUE> or <VALUE OUT OF RANGE> errors, the $ZDATE function returns the erropt value.

Caché performs standard numeric evaluation on hdate, which must evaluate to an integer within the mindate/maxdate range. Thus, 7, "7", +7, 0007, 7.0, "7 dwarves", and --7 all evaluate to the same date value: 01/07/1841. By default, values greater than 2980013 or less than 0 generate a <VALUE OUT OF RANGE> error. Fractional values generate an <ILLEGAL VALUE> error. Non-numeric strings (including the null string) evaluate to 0, and thus return the $HOROLOG initial date: 12/31/1840.

The erropt parameter only suppresses errors generated due to invalid or out of range values of hdate. Errors generated due to invalid or out of range values of other parameters will always generate errors whether or not erropt has been supplied. For example, an <ILLEGAL VALUE> error is always generated when $ZDATE specifies a sliding window where endwin is earlier than startwin. Similarly, an <ILLEGAL VALUE> error is generated when maxdate is less than mindate.

Invalid Date Handling with ZDateNull

The behavior of $ZDATE when given an invalid value for hdate can be set using ZDateNull. To set this behavior for the current process, use the [ZDateNull()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process#ZDateNull) method of the [%SYSTEM.Process](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process) class. The system-wide default behavior can be established by setting the [ZDateNull](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=Config.Miscellaneous#ZDateNull) property of the Config.Miscellaneous class. $ZDATE can either issue an error, or return a null value.

The system-wide default behavior is configurable. Go to the Management Portal, select \[Home\] > \[Configuration\] > \[Compatibility Settings\]. View and edit the current setting of ZDateNull. The default is “false”, meaning that $ZDATE returns an error.

localeopt

This Boolean parameter specifies either the user’s current locale definition or the ODBC locale definition as the source for defaults for the locale-specified parameters dformat, monthlist, yearopt, mindate and maxdate:

*   If localeopt\=0, all of these parameters take the current locale definition defaults.
    
*   If localeopt\=1, all of these parameters take the ODBC defaults.
    
*   If localeopt is not specified, the dformat parameter determine the default for these parameters. If dformat\=3, the ODBC defaults are used. If dformat is 18, 19, 20, or 21 the Islamic date format defaults are used, regardless of the current locale definition. For all other dformat values, the current locale definition defaults are used. Refer to the [dformat](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdate#RCOS_fzdate_dformat) description for further details.
    

The ODBC locale cannot be changed; it is used to format date strings that are portable between Caché processes that have made different National Language Support (NLS) choices. If localeopt\=1, the ODBC locale date definitions are as follows:

*   Date format defaults to 3. Therefore, if dformat is undefined or -1, date format 3 is used.
    
*   Date separator defaults to "/". However, date format defaults to 3, which always uses "-" as the date separator.
    
*   Year option defaults to 4 digits.
    
*   Date minimum and maximum: 0 and 2980013 ($HOROLOG date count).
    
*   English month names, month abbreviations, weekday names, and weekday abbreviations are used.
    

Examples

Date Format Examples

The following example illustrates how $ZDATE returns the various dformat formats for the current date. The yearopt takes the default values. The date separator, and the names and abbreviations of months and days of the week are, of course, locale-dependent. This example uses the current user locale definition:

   WRITE $ZDATE($HOROLOG),  "   default date format",!
   WRITE $ZDATE($HOROLOG,1),"   1=American numeric format",!
   WRITE $ZDATE($HOROLOG,2),"   2=Month abbreviation format",!
   WRITE $ZDATE($HOROLOG,3),"   3=ODBC numeric format",!
   WRITE $ZDATE($HOROLOG,4),"   4=European numeric format",!
   WRITE $ZDATE($HOROLOG,5),"   5=Month abbreviation format",!
   WRITE $ZDATE($HOROLOG,6),"   6=Month abbreviation format",!
   WRITE $ZDATE($HOROLOG,7),"   7=Month abbreviation format",!
   WRITE $ZDATE($HOROLOG,8),"   8=Numeric format no spaces",!
   WRITE $ZDATE($HOROLOG,9),"   9=Month name format",!
   WRITE $ZDATE($HOROLOG,10),"  10=Day-of-week format",!
   WRITE $ZDATE($HOROLOG,11),"  11=Day abbreviation format",!
   WRITE $ZDATE($HOROLOG,12),"  12=Day name format",!
   WRITE $ZDATE($HOROLOG,13),"  13=Thai numeric format",!
   WRITE $ZDATE($HOROLOG,14),"  14=Day-of-year format",!
   WRITE $ZDATE($HOROLOG,15),"  15=European numeric format",!
   WRITE $ZDATE($HOROLOG,16),"  16=Japanese date format",!
   WRITE $ZDATE($HOROLOG,17),"  17=Japanese date format with spaces"

 

The following example compares dates with the locale defaulting to the current user locale with dates when localeopt\=1 activates the ODBC locale definition. To make this example more interesting, the current user locale is set to French:

  SET fmt\=##class(%SYS.NLS.Format).%New("fraw")
  WRITE "default: local=",$ZDATE($HOROLOG),"   ODBC=",$ZDATE($HOROLOG,,,,,,,,,1),!
  WRITE "-1:      local=",$ZDATE($HOROLOG,\-1),"   ODBC=",$ZDATE($HOROLOG,\-1,,,,,,,,1),!!
  FOR x\=1:1:17 {
    WRITE x,": local=",$ZDATE($HOROLOG,x),"   ODBC=",$ZDATE($HOROLOG,x,,,,,,,,1),! }

 

Two-digit Year Sliding Window Example

To illustrate how to use an explicit sliding window, suppose you enter the following function call in 1997. The hdate of 59461 represents October 19, 2003; the dformat of 1 allows it to return two-digit or four-digit years, and the yearopt of 5 specifies a sliding window for four-digit years. Because of the yearopt setting, the startwin and endwin are calculated relative to the current year (in this case 1997) by addition and subtraction.

   WRITE $ZDATE(59461,1,,5,90,10)

 

The sliding window for displaying the year as two digits extends from 1/1/1907 to 12/31/2006. Thus Caché displays the date as 10/19/03.

Date Range Example

The following example uses mindate and maxdate to test for plausible birth dates. The maxdate assumes that a birth date cannot be in the future; the mindate assumes that no person listed will be more that 124 years old. The dates are specified in $HOROLOG format:

PlausibleBirthdate
   SET bdateh(1)\=62142
   SET bdateh(2)\=16800
   SET bdateh(3)\=70000
   DO $SYSTEM.Process.ZDateNull(1)
   SET maxdate\=$PIECE($HOROLOG,",",1)+1
   SET mindate\=maxdate\-(365.25\*124)
   FOR x\=1:1:3 {
      SET bdate\=$ZDATE(bdateh(x),,,,,,mindate,maxdate)
      IF bdate\="" {WRITE "Birth date ",bdateh(x)," is out of range",!}
      ELSE {WRITE "Birth date ",bdateh(x)," is ",bdate,!}
   }

 

Two of the above $ZDATE input values fall outside of the date range for a birth date test: 16800 (12/30/1886) is more than 124 years ago and 70000 (08/26/2032) is in the future. By default, these invocations of $ZDATE would generate a <VALUE OUT OF RANGE> error, but because ZDateNull(1) is set, they return the empty string ("").

Notes

Invalid Values with $ZDATE

You receive a <FUNCTION> error in the following conditions:

*   If you specify an invalid dformat code (an integer value less than -1 or greater than 17, or a non-integer value).
    
*   If you do not specify a startwin value when yearopt is 3 or 5.
    

You receive an <ILLEGAL VALUE> error under the following conditions:

*   If you specify an invalid value for hdate and do not either supply an erropt value or set ZDateNull (as described below).
    
*   If the given month number is greater than the number of month values in monthlist.
    
*   If maxdate is less than mindate.
    
*   If endwin is less than startwin.
    
*   If startwin and endwin specify a sliding temporal window whose duration is greater than 100 years.
    

You receive a <VALUE OUT OF RANGE> error under the following conditions:

*   If you specify an hdate value that is out of the range of valid dates. For standard Caché this is 0 through 298013. For ISM-compatible Caché this is 1 through 94232. You can use the [ZDateNull()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process#ZDateNull) method of the [%SYSTEM.Process](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process) class to set the date range and invalid date behavior for the current process.
    
*   If you specify an otherwise valid date which is outside the range defined by the values assumed for maxdate and mindate, and do not supply an erropt value.
    

Using $ZDATE Instead of Utilities

Keep the following points in mind when you need to choose between the $ZDATE function and a date utility:

*   You can use the $ZDATE function in place of most existing entry points of the %DO or %D utilities.
    
*   You can invoke $ZDATE($HOROLOG,7) directly instead of calling INT ^%D. This provides the current date in “Mmm DD \[YY\]YY” format.
    
*   $ZDATEH and $ZDATE are much faster than calling entry points of %DATE, %DI or %DO.
    

See Also

*   [JOB](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cjob) command
    
*   [$ZDATEH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdateh) function
    
*   [$ZDATETIME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetime) function
    
*   [$ZDATETIMEH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetimeh) function
    
*   [$ZTIME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fztime) function
    
*   [$HOROLOG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog) special variable
    
*   [$ZTIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztimestamp) special variable
    
*   %DATE utility, which is documented in the “[Legacy Documentation](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GDOC_legacy)” chapter in Using InterSystems Documentation
    
*   More information on locales in the section on “[System Classes for National Language Support](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSTU_customize#GSTU_customize_nls)” in Caché Specialized System Tools and Utilities.
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzcsc "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdateh "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fzdate.xml**