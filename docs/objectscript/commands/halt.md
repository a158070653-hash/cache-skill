# HALT

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_chalt

---

 

Caché ObjectScript Reference

HALT

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cgoto "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_chang "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_COMMANDS "Caché ObjectScript Commands") \]  >  \[ [HALT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_chalt "HALT") \]

Go to: Description Arguments Examples $SYSTEM.Process.Terminate() HALT and ^RESJOB See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Terminates execution of the current process.

Synopsis

HALT:pc
H:pc

Argument

pc

Optional — A postconditional expression.

Description

The HALT command terminates execution of the current process. If a $HALT special variable is defined in the current context (or a prior context), issuing a HALT command invokes the halt trap routine specified in $HALT, rather than terminating the current process. Typically, a halt trap routine performs some cleanup or reporting operations, then issues a second HALT command to terminate execution.

HALT behaves the same whether it is encountered by running routine code or is entered from the programmer mode prompt. In either case, it terminates the current process.

HALT has the same minimum abbreviation as the HANG command. HANG is distinguished by its required hangtime argument.

Effects of HALT

When HALT terminates a process, the system automatically relinquishes all [locks](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_clock) and closes all devices owned by the process. This ensures that the halted process does not leave behind locked variables or unreleased devices.

If there is a [transaction in progress](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_tp) when HALT terminates a process, the resolution of the transaction depends on the type of process. A HALT in a background job (non-interactive process) always rolls back the transaction in progress. A HALT in an interactive process (such as using Caché Terminal to run a routine) prompts you to resolve the transaction in progress. The prompt is as follows:

You have an open transaction.
Do you want to perform a (C)ommit or (R)ollback? R =>

Specify “C” to commit the current transaction. Specify “R” (or just press the Enter key) to roll back the current transaction.

Halt Traps

Execution of a HALT command is interrupted by a halt trap. Halt traps are established using the $HALT special variable.

If a halt trap has been established for the current context frame, issuing a HALT command invokes the halt trap routine specified by $HALT. The HALT command itself is not executed.

If a halt trap has been established for a lower context frame, a HALT command removes context frames from the frame stack until the context frame with the halt trap is reached. HALT then invokes the halt trap routine specified by $HALT and ceases execution.

Arguments

pc

An optional postconditional expression that can make the command conditional. Caché executes the HALT command if the postconditional expression is true (evaluates to a nonzero numeric value). Caché does not execute the command if the postconditional expression is false (evaluates to zero). For further details, refer to [Command Postconditional Expressions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_commands#GCOS_commands_pc) in Using Caché ObjectScript.

Examples

In the following example, HALT allows the user to end the current application and return to the operating system. The system performs all necessary cleanup for the user. Note the use of the postconditional on the command.

Main
  READ !,"Do you really want to stop (Y or N)? ",ans QUIT:ans\=""
  HALT:(ans\["Y")!(ans\="y")
  DO Start
Start()
  WRITE !,"This is the Start routine"
  QUIT

In the following example, HALT invokes the halt trap routine specified in $HALT. In this case, it is the second HALT command that actually halts execution. (For demonstration purposes, this example uses HANG statements so that you have time to view the displayed output.)

Main
   NEW $ESTACK
   SET $HALT\="OnHalt"
   WRITE !,"Main $ESTACK= ",$ESTACK   // 0
   HANG 2
   DO SubA
   WRITE !,"this should never display"
SubA()
   WRITE !,"SubA $ESTACK= ",$ESTACK   // 1
   HANG 2
   HALT    // invoke the OnHalt routine
   WRITE !,"this should never display"
OnHalt()
   WRITE !,"OnHalt $ESTACK= ",$ESTACK   // 0
   HANG 2
   // clean-up and reporting operations
   HALT   // actually halt the current process

$SYSTEM.Process.Terminate()

You can use the [$SYSTEM.Process.Terminate()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process#Terminate) method to halt the current process or to halt other running processes.

The following example halts the current process:

  DO $SYSTEM.Process.Terminate()

The following example halts the process with the PID 7732:

  DO $SYSTEM.Process.Terminate(7732)

The effects of the Terminate() method are the same as the HALT command for the current process, or the ^RESJOB utility for other processes.

HALT and ^RESJOB

The HALT command is used to halt the current process.

The ^RESJOB utility is used to halt other running processes. ^RESJOB cannot be used to halt the current process.

^RESJOB must be invoked from the %SYS namespace. You must have appropriate privileges to invoke ^RESJOB. The following is an example invocation of ^RESJOB from Caché Terminal:

%SYS>DO ^RESJOB
 
Force a process to quit Cache
 
Process ID (? for status report): 7732

Process ID (? for status report):
 
%SYS>

At the prompt, you type the process ID (PID) for the process you wish to halt. ^RESJOB halts the process, then prompts you for the next process ID. Press the Enter key at the prompt when you are finished entering process IDs. You can specify ? at the prompt to display a list of currently running processes.

*   Current process: attempting to use ^RESJOB to halt the current process fails with the message This is your current process, not proceeding with kill. ^RESJOB then prompts you for another process ID.
    
*   Non-running process: specifying the process ID of a non-running process fails with the message \[no such Cache process\]. ^RESJOB then prompts you for another process ID.
    
*   System processes: you cannot use ^RESJOB to halt certain system processes. Attempting to do so fails with the message Can NOT kill the name process. ^RESJOB then prompts you for another process ID.
    
*   Transaction-in-progress: using ^RESJOB to halt a process with a transaction-in-progress is the same as issuing a HALT command in that process. A non-interactive process rolls back the incomplete transaction; an interactive process prompts you at its Terminal prompt to either commit or roll back the incomplete transaction.
    

You can also use the ^JOBEXAM utility to halt a running process that is not the current process.

See Also

*   [$HALT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhalt) special variable
    
*   [Debugging](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_debug) in Using Caché ObjectScript
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cgoto "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_chang "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_chalt.xml**