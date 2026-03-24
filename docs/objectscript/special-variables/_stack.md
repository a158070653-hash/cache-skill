# $STACK

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_vstack

---

 

Caché ObjectScript Reference

$STACK

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vroles "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vstorage "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Special Variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_VARIABLES "Caché ObjectScript Special Variables") \]  >  \[ [$STACK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vstack "$STACK") \]

Go to: Description Examples Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Contains the number of context frames saved on the call stack.

Synopsis

$STACK
$ST

Description

$STACK contains the number for context frames currently saved on the call stack for your process. You can also look at $STACK as the zero-based context level number of the currently executing context. Therefore, when a Caché job is started, before any contexts have been saved on the call stack, the value of $STACK is zero (0).

Each time a routine calls another routine with a DO command, the context of the currently executing routine is saved on the call stack and execution starts in the newly created context of the called routine. The called routine can, in turn, call another routine and so on. Each additional call causes another saved context to be placed on the call stack.

An XECUTE command and a user-defined function reference also establish a new execution context. A GOTO command does not.

As new contexts are created by DO commands, XECUTE commands, or user-defined function references, the value of $STACK is incremented. As contexts are exited with the QUIT command, previous context are restored from the call stack and the value of $STACK is decremented.

This special variable cannot be modified using the SET command. Attempting to do so results in a <SYNTAX> error.

[$ESTACK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vestack) is identical to $STACK, except that you can establish a $ESTACK level of 0 (zero) at any point by issuing a NEW $ESTACK command. You cannot NEW the $STACK special variable.

Examples

The following example demonstrates how the value of $STACK is incremented as new contexts are create and decremented as contexts are exited.

The sample code is as follows:

STA
  WRITE !,"Context level in routine STA = ",$STACK
  DO A
  WRITE !,"Context level after routine A = ",$STACK
  QUIT
A
  WRITE !,"Context level in routine A = ",$STACK
  DO B
  WRITE !, "Context level after routine B = ",$STACK
  QUIT
B
  WRITE !,"Context level in routine B = ",$STACK
  XECUTE "WRITE !,""Context level in XECUTE = "",$STACK" 
  WRITE !,"Context level after XECUTE = ",$STACK
  QUIT

 

A sample session using this code might run as follows:

USER>DO ^STA 
Context level in routine STA = 1 
Context level in routine A = 2 
Context level in routine B = 3 
Context level in XECUTE = 4 
Context level after XECUTE = 3 
Context level after routine B = 2 
Context level after routine A = 1

Notes

Context levels in Application Mode and Programmer Mode

A routine that is invoked in application mode starts at a different context level than a routine invoked from the programmer mode prompt with a DO command. The DO command typed at the programmer mode prompt causes a new context to be created. The following example shows the routine START invoked in application mode and in programmer mode.

Consider the following routine:

START
  ; Display the context level and exit
  WRITE !,"Context level in routine START is ",$STACK
  QUIT

When you run START in application mode, you see the following display:

Context level in routine START is 0

When you run START in programmer mode (by issuing DO ^START at the terminal prompt), you see the following display:

Context level in routine START is 1

See Also

*   [$STACK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fstack) function
    
*   [$ESTACK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vestack) special variable
    
*   [Error Handling](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_errors) in Using Caché ObjectScript
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vroles "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vstorage "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_vstack.xml**