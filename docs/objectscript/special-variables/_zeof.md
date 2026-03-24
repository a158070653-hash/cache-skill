# $ZEOF

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_vzeof

---

 

Caché ObjectScript Reference

$ZEOF

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzchild "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzeos "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Special Variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_VARIABLES "Caché ObjectScript Special Variables") \]  >  \[ [$ZEOF](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzeof "$ZEOF") \]

Go to: Description See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Contains flag indicating whether end-of-file has been reached.

Synopsis

$ZEOF

Description

Following each sequential file READ, Caché sets the $ZEOF special variable to indicate whether or not the end of the file has been reached. This special variable is provided for compatibility with MSM routines that use $ZC device status checking.

Caché sets $ZEOF to the file status of the last device used. For example, if you read from a sequential file then write to the principal device, Caché resets $ZEOF from the sequential file end-of-file status to the principal device status. Therefore, you should check the $ZEOF value (and, if necessary, copy it to a variable) immediately after a sequential file READ.

Caché sets $ZEOF to the following values:

–1 End-of-file reached

0 Not at end-of-file

To use this feature, you must disable the <ENDOFFILE> error for sequential files.

*   To disable this for the current process, call the [SetZEOF()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process#SetZEOF) method of the [%SYSTEM.Process](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process) class.
    
*   To disable this system-wide, either set the [SetZEOF](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=Config.Miscellaneous#SetZEOF) property of the Config.Miscellaneous class, or go to the Management Portal and select System Administration, Configuration, Additional Settings, Compatibility (\[Home\] > \[Configuration\] > \[Compatibility Settings\]). View and edit the current setting of SetZEOF. This option controls the behavior when Caché encounters an unexpected end-of-file when reading a sequential file. When set to “true”, Caché sets the $ZEOF special variable to indicate that you have reached the end of the file. When set to “false”, Caché issues an <ENDOFFILE> error. The default is “false”.
    

When the end of a file is reached, rather than issuing an <ENDOFFILE> error, the READ will return a null string, set $ZB\=null and set $ZEOF\=–1.

$ZEOF does not support all of the features of the MSM $ZC function. Unlike $ZC, $ZEOF does not identify file delimiter characters or I/O errors. $ZEOF does not check for proper file termination with file delimiter characters. I/O errors are detected by a READ command error, not by $ZEOF.

You cannot modify this special variable using the SET command. Attempting to do so results in a <SYNTAX> error.

See Also

*   [$ZCHILD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzchild) special variable
    
*   [$ZB](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzb) special variable
    
*   [Sequential File I/O](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_rmsseqfiles) in Caché I/O Device Guide
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzchild "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzeos "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_vzeof.xml**