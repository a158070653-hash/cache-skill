# ZKILL

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_czkill

---

 

Caché ObjectScript Reference

ZKILL

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cxecute "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cznspace "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_COMMANDS "Caché ObjectScript Commands") \]  >  \[ [ZKILL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czkill "ZKILL") \]

Go to: Description Arguments Example See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Deletes a node while preserving the node’s descendants.

Synopsis

ZKILL:pc array-node,...
ZK:pc array-node,...

Arguments

pc

Optional — A postconditional expression.

array-node

A local variable, a process-private global, or a global that is an array node, or a comma-separated list of local, process-private global, or global array nodes.

Description

The ZKILL command removes the value of a specified array-node without killing that node’s descendants. In contrast, the KILL command removes the value of a specified array node and all of that node’s descendants. An array node can be a local variable, a process-private variable, or a global variable.

By default, any subsequent reference to this killed array-node generates an <UNDEFINED> error. You can change Caché behavior to not generate an <UNDEFINED> error when referencing an undefined subscripted variable by setting the [%SYSTEM.Process.Undefined()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process#Undefined) method.

Arguments

pc

An optional postconditional expression. Caché executes the command if the postconditional expression is true (evaluates to a nonzero numeric value). Caché does not execute the command if the postconditional expression is false (evaluates to zero). For further details, refer to [Command Postconditional Expressions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_commands#GCOS_commands_pc) in Using Caché ObjectScript.

array-node

A local, process-private global, or global array node. You can specify a single array node, or a comma-separated list of array nodes. For further details on subscripts and nodes, refer to [Global Structure](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GGBL_structure) in Using Caché Globals.

Attempting to use ZKILL on a [structured system variable (SSVN)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_SVARIABLES) (such as ^$GLOBAL) results in a <COMMAND> error.

Example

In this example, the ZKILL command deletes node a(1), but does not remove node a(1,1).

    SET a(1)\=1,a(1,1)\=11
    SET x\=a(1)
    SET y\=a(1,1)
    ZKILL a(1)
    SET z\=a(1,1)
    WRITE "x=",x," y=",y," z=",z

 

returns x=1 y=11 z=11. However, then issuing a:

   WRITE a(1)

generates an <UNDEFINED> error.

See Also

*   [KILL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ckill) command
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cxecute "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cznspace "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_czkill.xml**