# ELSE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_celse

---

 

Caché ObjectScript Reference

ELSE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cdowhile "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_celseif "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_COMMANDS "Caché ObjectScript Commands") \]  >  \[ [ELSE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_celse "ELSE") \]

Go to: Description See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Clause of block-oriented IF command.

Synopsis

ELSE { code }

Refer to [IF](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cif) command for complete syntax.

Description

ELSE is not a separate command, but a clause of the block-oriented IF command. You can specify a single ELSE clause as the final clause of an IF command, or you can omit the ELSE clause. Refer to the IF command for details and examples.

Note:

An earlier version of the ELSE command may exist in legacy applications where it is used with a line-oriented IF command. These commands may be recognized because they do not use curly braces. The old and new forms of IF and ELSE are syntactically different and should not be combined; therefore, an IF of one type should not be paired with an ELSE of the other type.

The earlier line-oriented ELSE command could be abbreviated as E. The block-oriented ELSE keyword cannot be abbreviated.

The ELSE keyword must be followed by an opening and closing curly brace ({) and (}). Usually these curly braces enclose a block of code. However, an ELSE with no code block is permissible, as in the following:

   SET x\=1
Loop
   IF x\=1{
          WRITE "Once only"
          SET x\=x+1
          GOTO Loop
          }
   ELSE{}
   WRITE !,"All done"

 

There are no whitespace restrictions on the ELSE keyword.

See Also

*   [IF](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cif) command
    
*   [IF (legacy version)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cif_legacy) command
    
*   [Flow of Control Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_commands#GCOS_cmds_flow) in Using Caché ObjectScript
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cdowhile "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_celseif "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_celse.xml**