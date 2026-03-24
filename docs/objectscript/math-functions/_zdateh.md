# $ZDATEH

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fzdateh

---

 

Caché ObjectScript Reference

$ZDATEH

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdate "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetime "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Additional Math and Time Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_MATHFUNCTIONS "Additional Math and Time Functions") \]  >  \[ [$ZDATEH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdateh "$ZDATEH") \]

Go to: Description Parameters Examples Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Validates a date and converts it from display format to Caché internal format.

Synopsis

$ZDATEH(date,dformat,monthlist,yearopt,startwin,endwin,mindate,maxdate,erropt,localeopt)
$ZDH(date,dformat,monthlist,yearopt,startwin,endwin,mindate,maxdate,erropt,localeopt)

Parameters

date

An expression that evaluates to a date string in display format. $ZDATEH converts this date string to $HOROLOG format. This can be either an explicit date (specified in various formats) or the string “T” or “t”, representing the current date. The “T” or “t” string can optionally include a signed integer offset. For example “T-7” meaning seven days before the current date. See [date](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdateh#RCOS_fzdateh_paramdate) below.

dformat

Optional — An integer code that specifies a date format option for date. If date is “T”, dformat must be 5, 6, 7, 8, 9, or 15. See [dformat](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdateh#RCOS_fzdateh_paramdformat) below.

monthlist

Optional — A string or the name of a variable that specifies a set of month names. This string must begin with a delimiter character, and its 12 entries must be separated by this delimiter character. See [monthlist](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdateh#RCOS_fzdateh_parammonthlist) below.

yearopt

Optional — An integer code that specifies whether to represent years as two- or four-digit values. See [yearopt](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdateh#RCOS_fzdateh_paramyearopt) below.

startwin

Optional — The start of the sliding window during which dates must be represented with two-digit years. See [startwin](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdateh#RCOS_fzdateh_paramstartwin) below.

endwin

Optional — The end of the sliding window during which dates are represented with two-digit years. See [endwin](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdateh#RCOS_fzdateh_paramendwin) below.

mindate

Optional — The lower limit of the range of valid date dates. Specified as a $HOROLOG integer date count, with 0 representing December 31, 1840. Can be specified as a positive or negative integer. See [mindate](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdateh#RCOS_fzdateh_parammindate) below.

maxdate

Optional — The upper limit of the range of valid dates. Specified as a $HOROLOG integer date count. See [maxdate](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdateh#RCOS_fzdateh_parammaxdate) below.

erropt

Optional — An expression to return when date is invalid. Specifying a value for this parameter suppresses error codes associated with invalid or out of range date values. Instead of issuing an error message, $ZDATEH returns erropt. See [erropt](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdateh#RCOS_fzdateh_paramerropt) below.

localeopt

Optional — A boolean flag that specifies which locale to use for the dformat, monthlist, yearopt, mindate and maxdate default values, and other date characteristics, such as the date separator character:

localeopt\=0: the current locale property settings determine these parameter defaults.

localeopt\=1: the ODBC standard locale determines these parameter defaults.

localeopt not specified: the dformat value determines these parameter defaults. If dformat\=3, ODBC defaults are used. Japanese and Islamic date dformatvalues use their own defaults. For all other dformat values, current locale property settings are used as defaults. See [localeopt](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdateh#RCOS_fzdateh_paramlocaleopt) below.

Omitted parameters between specified parameter values are indicated by placeholder commas. Trailing placeholder commas are not required, but are permitted. Blank spaces are permitted between the commas that indicate an omitted parameter.

Description

The $ZDATEH function validates a specified date and converts it from any of the formats supported by the $ZDATE function to $HOROLOG format. The exact action $ZDATEH performs depends on the parameters you use.

Simple $ZDATEH Format

$ZDATEH(date) converts a date in the form MM/DD/\[YY\]YY to the first integer in the $HOROLOG format. (The $HOROLOG format consists of two integers: the first integer is a date, the second integer is a time.) Two or four digits may be specified for years in the range 1900 to 1999. Four digits must be specified for years before 1900 or after 1999.

Customizable $ZDATEH Format

$ZDATEH(date,dformat,monthlist,yearopt,startwin,endwin,mindate,maxdate,erropt) converts a date in the specified dformat to $HOROLOG format. The dformat, monthlist, yearopt, startwin, endwin, mindate, maxdate and erropt values are identical to the values used by $ZDATE. However, when you use a dformat of 5, 6, 7, 8, or 9, $ZDATEH recognizes and converts a date in any of the external date formats defined for dformat codes 1, 2, 3, 5, 6, 7, 8, and 9. (But not dformat code 4.) It also recognizes a special relative date format that consists of a string beginning with the letter T or t (indicating “today”,) optionally followed by a plus (+) or a minus (-) sign, and an integer number of days after or before the current date.

Parameters

date

The date you want converted to $HOROLOG format, specified as a quoted string. This can be an explicit date, or the implicit current date, represented by the string “T” or “t”.

An explicit date must be specified in one of the formats supported by dformat. The permitted format(s) depends on the dformat parameter. If dformat is not specified or is 1, 2, 3, or 4, only one date format is permitted. If dformat is 5, 6, 7, 8, 9 or 15, multiple date formats are permitted.

If dformat is 5, 6, 7, 8, or 9, $ZDATEH accepts all unambiguous American date formats. If dformat is 15, $ZDATEH accepts all unambiguous European date formats. A list of valid date formats is provided below. Note that dformat\=4 is not a valid American date format because $ZDATEH cannot differentiate between 02/03/02 (meaning February 3, 2002) and the European 02/03/02 (meaning March 2, 2002). If you specify a date in a non-permitted format, or a nonexistent date (such as February 31, 2002), $ZDATEH generates an <ILLEGAL VALUE> error code. ($ZDATEH does check for leap year dates, permitting Feb. 29, 2004 but not Feb. 29, 2003.)

In the Russian, Ukrainian, and Czech locales, a date must be specified with periods, rather than slashes, as date part separators: DD.MM.YYYY.

An implicit date is specified as a string consisting of the letter “T” or “t”, indicating the current date (today). This string can optionally include a plus or minus sign and an integer, which specify the number of days offset from the current date. For example, “t+9” (nine days after the current date) or “t-12” (twelve days before the current date). Implicit dates are only permitted if dformat is 5, 6, 7, 8, 9, or 15. The only permitted implicit date forms are “T” (or “t”), and “T” (or “t”) followed by a sign and integer. Caché generates an <ILLEGAL VALUE> error if you specify a noninteger number, an arithmetic expression, an integer without a sign, or a sign without an integer. “T+0” and “T-0” are permitted, and return the current date. Caché generates a <VALUE OUT OF RANGE> error if you specify an offset that would result in a $HOROLOG date beyond the range of valid dates.

By default, the earliest valid date is December 31, 1840 (0 in internal $HOROLOG representation). Dates are limited to positive integers by default because the DateMinimum property defaults to 0. You can specify earlier dates as negative integers, provided the DateMinimum property of the current locale is set to a greater or equal negative integer. The lowest valid DateMinimum value is -672045, which corresponds to January 1, 0001. Caché uses the proleptic Gregorian calendar, which projects the Gregorian calendar back to “Year 1”, in conformance with the ISO 8601 standard. This is, in part, because the Gregorian calendar was adopted at different times in different countries. For example, much of continental Europe adopted it in 1582; Great Britain and the United States adopted it in 1752. Thus Caché dates prior to your local adoption of the Gregorian calendar may not correspond to historical dates that were recorded based on the local calendar then in effect. For further details on dates prior to 1840, refer to the mindate parameter.

dformat

Format for the date. Valid values are:

Value

Meaning

\-1

Get effective dformat value from the DateFormat property of the [current locale](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_dates). This is the default behavior if you do not specify dformat.

1

MM/DD/\[YY\]YY (07/01/97 or 03/27/2002) — [American numeric format](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_dates). You must specify the correct dateseparator character (/ or .) for the current locale.

2

DD Mmm \[YY\]YY (01 Jul 97)

3

\[YY\]YY-MM-DD (1997-07-01) - ODBC format

4

DD/MM/\[YY\]YY (01/07/97 or 27/03/2002) — [European numeric format](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_dates). You must specify the correct dateseparator character (/ or .) for the current locale.

5

Mmm D, YYYY (Jul 1, 1997) or any [unambiguous American date format](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdateh#RCOS_fzdateh_dformatstable).

6

Mmm D YYYY (Jul 1 1997) or any [unambiguous American date format](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdateh#RCOS_fzdateh_dformatstable).

7

Mmm DD \[YY\]YY (Jul 01 1997) or any [unambiguous American date format](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdateh#RCOS_fzdateh_dformatstable).

8

\[YY\]YYMMDD (19970701) - Numeric format, or any [unambiguous American date format](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdateh#RCOS_fzdateh_dformatstable).

9

Mmmmm D, YYYY (July 1, 1997), or any [unambiguous American date format](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdateh#RCOS_fzdateh_dformatstable).

15

DD/MM/\[YY\]YY or YYYY-MM-DD or any [unambiguous European date format](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdateh#RCOS_fzdateh_dformatstable) with any dateseparator character, or YYYYMMDD with no date separators. The dateseparator character may be any non-alphanumeric character, including blank spaces, regardless of the dateseparator character specified in the current locale. Also accepts monthlist names and “T”.

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

Where:

Syntax

Meaning

YYYY

YYYY is a four-digit year. \[YY\]YY is a two-digit year if the date falls within the active window for two-digit years; otherwise it is a four-digit years. You must supply the year value when using date formats (dformat) 1 through 4; these date formats do not supply a missing year value. Date formats 5 through 9 assume the current year if the date you specify does not include a year.

MM

Two-digit month.

D

One-digit day if the day number <10. Otherwise, two digits.

DD

Two-digit day.

Mmm

Month abbreviation extracted from the MonthAbbr property of the current locale. The default values in English are: “Jan Feb Mar Apr May Jun Jul Aug Sep Oct Nov Dec”. Or an alternate month abbreviation (or name of any length) extracted from an optional list specified as the monthlist parameter to $ZDATEH.

Mmmmm

Full name of the month as specified by the MonthName property of the current locale. The default values in English are: “January February March ... November December”.

dformat Default

If you omit dformat or set it to -1, the dformat default depends on the localeopt parameter and the NLS DateFormat property:

*   If localeopt\=1 the dformat default is ODBC format. The monthlist, yearopt, mindate and maxdate parameter defaults are also set to ODBC format. This is the same as setting dformat\=3.
    
*   If localeopt\=0 or is unspecified, the dformat default is taken from the NLS DateFormat property. If DateFormat\=3, the dformat default is ODBC format. However, DateFormat=3 does not affect the monthlist, yearopt, mindate and maxdate parameter defaults, which are as specified in the current NLS locale definition.
    

To determine the default date properties for your locale, invoke the [GetFormatItem()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYS.NLS.Format#GetFormatItem) NLS class method:

  WRITE ##class(%SYS.NLS.Format).GetFormatItem("DateFormat"),!
  WRITE ##class(%SYS.NLS.Format).GetFormatItem("DateSeparator")

 

$ZDATEH will use the value of the [DateSeparator property of the current locale](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_dates) (either / or .) as the delimiter between months, days, and the year when dformat\=1 or 4.

European date format (dformat\=4, DD/MM/YYYY order) is the default for many (but not all) European languages, including British English, French, German, Italian, Spanish, and Portuguese (which use a “/” DateSeparator character), as well as Czech (csyw), Russian (rusw), Slovak (skyw), Slovenian (svnw), and Ukrainian (ukrw) (which use a “.” DateSeparator character). For further details on default date formats for supported locales, refer to [Dates](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_dates) in Using Caché ObjectScript.

dformat Settings

If dformat is 3 (ODBC format date), ODBC format defaults are also used for the monthlist, yearopt, mindate and maxdate parameter defaults. The date separator will always be a “-”. Current locale defaults are ignored.

If dformat is 16 or 17 (Japanese date formats), the date format is independent of the locale setting. To use Japanese-format dates you must have a Unicode Caché instance. On an 8-bit Caché instance specifying dformat is 16 or 17 causes a <ERWIDECHAR> error.

If dformat is 18, 19, 20, or 21 (Islamic date formats) and localeopt is unspecified, parameters default to Islamic defaults, rather than current locale defaults. The monthlist parameter defaults to Arabic month names transliterated with Latin characters. The yearopt, mindate and maxdate parameters default to ODBC defaults. The date separator defaults to the Islamic default (a space), not the ODBC default or the current locale DateSeparator property value. If localeopt\=0 current locale property defaults are used for these parameters. If localeopt\=1 ODBC defaults are used for these parameters.

monthlist

An expression that resolves to a string of month names or month name abbreviations, separated by a delimiter character. The names in monthlist replace the default month abbreviation values from the MonthAbbr property or the month name values from the MonthName property of the current locale.

monthlist is valid only if dformat is 2, 5, 6, 7, 8, 9, 15, 18, or 20. If dformat is any other value $ZDATEH ignores monthlist.

The monthlist string has the following format:

*   The first character of the string is a delimiter character (usually a space). The same delimiter must appear before the first month name and between each month name in monthlist. You can specify any single-character delimiter; this delimiter must be specified between the month, day, and year portions of the specified date value, which is why a space is usually the preferred character.
    
*   The month names string should contain twelve delimited values, corresponding to January through December. It is possible to specify more or less than twelve month names, but if there is no month name corresponding to the month in date an <ILLEGAL VALUE> error is generated.
    

If you omit monthlist or specify a monthlist value of -1, $ZDATEH uses the list of month names defined in the MonthAbbr or MonthName property of the current locale, unless one of the following is true: If localeopt\=1, the monthlist default is the ODBC month list (in English). If localeopt is unspecified and dformat is 18 or 20 (Islamic date formats) the monthlist default is the Islamic month list (Arabic names expressed using Latin letters), ignoring the MonthAbbr or MonthName property value.

To determine the default month names and month abbreviations for your locale, invoke the [GetFormatItem()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYS.NLS.Format#GetFormatItem) NLS class method:

  WRITE ##class(%SYS.NLS.Format).GetFormatItem("MonthName"),!
  WRITE ##class(%SYS.NLS.Format).GetFormatItem("MonthAbbr"),!

 

yearopt

A numeric code that specifies whether to represent years as either two-digit values or four-digit values. Valid values are:

Value

Meaning

\-1

Get effective yearopt value from YearOption property of current locale which defaults to a value of 0. This is the default behavior if you do not specify yearopt .

0

Represent 20th century dates (1900 through 1999) with two-digit years, unless a process-specific sliding window (established via the %DATE utility) is in effect. If such a window is in effect, represent only those dates falling within the sliding window by two-digit years. Represent all dates falling outside the 20th century or outside the process-specific sliding window by four-digit years.

1

Represent 20th century dates with two-digit years and all other dates with four-digit years, regardless of any sliding temporal window in effect.

2

Represent all dates with two-digit years, regardless of any sliding temporal window in effect. All dates are assumed to be in the 20th century. Because this option deletes two digits from four-digit years, its use results in a nonreversible loss of century information. (This loss may be trivial if all dates are in the same century).

3

Represent with two-digit years those dates falling within the sliding temporal window defined by startwin and (optionally) endwin. Represent all other dates with four-digit years. When yearopt\=3, startwin and endwin are absolute dates in $HOROLOG format.

4

Represent all dates with four-digit years. Dates input with two- digit years are rejected as invalid.

5

Represent with two-digit years all dates falling within the sliding temporal window defined by startwin and (optionally) endwin. Represent all other dates with four-digit years. When yearopt=5, startwin and endwin are relative years.

6

Represent all dates in the current century with two-digit years and all other dates with four-digit years.

If you omit yearopt or specify a yearopt value of -1, $ZDATEH uses the YearOption property of the current locale, unless one of the following is true: If localeopt\=1, the yearopt default is the ODBC year option. If localeopt\=0 or is unspecified and dformat is 18, 19, 20, or 21 (Islamic date formats) the yearopt default is the ODBC year option (4-digit years); the YearOption property value is ignored for Islamic dates.

startwin

A numeric value that specifies the start of the sliding window during which dates must be represented with two-digit years. You must supply startwin when you use a yearopt of 3 or 5.startwin is not valid with any other yearopt values.

When yearopt\=3, startwin is an absolute date in $HOROLOG date format that indicates the start date of the sliding window.

When yearopt\= 5, startwin is a numeric value that indicates the start year of the sliding window expressed in the number of years before the current year. The sliding window always begins on the first day of the year (January 1) specified in startwin.

endwin

A numeric value that specifies the end of the sliding window during which dates are represented with two-digit years. You may optionally supply endwin when yearopt is 3 or 5. endwin is not valid with any other yearopt values.

When yearopt\=3, endwin is an absolute date in $HOROLOG date format that indicates the end date of the sliding window.

When yearopt\=5, endwin is a numeric value that indicates the end year of the sliding window expressed as the number of years past the current year. The sliding window always ends on the last day of the year (December 31) of the year specified in endwin or of the implied end year (if you omit endwin).

If endwin is omitted (or specified as -1) the effective sliding window will be 100 years long. The endwin value of -1 is a special case that always returns a date value, even when higher and lower endwin values return erropt. For this reason, it is preferable to omit endwin when specifying a 100-year window, and to avoid the use of negative endwin values.

If you supply both startwin and endwin, the sliding window they specify must not have a duration of more than 100 years.

mindate

An expression that specifies the lower limit of the range of valid dates (inclusive). Can be specified as a $HOROLOG integer date count (for example, 1/1/2013 is represented as 62823) or a $HOROLOG string value. You can include or omit the time portion of the $HOROLOG date (for example “62823,43200”), but only the date portion of mindate is parsed. Specifying a date value earlier than mindate generates a <VALUE OUT OF RANGE> error.

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
  WRITE $ZDATEH("05/29/1805",1,,,,,\-14974),!!
ResetDateMinimumToDefault
  SET oldmin\=##class(%SYS.NLS.Format).SetFormatItem("DateMinimum",min)
  WRITE "reset DateMinimum value from ",oldmin," to ",min 

 

You may specify mindate with or without maxdate. Specifying a mindate larger than maxdate generates an <ILLEGAL VALUE> error.

maxdate

An expression that specifies the upper limit of the range of valid dates (inclusive). Can be specified as a $HOROLOG integer date count (for example, 1/1/2100 is represented as 94599) or a $HOROLOG string value. You can include or omit the time portion of the $HOROLOG date (for example “94599,43200”), but only the date portion of maxdate is parsed.

If maxdate is omitted or if specified as -1, the maximum date limit is obtained from the DateMaximum property of the current locale, which defaults to the maximum permissible value for the date portion of $HOROLOG: 2980013 (corresponding to December 31, 9999). However, the application of the DateMaximum property is governed by the localeopt setting. When localeopt\=1 (which is the default for dformat\=3) the date maximum default is the ODBC value (2980013), regardless of the current locale setting. Islamic date formats also take the ODBC default.

Specifying a date larger than maxdate generates a <VALUE OUT OF RANGE> error.

Specifying a maxdate larger than 2980013 generates an <ILLEGAL VALUE> error.

You may specify maxdate with or without mindate. Specifying a maxdate smaller than mindate generates an <ILLEGAL VALUE> error.

erropt

Specifying a value for this parameter suppresses errors associated with invalid or out of range date values. Instead of generating <ILLEGAL VALUE> or <VALUE OUT OF RANGE> errors, the $ZDATEH function returns the erropt value.

Caché performs standard numeric evaluation on date, which must evaluate to an integer within the mindate/maxdate range. Thus, 7, "7", +7, 0007, 7.0, "7 dwarves", and --7 all evaluate to the same date value: 01/07/1841. By default, values greater than 2980013 or less than 0 generate a <VALUE OUT OF RANGE> error. Fractional values generate an <ILLEGAL VALUE> error. Non-numeric strings (including the null string) evaluate to 0, and thus return the $HOROLOG initial date: 12/31/1840.

The erropt parameter only suppresses errors generated due to invalid or out of range values of date. Errors generated due to invalid or out of range values of other parameters will always generate errors whether or not erropt has been supplied. For example, an <ILLEGAL VALUE> error is always generated when $ZDATEH specifies a sliding window where endwin is earlier than startwin. Similarly, an <ILLEGAL VALUE> error is generated when maxdate is less than mindate.

localeopt

This Boolean parameter specifies either the user’s current locale definition or the ODBC locale definition as the source for defaults for the locale-specified parameters dformat, monthlist, yearopt, mindate and maxdate:

*   If localeopt\=0, all of these parameters take the current locale definition defaults.
    
*   If localeopt\=1, all of these parameters take the ODBC defaults.
    
*   If localeopt is not specified, the dformat parameter determine the default for these parameters. If dformat\=3, the ODBC defaults are used. If dformat is 18, 19, 20, or 21 the Islamic date format defaults are used, regardless of the current locale definition. For all other dformat values, the current locale definition defaults are used. Refer to the [dformat](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdateh#RCOS_fzdateh_paramdformat) description for further details.
    

The ODBC locale cannot be changed; it is used to format date strings that are portable between Caché processes that have made different National Language Support (NLS) choices. If localeopt\=1, the ODBC locale date definitions are as follows:

*   Date format defaults to 3. Therefore, if dformat is undefined or -1, date format 3 is used.
    
*   Date separator defaults to "/". However, date format defaults to 3, which always uses "-" as the date separator.
    
*   Year option defaults to 4 digits.
    
*   Date minimum and maximum: 0 and 2980013 ($HOROLOG date count).
    
*   English month names, month abbreviations, weekday names, and weekday abbreviations are used.
    

Examples

The following example returns the $HOROLOG date for June 12, 1983:

   WRITE $ZDATEH("06/12/83")

 

returns 52027.

The following example returns the $HOROLOG date for June 12, 1902 (which may not have been your intent):

   WRITE $ZDATEH("06/12/02")

 

returns 22442.

Note:

Two-digit years, by default, are considered 20th Century dates; for 21st Century dates, specify a four-digit year, or change the two-digit sliding window by specifying the yearopt, startwin and endwin parameters. This sliding window can also be set for your locale.

The following example shows how the dformat parameter is used to permit multiple date entry formats:

   WRITE !,$ZDATEH("November 2, 1954",5)
   WRITE !,$ZDATEH("Nov 2, 1954",5)
   WRITE !,$ZDATEH("Nov. 2 1954",5)
   WRITE !,$ZDATEH("11/2/1954",5)
   WRITE !,$ZDATEH("11.02.54",5)
   WRITE !,$ZDATEH("11 02 1954",5)

 

all return 41578.

In the following examples, suppose the current date is January 16, 2007:

   WRITE $HOROLOG

 

returns 60646,37854, the first integer of which is the current date (the second integer is the current time, in elapsed seconds).

The next example uses the “T” date to return today’s date (here, January 16, 2007):

   WRITE $ZDATEH("T",5)

 

returns 60646.

The next examples returns the current date with an offset of plus 2 days and minus 2 days:

   WRITE !,$ZDATEH("T+2",5)
   WRITE !,$ZDATEH("T-2",5)

 

returns 60648 and 60644.

The final example illustrates that when no year is specified, $ZDATEH assumes the current year (in this case, 2007):

   WRITE $ZDATEH("25 Nov",5)

 

returns 60959.

Notes

Invalid Values with $ZDATEH

You receive a <FUNCTION> error in the following conditions:

*   If you specify an invalid dformat code (an integer other than -1, 1, 2, 3, 4, 5, 6, 7, 8, 9, or 15, a zero, or a noninteger value)
    
*   If you specify an invalid yearopt code (an integer less than -1 or greater than 6, a value of zero, or a noninteger value)
    
*   If you do not specify a startwin value when yearopt is 3 or 5
    

You receive a <ILLEGAL VALUE> error under the following conditions:

*   If you specify an invalid value for any date unit (day, month, or year). If specified, the erropt value is returned rather than issuing an <ILLEGAL VALUE>.
    
*   If you specify excess leading zeros for any date unit (day, month, or year) in an ODBC date. For example, you can represent the February 3, 2007 as “2007–2–3” or “2007–02–03”, but will receive an <ILLEGAL VALUE> for “2007–002–03”. If specified, the erropt value is returned rather than issuing an <ILLEGAL VALUE>.
    
*   If the given month number is greater than the number of month values in monthlist.
    
*   If maxdate is less than mindate.
    
*   If endwin is less than startwin.
    
*   If startwin and endwin specify a sliding temporal window whose duration is greater than 100 years.
    

You receive a <VALUE OUT OF RANGE> error under the following conditions:

*   If you specify a date (or an offset to “T”) which is earlier than Dec. 31, 1840 or later than Dec. 31, 9999, and do not supply an erropt value
    
*   If you specify an otherwise valid date (or an offset to “T”) which is outside the range of mindate and maxdate and do not supply an erropt value.
    

Acceptable Date Formats with dformat 5 through 9 and 15

The $ZDATEH dformat date formats 5 through 9 accept any American format date value that is unambiguous. $ZDATEH dformat date format 15 accepts any European format date value that is unambiguous. These date formats assume the current year if the date you specify does not include a year.

The following formats are supported:

American Formats: dformat 5, 6, 7, 8, or 9

European Formats: dformat 15

MM/DD

MM-DD

MM DD

MM/DD/YY

MM-DD-YY

MM DD YY

MM/DD/YYYY

MM-DD-YYYY

MM DD YYYY

YYYYMMDD

YYMMDD

YYYY-MM-DD

YYYY MM DD

Mmm D

Mmm D, YY

Mmm D, YYYY

Mmm D YY

Mmm D YYYY

Mmm DD

Mmm DD YY

Mmm DD YYYY

DD Mmm

DD Mmm YY

DD Mmm YYYY

DD-Mmm

DD-Mmm-YY

DD-Mmm-YYYY

YYYY Mmm DD

DD/MM

DD-MM

DD MM

DD/MM/YY

DD-MM-YY

DD MM YY

DD/MM/YYYY

DD-MM-YYYY

DD MM YYYY

YYYYMMDD

YYMMDD

YYYY-MM-DD

YYYY MM DD

Mmm D

Mmm D, YY

Mmm D, YYYY

Mmm D YY

Mmm D YYYY

Mmm DD

Mmm DD YY

Mmm DD YYYY

DD Mmm

DD Mmm YY

DD Mmm YYYY

DD-Mmm

DD-Mmm-YY

DD-Mmm-YYYY

YYYY Mmm DD

MMDD is not an implemented format.

Using $ZDATEH Instead of Utilities

Keep the following points in mind when you need to choose between the $ZDATEH function and a date utility:

*   You can use the $ZDATEH function in place of the existing entry points of the %DATE and %DI utility.
    
*   $ZDATEH and $ZDATE are much faster than calling entry points of %DATE, %DI or %DO.
    

See Also

*   [JOB](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cjob) command
    
*   [$ZDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdate) function
    
*   [$ZDATETIME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetime) function
    
*   [$ZDATETIMEH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetimeh) function
    
*   [$ZTIME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fztime) function
    
*   [$HOROLOG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog) special variable
    
*   [$ZTIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztimestamp) special variable
    
*   %DATE utility, which is documented in the “[Legacy Documentation](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GDOC_legacy)” chapter in Using InterSystems Documentation
    
*   More information on locales in the section on “[System Classes for National Language Support](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSTU_customize#GSTU_customize_nls)” in Caché Specialized System Tools and Utilities.
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdate "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetime "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fzdateh.xml**