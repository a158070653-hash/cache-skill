# TRY

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_ctry

---

 

Caché ObjectScript Reference

TRY

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ctrollback "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ctstart "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_COMMANDS "Caché ObjectScript Commands") \]  >  \[ [TRY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ctry "TRY") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Identifies a block of code to monitor for errors during execution.

Synopsis

TRY {
   . . .
}

Description

The TRY command takes no arguments. It is used to identify a block of Caché ObjectScript code statements enclosed in curly braces. This block of code is protected code for structured exception handling. If an exception occurs within this block of code, Caché sets $ZERROR and $ECODE, then transfers execution to an exception handler, identified by the [CATCH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ccatch) command.

An exception may occur as a result of a runtime error, such as attempting to divide by 0, or it may be explicitly propagated by issuing a [THROW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cthrow) command. If no error occurs, execution continues with the next ObjectScript statement after the CATCH block of code.

A TRY block must be immediately followed by a CATCH block. You cannot specify either executable code statements or a label between the closing curly brace of the TRY code block and the CATCH command. However, you can specify comments between the TRY and block and its CATCH block. Only one CATCH block is permitted for each TRY block. However, it is possible to nest paired TRY/CATCH blocks, such as the following:

  TRY {
       /\* TRY code \*/
       TRY {
           /\* nested TRY code \*/
       }
       CATCH {
          /\* nested CATCH code \*/
       }
  }
  CATCH {
      /\* CATCH code \*/
  }

Commonly, a Caché ObjectScript program consists of multiple TRY blocks, each TRY block immediately followed by its associated CATCH block.

QUIT and RETURN

You exit a TRY block using [QUIT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cquit) or [RETURN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_creturn). QUIT exits the current block structure and continues execution with the next command outside of that block structure. For example, if you are within a nested TRY block, issuing a QUIT exits that TRY block to the enclosing block structure. Issuing a QUIT command within a TRY block transfers execution to the first code line after the corresponding CATCH block. You cannot use an argumented QUIT to exit a TRY block; attempted to do so results in a compile error. To exit a routine completely from within a TRY block, issue a RETURN statement.

In rare circumstances, a TRY block QUIT or RETURN command may generate an exception. This could happen if the TRY created a new context and then deleted some aspect of the old context; attempting to revert to the old context would cause an exception. A TRY block QUIT or RETURN exception does not invoke the CATCH block exception handler.

$ZTRAP and $ETRAP

The TRY and CATCH commands perform error handling within an execution level. When an exception occurs within a TRY block, Caché normally executes the CATCH block of exception handler code that immediately follows the TRY block. This is the preferred error handling behavior.

You cannot set $ZTRAP or $ETRAP within a TRY block. However, you can set $ZTRAP or $ETRAP within an [argumentless DO](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cdo#RCOS_cdo39) block within the TRY block. This is because the lines of the DO block are in their own execution level and can thus have their own error handler.

If a $ZTRAP was set before entering the TRY block and an exception occurs within the TRY block, Caché takes the CATCH block rather than the $ZTRAP.

If $ETRAP was set before entering the TRY block and an exception occurs within the TRY block, Caché may take $ETRAP rather than CATCH unless you forestall this possibility. If both $ETRAP and CATCH are present when an exception occurs, Caché executes the error code (CATCH or $ETRAP) that applies to the current execution level. Because $ETRAP is intrinsically not associated with an execution level, Caché assumes that it is associated with the current execution level unless you specify otherwise. You must NEW $ETRAP before setting $ETRAP to establish a level marker for $ETRAP, so that Caché will correctly take CATCH as the current level exception handler, rather than $ETRAP. Otherwise, a system error (including a system error thrown by the THROW command) may take the $ETRAP exception handler.

GOTO and DO

You can use a [GOTO](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cgoto) or [DO](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cdo) command to enter a TRY block at a label within the TRY block. If an exception occurs later in the TRY block, the CATCH block exception handler is taken, just as if you had entered the TRY block at the TRY keyword. However, for clarity of coding, entering a TRY block using GOTO or DO should be avoided.

You can, of course, issue a GOTO from within a TRY block or a CATCH block.

Using a GOTO or DO to enter a CATCH block is strongly discouraged.

DO Within a TRY Block

When using a TRY statement, a THROW causes a search of the frame stack trying to find the appropriate CATCH block. When the frame stack indicates execution within a TRY block then execution will resume at the corresponding CATCH block. However, Caché must remove any "local" calls within the current TRY block before executing the CATCH block.

If a TRY block contains a DO statement that results in a reentry to that TRY block, one of two things may happen:

A “local” DO call (DO call that remains within the current TRY block): If the previous frame stack entry is a DO call located in the same TRY block, that DO is assumed to be a "local" subroutine call within the current TRY block. In this case, the CATCH is not immediately entered, but instead the frame stack is popped (possibly removing some recently allocated NEW variables) and the search resumes at the DO call in the current TRY block. If the new previous frame stack entry is not a DO from inside the current TRY block then the corresponding CATCH block is entered. However, if the previous frame stack entry is another DO in the same TRY then the frame stack is popped again (along with recently allocated NEW variables). This operation continues until the previous frame stack entry is not a DO, at which point the CATCH block is entered.

A “recursive” DO call (DO call inside a TRY block that leaves the TRY block but later execution reenters that TRY block): When searching for a CATCH block, if the previous frame stack entry is a DO inside the current TRY block, but the target label of that previous stack frame is not within the current TRY block (including any nested TRY blocks) then the frame stack is not popped (and no recently allocated local variables are popped) and theCATCH block is immediately entered. Note that if that CATCH block does another THROW then it is possible that the current CATCH block will be reentered because the recursive DO frame is still on the frame stack.

Examples

The examples in this section show runtime errors ([%Exception.SystemException](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Exception.SystemException) errors). For examples of user-specified exceptions invoked by issuing a THROW, refer to the [THROW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cthrow) and [CATCH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ccatch) commands.

In the following examples, the TRY code block is executed. It attempts to set the local variable a. In the first example, the code completes successfully, and the CATCH is skipped over. In the second example, the code fails with an <UNDEFINED> error, and execution is passed to the CATCH exception handler.

TRY succeeds. CATCH block is skipped. Execution continues with the 2nd TRY block:

  TRY {
    WRITE "1st TRY block",!
    SET x\="fred"
    WRITE "x is a defined variable",!
    SET a\=x
  }
  CATCH exp
  {
     WRITE !,"This is the CATCH exception handler",!
      IF 1\=exp.%IsA("%Exception.SystemException") {
         WRITE "System exception",!
         WRITE "Name: ",$ZCVT(exp.Name,"O","HTML"),!
         WRITE "Location: ",exp.Location,!
         WRITE "Code: ",exp.Code,!
         WRITE "Data: ",exp.Data,!!
      }
      ELSE { WRITE "not a system exception",!!}
      WRITE "$ZERROR: ",$ZERROR,!
      WRITE "$ECODE: ",$ECODE
      RETURN
  }
  TRY {
    WRITE !,"2nd TRY block",!
    WRITE "This is where the code falls through",!
    WRITE "$ZERROR: ",$ZERROR,!
    WRITE "$ECODE: ",$ECODE
  }
  CATCH exp2 {
    WRITE !,"This is the 2nd CATCH exception handler",!
  }

 

TRY fails. Execution continues with the CATCH block. CATCH block ends with RETURN, so 2nd TRY block is not executed:

  TRY {
    WRITE "1st TRY block",!
    KILL x
    WRITE "x is an undefined variable",!
    SET a\=x
  }
  CATCH exp {
      WRITE !,"This is the CATCH exception handler",!
      IF 1\=exp.%IsA("%Exception.SystemException") {
         WRITE "System exception",!
         WRITE "Name: ",$ZCVT(exp.Name,"O","HTML"),!
         WRITE "Location: ",exp.Location,!
         WRITE "Code: ",exp.Code,!
         WRITE "Data: ",exp.Data,!!
      }
      ELSE { WRITE "not a system exception",!!}
      WRITE "$ZERROR: ",$ZERROR,!
      WRITE "$ECODE: ",$ECODE
      RETURN
  }
  TRY {
    WRITE !,"2nd TRY block",!
    WRITE "This is where the code falls through",!
    WRITE "$ZERROR: ",$ZERROR,!
    WRITE "$ECODE: ",$ECODE
  }
  CATCH exp2 {
    WRITE !,"This is the 2nd CATCH exception handler",!
  }

 

TRY quits. In the following example, the CATCH block is not executed because execution of the TRY block is ended by either a QUIT or a RETURN, not an error. If RETURN, program execution stops. If QUIT, program execution continues with the 2nd TRY block:

  TRY {
    WRITE "1st TRY block",!
    KILL x
    WRITE "x is an undefined variable",!
    SET decide\=$RANDOM(2)
    IF decide\=0 { WRITE "issued a QUIT",!
                  QUIT }
    IF decide\=1 { WRITE "issued a RETURN",!
                  RETURN }
    WRITE "This should never display",!
    SET a\=x
  }
  CATCH exp {
      WRITE !,"This is the CATCH exception handler",!
      IF 1\=exp.%IsA("%Exception.SystemException") {
         WRITE "System exception",!
         WRITE "Name: ",$ZCVT(exp.Name,"O","HTML"),!
         WRITE "Location: ",exp.Location,!
         WRITE "Code: ",exp.Code,!
         WRITE "Data: ",exp.Data,!!
      }
      ELSE { WRITE "not a system exception",!!}
      WRITE "$ZERROR: ",$ZERROR,!
      WRITE "$ECODE: ",$ECODE
      RETURN
  }
  TRY {
    WRITE !,"2nd TRY block",!
    WRITE "This is where the code falls through",!
    WRITE "$ZERROR: ",$ZERROR,!
    WRITE "$ECODE: ",$ECODE
  }
  CATCH exp2 {
    WRITE !,"This is the 2nd CATCH exception handler",!
  }

 

See Also

*   [CATCH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ccatch) command
    
*   [THROW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cthrow) command
    
*   [$ETRAP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vetrap) special variable
    
*   [Error Processing](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_errors) in Using Caché ObjectScript
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ctrollback "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ctstart "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_ctry.xml**