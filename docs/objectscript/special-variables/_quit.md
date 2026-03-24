# $QUIT

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_vquit

---

 

Caché ObjectScript Reference

$QUIT

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vprincipal "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vroles "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Special Variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_VARIABLES "Caché ObjectScript Special Variables") \]  >  \[ [$QUIT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vquit "$QUIT") \]

Go to: Description Example Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Contains a flag indicating what kind of QUIT is required to exit the current context.

Synopsis

$QUIT
$Q

Description

$QUIT contains a value that indicates whether an argumented QUIT command is required to exit from the current context. If an argumented QUIT is required to exit from the current context, $QUIT contains a one (1). If an argumented QUIT is not required to exit from the current context, $QUIT contains a zero (0).

In a context created by issuing a DO or XECUTE command, an argumented QUIT is not required to exit. In a context created by a user-defined function, an argumented QUIT is required to exit.

This special variable cannot be modified using the SET command. Attempting to do so results in a <SYNTAX> error.

Example

The following example demonstrates $QUIT values in a DO context, in an XECUTE context, and in a user-defined function context.

The sample code is as follows:

QUI
  DO
  .  WRITE !,"$QUIT in a DO context = ",$QUIT
  .  QUIT
  XECUTE "WRITE !,""$QUIT in an XECUTE context = "",$QUIT"
  SET A\=$$A
  QUIT
A()
  WRITE !,"$QUIT in a User-defined function context =",$QUIT
  QUIT 1

A sample session using this code might run as follows:

USER>DO ^QUI 
$QUIT in a DO context = 0 
$QUIT in an XECUTE context = 0 
$QUIT in a User-defined function context = 1

Notes

$QUIT and Error Processing

The $QUIT special variable is particularly useful during error processing when the same error handler can be invoked at context levels that require an argumented QUIT and at context levels that require an argumentless QUIT.

See the [Error Handling](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_errors) in Using Caché ObjectScript for more information about error processing.

See Also

*   [DO](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cdo) command
    
*   [QUIT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cquit) command
    
*   [XECUTE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cxecute) command
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vprincipal "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vroles "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_vquit.xml**