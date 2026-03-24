# WHILE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_cwhile

---

 

Caché ObjectScript Reference

WHILE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cview "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cwrite "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_COMMANDS "Caché ObjectScript Commands") \]  >  \[ [WHILE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cwhile "WHILE") \]

Go to: Description Arguments Examples Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Executes code while a condition is true.

Synopsis

WHILE expression,... {
  code
}

Arguments

expression

A test condition. You can specify one or more comma-separated test conditions, all of which must be TRUE for execution of the code block.

code

A block of Caché ObjectScript commands enclosed in curly braces.

Description

WHILE tests expression and, if expression evaluates to TRUE, it then executes the block of code (one or more commands) between the opening and closing curly braces. WHILE can execute a block of code repeatedly, as long as expression evaluates to TRUE. If expression is not TRUE, the block of code within the curly braces is not executed, and the next command following the closing curly brace ( } ) is executed.

Programmers must be careful to avoid a WHILE infinite loop.

An opening or closing curly brace may appear on its own code line or on the same line as a command. An opening or closing curly brace may even appear in column 1 (though this is not recommended). It is a recommended programming practice to indent curly braces to indicate the beginning and end of a nested block of code. No whitespace is required before or after an opening curly brace. No whitespace is required before a closing curly brace, including a curly brace that follows an argumentless command. There is only one whitespace requirement for curly braces: a closing curly brace must be separated from the command that follows it by a space, tab, or line return.

The block of code within the curly braces can consist of one or more Caché ObjectScript commands and function calls. This block of code may span several lines. Indents, line returns, and blank spaces are permitted within the block of code. Commands within this code block and arguments within commands may be separated by one or more blank spaces or line returns.

Arguments

expression

A boolean test condition. It can take the form of a single expression or a comma-separated list of expressions. Caché executes the WHILE loop if it evaluates expression as TRUE (any non-zero numeric value). Commonly expression is a condition test, such as x<10 or "apple"="apple", but any value that evaluates to a non-zero number is TRUE. For example 7, 00.1, “700”, “7dwarves” all evaluate to TRUE. Any value that evaluates to zero is FALSE. For example, 0, -0, and any non-numeric string all evaluate to FALSE.

For an expression list, Caché evaluates the individual expressions in left-to-right order. It stops evaluation if it encounters an expression that evaluates to 0 (FALSE). Any expressions to the right of an expression that evaluates to FALSE are not validated or tested.

If all expressions evaluate to a non-zero numeric value (TRUE), Caché executes the WHILE loop code block. As long as expression evaluates to TRUE, Caché continues to execute the WHILE loop repeatedly, testing expression at the top of each loop. If any expression evaluates to FALSE, Caché executes the next line of code after the WHILE closing curly brace.

Examples

The following example performs a WHILE loop a specified number of times. It tests the expression before executing the loop:

Mainloop
   SET x\=1
   WHILE x<10 {
      WRITE !," Looping",x
      SET x\=x+1
   }
   WRITE !,"DONE"
   QUIT

 

The following pair of examples perform two expression tests. The two tests are separated by a comma. If both tests evaluate to true, it executes WHILE loop. Thus, these programs either return all of the items in a list, or a specified sample size of the items in a list:

  SET mylist\=$LISTBUILD("a","b","c","d","e")
  SET ptr\=0,sampcnt\=1,sampmax\=4
  WHILE 1\=$LISTNEXT(mylist,ptr,value),sampcnt<sampmax {
    WRITE value," is item ",sampcnt,!
    SET sampcnt\=sampcnt+1
  }
  IF sampcnt<sampmax {WRITE "This is the whole list"}
  ELSE {WRITE "This is a ",sampcnt\-1," item sample of the list"}

 

  SET mylist\=$LISTBUILD("a","b","c","d","e")
  SET ptr\=0,sampcnt\=1,sampmax\=10
  WHILE 1\=$LISTNEXT(mylist,ptr,value),sampcnt<sampmax {
    WRITE value," is item ",sampcnt,!
    SET sampcnt\=sampcnt+1
  }
  IF sampcnt<sampmax {WRITE "This is the whole list"}
  ELSE {WRITE "This is a ",sampcnt\-1," item sample of the list"}

 

Notes

WHILE and DO WHILE

The WHILE command tests expression before executing the loop. The DO WHILE command executes the loop once and then tests expression.

WHILE and CONTINUE

Within the code block of a WHILE command, encountering a CONTINUE command causes execution to immediately jump back to the WHILE command. The WHILE command then evaluates its expression test condition, and, based on that evaluation, determines whether to re-execute the code block loop. Thus, the CONTINUE command has exactly the same effect on execution as reaching the closing curly brace of the code block.

WHILE, QUIT, and RETURN

The [QUIT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cquit) command within the code block ends the WHILE loop and transfers execution to the command following the closing curly brace, as shown in the following example:

Testloop
   SET x\=1
   WHILE x < 10
   {
      WRITE !,"Looping",x 
      QUIT:x\=5
      SET x\=x+1 
    }
    WRITE !,"DONE"

 

This program writes Looping1 through Looping5 and then DONE.

WHILE code blocks may be nested. That is, a WHILE code block may contain another flow-of-control loop (another WHILE, or a FOR or DO WHILE code block). A QUIT in an inner nested loop breaks out of the inner loop, to the next enclosing outer loop. This is shown in the following example:

Nestedloops
  SET x\=1,y\=1
  WHILE x<6 {
     WRITE "outer loop ",!
     WHILE y<100 {
        WRITE "inner loop " 
        WRITE " y=",y,!
        QUIT:y\=7
        SET y\=y+2 
        }
     WRITE "back to outer loop x=",x,!!
     SET x\=x+1 
     }
  WRITE "Done"

 

You can use [RETURN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_creturn) to terminate execution of a routine at any point, including from within a WHILE loop or nested loop structure. RETURN always exits the current routine, returning to the calling routine or terminating the program if there is no calling routine. RETURN always behaves the same, regardless of whether it is issued from within a code block.

WHILE and GOTO

A GOTO command within the block of code may direct execution to a [label](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_syntax#GCOS_syntax_labels) outside the loop, terminating the loop. A GOTO command within the block of code may direct execution to a label within the same block of code; this label may be in a nested code block.

A GOTO command should not direct execution to a label within another code block. While such a construct may execute, it is considered “illegal” because it defeats the test condition for the code block it is entering.

The following forms of GOTO are legal:

mainloop ; GOTO to outside of the code block
  WHILE 1\=1 {
      WRITE !,"In an infinite WHILE loop"
      GOTO label1
      WRITE !,"This should not display"
  } 
  WRITE !,"This should not display"
label1
  WRITE !,"Went to label1 and quit"

 

mainloop ; GOTO to elsewhere within the same code block
  SET x\=1
  WHILE x<3 {
      WRITE !,"In the WHILE loop"
      GOTO label1
      WRITE !,"This should not display"
label1
      WRITE !,"Still in the WHILE loop after GOTO"
      SET x\=x+1
      WRITE !,"x=",x
      }
  WRITE !,"WHILE loop done" 

 

mainloop ; GOTO from an inner to an outer nested code block
   SET x\=1,y\=1
   WHILE x<6 {
     WRITE !,"Outer loop",!
     SET x\=x+1
label1
     WRITE "outer loop iteration ",x\-1,!
        WHILE y<4 {
           WRITE !,"   Inner loop iteration ",y,!
           SET y\=y+1
           WRITE "   return to "
           GOTO label1
           WRITE "   This should not display",!
        }
   WRITE "Inner loop completed",!
   }
   WRITE "All done"

 

mainloop ; GOTO from an outer to an inner nested code block
   SET x\=1,y\=1
   WHILE x<6 {
     WRITE !,"Outer loop",!
     SET x\=x+1
     WRITE "outer loop iteration ",x\-1,!
     WRITE "Jumping into the "
     GOTO label1
     WRITE "This should not display",!
         WHILE y<4 {
           WRITE !,"   Inner loop iteration ",y,!
           SET y\=y+1
label1
           WRITE "inner loop ",!
        }
   WRITE "Inner loop completed",!
   }
   WRITE "All done"

 

The following forms of GOTO may execute, but they are considered “illegal” because they defeat (ignore) the condition test for the block that the GOTO enters into:

mainloop ; GOTO into a code block
  SET x\=1
  WRITE "Jumped into the "
  GOTO label1
  WHILE x\>1,x<6 {
      WRITE "Top of WHILE loop x=",x,!
label1
      WRITE "Bottom of WHILE loop x=",x,!!
      SET x\=x+1
  }

 

mainloop ; GOTO from a code block into an IF clause block
  SET x\=1
  WHILE x<6 {
      WRITE !,"WHILE loop interation=",x,!
      SET x\=x+1
      GOTO label1
      WRITE "This should never display",!
    IF x#2 { WRITE "in the IF clause",!
label1
    WRITE "GOTO entry into the IF clause",!
    WRITE x," is an odd number",!
    }
    ELSE {WRITE "in the ELSE clause",!
          WRITE x," is an even number",! }
  WRITE "Bottom of WHILE loop",!
  }
    WRITE "All done"

 

See Also

*   [DO WHILE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cdowhile) command
    
*   [FOR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cfor) command
    
*   [IF](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cif) command
    
*   [CONTINUE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ccontinue) command
    
*   [GOTO](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cgoto) command
    
*   [QUIT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cquit) command
    
*   [RETURN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_creturn) command
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cview "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cwrite "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_cwhile.xml**