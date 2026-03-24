# $ZTIMEZONE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_vztimezone

---

 

Caché ObjectScript Reference

$ZTIMEZONE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztimestamp "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztrap "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Special Variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_VARIABLES "Caché ObjectScript Special Variables") \]  >  \[ [$ZTIMEZONE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztimezone "$ZTIMEZONE") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Contains the time zone offset from the Greenwich meridian.

Synopsis

$ZTIMEZONE
$ZTZ

Description

$ZTIMEZONE can be used in two ways:

*   To return the local time zone offset for your computer.
    
*   To [set the local time zone](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztimezone#RCOS_vztimezone_set) offset for the current process.
    

$ZTIMEZONE contains the time zone offset from the Greenwich meridian in minutes. (The Greenwich meridian includes all of Great Britain and Ireland.) This offset is expressed as a signed integer in the range -1440 to 1440. Time zones west of Greenwich are specified as positive numbers; time zones east of Greenwich are specified as negative numbers. (Time zones must be expressed in minutes, because not all time zone differences are in whole hours.) By default, $ZTIMEZONE is initialized to the time zone set for the computer’s operating system.

Caution:

$ZTIMEZONE adjusts the local time by a fixed offset. It does not adjust for Daylight Saving Time or other local time variants. Caché takes its local time from the underlying operating system, which applies the local time variant for the location configured for that computer. Therefore, a local time adjusted using $ZTIMEZONE takes its local time variation from the configured locale, not the time zone specified in $ZTIMEZONE.

UTC time is calculated using a count of time zones from the Greenwich meridian ($ZTIMEZONE\=0). It is not the same as local Greenwich time. The term Greenwich Mean Time (GMT) may be confusing; local time at Greenwich is the same as UTC during the winter; during the summer it differs from UTC by one hour. This is because a local time variant, known as British Summer Time, is applied.

For functions and programs that use $ZTIMEZONE, elapsed local time is always continuous, but the time value may need to be seasonally adjusted to correspond to local clock time. For more details on local seasonal time variants, refer to [$HOROLOG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog).

Setting the Time Zone

You can use $ZTIMEZONE to set the time zone used by the current Caché process. Setting $ZTIMEZONE does not change the default Caché time zone or your computer’s time zone setting.

Note:

Changing the $ZTIMEZONE special variable is a feature designed for some special situations. Changing $ZTIMEZONE is not a consistent way to change the time zone that Caché uses for local date/time operations. The $ZTIMEZONE special variable should not be changed except by those programs that are prepared to handle all the inconsistencies that result.

On some platforms there may be a better way to change time zones than changing the $ZTIMEZONE special variable. If the platform has a process-specific time zone setting (for example, the TZ environment variable on POSIX systems, or the SYS$TIME\_ZONE logical name on OpenVMS systems) then making an external system call to change the process-specific time zone may work better than changing $ZTIMEZONE. Changing the process-specific time zone at the operating system level will change both the local time offset from UTC and apply the corresponding algorithm that determines when local time variants are applied. This is especially important if the default system time zone is in the Northern Hemisphere, while the desired process time zone is in the Southern Hemisphere. Changing $ZTIMEZONE changes the local time to a new time zone offset from UTC, but the algorithm that determines when local time variants are applied remains unchanged.

Use the SET command to set $ZTIMEZONE to a specified signed integer number of minutes. Leading zeros and decimal portions of numbers are ignored. If you specify a nonnumeric value or no value when setting $ZTIMEZONE, Caché sets $ZTIMEZONE to 0 (Greenwich meridian).

For example, North American Eastern Standard Time (EST) is five hours west of Greenwich. Therefore, to set the current Caché process to EST you would specify 300 minutes. To specify a time zone one hour east of Greenwich, you would specify –60 minutes. To specify Greenwich itself, you would specify 0 minutes.

Setting $ZTIMEZONE:

*   affects the argumentless $NOW() local time value. It changes the time portion of $NOW(), and this change of time can also change the date portion of $NOW() for the current process. $NOW() reflects the $ZTIMEZONE setting exactly, its value is not adjusted for local time variants.
    
*   affects the $HOROLOG local time value. $HOROLOG takes its time zone value from $ZTIMEZONE, then seasonally adjusts for local time variants such as Daylight Saving Time. $HOROLOG therefore always conforms to local clock time, but $HOROLOG elapsed time is not continuous throughout the year.
    
*   affects the [%SYSTEM.Util](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Util) class methods [IsDST()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Util#IsDST), [UTCtoLocalWithZTIMEZONE()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Util#UTCtoLocalWithZTIMEZONE), and [LocalWithZTIMEZONEtoUTC()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Util#LocalWithZTIMEZONEtoUTC).
    
*   does not affect $ZTIMESTAMP or $ZHOROLOG values.
    
*   does not affect the date and time format conversions performed by $ZDATE, $ZDATEH, $ZDATETIME, $ZDATETIMEH, $ZTIME, and $ZTIMEH functions.
    
*   does not affect the $NOW(n) function.
    
*   does not affect the [FixedDate()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process#FixedDate) class method of the [%SYSTEM.Process](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process) class, which sets the date in $HOROLOG to a fixed value.
    

The following anomalies occur after changing $ZTIMEZONE:

*   $ZDATETIME($HOROLOG,1,7) usually returns an UTC time, but it will not return UTC time if $ZTIMEZONE has changed.
    
*   $ZDATETIME($HOROLOG,1,5) will not return the correct time zone offset if $ZTIMEZONE has changed.
    
*   $ZDATETIME($HOROLOG,-3) and $ZDATETIMEH($ZTIMESTAMP,-3) conversions between Local time and UTC time will not be correct if $ZTIMEZONE has changed.
    

Other Time Zone Methods

You can obtain the same time zone information by invoking the [TimeZone()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SYS#TimeZone) class method, as follows:

   WRITE $SYSTEM.SYS.TimeZone()

 

Refer to the [%SYSTEM.SYS](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SYS) class in the InterSystems Class Reference for further details.

You can return your local time variation as part of a date and time string by using the $ZDATETIME and $ZDATETIMEH functions with tformat values 5 or 6, as shown in the following example:

   WRITE !,$ZDATETIME($HOROLOG,1,5)

 

This returns a value such as:

04/06/2011T12:31:16-04:00

The last part of this string (\-04:00) indicates the system’s local time variation setting as an offset in hours and minutes from the Greenwich meridian. Note that this variation is not necessarily the time zone offset. In the above case, the time zone is 5 hours west of Greenwich (\-5:00), but the local time variant (Daylight Saving Time) offsets the time zone time by one hour to \-04:00. Setting $ZTIMEZONE changes the current process date and time returned by $ZDATETIME($HOROLOG,1,5), but does not change the system local time variation setting.

$ZDATETIMEH Uses Time Zone Setting

You can use $ZDATETIMEH with dformat\=-3 to convert a Coordinated Universal Time (UTC) date and time value to a local time. This function takes as input a UTC value ($ZTIMESTAMP). It uses the local time zone setting to return the corresponding date and time with local time variants (such as Daylight Saving Time) applied where applicable.

   SET clock\=$HOROLOG
   SET stamp\=$ZDATETIMEH($ZTIMESTAMP,\-3)
   WRITE !,"local/local date and time: ",$ZDATETIME(clock,1,1,2)
   WRITE !,"UTC/local date and time:   ",$ZDATETIME(stamp,1,1,2)

 

Local/UTC Conversion Methods Using $ZTIMEZONE

Two class methods of the [%SYSTEM.Util](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Util) class convert between local date and time and UTC date and time: [UTCtoLocalWithZTIMEZONE()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Util#UTCtoLocalWithZTIMEZONE) and [LocalWithZTIMEZONEtoUTC()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Util#LocalWithZTIMEZONEtoUTC). These methods are affected by changes to $ZTIMEZONE.

  WRITE $SYSTEM.Util.UTCtoLocalWithZTIMEZONE($ZTIMESTAMP),!
  WRITE $HOROLOG,!
  WRITE $SYSTEM.Util.LocalWithZTIMEZONEtoUTC($H),!
  WRITE $ZTIMESTAMP 

 

Examples

The following example returns your current time zone:

   SET zone\=$ZTIMEZONE
   IF zone\=0 {
     WRITE !,"Your time zone is Greenwich Mean Time" }
   ELSEIF zone\>0 {
     WRITE !,"Your time zone is ",zone/60," hours west of Greenwich" }
   ELSE {
     WRITE !,"Your time zone is ",(\-zone)/60," hours east of Greenwich" }

 

The following example shows that setting the time zone can change the date as well as the time:

   SET zonesave\=$ZTIMEZONE
   WRITE !,"Date in my current time zone:  ",$ZDATE($HOROLOG)
   IF $ZTIMEZONE\=0 {
     SET $ZTIMEZONE\=720 }
   ELSEIF $ZTIMEZONE\>0 {
     SET $ZTIMEZONE\=($ZTIMEZONE\-720) }
   ELSE {
     SET $ZTIMEZONE\=($ZTIMEZONE+720) }
   WRITE !,"Date halfway around the world: ",$ZDATE($HOROLOG)
   WRITE !,"Date at Greenwich Observatory: ",$ZDATE($ZTIMESTAMP)
   SET $ZTIMEZONE\=zonesave

 

The following example determines if your local time is the same as your time zone time:

   SET localnow\=$HOROLOG, stamp\=$ZTIMESTAMP
   WRITE !,"local date and time: ",$ZDATETIME(localnow,1,1)
   SET clocksecs\=$PIECE(localnow,",",2)
   SET stampsecs\=$EXTRACT(stamp,7,11)\-($ZTIMEZONE\*60)
   IF clocksecs\=stampsecs {
      WRITE !,"No local time variant:" 
      WRITE !,"Local time is timezone time" }
   ELSE {
         IF clocksecs\=stampsecs+3600 {
           WRITE !,"Daylight Saving Time variant:"
           WRITE !,"Local time offset 1 hour from timezone time" }
         ELSE { WRITE !,"Local time and time zone time are "
                WRITE !,(clocksecs\-stampsecs)/60," minutes different" }
   }
   QUIT

 

See Also

*   [$HOROLOG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog) special variable
    
*   [$ZTIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztimestamp) special variable
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztimestamp "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztrap "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_vztimezone.xml**