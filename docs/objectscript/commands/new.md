# NEW

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_cnew

---

 

Caché ObjectScript Reference

NEW

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cmerge "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_copen "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_COMMANDS "Caché ObjectScript Commands") \]  >  \[ [NEW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cnew "NEW") \]

Go to: Description Arguments Examples Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Creates empty local variable environment.

Synopsis

NEW:pc newargument,...
N:pc newargument,...

where newargument can be:

variable,...
or
(variable,...)

Arguments

pc

Optional — A postconditional expression.

variable

Optional — Name of variable(s) to be added to the existing local variable environment. The effect of a NEW on existing local variables depends on whether variable is enclosed in parentheses (exclusive NEW) or is not enclosed in parentheses (inclusive NEW). A variable must be a valid [local variable name](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_variables#GCOS_variables_local), but does not have to be a defined variable; specifying an undefined variable neither issues an error nor defines the variable.

Description

The NEW command has two forms:

*   Without an argument
    
*   With an argument
    

NEW without an argument creates an empty local variable environment for a called subroutine or user-defined function. Existing local variable values are not available in this new local environment. They can be restored by returning to the previous local environment.

The action NEW with an argument performs depends on the argument form you use.

*   NEW variable (inclusive NEW) retains the existing local variable environment and adds the specified variable(s) to it. If any of the specified local variables has the same name as an existing local variable, the old value for that named variable is no longer accessible in the current environment.
    
*   NEW (variable) (exclusive NEW) replaces all existing variables in the local variable environment except the specified variable(s).
    

NEW Restrictions

The NEW command (inclusive or exclusive) cannot be used on the following:

*   [Globals](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GGBL_structure)
    
*   [Process-Private Globals](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GGBL_structure)
    
*   [Local variable subscripts](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_mdarrays)
    
*   [Private variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_usercode#GCOS_ucode_vars)
    
*   [Special variables, except $ESTACK, $ETRAP, $NAMESPACE, and $ROLES](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cnew#RCOS_cnew66)
    

Attempting to use NEW in any of these contexts results in a <SYNTAX> error.

Arguments

pc

An optional postconditional expression. Caché executes the NEW command if the postconditional expression is true (evaluates to a nonzero numeric value). Caché does not execute the command if the postconditional expression is false (evaluates to zero). For further details, refer to [Command Postconditional Expressions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_commands#GCOS_commands_pc) in Using Caché ObjectScript.

variable

Name of a single variable or a comma-separated list of variable names. You can specify only unsubscripted variable names, although you can NEW an entire array (that is, an array name without subscripts). You can specify undefined variable names or you can reuse the names of existing local variables. For an inclusive NEW, when you specify an existing local variable, Caché reinitializes that variable in the local environment, but saves its current value on the program stack and restores it after the subroutine or function terminates.

When a variable name or comma-separated list of variable names is enclosed in parentheses (exclusive NEW), Caché performs the opposite operation. That is, all local variables are reinitialized except the specified variable names, which retain their previous values. Caché saves the current values of all variables on the program stack and restores them after the subroutine or function terminates.

Examples

The following example illustrates an inclusive NEW, which keeps the existing local variables a, b, and c, and adds variables d and e, in this case, overlaying the prior value of d.

Main
  SET a\=7,b\=8,c\=9,d\=10
  WRITE !,"Before NEW:",!,"a=",a,!,"b=",b,!,"c=",c,!,"d=",d
  DO Sub1
  WRITE !,"Returned to prior context:"
  WRITE !,"a=",a,!,"b=",b,!,"c=",c,!,"d=",d
  QUIT
Sub1
  NEW d,e
  SET d\="big number"
  WRITE !,"After NEW:",!,"a=",a,!,"b=",b,!,"c=",c,!,"d=",d
  QUIT

 

The following example illustrates an exclusive NEW, which removes all existing local variables except the specified variables a and c.

  SET a\=7,b\=8,c\=9,d\=10
  NEW (a,c)
  WRITE

 

Notes

Where to Use NEW

NEW allows you to insulate the current process’s local variable environment from changes made by a subroutine, user-defined function, or XECUTE string. NEW is most frequently used within a subroutine called by the DO command.

The basic purpose of the NEW command is to redefine the local variable environment within a called subroutine or user-defined function. A subroutine or user-defined function called without parameter passing inherits its local variable environment from the calling routine. To redefine this environment for a subroutine or function, you can use NEW for all local variables (argumentless NEW), for named local variables (inclusive NEW) or for all local variables except the named variables (exclusive NEW).

Within a procedure, variables are either private or public:

*   By default, local variables are private to that procedure. The procedure block uses private variables that do not interact with variables with the same names outside the procedure block. You cannot perform a NEW on a private variable; attempting an inclusive or exclusive NEW on a private variable within a procedure block results in a <SYNTAX> error.
    
*   When declaring a procedure, you can explicitly list local variables as public variables. You can perform a NEW on a public variable within a procedure block. This NEW only affects the variable value within that procedure. You can repeatedly NEW a public variable within a procedure.
    

For further details, refer to “[Procedure Variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_usercode#GCOS_ucode_vars)” in Using Caché ObjectScript.

Special considerations apply in the case of a subroutine called by the DO command with parameter passing. These considerations are described under ["Subroutines with Parameter Passing"](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cnew#RCOS_cnew68).

NEW and KILL

Variables created by NEW do not require explicit and corresponding KILL commands. When a called subroutine or a user-defined function terminates, Caché executes an implicit KILL for each variable initialized by a NEW command within that subroutine or function.

Inclusive NEW

An inclusive NEW — NEW variable — retains the existing local variable environment and adds the specified variables to it. If an existing variable is named, the "new" variable replaces the existing variable, which is saved on the stack and then restored when the subroutine or function terminates.

Inclusive NEW does not restrict the number of variables you can specify as a comma-separated list. Inclusive NEW also does not limit the number of local variable environment levels (number of times NEW is issued).

In the following example, assume that the local variable environment of the calling routine (Main) consists of variables a, b, and c. When the DO calls Subr1, the NEW command redefines Subr1’s local variable environment to new variable c and add variable d. After the NEW, the subroutine’s environment consists of the existing variables a and b plus the new variables c and d. The variables a and b are inherited and retain their existing values. The new variables c and d are created undefined. Since c is the name of an existing local variable, Caché saves the existing value on the stack and restores it when Subr1 QUITs. Note that the first SET command in Subr1 references a and b to assign a value to d. Note that variable c in this context is undefined.

Main
    SET a\=2,b\=4,c\=6
    WRITE !,"c in Main before DO: ",c
    DO Subr1(a,b,c)
    WRITE !,"c in Main after DO: ",c
    QUIT
Subr1(a,b,c)
    NEW c,d
    IF $DATA(c) {WRITE !,"c=",c}
     ELSE {WRITE !,"c in Subr1 is undefined"}
    SET d\=a\*b
    SET c\=d\*2
    WRITE !,"c in Subr1: ",c
    QUIT

 

When executed, this code produces the following results:

c before DO: 6
c in Subr1 is undefined
c in Subr1: 16
c after DO: 6

The results are the same whether passing parameters by value, as in the previous example, or [passing parameters by reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_usercode#GCOS_usercode_args_byref):

Main
    SET a\=2,b\=4,c\=6
    WRITE !,"c in Main before DO: ",c
    DO Subr1(.a,.b,.c)
    WRITE !,"c in Main after DO: ",c
    QUIT
Subr1(&a,&b,&c)
    NEW c,d
    IF $DATA(c) {WRITE !,"c=",c}
     ELSE {WRITE !,"c in Subr1 is undefined"}
    SET d\=a\*b
    SET c\=d\*2
    WRITE !,"c in Subr1: ",c
    QUIT

 

Note that variable c is passed to Subr1 and then immediately redefined using NEW. In this case, passing variable c was unnecessary; the program results are identical whether or not c is passed. If you NEW any of the variables named in the subroutine’s formal parameter list, you render them undefined and make their passed values inaccessible.

Exclusive NEW

An exclusive NEW — NEW (var1,var2) — replaces the entire existing local variable environment except the specified variables. If an existing variable is named, it is retained and can be referenced in the new environment. However, any changes made to such a variable are reflected in the existing variable when the function or subroutine terminates.

An exclusive NEW can specify a maximum of 255 variables as a comma-separated list. Exceeding this number causes Caché to issue a <SYNTAX> error.

Exclusive NEW (NEW (x,y,z)) temporarily removes local variables from the current scope. This can affect local variables created by Caché objects. For example, Caché maintains %objcn which is the cursor pointer for Caché object queries. Removing this from the current scope can result in collisions with other internal structures. Therefore, do not use exclusive NEW in any context where it might affect system structures.

Attempting to issue more than 31 levels of exclusive NEW or argumentless NEW results in a <MAXSCOPE> error.

When using exclusive NEW in a FOR code block, you must specify the FOR count variable as an excluded variable. For further details, refer to the [FOR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cfor) command.

In the following example, assume that the local variable environment of the calling routine (Start) consists of variables a, b, and c. When the DO calls Subr1, the NEW command redefines Subr1’s local variable environment to exclude all variables except c and d.

After the NEW, the subroutine’s environment consists only of the new variables c and d. The new variable c is retained from the calling routine’s environment and keeps its existing value. The new variable d is created undefined.

The first SET command in Subr1 references c to assign a value to d. The second SET command assigns a new value (24) to c. When the subroutine QUITs, c will have this updated value (and not the original value of 6) in the calling routine’s environment.

Start    SET a\=2,b\=4,c\=6
    DO Subr1
    WRITE !,"c in Start: ",c
    QUIT
Subr1    NEW (c,d)
    SET d\=c+c
    SET c\=d\*2
    WRITE !,"c in Subr1: ",c
    QUIT

 

When executed, this code produces the following results:

c in Subr1: 24

c in Start: 24

Argumentless NEW

The argumentless NEW provides an empty local variable environment for a called subroutine or user-defined function. The existing local variable environment (in the calling routine) is saved and then restored when the subroutine or function terminates. Any variables created after the NEW are deleted when the subroutine or function terminates.

If a command follows the NEW on the same line, be sure to separate the NEW command from the command following it by (at least) two spaces.

Argumentless NEW should not be used within the body of a FOR loop or in a context in which it can affect Caché objects.

Attempting to issue more than 31 levels of exclusive NEW or argumentless NEW results in a <MAXSCOPE> error.

Special Variables: $ESTACK, $ETRAP, $NAMESPACE, and $ROLES

You cannot use NEW on most special variables; attempting to do so results in a <SYNTAX> error. There are four exceptions: $ESTACK, $ETRAP, $NAMESPACE, and $ROLES.

[$ETRAP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vetrap): When you issue the command NEW $ETRAP, Caché creates a new context for error trapping. You can then set $ETRAP in this new context with the desired error trapping command(s). The $ETRAP value in the previous context is preserved. If you set $ETRAP without first issuing the NEW $ETRAP command, Caché sets $ETRAP to this value in all contexts. It is therefore recommended that you always NEW the $ETRAP special variable before setting it.

[$NAMESPACE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vnamespace): When you issue the command NEW $NAMESPACE, Caché creates a new namespace context. Within this context you can change the namespace. When you exit this context (with QUIT, for example) Caché reverts to the prior namespace.

Subroutines with Parameter Passing

If you call a subroutine with parameter passing, Caché issues an implicit NEW command for each of the variables named in the subroutine’s formal parameter list. It then assigns the values passed in the DO command’s actual parameter list (by value or by reference) to these variables.

If the DO command uses parameter passing by value and if the formal list names any existing local variables, Caché places the existing variables and their values on the stack. When the subroutine terminates (with either an explicit or an implicit QUIT), Caché issues an implicit KILL command for each of the formal list variables to restore them from the stack.

See Also

*   [DO](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cdo) command
    
*   [QUIT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cquit) command
    
*   [SET](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cset) command
    
*   [$ETRAP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vetrap) special variable
    
*   [$NAMESPACE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vnamespace) special variable
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cmerge "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_copen "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_cnew.xml**