# GOTO

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_cgoto

---

 

Caché ObjectScript Reference

GOTO

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cfor "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_chalt "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_COMMANDS "Caché ObjectScript Commands") \]  >  \[ [GOTO](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cgoto "GOTO") \]

Go to: Description Arguments Examples Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Transfers control.

Synopsis

GOTO:pc
GOTO:pc goargument,...

G:pc
G:pc goargument,...

where goargument is:

location:pc

Arguments

pc

Optional — A postconditional expression.

location

Optional — The point to which control will be transferred.

Description

The GOTO command has two forms:

*   [Without an argument](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cgoto#RCOS_cgoto_noarg)
    
*   [With an argument](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cgoto#RCOS_cgoto_witharg)
    

GOTO Without an Argument

GOTO without an argument resumes normal program execution after Caché encounters an error or a [BREAK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cbreak) command in the currently executing code. You can use the argumentless GOTO only from the Programmer Mode prompt.

The following example shows the use of an argumentless GOTO. In this example, the second WRITE is not executed because of the <BREAK> error; issuing a GOTO resumes execution, executing the second WRITE:

USER>WRITE "before" BREAK  WRITE "after"
before
WRITE "before" BREAK  WRITE "after"
               ^
<BREAK>
USER 1S0>GOTO
after
USER>

Note that there must be two spaces after the BREAK command.

If a NEW command is in effect when you issue an argumentless GOTO, Caché issues a <COMMAND> error, and the new context is maintained. Use the QUIT 1 command, then argumentless GOTO to resume after a NEW.

Argumentless GOTO can also be used at the Programmer Mode prompt to continue execution after an error. See [Processing Errors in Programmer Mode](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_errors#GCOS_errors_progmode) in the “Error Processing” chapter of Using Caché ObjectScript.

GOTO With an Argument

GOTO with the argument location transfers control to the specified location. If you specify a postconditional expression on either the command or the argument, Caché transfers control only if the postconditional expression evaluates to TRUE (nonzero).

You can use GOTO location from the Programmer Mode prompt to resume an interrupted program at a different location.

You can specify a $CASE function as a GOTO command argument.

Arguments

pc

An optional postconditional expression that can make the command conditional. If the postconditional expression is appended to the GOTO command keyword, Caché executes the GOTO command if the postconditional expression is true (evaluates to a nonzero numeric value). Caché does not execute the GOTO command if the postconditional expression is false (evaluates to zero). If the postconditional expression is appended to an argument, Caché executes the argument if the postconditional expression is true (evaluates to a nonzero numeric value). If the postconditional expression is false (evaluates to zero), Caché skips that argument and evaluates the next argument (if there is one) or the next command. For further details, refer to [Command Postconditional Expressions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_commands#GCOS_commands_pc) in Using Caché ObjectScript.

location

The point to which control will be transferred. It is required in routine code. It is optional from the Programmer Mode prompt. You can specify location as a single value or as a comma-separated list of values (with postconditionals) and can take any of the following forms:

label+offset specifies a [line label](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_syntax#GCOS_syntax_labels) within the current routine. The optional +offset is a nonnegative integer that specifies the number of lines after the label at which execution is to start. The offset counts lines of code (including label lines), and counts comment lines; offset does not count blank lines and blank lines within comments. However, offset does count all [Embedded SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql) lines, including all blank lines.

label+offset^routine specifies a [line label](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_syntax#GCOS_syntax_labels) within the named routine, which resides on disk. Caché loads the routine from disk and continues execution at the indicated label. The optional +offset is a nonnegative integer that specifies the number of lines after the label at which execution is to start.

^routine specifies a routine that resides on disk. Caché loads the routine from disk and continues execution at the first line of executable code within the routine. If the routine has been modified, Caché loads the updated version of the routine when GOTO invokes the routine. Unlike the [DO](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cdo) command, GOTO does not return to the invoking program following routine execution. If you specify a nonexistent routine, Caché issues a <NOROUTINE> error message. For more information, refer to the [$ZERROR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzerror) special variable.

Note:

GOTO does not support [extended routine reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_syntax#GCOS_syntax_namespaces_extendedrefs). To execute a routine in another namespace, use the [DO](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cdo) command.

You can also reference location as a variable containing any of the above forms. In this case, though, you must use name indirection. location cannot specify a subroutine label that is defined with a formal parameter list or the name of a user-defined function or procedure. If you specify a nonexistent label, Caché issues a <NOLINE> error message. For more information, refer to [Indirection](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_operators#GCOS_operators_indirection) in Using Caché ObjectScript.

You cannot specify an offset when calling a CACHESYS % routine. If you attempt to do so, Caché issues a <NOLINE> error.

Examples

In the following example, GOTO directs execution to one of three locations depending on the user-supplied age value. The location is a subroutine label that is stored in variable loc and then referenced by means of name indirection (@loc).

mainloop
  SET age\=""
  READ !,"What is your age? ",age QUIT:age\=""
  IF age<30 {
    SET loc\="Young" }
  ELSEIF (age\>29)&(age<60) {
    SET loc\="Midage" }
  ELSEIF age\>59 {
    SET loc\="Elder" }
  ELSE {
    WRITE "data input error"
    QUIT }
  GOTO @loc
  QUIT
Young
  WRITE !,"You're still young"
  QUIT
Midage
  WRITE !,"You're in your prime"
  QUIT
Elder
  WRITE !,"You have a lifetime of wisdom to impart"
  QUIT

Note that this type of GOTO using name indirection is not permitted from within a procedure block.

As an alternative, you could omit the IF command and code the GOTO with a comma-separated list using postconditionals on the arguments, as follows:

   GOTO Young:age<30,Midage:(age\>29)&(age<60),Elder:age\>59

You might also code this example using a DO command to call the appropriate subroutine location. In this case, though, when Caché encounters a QUIT, it returns control to the command following the DO.

The following example shows how offset counts lines of code. It counts the intervening label line and the comment line; it does not count the blank line:

Main
  GOTO Branch+7
  QUIT
Branch
  WRITE "Line 1",!
SubBranch
  WRITE "Line 3",!
  /\* comment line \*/
  WRITE "Line 5",!

  WRITE "Line 6",!
  WRITE "Line 7",!
  WRITE "Line 8",!
  QUIT

 

Notes

How Control is Transferred When QUIT is Encountered

Unlike the DO command, GOTO transfers control unconditionally. When Caché encounters a QUIT in a subroutine called by DO, it passes control to the command following the most recent DO.

When Caché encounters a QUIT after a GOTO transfer, it does not return control to the command following the GOTO. If there was a preceding DO, it returns control to the command following the most recent DO. If there was no preceding DO, then it returns to the programmer prompt.

In the following code sequence, the QUIT in C returns control to the WRITE command following the DO in A:

testgoto
A
  WRITE !,"running A"
  DO B
  WRITE !,"back to A, all done"
  QUIT
B  
  WRITE !,"running B"
  GOTO C
  WRITE !,"this line in B should never execute"
  QUIT
C  
  WRITE !,"running C"
  QUIT

 

Using GOTO with Code Blocks

GOTO can be used to exit a code block, but not to enter a code block.

If you use GOTO inside a FOR, IF, DO WHILE, or WHILE loop, you can go to a location outside of all code blocks, a location within the current code block, or go from a nested code block to a location in the code block that encloses it. You cannot go from a code block to a location within another code block, either an independent code block, or a code block nested within the current code block. For code examples, refer to the individual commands.

A GOTO to a location outside a code block terminates the loop. A GOTO to a location within a code block does not terminate the loop. A GOTO from a nested code block to an enclosing code block terminates the inner (nested) loop, but not the outer loop.

A GOTO can be used to exit a TRY or CATCH code block, but not to enter one of these code blocks. You also cannot specify a GOTO to a label on the same line as the TRY or CATCH keyword. Attempting to do so results in a <NOLINE> error.

GOTO Restrictions

The following GOTO operations are not permitted:

*   GOTO should not be used to enter or exit a procedure.
    
*   GOTO should not be used within nested argumentless DO code blocks to jump between levels.
    
*   GOTO cannot be used with name indirection (GOTO @name) within a procedure block.
    

See Also

*   [DO](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cdo) command
    
*   [FOR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cfor) command
    
*   [IF](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cif) command
    
*   [DO WHILE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cdowhile) command
    
*   [WHILE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cwhile) command
    
*   [BREAK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cbreak) command
    
*   [QUIT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cquit) command
    
*   [$CASE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fcase) function
    
*   [Processing Errors in Programmer Mode](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_errors#GCOS_errors_progmode) in the “Error Processing” chapter of Using Caché ObjectScript
    
*   [Debugging With BREAK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_debug#GCOS_debug_break) in the “Command-line Routine Debugging” chapter of Using Caché ObjectScript
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cfor "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_chalt "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_cgoto.xml**