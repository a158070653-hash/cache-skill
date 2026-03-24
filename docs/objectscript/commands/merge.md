# MERGE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_cmerge

---

 

Caché ObjectScript Reference

MERGE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_clock "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cnew "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_COMMANDS "Caché ObjectScript Commands") \]  >  \[ [MERGE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cmerge "MERGE") \]

Go to: Description Arguments Examples Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Merges global nodes or subtrees from source into destination.

Synopsis

MERGE:pc mergeargument,...
M:pc mergeargument,...

where mergeargument is:

destination=source

Arguments

pc

Optional — A postconditional expression.

destination and source

Local variables, process-private globals, or globals to be merged. If specified as a class property, the source variable must be a multidimensional (subscripted) variable.

Description

MERGE destination\=source copies source into destination and all descendants of source into descendants of destination. It does not modify source, or kill any nodes in destination.

MERGE simplifies the copying of a subtree (multiple subscripts) of a variable to another variable. Either variable can be a subscripted local variable, process-private global, or global. A subtree is all variables that are descendants of a specified variable. MERGE offers a one-command alternative to the current technique for doing subtree copy: a series of SET commands with $ORDER references.

MERGE issues a <COMMAND> error if the source and destination have a parent-child relationship.

The MERGE command can take longer than most other Caché ObjectScript commands to execute. As a result, it is more prone to interruption. The effect of interruption is implementation-specific. Under Caché, an interruption may cause an unpredictable subset of the source to have been copied to the destination subtree.

Arguments

pc

An optional postconditional expression. Caché executes the MERGE command if the postconditional expression is true (evaluates to a nonzero numeric value). Caché does not execute the command if the postconditional expression is false (evaluates to zero). For further details, refer to [Command Postconditional Expressions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_commands#GCOS_commands_pc) in Using Caché ObjectScript.

destination and source

Variables to be merged. Either variable can be a local variable, a process-private global, or a global. If destination is undefined, MERGE defines it and sets it to source. If source is undefined, MERGE completes successfully, but does not change destination.

You can specify multiple, comma-separated destination\=source pairs. They are evaluated in left-to-right order.

A mergeargument can reference a destination\=source pair by [indirection](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_operators#GCOS_operators_indirection). For example, MERGE @tMergeString.

Examples

The following example copies a subtree from one global variable (^a) to another global variable (^b). In this case, the merge is being used to create a smaller global ^b, which contains only the ^a(1,1) subtree of the information in ^a.

   SET ^a\="cartoons"
   SET ^a(1)\="The Flintstones",^a(2)\="The Simpsons"
   SET ^a(1,1)\="characters",^a(1,2)\="place names"
   SET ^a(1,1,1)\="Flintstone family"
   SET ^a(1,1,1,1)\="Fred"
   SET ^a(1,1,1,2)\="Wilma"
   SET ^a(1,1,2)\="Rubble family"
   SET ^a(1,1,2,1)\="Barney"
   SET ^a(1,1,2,2)\="Betty"
   MERGE ^b\=^a(1,1)
   WRITE ^b,!,^b(2),!,^b(2,1)," and ",^b(2,2)

 

The following example shows how a destination global variable looks after it has been merged with a subtree of a source global variable.

Suppose you execute the following:

  KILL ^X,^Y
  SET ^X(2,2)\="first"
  SET ^X(2,2,4)\="second"
  SET ^Y(3,6,7)\="third"
  SET ^Y(3,6,8)\="fourth"
  SET ^Y(3,6,7,8,4)\="fifth"
  SET ^Y(3,6,7,8,9)\="sixth"
  WRITE ^X(2,2),!,^X(2,2,4),!
  WRITE ^Y(3,6,7),!,^Y(3,6,8),!
  WRITE ^Y(3,6,7,8,4),!,^Y(3,6,7,8,9)

 

The following figure shows the resulting logical structure of ^X and ^Y.

Initial Structure of ^X and ^Y

![](images/rcos_cmerge_initial.png)

Consider the following MERGE command:

  MERGE ^X(2,3)\=^Y(3,6,7,8)

When you issue the previous statement, Caché copies part of ^Y into ^X(2,3). The global ^X now has the structure illustrated in the figure below.

Result on ^X and ^Y of MERGE Command

![](images/rcos_cmerge_result.png)

Notes

Naked Indicator

When both destination and source are local variables, the naked indicator is not changed. If source is a global variable and destination is a local variable, then the naked indicator references source.

When both source and destination are global variables, the naked indicator is unchanged if source is undefined ($DATA(source)=0). In all other cases (including $DATA(source)=10), the naked indicator takes the same value that it would have if the SET command replaced the MERGE command and source had a value. For more details on the naked indicator, see [Naked Global Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GGBL_using) in Using Caché Globals.

Merge to Self

When the destination and source are the same variable, no merge occurs. Nothing is recorded in the journal file. However, the naked indicator may be changed, based on the rules described in the previous section.

Watchpoints

The MERGE command supports watchpoints. If a watchpoint is in effect, Caché triggers that watchpoint whenever that MERGE alters the value of a watched variable. To set watchpoints, use the ZBREAK command.

See Also

*   [ZBREAK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czbreak) command
    
*   [Debugging](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_debug) chapter in Using Caché ObjectScript
    
*   [Global Structure](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GGBL_structure) chapter in Using Caché Globals
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_clock "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cnew "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_cmerge.xml**