# $ZVERSION

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_vzversion

---

 

Caché ObjectScript Reference

$ZVERSION

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztrap "Go back to the previous section.") 

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Special Variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_VARIABLES "Caché ObjectScript Special Variables") \]  >  \[ [$ZVERSION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzversion "$ZVERSION") \]

Go to: Description Example See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Contains a string describing the current version of Caché.

Synopsis

$ZVERSION
$ZV

Description

$ZVERSION contains a string showing the version of the currently running Caché system.

The following example returns the $ZVERSION string:

   WRITE $ZVERSION

 

This returns a version string such as the following:

Cache for Windows (x86-32) 2010.2.3 (Build 702U) Tue Feb 15 2011 14:14:23 EST

This string includes the type of Caché installation (product and platform, including CPU type), the version number (2010.2.3), the build number within that version (the “U” in the build number indicates a Unicode version), and the date and time that this version of Caché was created. “EST” is Eastern Standard Time (the time zone for the Eastern United States), “EDT” is Eastern Daylight Saving Time (see [Daylight Saving Time](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog#RCOS_vhorolog_dst) for details).

The same information can be viewed by going to the Caché Cube and selecting “About...” or by invoking the [GetVersion()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Version#GetVersion) class method, as follows:

   WRITE $SYSTEM.Version.GetVersion()

 

You can get the component parts of this version string by invoking other [%SYSTEM.Version](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Version) methods, which you can list by invoking:

   DO $SYSTEM.Version.Help()

 

The $ZVERSION special variable cannot be modified using the SET command. Attempting to do so results in a <SYNTAX> error.

Example

The following example extracts the create date from the version string to calculate how old the current version of Caché is, in days. Note that this example is specific to Windows platforms:

   SET createdate\=$PIECE($ZVERSION," ",9,11)
   WRITE !,"Creation date: ",createdate
   WRITE !,"Current date:  ",$ZDATE($HOROLOG,6)
   SET nowcount\=$PIECE($HOROLOG,",")
   SET thencount\=$ZDATEH(createdate,6)
   WRITE !,"This version is ",(nowcount\-thencount)," days old"

 

The following example performs the same operation by calling a class method:

   SET createdate\=$SYSTEM.Version.GetBuildDate()
   WRITE !,"Creation date: ",$ZDATE(createdate,6)
   WRITE !,"Current date:  ",$ZDATE($HOROLOG,6)
   SET nowcount\=$PIECE($HOROLOG,",")
   WRITE !,"This version is ",(nowcount\-createdate)," days old"

 

See Also

*   [$SYSTEM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vsystem) special variable
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztrap "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_vzversion.xml**