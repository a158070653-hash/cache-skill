# $NOW

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fnow

---

 

Caché ObjectScript Reference

$NOW

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fnormalize "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fnumber "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$NOW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fnow "$NOW") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Returns the local date and time with fractional seconds for the current process.

Synopsis

$NOW(tzmins)

Parameter

tzmins

Optional — A positive or negative integer value that specifies the desired time zone offset from the Greenwich meridian, in minutes. A value of 0 corresponds to the Greenwich meridian. Positive integers correspond to time zones west of Greenwich; negative integers correspond to time zones east of Greenwich. For example, a value of 300 corresponds to United States Eastern Standard Time, 5 hours (300 minutes) west of Greenwich. The range of permitted values is -1440 through 1440; values beyond this range result in an <ILLEGAL VALUE> error.

If you omit tzmins, the $NOW function returns the local date and time based on the $ZTIMEZONE special variable value. The range of $ZTIMEZONE values that the $NOW function supports is -1440 through 1440; values beyond this range result in an <ILLEGAL VALUE> error.

Description

$NOW can return the following:

*   The current local date and time with fractional seconds for the current process.
    
*   The local date and time for a specified time zone, with fractional seconds, for the current process.
    

The $NOW function returns a character string that consists of two numeric values, separated by a comma. The first number is an integer that represents the current local date. The second is a fractional number that represents the current local time. These values are counters, not user-readable dates and times.

$NOW returns the date and time in Caché storage ($HOROLOG) format, with the additional feature of fractional seconds. $NOW returns the current local date and time in the following format: ddddd,sssss.ffffff

The first integer (ddddd) is the number of days since December 31, 1840, where day 1 is January 1, 1841. The maximum value for this date integer is 2980013, which corresponds to December 31, 9999.

The second number (sssss.ffffff) is the number of seconds (and fractional seconds) since midnight of the current day. Caché increments the sssss field from 0 to 86399 seconds. When it reaches 86399 at midnight, Caché resets the sssss field to 0 and increments the date field by 1. The number of ffffff fractional digits is the maximum precision supported by the current operating system. For more on [Windows fractional seconds](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fnow#RCOS_fnow_winmicrosecs), see below.

The $NOW function can be invoked with or without a parameter value. The parentheses are mandatory.

$NOW with no parameter value returns the current local date and time for the current process. It determines the local time zone from the value set in the $ZTIMEZONE special variable. Setting $ZTIMEZONE changes the time portion of $NOW, and this change of time can also change the date portion of $NOW.

Caution:

The $NOW local time value may not correspond to local clock time. $NOW determines local time using the $ZTIMEZONE value. $ZTIMEZONE is continuous throughout the year; it does not adjust for Daylight Saving Time (DST) or other local time variants.

Offset from UTC time is calculated using a count of time zones from the Greenwich meridian. It is not a comparison of your local time with local Greenwich time. The term Greenwich Mean Time (GMT) may be confusing; local time at Greenwich is the same as UTC during the winter; during the summer it differs from UTC by one hour. This is because a local time variant, known as British Summer Time, is applied. $NOW ignores all local time variants.

Also, because $NOW resynchronizes its time value with the system clock, comparisons of time values between $NOW and other Caché time functions and special variables may show slight variations. This variation is limited to 0.05 seconds; however, within this range of variation, comparisons may yield misleading results. For example, WRITE $NOW(),!,$HOROLOG may yield results such as the following:

61438,38794.002085
61438,38793

This anomaly is caused both by the 0.05 second resynchronization variation and by $HOROLOG truncation of fractional seconds.

$NOW with a parameter value returns the time and date that correspond to the time zone specified in tzmins. The value of $ZTIMEZONE is ignored.

Time Functions Compared

The various ways to return the current date and time are compared, as follows:

*   $NOW returns the local date and time for the current process. $NOW returns the date and time in Caché storage format. It includes fractional seconds; the number of fractional digits is the maximum precision supported by the current operating system.
    
    *   $NOW() determines the local time zone from the value of the $ZTIMEZONE special variable. The local time is not adjusted for local time variants, such as [Daylight Saving Time](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog#RCOS_vhorolog_dst). It therefore may not correspond to local clock time.
        
    *   $NOW(tzmins) returns the time and date that correspond to the specified tzmins time zone parameter. The value of $ZTIMEZONE is ignored.
        
*   $HOROLOG contains the local, variant-adjusted date and time in Caché storage format. The local time zone is determined from the current value of the $ZTIMEZONE special variable, and then adjusted for local time variants, such as Daylight Saving Time. It returns whole seconds only; fractions of a second are truncated.
    
*   $ZTIMESTAMP contains the UTC (Coordinated Universal Time) date and time, with fractional seconds, in Caché storage format. Fractional seconds are expressed in three digits of precision (on Windows systems), or six digits of precision (on UNIX® systems).
    

None of these ways to return the current date and time are an exact replacement for the obsolete [$ZUTIL(188)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_replacements) function, which provided local date and time with both adjustment for local time variants and fractional seconds.

Microseconds on Windows

The precision of microseconds on Windows systems is generally less accurate than other platforms. This is due to the limitations of the QueryPerformanceCounter() Windows library routine. Caché determines microseconds by comparing the result computed by QueryPerformanceCounter() with the system clock used for other date/time functions, which has at best 1 millisecond precision (and usually between 1 and 20 milliseconds of accuracy depending on the hardware running Windows.) When the $NOW microsecond precision clock differs from the millisecond precision clock by more than 50 milliseconds, Caché adjusts the microsecond precision clock forward or backwards to agree with the more accurate (but less precise) millisecond precision clock.

Because of this, on Windows, the $NOW function (and legacy [$ZUTIL(188)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_replacements) function) is suitable for measuring microsecond precision time intervals only over a very short duration, lasting no longer than a small number of seconds. On systems with multiple CPU cores, the Caché Windows process for each core will make microsecond adjustments that are local to the individual process. This means that $NOW values returned on different Caché CPU processes can disagree with each other. This discrepancy among CPU processes is more pronounced when Windows is running on a virtual machine that is hosted on a multi-core CPU chip.

Separating Date and Time

To get just the date portion or just the time portion of $NOW, you can use the [$PIECE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fpiece) function, specifying the comma as the delimiter character:

   SET dateval\=$PIECE($NOW(),",",1)
   SET timeval\=$PIECE($NOW(),",",2)
   WRITE !,"Date and time: ",$NOW()
   WRITE !,"Date only: ",dateval
   WRITE !,"Time only: ",timeval

 

Setting the Date

The value returned by $NOW and $ZTIMESTAMP cannot be set using the FixedDate() method of the [%SYSTEM.Process](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process) class.

The value contained in $HOROLOG can be set to a user-specified date for the current process using the [FixedDate()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process#FixedDate) method of the [%SYSTEM.Process](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process) class.

Examples

The following example shows two ways to return the current local date and time:

   WRITE $ZDATETIME($NOW(),1,1,3)," $NOW() date & time",!
   WRITE $ZDATETIME($HOROLOG,1,1,3)," $HOROLOG date & time"

 

Note that $HOROLOG adjusts for local time variants, such as [Daylight Saving Time](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog#RCOS_vhorolog_dst). $NOW does not adjust for local time variants.

The following example uses $ZDATE to convert the date field in $NOW to a date format.

   WRITE $ZDATE($PIECE($NOW(),",",1))

 

returns a value formatted like this: 04/29/2009

The following example converts the time portion of $NOW to a time in the form of hours:minutes:seconds.ffff on a 12-hour (a.m. or p.m.) clock.

CLOCKTIME    
  NEW
  SET Time\=$PIECE($NOW(),",",2)
  SET Sec\=Time#60
  SET Totmin\=Time\\60
  SET Min\=Totmin#60
  SET Milhour\=Totmin\\60
  IF Milhour\=12 { SET Hour\=12,Meridian\=" pm" }
  ELSEIF Milhour\>12 { SET Hour\=Milhour\-12,Meridian\=" pm" }
  ELSE { SET Hour\=Milhour,Meridian\=" am" }
  WRITE !,Hour,":",Min,":",Sec,Meridian
  QUIT

 

See Also

*   [$ZDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdate) function
    
*   [$ZDATEH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdateh) function
    
*   [$ZDATETIME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetime) function
    
*   [$ZDATETIMEH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetimeh) function
    
*   [$ZTIME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fztime) function
    
*   [$ZTIMEH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fztimeh) function
    
*   [$HOROLOG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog) special variable
    
*   [$ZTIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztimestamp) special variable
    
*   [$ZTIMEZONE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztimezone) special variable
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fnormalize "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fnumber "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fnow.xml**