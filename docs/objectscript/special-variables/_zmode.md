# $ZMODE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_vzmode

---

 

Caché ObjectScript Reference

$ZMODE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzjob "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzname "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Special Variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_VARIABLES "Caché ObjectScript Special Variables") \]  >  \[ [$ZMODE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzmode "$ZMODE") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Contains current I/O device OPEN parameters.

Synopsis

$ZMODE
$ZM

Description

$ZMODE contains the parameters specified with the OPEN or USE command for the current device. The string returned contains the parameters used to open the current I/O device in canonical form. These parameter values are separated by backslash delimiters. Open parameters like "M" on TCP/IP IO are canonicalized to "PSTE". The "Y" and "K" parameter values are always placed last.

This special variable cannot be modified using the SET command. Attempting to do so results in a <SYNTAX> error.

Examples

The following example uses $ZMODE to return the parameters of the current device:

  WRITE !,"The current OPEN modes are: ",$PIECE($ZMODE,"\\")
  WRITE !,"The collation is: ",$PIECE($ZMODE,"\\",2)
  WRITE !,"The network encoding is: ",$PIECE($ZMODE,"\\",4)

 

The following example sets parameters for the current device with the USE command. It checks the current parameters with $ZMODE before and after the USE command. To test whether a specific parameter was set, this example uses the $PIECE function with the backslash delimiter, and tests for a value using the Contains operator (\[). (See [Operators](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_operators) in Using Caché ObjectScript.):

Zmodetest
  WRITE !, $ZMODE
    IF $PIECE($ZMODE,"\\")\["S" {
      WRITE !, "S is set"  }
    ELSE {WRITE !, "S is not set" }
  USE 0:("":"IS":$CHAR(13,10))
  WRITE !, $ZMODE
    IF $PIECE($ZMODE,"\\")\["S" {
      WRITE !, "S is set"  }
    ELSE {WRITE !, "S is not set" }
  QUIT  

SAMPLES>DO ^zmodetest

RY\\Latin1\\K\\UTF8\\

S is not set

SIRY\\Latin1\\K\\UTF8\\

S is set

See Also

*   [OPEN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_copen) command
    
*   [USE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cuse) command
    
*   [$IO](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vio) special variable
    
*   [I/O Devices and Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_iodevcomms) in Caché I/O Device Guide
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzjob "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzname "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_vzmode.xml**