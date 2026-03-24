# QUIT

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_cquit

---

 

Caché ObjectScript Reference

QUIT

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_copen "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cread "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_COMMANDS "Caché ObjectScript Commands") \]  >  \[ [QUIT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cquit "QUIT") \]

Go to: Description In Program Code At the Programmer Mode Prompt. Arguments Examples Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Terminates execution of a loop structure or a routine.

Synopsis

QUIT:pc expression
Q:pc expression

QUIT n
Q n

Arguments

pc

Optional — A postconditional expression.

expression

Optional — A value to return to the invoking routine; a valid expression.

n

Optional — Programmer Mode prompt only: The number of program levels to clear; an expression that resolves to a positive integer.

Description

The QUIT command is used in two different contexts:

*   [QUIT in program code](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cquit#RCOS_cquit_code)
    
*   [QUIT at the Programmer Mode prompt](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cquit#RCOS_cquit_prompt)
    

In Program Code

The QUIT command terminates execution of the current context, exiting to the enclosing context. When invoked in a routine, QUIT returns to the calling routine, or terminates the program if there is no calling routine. When invoked from within a FOR, DO WHILE, or WHILE flow-of-control structure, a TRY or CATCH block, or a legacy DO structure, the QUIT breaks out of the structure and continues execution with the next command outside of that structure.

The similar [RETURN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_creturn) command terminates execution of a routine at any point, including from within a FOR, DO WHILE, or WHILE loop or nested loop structure, a TRY or CATCH block, or a legacy DO structure. RETURN always exits the current routine, returning to the calling routine or terminating the program if there is no calling routine.

The QUIT command has two forms:

*   Without an argument
    
*   With an argument
    

A postconditional is not considered an argument. The $QUIT special variable indicates whether or not an argumented QUIT command is required to exit from the current context. Two error codes are provided for this purpose: M16 “Quit with an argument not allowed” and M17 “Quit with an argument required.” For further details, see [$ECODE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vecode) and the [ISO 11756-1999 standard M programming language error messages](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RERR_system) listing in the Caché Error Reference.

QUIT Without an Argument

QUIT without an argument exits from the current context without returning a value. It is used to terminate the execution level of a process started with a DO or XECUTE command, or to exit from a FOR, DO WHILE, or WHILE flow-of-control loop.

If DO, XECUTE, or an (unnested) flow-of-control loop command was given in programmer mode, QUIT returns control to programmer mode. If the terminated process contains any NEW commands before QUIT, QUIT automatically KILLs the affected variables and restores them to their original values.

QUIT With an Argument

QUIT expression terminates a user-defined function or an object method and returns the value resulting from the specified expression. QUIT with an argument cannot be used to exit a routine from within a FOR, DO WHILE, or WHILE command loop. QUIT with an argument also cannot be used to exit from within a TRY block or a CATCH block.

If an argumented QUIT is invoked inside a subroutine, one of the following occurs:

*   If an argumented QUIT is invoked inside a subroutine (instead of a function), the QUIT argument is evaluated (which may produce side effects or throw an error) and the argument result is discarded. Execution returns to the caller of the subroutine.
    
*   If the subroutine was called by a DO command and is in the scope of that DO argument, then the QUIT command evaluates its argument (and any side effects of that evaluation occur), but it does not return the argument. For example, a subroutine called by DO that concludes with QUIT 4/0 generates a <DIVIDE> error. The same behavior occurs if the subroutine was called by DO terminated by an argumented QUIT, and the subroutine is terminated by an [$ETRAP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vetrap).
    

At the Programmer Mode Prompt.

Issuing a QUIT at the Programmer Mode prompt clears entities from the program stack.

*   QUIT n clears the specified number of levels from the program stack.
    
*   QUIT clears all levels from the program stack.
    

The $STACK special variable contains the current number of context frames on the call stack. This number is also displayed in the Programmer Mode prompt.

The following example use QUIT with an integer argument to clear the specified number of levels from the program stack, then uses argumentless QUIT to clear all remaining levels:

USER 5f0>QUIT 1
 
USER 4d0>QUIT 2
 
USER 2d0>QUIT

USER>

For further details see [Processing Errors in Programmer Mode](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_errors#GCOS_errors_progmode) in the “Error Processing” chapter of Using Caché ObjectScript.

Arguments

pc

An optional postconditional expression. Caché executes the command if the postconditional expression is true (evaluates to a nonzero numeric value). Caché does not execute the command if the postconditional expression is false (evaluates to zero). For further details, refer to [Command Postconditional Expressions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_commands#GCOS_commands_pc) in Using Caché ObjectScript. If the QUIT command takes no other arguments, there must be two or more spaces between the postconditional and the next command following it on the same line.

expression

Any valid Caché ObjectScript expression. It can be used only within a user-defined function to return the evaluated result to the calling routine.

Examples

The following two examples contrast QUIT and RETURN behavior when issued from within a flow-of-control structure. QUIT exits the FOR loop, continuing with the remainder of MySubroutine before returning to MyRoutine. RETURN exits MySubroutine, returning to MyRoutine.

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

 

In the following example, execution of the first QUIT command is controlled by a postconditional (:x>46). If the randomly generated number is greater than 46 Caché does not perform the Cube procedure; the first QUIT takes the postconditional, returning a string to the calling routine as num. If the randomly generated number is less than or equal to 46 the second QUIT returns the results of the expression x\*x\*x as num.

Main
  SET x \= $RANDOM(99)
  WRITE "Number is: ",x,!
  SET num\=$$Cube(x)
  WRITE "Cube is: ",num
  QUIT
Cube(x) QUIT:x\>46 "a six-digit number."
  WRITE "Calculating the cube",!
  QUIT x\*x\*x

 

The following two examples contrast QUIT and RETURN behavior with TRY and CATCH. The TRY block attempts a divide-by-zero operation, invoking the CATCH block; this CATCH block contains a nested TRY block which is exited by either a QUIT or a RETURN. (For the purpose of demonstration, these programs do not include the recommended code (QUIT or RETURN) to prevent “fall-through”.)

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

 

In this example, the argument of the QUIT command is an object method. Caché terminates execution of the method and returns control to the calling routine.

   QUIT inv.TotalNum()

Notes

QUIT Restores Variables

If a terminated process contains any NEW commands before QUIT, QUIT automatically KILLs the affected variables and restores them to their original values.

QUIT and Flow-of-Control Structures

A QUIT can be used to break out of a FOR loop, a DO WHILE loop, or a WHILE loop. Execution continues with the command that follows the end of the loop structure. If these loop structures are nested, the QUIT breaks out of the inner loop from which it was called to the next enclosing outer loop.

If a QUIT command is encountered within an IF code block (or an ELSEIF code block or an ELSE code block) the QUIT behaves as a regular QUIT command, as if the code block did not exist:

*   If the IF code block is nested within a loop structure (such as a FOR code block), the QUIT exits the loop structure block and continues execution with the command that follows the loop structure code block.
    
*   If the IF code block is not nested within a loop structure, the QUIT terminates the current routine.
    

A QUIT can be used to break out of a legacy DO structure (an argumentless DO followed by lines of code each of which is prefixed by a period). Execution continues with the command that follows the argumentless DO structure code block. This argumentless DO syntax is considered obsolete and should not be used for new coding. For further details, see [Argumentless DO (legacy version)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cdo_legacy).

Use with Indefinite FOR Loop

An indefinite FOR loop is a FOR without an argument; unless broken out of, it will loop infinitely. To control an indefinite FOR loop, you must include within the loop structure either a QUIT with a postconditional, or an IF command that invokes a QUIT. The QUIT within the IF terminates the enclosing FOR loop. Without one of these QUITs, the loop will be an infinite loop and will execute endlessly.

In the following example, the indefinite FOR loop is exited using a QUIT with a postconditional. The loop continues to execute as long as the user enters some value in response to the "Text =" prompt. Only when the user enters a null string (that is, just presses Return or Enter) is QUIT executed and the loop terminated.

Main
  FOR   {
    READ !,"Text =",var1 
    QUIT:var1\=""  DO Subr1
    }
Subr1
   WRITE "Thanks for the input", !
   QUIT

This command requires at least two spaces between the postconditional on the QUIT command and the following DO command on the same line. This is because Caché treats postconditionals as command modifiers, not as arguments.

Implicit QUIT

In the following cases, a QUIT command is not required, because Caché automatically issues an implicit QUIT to prevent execution "falling through" to a separate unit of code.

*   Caché executes an implicit QUIT at the end of a routine.
    
*   Caché executes an implicit QUIT when it encounters a [label](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_syntax#GCOS_syntax_labels) with parameters. A label with parameters is defined as one with parentheses, even if the parentheses contain zero parameters. All procedures begin with a label with parameters (even if no parameters are defined). Many subroutines and functions also have a label with parameters.
    

You can, of course, code an explicit QUIT in any of these circumstances.

Behavior with DO

When encountered in a subroutine called by the DO command, QUIT terminates the subroutine and returns control to the command following the DO command.

Behavior with XECUTE

When encountered in a line of code that is being executed, QUIT terminates execution of the line and returns control to the command following the XECUTE command. No argument is allowed.

Behavior with User-Defined Functions

When encountered in a user-defined function, QUIT terminates the function and returns the value that results from the specified expression. The expression argument is required.

In their use, user-defined functions are similar to DO commands with parameter passing. They differ from such DO commands, however, in that they return the value of an expression directly, rather than through a variable. To invoke a user-defined function, use the form:

$$name(parameters)

where name is the name of the function. It can be specified as label, ^routine, or label^routine.

parameters is a comma-separated list of parameters to be passed to the function. The label associated with the function must also have a parameter list. The parameter list on the invoked function is known as the actual parameter list. The parameter list on the function label is known as the formal parameter list.

In the following example, the FOR loop uses a READ command to first acquire the number to be squared and store it in the num variable. (Note the two spaces after the argumentless FOR and the postconditional QUIT.) It then uses a WRITE command to invoke the Square standard function, with num specified as the function parameter.

The only code for the function is the QUIT command followed by an expression to calculate the square. When it encounters the QUIT command, Caché evaluates the expression, terminates the function, and returns the resulting value directly to the WRITE command. The value of num is not changed.

Test  WRITE "Calculate the square of a number",!
  FOR   {
    READ !,"Number:",num QUIT:num\="" 
    WRITE !,$$Square(num),!
    QUIT
  }
Square(val) 
  QUIT val\*val

Using QUIT for Program Readability

Caché executes an implicit QUIT at the end of each routine, but you can include it explicitly to improve program readability.

See Also

*   [DO](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cdo) command
    
*   [DO WHILE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cdowhile) command
    
*   [FOR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cfor) command
    
*   [NEW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cnew) command
    
*   [RETURN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_creturn) command
    
*   [WHILE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cwhile) command
    
*   [XECUTE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cxecute) command
    
*   [ZQUIT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czquit) command
    
*   [$ESTACK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vestack) special variable
    
*   [$QUIT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vquit) special variable
    
*   [$STACK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vstack) special variable
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_copen "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cread "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_cquit.xml**