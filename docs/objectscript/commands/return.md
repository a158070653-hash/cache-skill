# RETURN

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_creturn

---

 

Caché ObjectScript Reference

RETURN

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cread "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cset "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_COMMANDS "Caché ObjectScript Commands") \]  >  \[ [RETURN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_creturn "RETURN") \]

Go to: Description Arguments Examples Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Terminates execution of a routine.

Synopsis

RETURN:pc expression
RET:pc expression

Arguments

pc

Optional — A postconditional expression.

expression

Optional — A Caché ObjectScript expression.

Description

The RETURN command is used to terminate execution of a routine. In many contexts it is a synonym for the [QUIT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cquit) command. RETURN and QUIT differ when issued from within a FOR, DO WHILE, or WHILE flow-of-control structure, or a TRY or CATCH block.

*   You can use RETURN to terminate execution of a routine at any point, including from within a FOR, DO WHILE, or WHILE loop or nested loop structure. RETURN always exits the current routine, returning to the calling routine or terminating the program if there is no calling routine. RETURN always behaves the same, regardless of whether it is issued from within a code block. This includes a TRY block or a CATCH block.
    
*   In contrast, QUIT exits only the current structure when issued from within a FOR loop, a DO WHILE loop, a WHILE loop, or a TRY or CATCH block. QUIT exits the structure block and continues execution of the current routine with the next command outside of that structure block. QUIT exits the current routine when issued outside of a block structure or from within an IF, ELSEIF, or ELSE code block.
    

The RETURN command has two forms:

*   Without an argument
    
*   With an argument
    

A postconditional is not considered an argument. The $QUIT special variable indicates whether or not an argumented RETURN command is required to exit from the current context. Two error codes are provided for this purpose: M16 “Quit with an argument not allowed” and M17 “Quit with an argument required.” For further details, see [$ECODE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vecode) and the [ISO 11756-1999 standard M programming language error messages](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RERR_system) listing in the Caché Error Reference.

RETURN Without an Argument

RETURN without an argument exits from the current context without returning a value. It is used to terminate the execution level of a process started with a DO or XECUTE command.

If DO, XECUTE was given in programmer mode, RETURN returns control to programmer mode. If the terminated process contains any NEW commands before RETURN, RETURN automatically KILLs the affected variables and restores them to their original values.

RETURN With an Argument

RETURN expression terminates a user-defined function or an object method and returns the value resulting from the specified expression. RETURN with an argument can be used to exit a routine from within a FOR, DO WHILE, or WHILE command loop, or from within a TRY block or a CATCH block.

If an argumented RETURN is invoked inside a subroutine, one of the following occurs:

*   If an argumented RETURN is invoked inside a subroutine (instead of a function), the RETURN argument is evaluated (which may produce side effects or throw an error) and the argument result is discarded. Execution returns to the caller of the subroutine.
    
*   If the subroutine was called by a DO command and is in the scope of that DO argument, then the RETURN command evaluates its argument (and any side effects of that evaluation occur), but it does not return the argument. For example, a subroutine called by DO that concludes with RETURN 4/0 generates a <DIVIDE> error. The same behavior occurs if the subroutine was called by DO terminated by an argumented RETURN, and the subroutine is terminated by an [$ETRAP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vetrap).
    

Arguments

pc

An optional postconditional expression. Caché executes the command if the postconditional expression is true (evaluates to a nonzero numeric value). Caché does not execute the command if the postconditional expression is false (evaluates to zero). For further details, refer to [Command Postconditional Expressions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_commands#GCOS_commands_pc) in Using Caché ObjectScript. If the RETURN command takes no other arguments, there must be two or more spaces between the postconditional and the next command following it on the same line.

expression

Any valid Caché ObjectScript expression. It can be used only within a user-defined function to return the evaluated result to the calling routine.

Examples

The following two examples contrast RETURN and QUIT behavior when issued from within a flow-of-control structure. RETURN exits MySubroutine, returning to MyRoutine. QUIT exits the FOR loop, continuing with the remainder of MySubroutine before returning to MyRoutine:

MyMain
  WRITE "In the main routine",!
  DO MySubroutine
  WRITE "Returned to main routine",!
  QUIT
MySubroutine
  WRITE "In MySubroutine",!
  FOR i\=1:1:5 {
    WRITE "FOR loop:",i,!
    IF i\=3 RETURN
    WRITE "  loop again",!
  }
  WRITE "MySubroutine line not displayed with RETURN",!
  QUIT

 

MyMain
  WRITE "In the main routine",!
  DO MySubroutine
  WRITE "Returned to main routine",!
  QUIT
MySubroutine
  WRITE "In MySubroutine",!
  FOR i\=1:1:5 {
    WRITE "FOR loop:",i,!
    IF i\=3 QUIT
    WRITE "  loop again",!
  }
  WRITE "MySubroutine line displayed with QUIT",!
  QUIT

 

In the following example, execution of the first RETURN command is controlled by a postconditional (:x>46). If the randomly generated number is greater than 46 Caché does not perform the Cube procedure; the first RETURN takes the postconditional, returning a string to the calling routine as num. If the randomly generated number is less than or equal to 46 the second RETURN returns the results of the expression x\*x\*x as num.

Main
  SET x \= $RANDOM(99)
  WRITE "Number is: ",x,!
  SET num\=$$Cube(x)
  WRITE "Cube is: ",num
  QUIT
Cube(x) RETURN:x\>46 "a six-digit number."
  WRITE "Calculating the cube",!
  RETURN x\*x\*x

 

The following two examples contrast QUIT and RETURN behavior with TRY and CATCH. The TRY block attempts a divide-by-zero operation, invoking the CATCH block; this CATCH block contains a nested TRY block which is exited by either a QUIT or a RETURN. (For the purpose of demonstration, these programs do not include the recommended code (QUIT or RETURN) to prevent “fall-through”.)

RETURN exits the routine. It therefore exits the nested TRY block and any enclosing blocks, and does not execute the fall-through line outside of the TRY/CATCH structures:

  TRY {
    WRITE "In the TRY block",!
    SET x \= 5/0
    WRITE "This line should never display"
  }
  CATCH exp1 {
    WRITE "In the CATCH block",!
    WRITE "Error Name: ",$ZCVT(exp1.Name,"O","HTML"),!
      TRY {
        WRITE "In the nested TRY block",!
        RETURN
      }
      CATCH exp2 {
        WRITE "In the nested CATCH block",!
        WRITE "Error Name: ",$ZCVT(exp1.Name,"O","HTML"),!
      }
    WRITE "RETURN does not display this outer CATCH block line",!
  }
  WRITE "fall-through at the end of the program"

 

QUIT exits the nested TRY block to the enclosing block, continuing execution with the remainder of the CATCH block. When it completes the CATCH block it execute the fall-through line outside of the TRY/CATCH structures:

  TRY {
    WRITE "In the TRY block",!
    SET x \= 5/0
    WRITE "This line should never display"
  }
  CATCH exp1 {
    WRITE "In the CATCH block",!
    WRITE "Error Name: ",$ZCVT(exp1.Name,"O","HTML"),!
      TRY {
        WRITE "In the nested TRY block",!
        QUIT
      }
      CATCH exp2 {
        WRITE "In the nested CATCH block",!
        WRITE "Error Name: ",$ZCVT(exp1.Name,"O","HTML"),!
      }
    WRITE "QUIT displays this outer CATCH block line",!
  }
  WRITE "fall-through at the end of the program"

 

In this example, the argument of the RETURN command is an object method. Caché terminates execution of the method and returns control to the calling routine.

   RETURN inv.TotalNum()

Notes

RETURN Restores Variables

If a terminated process contains any NEW commands before RETURN, RETURN automatically KILLs the affected variables and restores them to their original values.

Implicit RETURN

In the following cases, a RETURN command is not required, because Caché automatically issues an implicit RETURN to prevent execution "falling through" to a separate unit of code.

*   Caché executes an implicit RETURN at the end of a routine.
    
*   Caché executes an implicit RETURN when it encounters a [label](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_syntax#GCOS_syntax_labels) with parameters. A label with parameters is defined as one with parentheses, even if the parentheses contain zero parameters. All procedures begin with a label with parameters (even if no parameters are defined). Many subroutines and functions also have a label with parameters.
    

You can, of course, code an explicit RETURN in any of these circumstances.

Behavior with DO

When encountered in a subroutine called by the DO command, RETURN terminates the subroutine and returns control to the command following the DO command.

Behavior with XECUTE

When encountered in a line of code that is being executed, RETURN terminates execution of the line and returns control to the command following the XECUTE command. No argument is allowed.

Behavior with User-Defined Functions

When encountered in a user-defined function, RETURN terminates the function and returns the value that results from the specified expression. The expression argument is required.

In their use, user-defined functions are similar to DO commands with parameter passing. They differ from such DO commands, however, in that they return the value of an expression directly, rather than through a variable. To invoke a user-defined function, use the form:

$$name(parameters)

where name is the name of the function. It can be specified as label, ^routine, or label^routine.

parameters is a comma-separated list of parameters to be passed to the function. The label associated with the function must also have a parameter list. The parameter list on the invoked function is known as the actual parameter list. The parameter list on the function label is known as the formal parameter list.

Clearing Levels from the Program Stack

Each invocation of RETURN removes a context frame from the call stack for your process. The $STACK special variable contains the current number of context frames on the call stack.

You can use RETURN from the programmer prompt to clear some or all levels from the program stack. The following example clears the top two levels from the stack:

   RETURN 2

The argumentless RETURN clears all the levels from the stack.

Using RETURN for Program Readability

Caché executes an implicit RETURN at the end of each routine, but you can include it explicitly to improve program readability.

See Also

*   [DO](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cdo) command
    
*   [DO WHILE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cdowhile) command
    
*   [FOR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cfor) command
    
*   [NEW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cnew) command
    
*   [QUIT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cquit) command
    
*   [WHILE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cwhile) command
    
*   [XECUTE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cxecute) command
    
*   [ZQUIT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czquit) command
    
*   [$QUIT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vquit) special variable
    
*   [$STACK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vstack) special variable
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cread "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cset "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_creturn.xml**