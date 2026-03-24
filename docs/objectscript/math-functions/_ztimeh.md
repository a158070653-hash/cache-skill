# $ZTIMEH

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fztimeh

---

 

Caché ObjectScript Reference

$ZTIMEH

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fztime "Go back to the previous section.") 

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Additional Math and Time Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_MATHFUNCTIONS "Additional Math and Time Functions") \]  >  \[ [$ZTIMEH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fztimeh "$ZTIMEH") \]

Go to: Description Parameters Examples Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Validates a time and converts it from display format to Caché internal format.

Synopsis

$ZTIMEH(time,tformat,erropt,localeopt)
$ZTH(time,tformat,erropt,localeopt)

Parameters

time

The time value to be converted.

tformat

Optional — A numeric value that specifies the time format from which you are converting.

erropt

Optional — The expression returned if the time parameter is considered invalid.

localeopt

Optional — A boolean flag that specifies which locale to use. When 0, the current locale determines the time separator, and the other characters, strings, and options used to format times. When 1, the ODBC locale determines these characters, strings, and options. The ODBC locale cannot be changed; it is used to format date and time strings that are portable between Caché processes that have made different National Language Support (NLS) choices. The default is 0.

Omitted parameters between specified parameter values are indicated by placeholder commas. Trailing placeholder commas are not required, but are permitted. Blank spaces are permitted between the commas that indicate an omitted parameter.

Description

The $ZTIMEH function converts a time value from a format produced by the $ZTIME function to the format of the special variables $HOROLOG and $ZTIMESTAMP. If the optional parameter tformat is not specified, the input time must be in the format “hh:mm:ss.fff...”.Otherwise, the same integer format code used to produce the printable time from the $ZTIME function must be used for the time to be converted properly.

Parameters

tformat

Supported values are as follows:

Code

Description

\-1

Get the effective format value from the TimeFormat property of the current locale, which defaults to a value of 1. This is the default behavior if you do not specify tformat and localeopt is unspecified or 0.

1

Input time is in the form "hh:mm:ss" (24-hour clock).

2

Input time is in the form “hh:mm” (24-hour clock).

3

Input time is in the form “hh:mm:ss\[AM/PM\]” (12-hour clock).

4

Input time is in the form “hh:mm\[AM/PM\]” (12-hour clock).

To determine the default time format for your locale, invoke the [GetFormatItem()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYS.NLS.Format#GetFormatItem) NLS class method:

  WRITE ##class(%SYS.NLS.Format).GetFormatItem("TimeFormat")

 

erropt

This parameter suppresses error messages associated with invalid time values. Instead of generating <ILLEGAL VALUE> error messages, the function returns the value indicated by erropt.

localeopt

This parameter selects either the user’s current locale definition (0) or the ODBC locale definition (1) as the source for time options. The ODBC locale cannot be changed; it is used to format date and time strings that are portable between Caché processes that have made different National Language Support (NLS) choices.

The ODBC locale time definitions are as follows:

*   Time format defaults to 1. Time separator is ":". Time precision is 0 (no fractional seconds).
    
*   AM and PM indicators are "AM" and "PM". The words "Noon" and "Midnight" are used.
    

Examples

When the input time is “14:43:38”, the following examples both return 53018:

   SET time\="14:43:38"
   WRITE !,$ZTIMEH(time)
   WRITE !,$ZTIMEH(time,1)

 

When the input time is “14:43:38.974”, the following example returns 53018.974:

   SET time\="14:43:38.974"
   WRITE $ZTIMEH(time,1)

 

Notes

Fractional Seconds

Unlike the $ZTIME function, $ZTIMEH does not allow you to specify a precision. Any fractional seconds in the original time format returned by $ZTIME are retained in the value returned by $ZTIMEH.

Invalid Parameter Values

You receive a <FUNCTION> error if you specify an invalid tformat code (an integer less than -1 or greater than 4, a zero, or a noninteger value).

If you do not supply an erropt value, you receive an <ILLEGAL VALUE> error under the following conditions:

*   Specify a time with an hour value outside the allowed range of 0 to 23 (inclusive).
    
*   Specify a time with a minute value outside the allowed range of 0 to 59 (inclusive).
    
*   Specify a time with a second value outside the allowed range of 0 to 59 (inclusive).
    
*   Specify a time value which uses a delimiter other than the value of the TimeSeparator property of the current locale.
    

Time Delimiter

By default, Caché uses the value of the TimeSeparator property of the current locale to determine the delimiter character for the time string. By default, the delimiter is “:”; all documentation examples use this delimiter.

To determine the default time separator for your locale, invoke the [GetFormatItem()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYS.NLS.Format#GetFormatItem) NLS class method:

  WRITE ##class(%SYS.NLS.Format).GetFormatItem("TimeSeparator")

 

Time Suffixes

By default, Caché uses properties in the current locale to determine the names of its time suffixes. For $ZTIMEH, these properties (and their corresponding default values) are:

*   AM (“AM”)
    
*   PM (“PM”)
    
*   Midnight (“MIDNIGHT”)
    
*   Noon (“NOON”)
    

This documentation will always use these default values for these properties.

To determine the default time suffixes for your locale, invoke the [GetFormatItem()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYS.NLS.Format#GetFormatItem) NLS class method, as follows:

  WRITE ##class(%SYS.NLS.Format).GetFormatItem("AM"),!
  WRITE ##class(%SYS.NLS.Format).GetFormatItem("PM"),!
  WRITE ##class(%SYS.NLS.Format).GetFormatItem("Midnight"),!
  WRITE ##class(%SYS.NLS.Format).GetFormatItem("Noon")

 

See Also

*   [$ZDATETIME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetime) function
    
*   [$ZDATETIMEH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetimeh) function
    
*   [$ZTIME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fztime) function
    
*   [$HOROLOG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog) special variable
    
*   [$ZTIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztimestamp) special variable
    
*   More information on locales in the section on “[System Classes for National Language Support](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSTU_customize#GSTU_customize_nls)” in Caché Specialized System Tools and Utilities.
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fztime "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fztimeh.xml**