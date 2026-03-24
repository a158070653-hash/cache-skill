# $ESTACK

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_vestack

---

 

Caché ObjectScript Reference

$ESTACK

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vecode "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vetrap "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Special Variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_VARIABLES "Caché ObjectScript Special Variables") \]  >  \[ [$ESTACK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vestack "$ESTACK") \]

Go to: Description Examples Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Contains the number of context frames saved on the call stack from a user-defined point.

Synopsis

$ESTACK
$ES

Description

$ESTACK contains the number of context frames saved on the call stack for your job from a user-defined point. You specify this point by creating a new copy of $ESTACK using the NEW command.

The $ESTACK special variable is similar to the $STACK special variable. Both contain the number of context frames currently saved on the call stack for your job or process. Caché increments and restores both when changing context. The major difference is that you can reset the $ESTACK count to zero at any point by using the NEW command. You cannot reset the $STACK count.

Context Frames and Call Stacks

When a Caché image is started, before any contexts have been saved on the call stack, the values of both $ESTACK and $STACK are zero. Each time a routine calls another routine with DO, Caché saves the context of the currently executing routine on the call stack, increments $ESTACK and $STACK, and starts execution of the called routine in the newly created context. The called routine can, in turn, call another routine, and so on. Each time another routine is called, Caché increments $ESTACK and $STACK and places more saved contexts on the call stack.

Issuing a DO command, an XECUTE command, or a call to a user-defined function establishes a new execution context. Issuing a GOTO command does not.

As DO commands, XECUTE commands, or user-defined function references create new contexts, Caché increments the values of $STACK and $ESTACK. As QUIT commands cause contexts to exit, Caché restores the previous contexts from the call stack and decrements the values of $STACK and $ESTACK.

The $ESTACK and $STACK special variables cannot be modified using the SET command. Attempting to do so results in a <SYNTAX> error.

Creating a New $ESTACK

You can create a new copy of $ESTACK in any context by using the NEW command. Caché takes the following actions:

1.  Saves the old copy of $ESTACK.
    
2.  Creates a new copy of $ESTACK with a value of zero (0).
    

In this way, you can establish a particular context as the $ESTACK level 0 context. As you create new contexts with DO, XECUTE, or user-defined functions, Caché increments this $ESTACK value. However, when you exit the context in which the new $ESTACK was created ($ESTACK at level 0), Caché restores the value of the previous copy of $ESTACK.

Examples

The following example shows the effect of a NEW command on $ESTACK. In this example, the MainRoutine displays the initial values of $STACK and $ESTACK (which are the same value). It then calls Sub1. This call increments $STACK and $ESTACK. The NEW command creates a $ESTACK with a value of 0. Sub1 calls Sub2, incrementing $STACK and $ESTACK. Returning to MainRoutine restores the initial values of $STACK and $ESTACK:

Main
   WRITE !,"Initial: $STACK=",$STACK," $ESTACK=",$ESTACK
   DO Sub1
   WRITE !,"Return: $STACK=",$STACK," $ESTACK=",$ESTACK
   QUIT
Sub1
     WRITE !,"Sub1Call: $STACK=",$STACK," $ESTACK=",$ESTACK
     NEW $ESTACK
     WRITE !,"Sub1NEW: $STACK=",$STACK," $ESTACK=",$ESTACK
     DO Sub2
     QUIT
Sub2
     WRITE !,"Sub2Call: $STACK=",$STACK," $ESTACK=",$ESTACK
     QUIT

 

The following example demonstrates how the value of $ESTACK is incremented as new contexts are created by issuing DO and XECUTE commands, and decremented as these contexts are exited. It also shows that a GOTO command does not create a new context or increment $ESTACK:

Main
   NEW $ESTACK
   WRITE !,"Initial Main: $ESTACK=",$ESTACK   // 0
   DO Sub1
   WRITE !,"Return Main: $ESTACK=",$ESTACK   // 0
   QUIT
Sub1
     WRITE !,"Sub1 via DO: $ESTACK=",$ESTACK  // 1
     XECUTE "WRITE !,""Sub1 XECUTE: $ESTACK="",$ESTACK"   // 2
     WRITE !,"Sub1 post-XECUTE: $ESTACK=",$ESTACK   // 1
     GOTO Sub2
Sub1Return
     WRITE !,"Sub1 after GOTO: $ESTACK=",$ESTACK  // 1
     QUIT
Sub2
     WRITE !,"Sub2 via GOTO: $ESTACK=",$ESTACK   // 1
     GOTO Sub1Return

 

Notes

Context Levels in Programmer and Application Modes

A routine invoked in application mode starts at a different context level than a routine you invoke with the DO command in programmer mode. When you enter a DO command at the Caché Terminal programmer mode prompt, Caché creates a new context for the routine called.

The routine you call can compensate by establishing a $ESTACK level 0 context and then use $ESTACK for all context-level references.

Consider the following routine:

START
    ; Establish a $ESTACK Level 0 Context
  NEW $ESTACK
    ; Display the $STACK context level
  WRITE !,"$STACK level in routine START is ",$STACK
   ; Display the $ESTACK context level and exit
  WRITE !,"$ESTACK level in routine START is ",$ESTACK
  QUIT

When you run START in application mode, you see the following display:

$STACK level in routine START is 0 
$ESTACK level in routine START is 0

When you run START in programmer mode (by issuing DO ^START at the Caché Terminal prompt), you see the following display:

$STACK level in routine START is 1 
$ESTACK level in routine START is 0

$ESTACK and Error Processing

$ESTACK is particularly useful during error processing when error handlers must unwind the call stack to a specific context level. See [Error Handling](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_errors) in Using Caché ObjectScript for more information about error processing.

See Also

*   [$STACK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fstack) function
    
*   [$STACK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vstack) special variable
    
*   [Error Handling](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_errors) in Using Caché ObjectScript
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vecode "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vetrap "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_vestack.xml**