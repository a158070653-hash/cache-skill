# DO WHILE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_cdowhile

---

 

Caché ObjectScript Reference

DO WHILE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cdo "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_celse "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_COMMANDS "Caché ObjectScript Commands") \]  >  \[ [DO WHILE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cdowhile "DO WHILE") \]

Go to: Description Arguments Examples Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Executes code while a condition exists.

Synopsis

DO {code} WHILE expression,...

Arguments

code

A block of Caché ObjectScript commands enclosed in curly braces.

expression

A boolean test condition expression, or a comma-separated list of boolean test condition expressions.

Description

DO WHILE executes code, then evaluates expression. If expression evaluates to TRUE, DO WHILE loops and re-executes code. If expression is not TRUE, code is not re-executed, and the next command following DO WHILE is executed.

Note that DO WHILE is always written in block-oriented form. The code to be executed is placed between the DO and the WHILE keywords, and is enclosed by curly braces.

An opening or closing curly brace may appear on its own code line or on the same line as a command. An opening or closing curly brace may even appear in column 1 (though this is not recommended). It is a recommended programming practice to indent curly braces to indicate the beginning and end of a nested block of code. No whitespace is required before or after an opening curly brace. No whitespace is required before or after a closing curly brace, including a curly brace that follows an argumentless command.

DO WHILE (unlike the unrelated DO command) does not create a new execution level. Commands that are sensitive to the execution level, such as NEW and SET $ZTRAP, that are invoked during the DO WHILE loop remain in effect after the loop concludes.

Arguments

code

A block of one or more Caché ObjectScript commands. The code block may span several lines. The code block is enclosed by curly braces. The commands and comments within the code block and arguments within commands may be separated from one another by one or more blank spaces and/or line returns. However, as in all Caché ObjectScript commands, each command keyword must be separated from its first argument by exactly one space.

expression

A test condition which can take the form of a single expression or a comma-separated list of expressions. For an expression list, Caché evaluates the individual expressions in left to right order. It stops evaluation if it encounters an expression that is FALSE. If all expressions evaluate to TRUE, Caché re-executes the code commands. DO WHILE executes repeatedly, testing expression for each loop. If any expression evaluates to FALSE, Caché ignores any remaining expressions, and does not loop. It executes the next command after DO WHILE.

Examples

The following examples show first a DO WHILE in which expression is TRUE, and then a DO WHILE in which expression is FALSE. When expression is FALSE, the code block is executed once.

DoWhileTrue
   SET x\=1
   DO {
     WRITE !,"Looping",x
     SET x\=x+1
   } WHILE x<10
   WRITE !,"DONE"

 

This program writes Looping1 through Looping9 and then DONE.

DoWhileFalse
   SET x\=11
   DO {
      WRITE !,"Looping",x
      SET x\=x+1
   } WHILE x<10
   WRITE " DONE"

 

This program writes Looping11 DONE.

Notes

DO WHILE and WHILE

The DO WHILE command executes the loop once and then tests expression. The WHILE command tests expression before executing the loop.

DO WHILE and CONTINUE

Within the code block of a DO WHILE command, encountering a [CONTINUE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ccontinue) command causes execution to immediately jump to the WHILE keyword. The DO WHILE command then evaluates the WHILE expression test condition, and, based on that evaluation, determines whether to re-execute the code block loop. Thus, the CONTINUE command has exactly the same effect on execution as reaching the closing curly brace of the code block.

DO WHILE, QUIT, and RETURN

The [QUIT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cquit) command within the code block ends the DO WHILE loop and transfers execution to the command following the WHILE keyword, as shown in the following example:

Testloop
   SET x\=1
   DO {
      WRITE !,"Looping",x 
      QUIT:x\=5
      SET x\=x+1 
    } WHILE x<10 
    WRITE !,"DONE"

 

This program writes Looping1 through Looping5 and then DONE.

DO WHILE code blocks may be nested. That is, a DO WHILE code block may contain another flow-of-control loop (another DO WHILE, or a FOR or WHILE code block). A QUIT in an inner nested loop breaks out of the inner loop, to the next enclosing outer loop. This is shown in the following example:

Nestedloops
  SET x\=1,y\=1
  DO {
     WRITE "outer loop ",!
     DO {
        WRITE "inner loop " 
        WRITE " y=",y,!
        QUIT:y\=7
        SET y\=y+2 
        } WHILE y<100
     WRITE "back to outer loop x=",x,!!
     SET x\=x+1 
     } WHILE x<6 
  WRITE "Done"

 

You can use [RETURN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_creturn) to terminate execution of a routine at any point, including from within a DO WHILE loop or nested loop structure. RETURN always exits the current routine, returning to the calling routine or terminating the program if there is no calling routine. RETURN always behaves the same, regardless of whether it is issued from within a code block.

DO WHILE and GOTO

A GOTO command within the code block may direct execution to a [label](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_syntax#GCOS_syntax_labels) outside the loop, terminating the loop. (A label is also sometimes referred to as a tag.) A GOTO command within the code block may direct execution to a label within the same code block. A GOTO can be used to exit a nested code block. Other uses of GOTO, though supported, are not recommended.

The following example uses GOTO to exit a DO WHILE loop:

mainloop
  DO {
      WRITE !,"In an infinite DO WHILE loop"
      GOTO label1
      WRITE !,"This should not display"
  } WHILE 1\=1
  WRITE !,"This should not display"
label1
  WRITE !,"Went to label1 and quit"
  QUIT

 

The following example uses GOTO to transfer execution within a DO WHILE loop:

mainloop ; Example of a GOTO to within the code block
  SET x\=1
  DO {
      WRITE !,"In the DO WHILE loop"
      GOTO label1
      WRITE !,"This should not display"
label1
      WRITE !,"Still in the DO WHILE loop after GOTO"
      SET x\=x+1
      WRITE !,"x= ",x
      } WHILE x<3
  WRITE !,"DO WHILE loop done" 

 

See Also

*   [WHILE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cwhile) command
    
*   [FOR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cfor) command
    
*   [IF](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cif) command
    
*   [CONTINUE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ccontinue) command
    
*   [GOTO](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cgoto) command
    
*   [QUIT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cquit) command
    
*   [RETURN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_creturn) command
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cdo "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_celse "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_cdowhile.xml**