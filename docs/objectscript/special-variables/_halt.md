# $HALT

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_vhalt

---

 

Caché ObjectScript Reference

$HALT

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vetrap "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Special Variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_VARIABLES "Caché ObjectScript Special Variables") \]  >  \[ [$HALT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhalt "$HALT") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Contains a halt trap routine call.

Synopsis

$HALT

Description

$HALT contains the name of the current halt trap routine. A halt trap routine is called by your application when a HALT command is encountered. This halt trap routine may perform clean up or logging processing before issuing a HALT command, or it may substitute other processing rather than halting program execution.

You set $HALT to a halt trap routine using the SET command. The halt trap routine is specified by a quoted string with the following format:

SET $HALT=location

Here location can be specified as label (a [label](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_syntax#GCOS_syntax_labels) in the current routine or procedure), ^routine (the beginning of a specified external routine), or label^routine (a specified label in a specified external routine).

$HALT supports label+offset in some contexts (but not in procedures). This optional +offset is an integer specifying the number of lines to offset from label. InterSystems recommends that you avoid the use of a line offset when specifying location.

You cannot specify an +offset when calling a procedure or a CACHESYS % routine. If you attempt to do so, Caché issues a <NOLINE> error.

$HALT defines a halt trap routine for the current context. If there is already a halt trap defined for the current context, the new one replaces it. If you specify a nonexistent routine name, a HALT command ignores that $HALT and unwinds the stack to locate a valid $HALT at a previous context level.

To remove the halt trap for the current context, set $HALT to a null string. Attempting to remove a halt trap by using the NEW or KILL commands results in a <SYNTAX> error.

Halt Trap Execution

When you issue a HALT command, Caché checks the current context for $HALT. If no $HALT is defined for the current context (or it is set to a nonexistent routine name or the null string), Caché unwinds the stack to the previous context and looks for $HALT there. This process continues until either a defined $HALT is located or the stack is completely unwound. Caché uses the value of $HALT to transfer execution to the specified halt trap routine. The halt trap routine executes in the context at which $HALT was defined. No error code is set or error message issued.

If no valid $HALT is set in the current context or previous contexts, issuing a HALT command completely unwinds the stack and performs an actual program halt.

Commonly, a halt trap routine performs some cleanup or reporting processing, and then issues a HALT command. Note that with $HALT defined, the original HALT command invokes the halt trap, but does not perform an actual program halt. For an actual halt to occur, the halt trap routine must contain a second HALT command.

A HALT command issued by a halt trap routine is not trapped by that halt trap, but it may be trapped by a halt trap established at a lower context level. Thus a cascading series of halt traps may be invoked by a single HALT command.

Similar processing is performed by the error trap command [ZTRAP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cztrap), and the associated [$ZTRAP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztrap) and [$ETRAP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vetrap) special variables.

$HALT and ^%ZSTOP

If you have $HALT set and also have code defined for ^%ZSTOP when a HALT is issued, the $HALT is executed first. $HALT can prevent the termination of the process, if its halt trap routine does not contain a HALT command.

A ^%ZSTOP routine is executed when the process is actually terminating. For further details on ^%ZSTOP, see the section on “[Using the Caché ^%ZSTART and ^%ZSTOP Routines](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSTU_customize#GSTU_customize_startstop)” in Caché Specialized System Tools and Utilities.

Examples

The following example uses $HALT to establish a halt trap:

   SET $HALT\="MyTrap^CleanupRoutine"
   WRITE !,"the halt trap is: ",$HALT

 

Note that it is the programmer’s responsibility to make sure that the specified routine exists.

The following example shows how the halt trap routine executes in the context at which $HALT was defined. In this example, $HALT is defined at $ESTACK level 0, HALT is issued at $ESTACK level 1, and the halt trap routine executes at $ESTACK level 0.

Main
   NEW $ESTACK
   SET $HALT\="OnHalt"
   WRITE !,"Main $ESTACK= ",$ESTACK," $HALT= ",$HALT   // 0
   DO SubA
   WRITE !,"Returned from SubA"   // not executed
   QUIT
SubA
   WRITE !,"SubA $ESTACK= ",$ESTACK," $HALT= ",$HALT   // 1
   HALT
   WRITE !,"this should never display"
   QUIT
OnHalt
   WRITE !,"OnHalt $ESTACK= ",$ESTACK   // 0
   HALT
   QUIT

 

The following example is identical to the previous example, except that $HALT is defined at $ESTACK level 1. A HALT command is issued at $ESTACK level 1, and the halt trap routine executes at $ESTACK level 1. The HALT issued by the halt trap routine unwinds the stack, and, failing to find a $HALT defined at the previous context level, it halts program execution. Thus, the WRITE command following the DO command is not executed.

Main
   NEW $ESTACK
   WRITE !,"Main $ESTACK= ",$ESTACK," $HALT= ",$HALT   // 0
   DO SubA
   WRITE !,"Returned from SubA"   // not executed
   QUIT
SubA
   SET $HALT\="OnHalt"
   WRITE !,"SubA $ESTACK= ",$ESTACK," $HALT= ",$HALT   // 1
   HALT
   WRITE !,"this should never display"
   QUIT
OnHalt
   WRITE !,"OnHalt $ESTACK= ",$ESTACK   // 1
   HALT
   QUIT

 

The following example shows how a cascading series of halt traps can be invoked. Halt trap Halt0 is defined at $ESTACK level 0, and halt trap Halt1 is defined at $ESTACK level 1. The HALT command is issued at $ESTACK level 2. Caché unwinds the stack to invoke the halt trap Halt1 at $ESTACK level 1. This halt trap issues a HALT command; Caché unwinds the stack to invoke the halt trap Halt0 at $ESTACK level 0. This halt trap issues a HALT command that halts program execution.

Main
   NEW $ESTACK
   SET $HALT\="Halt0"
   WRITE !,"Main $ESTACK= ",$ESTACK," $HALT= ",$HALT   // 0
   DO SubA
   WRITE !,"Returned from SubA"   // not executed
   QUIT
SubA
   SET $HALT\="Halt1"
   WRITE !,"SubA $ESTACK= ",$ESTACK," $HALT= ",$HALT   // 1
   DO SubB
   WRITE !,"Returned from SubA"   // not executed
   QUIT
SubB
   WRITE !,"SubB $ESTACK= ",$ESTACK," $HALT= ",$HALT   // 2
   HALT
   WRITE !,"this should never display"
   QUIT
Halt0
   WRITE !,"Halt0 $ESTACK= ",$ESTACK   // 0
   WRITE !,"Bye-bye!"
   HALT
   QUIT
Halt1
   WRITE !,"Halt1 $ESTACK= ",$ESTACK   // 1
   HALT
   QUIT

 

See Also

*   [HALT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_chalt) command
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vetrap "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhorolog "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_vhalt.xml**