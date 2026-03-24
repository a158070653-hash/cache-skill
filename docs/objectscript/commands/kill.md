# KILL

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_ckill

---

 

Caché ObjectScript Reference

KILL

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cjob "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_clock "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_COMMANDS "Caché ObjectScript Commands") \]  >  \[ [KILL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ckill "KILL") \]

Go to: Description Arguments Examples Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Deletes variables.

Synopsis

KILL:pc killargument,...
K:pc killargument,...

where killargument can be:

variable,...

(variable,...)

Arguments

pc

Optional — A postconditional expression.

variable

Optional — A variable name or comma-separated list of variable names. Without parentheses: the variable(s) to be deleted. With parentheses: the variable(s) to be kept.

Description

There are three forms of the KILL command:

*   KILL without an argument, known as an argumentless KILL.
    
*   KILL with a variable list, known as an inclusive KILL.
    
*   KILL with a variable list enclosed in parentheses, known as an exclusive KILL.
    

The KILL command without an argument deletes all local variables. It does not delete process-private globals, globals, or user-defined special variables.

KILL with a variable or comma-separated variable list as an argument:

KILL variable,...

is called an inclusive KILL. It deletes only the variable(s) you specify in the argument. Killing a variable kills all subscripts of that variable at all lower levels than the specified variable. The variables can be local variables, process-private variables, or globals. They do not have to be actual defined variables, but they must be valid variable names. You cannot kill a special variable, even if its value is user-specified. Attempting to do so generates a <SYNTAX> error.

KILL with a variable or comma-separated variable list enclosed in parentheses as an argument:

KILL (variable,...)

is called an exclusive KILL. It deletes all local variables except those you specify in the argument. The variables you specify can only be local variables. You cannot specify a subscripted variable; specifying a local variable preserves the variable and all of its subscripts. The local variables you specify do not have to be actual defined variables, but they must be valid local variable names.

Note:

KILL can delete local variables created by Caché objects. Therefore, do not use either argumentless KILL or exclusive KILL in any context where they might affect system structures (such as %objTX currently used in %Save) or system objects (such as the stored procedure context object). In most programming contexts, these forms of KILL should be avoided.

You can use the [$DATA](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fdata) function to determine whether a variable is defined or undefined, and whether a defined variable has subscripts. Killing a variable changes its $DATA status to undefined.

Using KILL to delete variables frees up local variable storage space. To determine or set the maximum storage space (in kilobytes) for the current process, use the [$ZSTORAGE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzstorage) special variable. To determine the available storage space (in bytes) for the current process, use the [$STORAGE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vstorage) special variable.

Arguments

pc

An optional postconditional expression. Caché executes the KILL command if the postconditional expression is true (evaluates to a nonzero numeric value). Caché does not execute the command if the postconditional expression is false (evaluates to zero). For further details, refer to [Command Postconditional Expressions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_commands#GCOS_commands_pc) in Using Caché ObjectScript.

variable

If not enclosed in parentheses: the variable(s) to be deleted by the KILL command. variable can be a single variable or a comma-separated list of variables.

If enclosed in parentheses: the local variable(s) to be kept by the KILL command; the KILL command deletes all other local variables. variable can be a single variable or a comma-separated list of variables.

Examples

In the following example, an inclusive KILL deletes local variables a, b, and d, and the deletes the process-private global ^||ppglob and all of its subscripts. No other variables are affected:

   SET ^||ppglob(1)\="fruit"
   SET ^||ppglob(1,1)\="apples"
   SET ^||ppglob(1,2)\="oranges"
   SET a\=1,b\=2,c\=3,d\=4,e\=5
   KILL a,b,d,^||ppglob
   WRITE "a=",$DATA(a)," b=",$DATA(b)," c=",$DATA(c)," d=",$DATA(d)," e=",$DATA(e),!
   WRITE "^||ppglob(1)=",$DATA(^||ppglob(1)),
         " ^||ppglob(1,1)=",$DATA(^||ppglob(1,1)),
         " ^||ppglob(1,2)=",$DATA(^||ppglob(1,2))

 

In the following example, an inclusive KILL deletes the local variable a(1) and its subscripts a(1,1), a(1,2) and a(1,1,1); it does not delete the local variables a, a(2), or a(2,1):

   SET a\="food",a(1)\="fruit",a(2)\="vegetables"
   SET a(1,1)\="apple",a(1,1,1)\="mackintosh",a(1,2)\="banana"
   SET a(2,1)\="artichoke"
   WRITE "before KILL:",!
   WRITE $DATA(a)," ",$DATA(a(1))," ",$DATA(a(1,1))," ",$DATA(a(1,1,1))," ",
         $DATA(a(2))," ",$DATA(a(2,1)),!
   KILL a(1)
   WRITE "after KILL:",!
   WRITE $DATA(a)," ",$DATA(a(1))," ",$DATA(a(1,1))," ",$DATA(a(1,1,1))," ",
         $DATA(a(2))," ",$DATA(a(2,1))

 

In the following example, an exclusive KILL deletes all local variables except for variables d and e:

   SET a\=1,b\=2,c\=3,d\=4,e\=5
   KILL (d,e)
   WRITE "a=",$DATA(a)," b=",$DATA(b)," c=",$DATA(c)," d=",$DATA(d)," e=",$DATA(e)

 

Note that because an exclusive KILL deletes object variables, the above program works from a terminal session, but does not work within an object method.

The following example, an inclusive KILL deletes two process-private globals and an exclusive KILL deletes all local variables except for variables d and e.

   SET ^||a\="one",^||b\="two",^||c\="three"
   SET a\=1,b\=2,c\=3,d\=4,e\=5
   KILL ^||a,^||c,(d,e)
   WRITE "a=",$DATA(a)," b=",$DATA(b)," c=",$DATA(c)," d=",$DATA(d)," e=",$DATA(e),!
   WRITE "^||a=",$DATA(^||a)," ^||b=",$DATA(^||b)," ^||c=",$DATA(^||c)

 

Notes

KILL and Objects

Object variables (OREFs) automatically maintain a reference count — the number of items currently referring to an object. Whenever you SET a variable or object property to refer to an object, Caché increments the object’s reference count. When you KILL a variable, Caché decrements the corresponding object reference count. When this reference count goes to 0, the object is automatically destroyed; that is, Caché removes it from memory. The object reference count is also decremented when a variable is SET to a new value, or when the variable goes out of scope.

In the case of a persistent object, call the [%Save()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25Library.Persistent#%Save) method before removing the object from memory if you wish to preserve changes to the object. The [%Delete()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25Library.Persistent#%Delete) method deletes the stored version of a Caché object; it does not remove the in-memory version of that object.

Prior to Caché version 5.0, the %Close() method could be used to remove objects from memory. With Caché version 5.0 and subsequent, the %Close() method performs no operation and always completes successfully. Do not use %Close() when writing new code.

KILL Using an Object Method

You can specify an object method on the left side of a KILL expression. The following example specifies the %Get() method:

  SET obj\=##class(test).%New()  // Where test is class with a multidimensional property md
  SET myarray\=\[(obj)\]
  SET index\=0,subscript\=2
  SET myarray.%Get(index).md(subscript)\="value"
    WRITE $DATA(myarray.%Get(index).md(subscript)),!
  KILL myarray.%Get(index).md(subscript)
    WRITE $DATA(myarray.%Get(index).md(subscript))

Inclusive KILL

An inclusive KILL deletes only those variables explicitly named. The list can include local variables, process-private globals, and globals—either subscripted or unsubscripted. The inclusive KILL is the only way to delete global variables.

Exclusive KILL

An exclusive KILL deletes all local variables except those that you explicitly name. Listed names are separated by commas. The enclosing parentheses are required.

The exception list can contain only local unsubscripted variable names. For example, if you have a local variable array named fruitbasket, which has several subscript nodes, you can preserve the entire local variable array by specifying KILL (fruitbasket); you cannot use exclusive KILL to selectively preserve individual subscript nodes. An exclusive kill list cannot specify a process-private global, a global, or a special variable; attempting to do so results in a <SYNTAX> error. Local variables not named in the exception list are deleted; subsequent references to such variables generate an <UNDEFINED> error. The exclusive KILL has no effect on process-private globals, globals, and special variables. However, it does delete local variables created by system objects.

Using KILL with Arrays

You can use an inclusive KILL to delete an entire array or a selected node within an array. The specified array can be a local variable, a process-private global, or a global variable.

*   To delete a local variable array, use any form of KILL.
    
*   To delete a selected node within a local variable array, you must use an inclusive KILL.
    
*   To delete a global variable array, you must use an inclusive KILL.
    
*   To delete a selected node within a global variable array, you must use an inclusive KILL.
    

For further details on global variables with subscripted nodes, see [Global Structure](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GGBL_structure) in Using Caché Globals.

To delete an array, simply supply its name to an inclusive KILL. For example, the following command deletes global array ^fruitbasket and all of its subordinate nodes.

   SET ^fruitbasket(1)\="fruit"
   SET ^fruitbasket(1,1)\="apples"
   SET ^fruitbasket(1,2)\="oranges"
   WRITE "Before KILL:",!
   WRITE "^fruitbasket(1)=",$DATA(^fruitbasket(1)),
         " ^fruitbasket(1,1)=",$DATA(^fruitbasket(1,1)),
         " ^fruitbasket(1,2)=",$DATA(^fruitbasket(1,1)),!
   KILL ^fruitbasket
   WRITE "After KILL:",!
   WRITE "^fruitbasket(1)=",$DATA(^fruitbasket(1)),
         " ^fruitbasket(1,1)=",$DATA(^fruitbasket(1,1)),
         " ^fruitbasket(1,2)=",$DATA(^fruitbasket(1,1))

 

To delete an array node, supply the appropriate subscript. For example, the following KILL command deletes the node at subscript 1,2.

   SET ^fruitbasket(1)\="fruit"
   SET ^fruitbasket(1,1)\="apples"
   SET ^fruitbasket(1,2)\="oranges"
   SET ^fruitbasket(1,2,1)\="navel"
   SET ^fruitbasket(1,2,2)\="mandarin"
   WRITE ^fruitbasket(1)," contains ",^fruitbasket(1,1),
         " and ",^fruitbasket(1,2),!
   WRITE ^fruitbasket(1,2)," contains ",^fruitbasket(1,2,1),
         " and ",^fruitbasket(1,2,2),!
   KILL ^fruitbasket(1,2)
   WRITE "1st level node: ",$DATA(^fruitbasket(1)),!
   WRITE "2nd level node: ",$DATA(^fruitbasket(1,1)),!
   WRITE "Deleted 2nd level node: ",$DATA(^fruitbasket(1,2)),!
   WRITE "3rd level node under deleted 2nd: ",$DATA(^fruitbasket(1,2,1)),!
   QUIT

 

When you delete an array node, you automatically delete all nodes subordinate to that node and any immediately preceding node that contains only a pointer to the deleted node. If a deleted node is the only node in its array, the array itself is deleted along with the node.

To delete multiple local variable arrays, you can use either the inclusive form or exclusive form of KILL, as described above. For example, the following command removes all local arrays except array1 and array2.

   KILL (array1,array2)

To delete multiple array nodes, you can use only the inclusive form of KILL. For example, the following command removes the three specified nodes, deleting one node from each array.

   KILL array1(2,4),array2(3,2),array3(1,7)

The nodes can be in the same or different arrays.

You may delete a specified local or global array node by using the ZKILL command. Unlike KILL, ZKILL does not delete all nodes subordinate to the specified node.

Effects of KILL with Parameter Passing

With parameter passing, values are passed to a user-defined function or to a subroutine called with the DO command. The values to be passed to the user-defined function or subroutine are supplied in a comma-separated list called the actual parameter list. Each value supplied is mapped, by position, into a corresponding variable in the formal parameter list defined for the user-defined function or subroutine.

Depending on how the actual parameter list is specified, parameter passing can occur in either of two ways: by value or by reference. For more information on these two types of parameter passing, see [Parameter Passing](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_usercode#GCOS_ucode_parampassing) in Using Caché ObjectScript.

Killing a variable in the formal parameter list has different results depending on whether passing by value or passing by reference is in effect.

If you are [passing a variable by value](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_usercode#GCOS_usercode_args_byval):

*   Killing a variable in the formal list has no effect outside the context of the invoked function or subroutine. This is because Caché automatically saves the current value of the corresponding actual variable when the function or subroutine is invoked. It then automatically restores the saved value on exit from the function or subroutine.
    

In the following passing by value example, the KILL in Subrt1 deletes the formal variable x but does not affect the actual variable a:

Test
    SET a\=17
    WRITE !,"Before Subrt1 a: ",$DATA(a)
    DO Subrt1(a)
    WRITE !,"After Subrt1 a: ",$DATA(a)
    QUIT
Subrt1(x)
    WRITE !,"pre-kill x: ",$DATA(x)
    KILL x
    WRITE !,"post-kill x: ",$DATA(x)
    QUIT

 

If you are [passing a variable by reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_usercode#GCOS_usercode_args_byref):

*   Performing KILL and including the variable in the formal list also kills the corresponding actual variable. When the function or subroutine terminates, the actual variable will no longer exist.
    
*   Performing a KILL and excluding the variable in the formal list causes both the formal variable and the actual variable passed by reference to be preserved.
    

In the following passing by reference example, the KILL in Subrt1 deletes both the formal variable x and the actual variable a:

Test
    SET a\=17
    WRITE !,"Before Subrt1 a: ",$DATA(a)
    DO Subrt1(.a)
    WRITE !,"After Subrt1 a: ",$DATA(a)
    QUIT
Subrt1(&x)
    WRITE !,"pre-kill x: ",$DATA(x)
    KILL x
    WRITE !,"post-kill x: ",$DATA(x)
    QUIT

 

As a general rule, you should not KILL variables specified in a formal parameter list. When Caché encounters a function or subroutine that uses parameter passing (whether by value or by reference), it implicitly executes a NEW command for each variable in the formal list. When it exits from the function or subroutine, it implicitly executes a KILL command for each variable in the formal list. In the case of a formal variable that uses passing by reference, it updates the corresponding actual variable (to reflect changes made to the formal variable) before executing the KILL.

Transaction Processing

A KILL of a global variable is journaled as part of the current transaction; this global variable deletion is rolled back during transaction rollback. A KILL of a local variable or a process-private global variable is not journaled, and thus this variable deletion is unaffected by a transaction rollback.

See Also

*   [ZKILL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czkill) command
    
*   [$DATA](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fdata) function
    
*   [$STORAGE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vstorage) special variable
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cjob "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_clock "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_ckill.xml**