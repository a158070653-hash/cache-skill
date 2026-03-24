# BREAK

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_cbreak

---

 

Caché ObjectScript Reference

BREAK

 [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ccatch "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_COMMANDS "Caché ObjectScript Commands") \]  >  \[ [BREAK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cbreak "BREAK") \]

Go to: Description Arguments Argumentless BREAK BREAK Extended Arguments to Set Regular Breakpoints BREAK flag to Enable or Disable Interrupts See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Interrupts execution at a breakpoint. Enables or disables user interrupts.

Synopsis

BREAK:pc "extend"
B:pc "extend"

BREAK:pc flag
B:pc flag

Arguments

pc

Optional — A postconditional expression.

extend

Optional — A letter code indicating the kind of breakpoints to enable or disable, specified as a quoted string. Valid values are listed in [BREAK Extended Arguments](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cbreak#RCOS_cbreak_extend). Cannot be used with the flag argument.

flag

Optional — An integer flag that specifies interrupt behavior. The flag value can be quoted or unquoted. Valid values are: 0 and 4 which disable CTRL-C interrupts, and 1 and 5 which enable CTRL-C interrupts. The default is determined by context (see [BREAK flag](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cbreak#RCOS_cbreak_flag) for details). Cannot be used with the extend argument.

Description

The BREAK command has three forms:

*   [BREAK without an argument](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cbreak#RCOS_cbreak_noarg) breaks code execution at the current location.
    
*   [BREAK extend](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cbreak#RCOS_cbreak_extend) breaks code execution at regular breakpoint intervals.
    
*   [BREAK flag](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cbreak#RCOS_cbreak_flag) enables or disables CTRL-C interrupts.
    

Note:

Older versions of Caché ObjectScript accepted only the abbreviation (B) for the BREAK command. Current versions accept either form.

Required Permission

To use BREAK statements when running code, the user must be assigned to a role (such as %Developer or %Manager) that provides the %Development resource with U (use) permission. A user is assigned to a role either through the SQL [GRANT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_grant) statement, or by using the Management Portal \[Home\] > \[Security Management\] > \[Users\] options to edit the definition of the User.

Arguments

pc

An optional postconditional expression. Caché executes the BREAK command if the postconditional expression is true (evaluates to a nonzero numeric value). Caché does not execute the command if the postconditional expression is false (evaluates to zero). For further details, refer to [Command Postconditional Expressions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_commands#GCOS_commands_pc) in Using Caché ObjectScript.

extend

BREAK extend supports letter string codes to specify breakpoint behavior. Quotes are required. See [BREAK Extended Arguments](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cbreak#RCOS_cbreak_extend) for a table of these extend codes.

flag

BREAK flag supports four different ways to handle CTRL-C interrupts:

*   BREAK 0: dismisses any pending, but not yet signaled, CTRL-C trap. Disables future signaling of CTRL-C.
    
*   BREAK 1: dismisses any pending CTRL-C trap. Enables future signaling of CTRL-C.
    
    This means that typing CTRL-C following execution of the BREAK 1 causes a CTRL-C signal.
    
*   BREAK 4: does not dismiss any pending CTRL-C trap. Disables future signaling of CTRL-C.
    
    This means that a pending CTRL-C trap is signaled when a future BREAK 1 or BREAK 5 command enables CTRL-C.
    
*   BREAK 5: does not dismiss any pending CTRL-C trap. Enables future signaling of CTRL-C.
    
    This means that a pending CTRL-C trap should be signaled by a Caché ObjectScript command shortly following BREAK 5. Most, but not all Caché ObjectScript commands poll for CTRL-C.
    

See [BREAK flag to Enable or Disable Interrupts](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cbreak#RCOS_cbreak_flag) for further details and examples.

Argumentless BREAK

Argumentless BREAK interrupts code execution when encountered. You can use argumentless BREAK in program source code, with or without a postconditional, to interrupt program execution at that point and return control to programmer mode. Argumentless BREAK is used for debugging purposes.

If you include an argumentless BREAK within a routine, it sets a breakpoint, which interrupts routine execution and returns the process to programmer mode. By imbedding breakpoints in your code, you can establish specific contexts for debugging. Each time execution reaches a BREAK, Caché suspends the routine and returns you to the programmer prompt. You can then use other Caché ObjectScript commands to perform debugging activities. For example, you might use the WRITE command to examine the values of variables at the current stopping point and the SET command to supply new values for these or other variables. You can also invoke the Routine Line Editor (XECUTE ^%), which provides basic editing capabilities for modifying the routine After you suspend routine execution with a BREAK, you can resume normal execution by using an argumentless GOTO. Alternatively, you can resume execution at a different location by specifying this location as the GOTO command argument.

Note:

InterSystems recommends that you use the ZBREAK command to invoke the Caché Debugger, rather than using the BREAK command in code. The Debugger provides much more extensive debugging capabilities.

You can configure argumentless BREAK behavior for the current process using the [BreakMode()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process#BreakMode) method of the [%SYSTEM.Process](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process) class. You can configure argumentless BREAK behavior system-wide by setting the [BreakMode](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=Config.Miscellaneous#BreakMode) property in the Config.Miscellaneous class.

Like all argumentless commands, you must insert at least two blank spaces between an argumentless BREAK and a command following it on the same line.

Using Argumentless BREAK with a Condition

You may find it useful to specify a condition on an argumentless BREAK command in code so that you can rerun the same code simply by setting a variable rather than having to change the routine. For example, you may have the following line in a routine:

   BREAK:$DATA(debug)

You can then set the variable debug to suspend the routine and return the job to programmer mode or clear the variable debug to continue running the routine.

BREAK Extended Arguments to Set Regular Breakpoints

You do not have to place argumentless BREAK commands at every location where you want to suspend your routine. BREAK has a series of "extended" arguments (extend) that can periodically suspend a routine as if you scattered argumentless BREAKs throughout the code. BREAK command extended arguments are listed in the following table.

Argument

Description

"S"

Use BREAK "S" (Single Step) to step through your code a single command (generated token) at a time. Not all Caché ObjectScript commands generate a token; some generate multiple tokens and thus are parsed as multiple steps (see below). Caché stops breaking on commands invoked by a DO command or an XECUTE command, or within a FOR loop or a user-defined function, and resumes with the next token when the command or loop is done.

"S+"

BREAK "S+" acts like BREAK "S", except that Caché includes breaking on commands invoked by a DO command or an XECUTE command, or within a FOR loop or a user-defined function.

"S-"

Use BREAK "S-" to disable break stepping (“S” or “L”) at the current level and enable single stepping at the previous level. Acts like BREAK "C" at the current level and BREAK "S" at the previous level.

"L"

Use BREAK "L" (Line Stepping) to step through your code a single routine line at a time, breaking at the beginning of every line. Lines that do not generate tokens are ignored (see below). Caché stops breaking on commands invoked by a DO command or an XECUTE command, or within a FOR loop or a user-defined function, and resumes with the next line when the command or loop is done.

"L+"

BREAK "L+" acts like BREAK "L", except that Caché also continues to break at the beginning of every routine line on commands invoked by a DO command or an XECUTE command, or within a FOR loop or a user-defined function.

"L-"

Use BREAK "L-" to disable break stepping (“S” or “L”) at the current level and enable line stepping at the previous level. Acts like BREAK "C" at the current level and BREAK "L" at the previous level.

"C"

Use BREAK "C" (Clear Break) to stop all break stepping (“L” and “S”) at the current level. Breaking resumes at a previous routine level after the job executes a QUIT if a BREAK state is in effect at that previous level.

"C-"

Use BREAK "C-" to stop all break stepping (“L” and “S”) at the current level and all previous levels. This allows stepping to be removed at all levels without affecting other debugging features.

"OFF"

BREAK "OFF" removes all debugging that has been established for the process. It removes all breakpoints and watchpoints, and turns off stepping at all program stack levels. It also removes the association with the debug and trace devices, but does not close them.

BREAK “S” and BREAK “L” break on statements that generate tokens. Not all Caché ObjectScript commands or lines generate a token. For example, BREAK “S” and BREAK “L” both ignore label lines, comments, and TRY statements. BREAK “S” breaks at a CATCH statement (if the CATCH block is entered); BREAK “L” does not.

One difference between BREAK “S” and BREAK “L” is that many command lines generate more than one token and thus consist of more than one step. This is not always obvious. For example, the following are all one line (and one ObjectScript command), but BREAK “S” parses each as two steps: SET x=1,y=2, KILL x,y, WRITE “hello”,!, IF x=1,y=2.

To resume code execution after a breakpoint, issue a [GOTO](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cgoto) command (abbreviated as G) at the Terminal prompt. See [Debugging](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_debug) in Using Caché ObjectScript for more information.

Issuing a BREAK "OFF" command is equivalent to issuing the following series of commands:

  ZBREAK /CLEAR
  ZBREAK /TRACE:OFF
  ZBREAK /DEBUG:""
  ZBREAK /ERRORTRAP:ON
  BREAK "C-"

BREAK flag to Enable or Disable Interrupts

Use BREAK flag to control whether user interrupts, such as CTRL-C, are enabled or disabled. The practical difference between these disable/enable options is as follows:

BREAK 0 and BREAK 1 can be used to create a code block where a CTRL-C signal cannot interrupt a critical sequence of commands. However, a loop on such a block may be difficult for an interactive user to interrupt using CTRL-C. This is because there is a slight delay between detecting a CTRL-C trap and polling a CTRL-C signal. This delay may permit the next BREAK command loop to dismiss the CTRL-C user interrupt.

A program block containing BREAK 4 and BREAK 5 can be used to create code where a CTRL-C signal cannot interrupt a critical sequence of commands without affecting the ability of an interactive user to interrupt a loop operation on this block using CTRL-C.

The default flag behavior of BREAK is dependent upon the login mode, as follows:

*   If you log in as programmer mode, the default is BREAK 1. Interrupts, such as CTRL-C, are always enabled. The B (/break) protocol specified in an OPEN or USE command has no effect.
    
*   If you log in as application mode, the default is BREAK 0. Interrupts, such as CTRL-C, are enabled or disabled by the B (/break) protocol specified in an OPEN or USE command.
    

For further details on OPEN and USE mode protocols, refer to [Terminal I/O](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_termio) in the Caché I/O Device Guide.

BREAK flag Examples

The following example uses [$ZJOB](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzjob) to determine if interrupts are enabled or disabled:

  BREAK 0
    DO InterruptStatus
  BREAK 1
    DO InterruptStatus
    WRITE "all done"
InterruptStatus()
  IF $ZJOB\\4#2\=1 {WRITE "Interrupts enabled",!}
  ELSE {WRITE "Interrupts disabled",!}

 

The following example uses a READ in a FOR loop for user input of a series of numbers. It sets BREAK 0 to disable user interrupts during the READ operation. However, if the user inputs a value that is not a number, BREAK 1 enables user interrupts so that the user can either reject or accept the value they just input:

  SET y\="^" 
InputLoop
  TRY {
      FOR {
           BREAK 0
           READ "input a number ",x
           IF x\="" { WRITE !,"all done"  QUIT }
           ELSEIF 0\=$ISVALIDNUM(x) {
             BREAK 1
             WRITE !,x," is not a number",!
             WRITE "you have four seconds to press CTRL-C",!
             WRITE "or accept this input value",!
             HANG 4 }
           ELSE {  }
           SET y\=y\_x\_"^"
           WRITE !,"the number list is ",y,!
      }
   }
   CATCH { WRITE "Rejecting bad input",!
           DO InputLoop
   }

See Also

*   [ZBREAK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czbreak) command
    
*   [GOTO](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cgoto) command
    
*   [OPEN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_copen) command
    
*   [USE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cuse) command
    
*   [Debugging](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_debug) in Using Caché ObjectScript
    
*   [Terminal I/O](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GIOD_termio) in Caché I/O Device Guide
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

  [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ccatch "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_cbreak.xml**