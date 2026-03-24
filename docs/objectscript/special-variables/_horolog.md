# $HOROLOG

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_vhorolog

---

 

Caché ObjectScript Reference

$HOROLOG

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhalt "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vio "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Special Variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_VARIABLES "Caché ObjectScript Special Variables") \]  >  \[ [$HOROLOG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog "$HOROLOG") \]

Go to: Description Examples Historical Note See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Contains the local date and time for the current process.

Synopsis

$HOROLOG
$H

Description

$HOROLOG contains the date and time for the current process. It can contain the following values:

*   The current local date and time.
    
*   The current local date and time, adjusted for a different time zone offset.
    
*   A user-specified non-incrementing date. Time continues to be the current local time.
    

$HOROLOG contains a character string that consists of two integer values, separated by a comma. These two integers represent the current local date and time in Caché storage format. These integers are counters, not user-readable dates and times. $HOROLOG returns the current date and time in the following format:

ddddd,sssss

The first integer, ddddd, is the current date expressed as a count of the number of days since December 31, 1840, where day 1 is January 1, 1841. Because Caché represents dates using a counter from an arbitrary starting point, Caché is unaffected by the Year 2000 boundary. The maximum value for this date integer is 2980013, which corresponds to December 31, 9999.

The second integer, sssss, is the current time, expressed as a count of the number of seconds since midnight of the current day. The system increments the time field from 0 to 86399 seconds. When it reaches 86399 at midnight, the system resets the time field to 0 and increments the date field by 1. $HOROLOG truncates fractional seconds; it represents time in whole seconds only.

You can obtain the same current date and time information by invoking the [Horolog()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SYS#Horolog) method, as follows:

   WRITE $SYSTEM.SYS.Horolog()

 

Refer to [%SYSTEM.SYS](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SYS) in the InterSystems Class Reference for further details.

Separating Date and Time

To get just the date portion or just the time portion of $HOROLOG, you can use the [$PIECE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fpiece) function, specifying the comma as the delimiter character:

   SET dateint\=$PIECE($HOROLOG,",",1)
   SET timeint\=$PIECE($HOROLOG,",",2)
   WRITE !,"Date and time: ",$HOROLOG
   WRITE !,"Date only: ",dateint
   WRITE !,"Time only: ",timeint

 

To get just the date portion of a $HOROLOG value, you can also use the following programming trick:

   SET dateint\=+$HOROLOG
   WRITE !,"Date and time: ",$HOROLOG
   WRITE !,"Date only: ",dateint

 

The plus sign (+) causes Caché to parse the $HOROLOG string as a number. When Caché encounters a nonnumeric character (the comma), it truncates the rest of the string and returns the numeric portion. This is the date integer portion of the string.

Date and Time Functions Compared

The various ways to return the current date and time are compared, as follows:

*   $HOROLOG contains the local, variant-adjusted date and time in Caché storage format. The local time zone is determined from the current value of the $ZTIMEZONE special variable, and then adjusted for local time variants, such as Daylight Saving Time. It returns whole seconds only; fractions of a second are truncated.
    
*   $NOW returns the local date and time for the current process. $NOW returns the date and time in Caché storage format. It includes fractional seconds; the number of fractional digits is the maximum precision supported by the current operating system.
    
    *   $NOW() determines the local time zone from the value of the $ZTIMEZONE special variable. The local time is not adjusted for local time variants, such as Daylight Saving Time. It therefore may not correspond to local clock time.
        
    *   $NOW(tzmins) returns the time and date that correspond to the specified tzmins time zone parameter. The value of $ZTIMEZONE is ignored.
        
*   $ZTIMESTAMP contains the UTC (Coordinated Universal Time) date and time, with fractional seconds, in Caché storage format. Fractional seconds are expressed in three digits of precision (on Windows systems), or six digits of precision (on UNIX® systems).
    

Date and Time Conversions

You can use the $ZDATE function to convert the date portion of $HOROLOG into external, user-readable form. You can use the $ZTIME function to convert the time portion of $HOROLOG into external user-readable form. You can use the $ZDATETIME function to convert both the date and time. When using $HOROLOG, setting the precision for time values in these functions always returns zeros as fractional seconds.

You can use the $ZDATEH function to convert a user-readable date into the date portion of $HOROLOG. You can use the $ZTIMEH function to convert a user-readable time into the time portion of $HOROLOG. You can use the $ZDATETIMEH function to convert both the date and time to a $HOROLOG value.

Setting the Date and Time

$HOROLOG can be set to a user-specified date for the current process using the [FixedDate()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process#FixedDate) method of the [%SYSTEM.Process](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process) class. $HOROLOG cannot be modified using the SET command. Attempting to do so results in a <SYNTAX> error.

   DO ##class(%SYSTEM.Process).FixedDate(12345)  // set $HOROLOG date
   WRITE !,$ZDATETIME($HOROLOG,1,1,9)," $HOROLOG changed date"
   WRITE !,$ZDATETIME($NOW(),1,1,9)," $NOW() no date change"
   WRITE !,$ZDATETIME($ZDATETIMEH($ZTIMESTAMP,\-3),1,1,9)," $ZTS UTC-to-local",
           " no date change"
   DO ##class(%SYSTEM.Process).FixedDate(0)    // restore $HOROLOG
   WRITE !,$ZDATETIME($HOROLOG,1,1,9)," $HOROLOG current date"

 

Note that FixedDate() changes the $HOROLOG value, but not the $NOW or $ZTIMESTAMP value.

Time Zone

By default, $HOROLOG contains the date and time for the local time zone. This time zone default is supplied by the operating system, which Caché uses to set the $ZTIMEZONE default.

Changing $ZTIMEZONE affects the value of $HOROLOG for the current process. It changes the time portion of $HOROLOG, and this change of time can also change the date portion of $HOROLOG. $ZTIMEZONE is a fixed offset of time zones from the Greenwich meridian; it does not adjust for local seasonal time variants, such as Daylight Saving Time.

Daylight Saving Time

$HOROLOG adjusts for seasonal time variants based on the algorithm supplied by the underlying operating system. After applying the $ZTIMEZONE value, Caché uses the operating system local time to adjust $HOROLOG (if needed) for seasonal time variants, such as Daylight Saving Time.

You can determine if Daylight Saving Time is in effect for the current date, or for a specified date and time using the [IsDST()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Util#IsDST) method. The following example returns the Daylight Saving Time (DST) status for the current date and time. Because this status could change while the program is running, this example checks it twice:

CheckDST
  SET x\=$SYSTEM.Util.IsDST()
  SET local\=$ZDATETIME($HOROLOG)
  SET x2\=$SYSTEM.Util.IsDST()
  GOTO:x'=x2 CheckDST
  IF x\=1 {WRITE local," DST in effect"}
  ELSEIF x\=0 {WRITE local," DST not in effect"}
  ELSE {WRITE local," DST setting cannot be determined"}

 

The application of seasonal time variants may differ based on (at least) three considerations:

*   Operating system: Within a time zone, $HOROLOG for a given date may differ on different computers. This is because different operating systems use different algorithms to apply time variants. Because policies governing the beginning and end dates for Daylight Saving Time (and other time variants) have changed, older operating systems may not reflect current practice, and/or calculations using older $HOROLOG values may be adjusted using the current beginning and end dates, rather than the ones in force at that time.
    
*   Government policies have changed over time: There have been numerous changes to seasonal time variants since their first adoption in 1916 (much of Europe) and 1918 (United States). Daylight Saving Time has been adopted, rejected, and re-adopted by governmental policies in many places. The seasonal start and end dates for Daylight Saving Time have also changed numerous times. In the United States, recent changes of national policy have occurred in 1966, 1974–75, 1987, and 2007. Adoption of, or exemption from, national policies have also occurred due to local legislative actions. For example, the state of Arizona does not observe Daylight Saving Time.
    
*   Geography: Daylight Saving Time is summer time; the local clock shifts forwards (“Spring ahead”) at the start of DST and shifts backwards (“Fall back”) at the end of DST. Thus the calendar start and end dates for Daylight Saving Time within the same time zone are commonly reversed in the northern hemisphere and the southern hemisphere. Equatorial nations and most of Asia and Africa do not observe Daylight Saving Time.
    

Local Time Variant Thresholds

$HOROLOG calculates the number of seconds from midnight by consulting the system clock. Therefore, if the system clock is automatically reset when crossing a local time variant threshold, such as the beginning or end of Daylight Saving Time, the time value of $HOROLOG also shifts abruptly ahead or back by the appropriate number of seconds. For this reason, comparisons of two $HOROLOG time values may yield unanticipated results if the period between the two values includes a local time variant threshold.

$NOW does not adjust for local time variants. Its use may be preferable when comparing date and time values if the period between the two values includes a local time variant threshold.

Dates Before 1840

$HOROLOG cannot be directly used to represent dates outside of the range of years 1840 through 9999. However, you can represent historic dates far beyond this range using the Caché SQL Julian date feature. Julian dates can represent a date as an unsigned integer, counting from 4711 BC (BCE). Julian dates do not have a time-of-day component.

You can convert a Caché $HOROLOG date to a Caché Julian date using the [TO\_CHAR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_tochar) SQL function, or the [TOCHAR()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#TOCHAR) method of the [%SYSTEM.SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL) class. You can convert a Caché Julian date to a Caché $HOROLOG date using the [TO\_DATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_todate) SQL function, or the [TODATE()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#TODATE) method of the [%SYSTEM.SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL) class.

The following example takes the current $HOROLOG date and converts it to a Julian date. The + before $HOROLOG forces Caché to treat it as a number, and thus truncate at the comma, eliminating the time integer:

  WRITE !,"Horolog date = ",+$H
  SET x\=$SYSTEM.SQL.TOCHAR(+$HOROLOG,"J")
  WRITE !,"Julian date = ",x

 

The following example takes a Julian date and converts it to a Caché $HOROLOG date:

  SET x\=$SYSTEM.SQL.TODATE(2455030,"J")
  WRITE !,"$HOROLOG date = ",x," = ", $ZDATE(x,1)

 

Note that Julian date values smaller than 1721100 cannot be converted; an <ILLEGAL VALUE> error is generated.

For further information on Julian dates, refer to [TO\_DATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_todate) and [TO\_CHAR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_tochar) in the Caché SQL Reference.

For information on the starting date of $HOROLOG, see the section “[Historical Note](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog#RCOS_vhorolog_historical_note).”

Examples

The following example displays the current contents of $HOROLOG.

   WRITE $HOROLOG

 

This returns a value formatted like this: 62210,49170

The following example uses $ZDATE to convert the date field in $HOROLOG to a date format.

   WRITE $ZDATE($PIECE($HOROLOG,",",1))

 

returns a value formatted like this: 04/29/2011

The following example converts the time portion of $HOROLOG to a time in the form of hours:minutes:seconds on a 12-hour (a.m. or p.m.) clock.

CLOCKTIME    
  NEW
  SET Time\=$PIECE($HOROLOG,",",2)
  SET Sec\=Time#60
  SET Totmin\=Time\\60
  SET Min\=Totmin#60
  SET Milhour\=Totmin\\60
  IF Milhour\=12 { SET Hour\=12,Meridian\=" pm" }
  ELSEIF Milhour\>12 { SET Hour\=Milhour\-12,Meridian\=" pm" }
  ELSE { SET Hour\=Milhour,Meridian\=" am" }
  WRITE !,Hour,":",Min,":",Sec,Meridian
  QUIT

 

Historical Note

In the “Just Ask!” column of the September 1993 issue of “M Computing”, a publication of the M Technology Association, Silver Spring, MD 20903, James M. Poitras explained the choice of starting date for $HOROLOG:

    "Starting in early 1969, our group created the Chemistry Lab
     application at Massachusetts General Hospital (MGH), which was
     the first package in the MGH MUMPS with Global Data Storage and
     many of the features of the language today..."

    "When we started programming, there were no utility programs of
     any type. We had to write them all: time, date, verify database,
     global tally, print routine, etc. I ended up writing initial
     versions of most of these.

    "When I decided on specifications for the date routine, I
     remembered reading of the oldest (one of the oldest?)
     U.S. citizen, a Civil War veteran, who was 121 years old at the
     time. Since I wanted to be able to represent dates in a
     Julian-type form so that age could be easily calculated and to be
     able to represent any birth date in the numeric range selected, I
     decided that a starting date in the early 1840s would be 'safe'.
     Since my algorithm worked most logically when every fourth year
     was a leap year, the first year was taken as 1841. The zero point
     was then December 30, 1840...

    "That's the origin of December 31, 1840 or January 1, 1841. I
      wasn't party to the MDC (M Development Committee) negotiations,
      but I did explain the logic of my choice to members of the
      Committee." 

See Also

*   [$NOW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fnow) function
    
*   [$ZDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdate) function
    
*   [$ZDATEH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdateh) function
    
*   [$ZDATETIME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetime) function
    
*   [$ZDATETIMEH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzdatetimeh) function
    
*   [$ZTIME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fztime) function
    
*   [$ZTIMEH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fztimeh) function
    
*   [$ZTIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztimestamp) special variable
    
*   [$ZTIMEZONE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztimezone) special variable
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhalt "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vio "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_vhorolog.xml**