# $ZTIMESTAMP

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_vztimestamp

---

 

Caché ObjectScript Reference

$ZTIMESTAMP

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzstorage "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztimezone "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Special Variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_VARIABLES "Caché ObjectScript Special Variables") \]  >  \[ [$ZTIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztimestamp "$ZTIMESTAMP") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Contains the current date and time in Coordinated Universal Time format.

Synopsis

$ZTIMESTAMP
$ZTS

Description

$ZTIMESTAMP contains the current date and time as a Coordinated Universal Time value. This is a worldwide time and date standard; this value is very likely to differ from your local time (and date) value.

$ZTIMESTAMP represents date and time as a string with the format:

ddddd,sssss.fff

Where ddddd is an integer specifying the number of days since December 31, 1840; sssss is an integer specifying the number of seconds since midnight of the current day, and fff is a varying number of digits specifying fractional seconds. This format is similar to $HOROLOG, except that $HOROLOG does not contain fractional seconds.

Suppose the current date and time (UTC) is as follows:

2011-03-26 15:17:27.984

At that time, $ZTIMESTAMP has the value:

62176,55047.984

$ZTIMESTAMP reports the Coordinated Universal Time (UTC), which is independent of time zone. Consequently, $ZTIMESTAMP provides a time stamp that is uniform across time zones. This may differ from both the local time value and the local date value.

The $ZTIMESTAMP time value is a decimal numeric value that counts the time in seconds and fractions thereof. The number of digits in the fractional seconds may vary from zero to nine, depending on the precision of your computer’s time-of-day clock. On Windows systems the fractional precision is three decimal digits; on UNIX® systems it is six decimal digits. $ZTIMESTAMP suppresses trailing zeroes or a trailing decimal point in this fractional portion.

The various ways to return the current date and time are compared, as follows:

*   $ZTIMESTAMP contains the UTC date and time, with fractional seconds, in Caché storage ($HOROLOG) format. Fractional seconds are expressed in three digits of precision (on Windows systems), or six digits of precision (on UNIX® systems).
    
*   $NOW returns the local date and time for the current process; local time variants (such as Daylight Saving Time) are not applied. $NOW with no parameter value determines the local time zone from the value of the $ZTIMEZONE special variable. $NOW with a parameter value returns the time and date that correspond to the specified time zone parameter. $NOW(0) returns the UTC date and time. The value of $ZTIMEZONE is ignored. $NOW returns the date and time in Caché storage ($HOROLOG) format. It includes fractional seconds; the number of fractional digits is the maximum precision supported by the current operating system.
    
*   $HOROLOG contains the local, variant-adjusted date and time in Caché storage format. It does not record fractional seconds. How $HOROLOG resolves fractional seconds depends on the operating system platform: On Windows, it rounds up any fractional second to the next whole second. On UNIX®, it truncates the fractional portion.
    

Note:

Exercise caution when comparing local time and UTC time:

*   The preferred way to convert UTC time to local time is to use the $ZDATETIMEH(utc,-3) function. This function adjusts for local time variants.
    
*   You cannot interconvert local time and UTC time by simply adding or subtracting the value of [$ZTIMEZONE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztimezone) \* 60. This is because, in many cases, local time is adjusted for local time variants (such as Daylight Saving Time, which seasonally adjusts local time by one hour). These local time variants are not reflected in $ZTIMEZONE.
    
*   UTC time is calculated using a count of time zones from the Greenwich meridian. It is not the same as local Greenwich time. The term Greenwich Mean Time (GMT) may be confusing; local time at Greenwich is the same as UTC during the winter; during the summer it differs from UTC by one hour. This is because a local time variant, known as British Summer Time, is applied.
    
*   Both the timezone offset from UTC and local time variants (such as the seasonal shift to Daylight Saving Time) can affect the date as well as the time. Converting from local time to UTC time (or vice versa) may change the date as well as the time.
    

This special variable cannot be modified using the SET command. Attempting to do so results in a <SYNTAX> error.

Coordinated Universal Time Conversions

You can represent local time information as Coordinated Universal Time (UTC) using the $ZDATETIME and $ZDATETIMEH functions with tformat values 7 or 8, as shown in the following example:

   WRITE !,$ZDATETIME($ZTIMESTAMP,1,1,2)
   WRITE !,$ZDATETIME($HOROLOG,1,7,2)
   WRITE !,$ZDATETIME($HOROLOG,1,8,2)
   WRITE !,$ZDATETIME($NOW(),1,7,2)
   WRITE !,$ZDATETIME($NOW(),1,8,2)

 

The above $ZDATETIME functions all return the current time as Coordinated Universal Time (rather than local time). These time value conversions from local time may differ, because [$NOW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fnow) does not adjust for local time variants; $ZTIMESTAMP and [$HOROLOG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog) do adjust for local time variants and may adjust the date accordingly, if necessary. The $ZTIMESTAMP display value and the tformat 7 or 8 converted display values are not identical. The tformat values 7 and 8 insert the letter “T” before, and the letter “Z” after the time value. Also, because $HOROLOG time does not contain fractional seconds, the precision of 2 in the above example pads those decimal digits with zeros.

You can obtain the same time stamp information as $ZTIMESTAMP by invoking the [TimeStamp()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SYS#TimeStamp) class method, using either of the following syntax forms:

   WRITE !,$SYSTEM.SYS.TimeStamp()
   WRITE !,##class(%SYSTEM.SYS).TimeStamp()

 

Refer to the [$SYSTEM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vsystem) special variable in this manual and the [%SYSTEM.SYS](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SYS) class in the InterSystems Class Reference for further details.

Examples

The following example converts the value of $ZTIMESTAMP to local time, and compares it with two representations of local time: $NOW() and $HOROLOG:

   SET stamp\=$ZTIMESTAMP,clock\=$HOROLOG,miliclock\=$NOW()
   WRITE !,"local date and time: ",$ZDATETIME(clock,1,1,2)
   WRITE !,"local date and time: ",$ZDATETIME(miliclock,1,1,2)
   WRITE !,"UTC date and time:   ",$ZDATETIME(stamp,1,1,2)
   IF $PIECE(stamp,",",2) \= $PIECE(clock,",",2) {
      WRITE !,"Local time is UTC time" }
   ELSEIF $PIECE(stamp,",") '= $PIECE(clock,",") {
     WRITE !,"Time difference affects date" }
   ELSE {
         SET localutc\=$ZDATETIMEH(stamp,\-3)
         WRITE !,"UTC converted to local: ",$ZDATETIME(localutc,1,1,2)
   }
   QUIT

 

The following example compares the values returned by $ZTIMESTAMP and $HOROLOG, and shows how the time portion of $ZTIMESTAMP may be converted. (Note that in this simple example only one adjustment is made for local time variations, such as Daylight Saving Time. Other types of local variation may cause clocksecs and stampsecs to contain irreconcilable values.)

   SET stamp\=$ZTIMESTAMP,clock\=$HOROLOG
   WRITE !,"local date and time: ",$ZDATETIME(clock,1,1,2)
   WRITE !,"UTC date and time:   ",$ZDATETIME(stamp,1,1,2)
   IF $PIECE(stamp,",") '= $PIECE(clock,",") {
     WRITE !,"Time difference affects date" }
   SET clocksecs\=$EXTRACT(clock,7,11)
   SET stampsecs\=$EXTRACT(stamp,7,11)\-($ZTIMEZONE\*60)
   IF clocksecs\=stampsecs {
      WRITE !,"No local time variant" 
      WRITE !,"Local time is timezone time" }
   ELSE {
         SET stampsecs\=stampsecs+3600
         IF clocksecs\=stampsecs {
           WRITE !,"Daylight Saving Time variant:"
           WRITE !,"Local time offset 1 hour from timezone time" }
         ELSE { WRITE !,"Cannot reconcile due to local time variant" }
   }
   QUIT

 

See Also

*   [$NOW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fnow) function
    
*   [$ZDATETIME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetime) function
    
*   [$ZDATETIMEH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetimeh) function
    
*   [$HOROLOG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog) special variable
    
*   [$ZTIMEZONE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztimezone) special variable
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzstorage "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztimezone "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_vztimestamp.xml**