# $PRINCIPAL

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_vprincipal

---

 

Caché ObjectScript Reference

$PRINCIPAL

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vnamespace "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vquit "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Special Variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_VARIABLES "Caché ObjectScript Special Variables") \]  >  \[ [$PRINCIPAL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vprincipal "$PRINCIPAL") \]

Go to: Description Examples Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Contains the ID of the principal I/O device.

Synopsis

$PRINCIPAL
$P

Description

$PRINCIPAL contains the ID of the principal I/O device for the current process. $PRINCIPAL operates like $IO. Refer to $IO for details of specific device types and system platforms.

If the principal device is closed, $PRINCIPAL does not change. If the principal input and output devices differ, $PRINCIPAL reflects the ID of the principal input device.

This special variable cannot be modified using the SET command. Attempting to do so results in a <SYNTAX> error.

Examples

This example uses $PRINCIPAL to test for a principal device.

   IF $PIECE($PRINCIPAL,"|",4) {
     WRITE "Principal device is: ",$PRINCIPAL }
   ELSE  { WRITE "Undefined" }

 

This example uses and writes to the principal device.

   USE $PRINCIPAL 
   WRITE "output to $PRINCIPAL"

 

Notes

$PRINCIPAL and USE 0

$PRINCIPAL is functionally equivalent to the widely used, but nonstandard, USE 0. Use $PRINCIPAL instead of USE 0 because it is standard, and because it makes your code more flexible.

See Also

*   [USE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cuse) command
    
*   [$IO](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vio) special variable
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vnamespace "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vquit "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_vprincipal.xml**