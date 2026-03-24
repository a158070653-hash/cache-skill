# ZZWRITE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_czzwrite

---

 

Caché ObjectScript Reference

ZZWRITE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czzdump "Go back to the previous section.") 

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_COMMANDS "Caché ObjectScript Commands") \]  >  \[ [ZZWRITE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czzwrite "ZZWRITE") \]

Go to: Description Arguments See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Displays the values of variables or expressions.

Synopsis

ZZWRITE:pc expr,...

Arguments

pc

Optional — A postconditional expression.

expr

An expression whose value is to be displayed. May be a variable or an expression, or a comma-separated list of variables or expressions.

Description

The ZZWRITE command evaluates an expression and displays the value. This expression can contain literals, [local variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_variables#GCOS_variables_local), [process-private globals](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_variables#GCOS_variables_procprivglbls), [global variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_variables#GCOS_variables_global), or [special variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_variables#GCOS_variables_special). ZZWRITE can evaluate a comma-separated list of expressions or variables; it lists the results in order specified. ZZWRITE lists the result of each expression as %val=value, one item per line, on the current device.

ZZWRITE displays a numeric value as a [canonical number](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_numcanonical). ZZWRITE displays a string that contains a number in canonical form as a canonical number (rather than as a string). ZZWRITE displays all other numeric and non-numeric strings as a quoted string. This is shown in the following example:

  SET a\=7,b\="14",c\="+21.0",d\="7dwarves"
  ZZWRITE a,b,c,d

 

ZZWRITE displays a list as a $lb ([$LISTBUILD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistbuild)) construct, rather than displaying the resulting encoded list string. This is shown in the following example:

  SET a\=7,b\=14,c\=21
  ZZWRITE $LISTBUILD(a,b)\_$LISTBUILD(c)

 

For a comparison of ZZWRITE with the [WRITE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cwrite), [ZWRITE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czwrite), and [ZZDUMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czzdump) commands, refer to the [Write Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_commands#GCOS_cmds_iowrite) features tables in the “Commands” chapter of Using Caché ObjectScript.

ZZWRITE with an Object Reference

If the ZZWRITE argument is an object reference, ZZWRITE displays all of the attributes of the object, one attribute per line. The display format is the same as the [%SYSTEM.OBJ.Dump()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.OBJ#Dump) method. If the object reference is an array, ZZWRITE displays only the attributes of the top-level node; it does not display the subnode attributes.

This is shown in the following examples:

  SET tStatement \= ##class(%SQL.Statement).%New()
  ZZWRITE tStatement

 

  SET doref\=##class(%iKnow.Domain).Create("mytempdomain")
  SET domId\=doref.Id
  ZZWRITE doref
  SET stat\=##class(%iKnow.Domain).DeleteId(domId)

 

This behavior is the same as ZWRITE.

ZZWRITE with a Bitstring

If the ZZWRITE expr argument is a Caché compressed bitstring (created using the [$BIT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fbit) function), ZZWRITE displays the decimal representation of the compressed binary string as $ZWCHAR ($zwc) two-byte (wide) characters.

ZZWRITE also displays a comment that lists the uncompressed “1” bits in left-to-right order as a comma-separated list. If there are three or more consecutive “1” bits, it lists them as a range (inclusive) with two dot syntax (n..m). For example, the bitstring \[1,0,1,1,1,1,0,1\] is shown as /\*$bit(1,3..6,8)\*/. The bitstring \[1,1,1,1,1,1,1,1\] is shown as /\*$bit(1..8)\*/. The bitstring \[0,0,0,0,0,0,0,0\] is shown as /\*$bit()\*/.

The following example shows ZZWRITE bitstring output:

  SET $BIT(a,1) \= 0
  SET $BIT(a,2) \= 0
  SET $BIT(a,3) \= 1
  SET $BIT(a,4) \= 0
  SET $BIT(a,5) \= 1
  SET $BIT(a,6) \= 1
  SET $BIT(a,7) \= 1
  SET $BIT(a,8) \= 0
  ZZWRITE a

 

Arguments

pc

An optional postconditional expression. Caché executes the command if the postconditional expression is true (evaluates to a nonzero numeric value). Caché does not execute the command if the postconditional expression is false (evaluates to zero). For further details, refer to [Command Postconditional Expressions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_commands#GCOS_commands_pc) in Using Caché ObjectScript.

expr

An expression to evaluate, or a comma-separated list of expressions. An expr can consist of, or contain local variables, process-private globals, global variables, or special variables. It cannot be a private variable. Variables can be subscripted. Expressions are evaluated in strict left-to-right order.

You can use extended global reference to specify a global variable not mapped to the current namespace. If you specify a nonexistent namespace, Caché issues a <NAMESPACE> error. If you specify a namespace for which you do not have privileges, Caché issues a <PROTECT> error, followed by the global name and database path, such as the following: <PROTECT> ^myglobal,c:\\intersystems\\cache\\mgr\\. For further information on subscripted variables and extended global reference, refer to [Global Structure](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GGBL_structure) in Using Caché Globals.

See Also

*   [WRITE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cwrite) command
    
*   [ZWRITE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czwrite) command
    
*   [ZZDUMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czzdump) command
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czzdump "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_czzwrite.xml**