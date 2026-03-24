# $ZTIME

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fztime

---

 

Caché ObjectScript Reference

$ZTIME

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fztan "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fztimeh "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Additional Math and Time Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_MATHFUNCTIONS "Additional Math and Time Functions") \]  >  \[ [$ZTIME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fztime "$ZTIME") \]

Go to: Description Parameters Examples Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Validates a time and converts it from internal format to the specified display format.

Synopsis

$ZTIME(htime,tformat,precision,erropt,localeopt)
$ZT(htime,tformat,precision,erropt,localeopt)

Parameters

htime

The internal system time that can be specified as a numeric value, the name of a variable, or as an expression.

tformat

Optional — An integer value that specifies the format in which you want to return the time value.

precision

Optional — A numeric value that specifies the number of decimal places of precision in which you want to express the time. If omitted, fractional seconds are truncated.

erropt

Optional — The expression returned if the htime parameter is considered invalid.

localeopt

Optional — A boolean flag that specifies which locale to use. When 0, the current locale determines the time separator, and the other characters, strings, and options used to format times. When 1, the ODBC locale determines these characters, strings, and options. The ODBC locale cannot be changed; it is used to format date and time strings that are portable between Caché processes that have made different National Language Support (NLS) choices. The default is 0.

Omitted parameters between specified parameter values are indicated by placeholder commas. Trailing placeholder commas are not required, but are permitted. Blank spaces are permitted between the commas that indicate an omitted parameter.

Description

The $ZTIME function converts an internal system time, htime, specified in the time format from the special variable $HOROLOG or $ZTIMESTAMP, to a printable format. If no optional parameters are used, the time will be returned in the format: “hh:mm:ss”; where “hh” is hours in a 24–hour clock, “mm” is minutes, and “ss” is seconds. Otherwise, the time will be returned in the format specified by the value of the tformat and precision parameters.

Parameters

htime

This value represents the number of elapsed seconds since midnight. It is the second component of a [$HOROLOG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog) value, which can be extracted by using $PIECE($HOROLOG,",",2). htime can be an integer, or a fractional number with the number of fractional digits of precision specified by precision.

For tformat values –1 through 4, htime valid values must have their integer portion in the range 0 through 86399. (-0 is treated as 0.) Values outside of this range generate an <ILLEGAL VALUE> error. For tformat values 9 and 10, htime valid values can also include negative numbers and numbers greater than 86399.

tformat

Supported values are as follows:

tformat

Description

\-1

Get the effective format value from the TimeFormat property of the current locale, which defaults to a value of 1. This is the default behavior if you do not specify tformat and localeopt is unspecified or 0.

1

Express time in the form "hh:mm:ss" (24-hour clock).

2

Express time in the form “hh:mm” (24-hour clock).

3

Express time in the form “hh:mm:ss\[AM/PM\]” (12-hour clock).

4

Express time in the form “hh:mm\[AM/PM\]” (12-hour clock).

9

For MultiValue support. Same as tformat 1 ("hh:mm:ss" 24-hour clock) for numeric values in the range 0 through 86399. Also accepts negative time values and time values greater than 86399.

10

For MultiValue support. Same as tformat 2 ("hh:mm" 24-hour clock) for numeric values in the range 0 through 86399. Also accepts negative time values and time values greater than 86399.

To determine the default time format for your locale, invoke the [GetFormatItem()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYS.NLS.Format#GetFormatItem) NLS class method:

  WRITE ##class(%SYS.NLS.Format).GetFormatItem("TimeFormat")

 

In 12-hour clock formats, morning and evening are represented by time suffixes, here shown as AM and PM. To determine the default time suffixes for your locale, invoke the following NLS class methods:

  WRITE ##class(%SYS.NLS.Format).GetFormatItem("AM"),!
  WRITE ##class(%SYS.NLS.Format).GetFormatItem("PM"),!

 

tformat 9 and 10

Time formats 9 and 10 are provided for MultiValue support. They are identical to tformats 1 and 2 for all internal time values within the allowed range for Caché ObjectScript: 0 through 86399. Like other time formats, -0 is treated as 0.

Time formats 9 and 10 accept negative numeric values and returns a corresponding negative time value. tformat 9 returns a negative htime as "-hh:mm:ss" (24-hour clock). tformat 10 returns a negative htime as "-hh:mm" (24-hour clock). For time format 10, “–59.9” returns “–00:00” and “–60” returns “–00:01”.

Time formats 9 and 10 accept numeric values greater than 86399, and return the corresponding time. For time format 10, “86400” returns “24:00”, and “400000” returns “111:06”.

Time format 9 returns whole seconds, and optionally returns fractional seconds, for all numeric values. The precision argument specifies the number of digits of fractional seconds.

precision

The function displays fractional seconds carried out to the number of decimal places specified in the precision parameter. For example, if you enter a value of 3 as precision, $ZTIME displays fractional seconds to three decimal places. If you enter a value of 9, $ZTIME displays fractional seconds to nine decimal places. Supported values are as follows:

Value

Description

\-1

Gets the precision value from the TimePrecision property of the current locale, which defaults to a value of 0. This is the default behavior if you do not specify precision.

n

A value that is greater than or equal to 0 results in the expression of time to n decimal places.

0

If set to 0, or defaults to a value of 0, fractional seconds are truncated.

To determine the default time precision for your locale, invoke the [GetFormatItem()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYS.NLS.Format#GetFormatItem) NLS class method:

  WRITE ##class(%SYS.NLS.Format).GetFormatItem("TimePrecision")

 

erropt

This parameter suppresses error messages associated with invalid htime values. Instead of generating <ILLEGAL VALUE> error messages, the function returns the value indicated by erropt.

localeopt

This parameter selects either the user’s current locale definition (0) or the ODBC locale definition (1) as the source for time options. The ODBC locale cannot be changed; it is used to format date and time strings that are portable between Caché processes that have made different National Language Support (NLS) choices.

The ODBC locale time definitions are as follows:

*   Time format defaults to 1. Time separator is ":". Time precision is 0 (no fractional seconds).
    
*   AM and PM indicators are "AM" and "PM". The words "Noon" and "Midnight" are used.
    

Examples

To return the current local time using the special variable $HOROLOG, you must use the $PIECE function to specify the second piece of $HOROLOG. The following returns the time in the 24–hour clock format “13:55:11”:

   WRITE $ZTIME($PIECE($HOROLOG,",",2),1)

 

In the examples that follow, htime is set to $PIECE($HOROLOG,",",2) for the current time. These examples show how to use the various forms of $ZTIME to return different time formats.

The following example in many cases returns time in the format "13:28:55"; however, this format is dependent on locale:

   SET htime\=$PIECE($HOROLOG,",",2)
   WRITE $ZTIME(htime)

 

The following example returns time in the format "13:28:55":

   SET htime\=$PIECE($HOROLOG,",",2)
   WRITE $ZTIME(htime,1)

 

The following example returns time in the format "13:28:55.999":

   SET htime\=$PIECE($HOROLOG,",",2)
   WRITE $ZTIME(htime,1,3)

 

The following example returns time in the format "13:28:55.999999999":

   SET htime\=$PIECE($HOROLOG,",",2)
   WRITE $ZTIME(htime,1,9)

 

The following example returns time in the format "13:28":

   SET htime\=$PIECE($HOROLOG,",",2)
   WRITE $ZTIME(htime,2)

 

The following example returns time in the format "01:28:24PM":

   SET htime\=$PIECE($HOROLOG,",",2)
   WRITE $ZTIME(htime,3)

 

The following example returns time in the format "01:28PM":

   SET htime\=$PIECE($HOROLOG,",",2)
   WRITE $ZTIME(htime,4)

 

The following example returns time in the format “13:45:56.021”, the current UTC time with three decimal places of precision:

   SET t\=$ZTIME($PIECE($ZTIMESTAMP,",",2),1,3)
   WRITE "Current UTC time is ",t

 

Notes

Invalid Parameter Values

*   You receive a <FUNCTION> error if you specify an invalid tformat value.
    
*   You receive an <ILLEGAL VALUE> error for all tformat except 9 and 10 if you specify a value for htime outside the allowed range of 0 to 86399 (inclusive) and do not supply an erropt value.
    

Decimal Delimiter

$ZTIME will use the value of the DecimalSeparator property of the current locale as the delimiter between the whole and fractional parts of numbers. The default value of DecimalSeparator is “.”; all documentation examples use this delimiter.

To determine the default decimal separator for your locale, invoke the [GetFormatItem()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYS.NLS.Format#GetFormatItem) NLS class method:

  WRITE ##class(%SYS.NLS.Format).GetFormatItem("DecimalSeparator")

 

Time Delimiter

By default, Caché uses the value of the TimeSeparator property of the current locale to determine the delimiter character for the time string. By default, the delimiter is “:”; all documentation examples use this delimiter.

To determine the default time separator for your locale, invoke the following NLS class method:

  WRITE ##class(%SYS.NLS.Format).GetFormatItem("TimeSeparator")

 

Time Suffixes

By default, Caché uses properties in the current locale to determine the names of its time suffixes. For $ZTIME, these properties (and their corresponding default values) are:

*   AM (“AM”)
    
*   PM (“PM”)
    

This documentation will always use these default values for these properties.

To determine the default time suffixes for your locale, invoke the following NLS class methods:

  WRITE ##class(%SYS.NLS.Format).GetFormatItem("AM"),!
  WRITE ##class(%SYS.NLS.Format).GetFormatItem("PM")

 

See Also

*   [$ZDATETIME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetime) function
    
*   [$ZDATETIMEH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetimeh) function
    
*   [$ZTIMEH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fztimeh) function
    
*   [$PIECE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fpiece) function
    
*   [$HOROLOG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog) special variable
    
*   [$ZTIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztimestamp) special variable
    
*   More information on locales in the section on “[System Classes for National Language Support](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSTU_customize#GSTU_customize_nls)” in Caché Specialized System Tools and Utilities.
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fztan "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fztimeh "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fztime.xml**