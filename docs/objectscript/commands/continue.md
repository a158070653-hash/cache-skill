# CONTINUE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_ccontinue

---

 

Caché ObjectScript Reference

CONTINUE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cclose "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cdo "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_COMMANDS "Caché ObjectScript Commands") \]  >  \[ [CONTINUE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ccontinue "CONTINUE") \]

Go to: Description Arguments Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Jumps to FOR, WHILE, or DO WHILE command and re-executes test and loop.

Synopsis

CONTINUE:pc

Argument

pc

Optional — A postconditional expression.

Description

The CONTINUE command is used within the code block following a FOR, WHILE, or DO WHILE command. CONTINUE causes execution to jump back to the FOR, WHILE, or DO WHILE command. The FOR, WHILE, or DO WHILE command evaluates its test condition, and, based on that evaluation, re-executes the code block loop. Thus, the CONTINUE command has exactly the same effect on execution as reaching the closing curly brace ( } ) of the code block.

CONTINUE takes no arguments (other than the postconditional). At least two blank spaces must separate it from a command following it on the same line.

A CONTINUE can cause execution to jump out of a TRY or CATCH block to return to its control flow statement.

Arguments

pc

An optional postconditional expression that can make the command conditional. Caché executes the CONTINUE command if the postconditional expression is true (evaluates to a nonzero numeric value). Caché does not execute the command if the postconditional expression is false (evaluates to zero). For further details, refer to [Command Postconditional Expressions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_commands#GCOS_commands_pc) in Using Caché ObjectScript.

Examples

The following example uses a CONTINUE with a postconditional expression. It loops through and prints out all the numbers from 1 to 10, except 3:

Loop
  FOR i\=1:1:10 {
    IF i # 2 { 
      CONTINUE:i\=3
      WRITE !,i," is odd" }
    ELSE { WRITE !,i," is even" }
    }
  WRITE !,"done with the loop"
  QUIT

 

The following example shows two nested FOR loops. The CONTINUE jumps back to the FOR in the inner loop:

Loop
  FOR i\=1:1:3 {
    WRITE !,"outer loop: i=",i
    FOR j\=2:2:10 {
      WRITE !,"inner loop: j=",j
      IF j '= 8 {CONTINUE  }
      ELSE { WRITE " crazy eight"}
      }
    WRITE !,"back to outer loop"
  }
  QUIT

 

The following example shows a CONTINUE that exits a TRY block. The CONTINUE jumps back to the FOR statement outside the TRY block.

TryLoop
  FOR i\=1:1:10 {
  WRITE !,"Top of FOR loop"
    TRY {
    WRITE !,"In TRY block: i=",i
    IF i\=7 {
      WRITE " lucky seven" }
      ELSE { CONTINUE }
    }
    CATCH exp {
       WRITE !,"CATCH block exception handler",!
       WRITE "Error code=",exp.Code
       RETURN
    }
    WRITE !,"Bottom of the FOR loop"
  }
  QUIT

 

See Also

*   [DO WHILE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cdowhile) command
    
*   [FOR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cfor) command
    
*   [WHILE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cwhile) command
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cclose "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cdo "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_ccontinue.xml**