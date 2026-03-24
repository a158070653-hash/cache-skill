# DO

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_cdo

---

 

Caché ObjectScript Reference

DO

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ccontinue "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cdowhile "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_COMMANDS "Caché ObjectScript Commands") \]  >  \[ [DO](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cdo "DO") \]

Go to: Description Arguments The Argumentless DO Command The DO Command with Arguments See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Calls a routine.

Synopsis

DO:pc doargument,...
D:pc doargument,...

where doargument is:

entryref(param,...):pc

Arguments

pc

Optional — A postconditional expression.

entryref

The name of the routine to be called.

param

Optional — Parameter values to be passed to the called routine.

Description

The DO command and the DO WHILE command are separate and unrelated commands. This page documents the DO command. In the [DO WHILE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cdowhile) command, the DO keyword and the WHILE keyword may be separated by several lines of code; however, you can immediately identify a DO WHILE command because the DO keyword is followed by an open curly brace.

The DO command has two forms:

*   Without an Argument
    
    Note:
    
    [DO without an argument](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cdo#RCOS_cdo39) uses an older block structure syntax (using a period (.) prefix for each line) that has been superseded by the DO WHILE curly brace block structure. Therefore, DO without an argument is considered obsolete. It should only be used to maintain existing applications and should not be used in new code.
    
*   With an Argument
    

DO with an argument calls a specified object method, subroutine, function, or procedure. Caché executes the called routine, then executes the next command after the DO command. You can call the routine with or without parameter passing.

The DO command cannot accept a return value from the called routine. If the called routine concludes with an argumented QUIT, the DO command completes successfully, but ignores the QUIT argument value.

Each invocation of DO places a new context frame on the call stack for your process. The $STACK special variable contains the current number of context frames on the call stack. This context frame establishes a new execution level, incrementing $STACK and $ESTACK, and providing scope for NEW and SET $ZTRAP operations issued during the DO operation. Upon successful completion, DO decrements $STACK and $ESTACK and reverts NEW and SET $ZTRAP operations.

Arguments

pc

An optional postconditional expression. If the postconditional expression is appended to the DO command keyword, Caché executes the DO command if the postconditional expression is TRUE (evaluates to a nonzero numeric value). Caché does not execute the DO command if the postconditional expression is FALSE (evaluates to zero).

If the postconditional expression is appended to an argument, Caché executes the argument if the postconditional expression is TRUE (evaluates to a nonzero numeric value). If the postconditional expression is FALSE (evaluates to zero), Caché skips that argument and proceeds to evaluate the next argument (if there is one) or the next command. Note that because Caché processes expressions from left to right, any parts of the argument containing expressions (such as a parameter value or an object reference) is evaluated and can cause an error before the postconditional expression is evaluated. If DO invokes an object method with an appended postconditional, the maximum number of object method parameters is 253.

For further details, refer to [Command Postconditional Expressions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_commands#GCOS_commands_pc) in Using Caché ObjectScript.

entryref

The name of the routine (Object Method, [Subroutine](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_usercode), [Procedure](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_usercode), or Extrinsic [Function](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_usercode)) to be called. You can specify multiple routines as a comma-separated list.

entryref can take any of the following forms.

label+offset

Specifies a [line label](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_syntax#GCOS_syntax_labels) within the current routine. The optional +offset can only be used when calling a subroutine to which no parameters are passed; it cannot be used when calling a procedure or when passing parameters to a subroutine. offset is a nonnegative integer that specifies the number of lines after the label at which execution of the subroutine is to start.

label+offset^routine 

Specifies a [line label](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_syntax#GCOS_syntax_labels) within the named routine that resides on disk. Caché loads the routine from disk and begins execution at the indicated label. The +offset is optional.

^routine

The name of a routine that resides on disk. The system loads the routine from disk and begins execution at the first executable line of the routine. Must be a literal value; a variable cannot be used to specify routine. (Note that the ^ character is a separator character, not part of the routine name.) If the routine has been modified, Caché loads the updated version of the routine when DO invokes the routine. If the routine is not in the current namespace, you can specify the namespace that contains the routine using an [extended routine reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_syntax#GCOS_syntax_namespaces_extendedrefs), as follows: ^|"namespace"|routine.

oref.Method()

Specifies an object method. The system accesses the object and executes the specified method, passing the arguments (if any) specified in param, the method’s argument list. Object calls use dot syntax: oref (the object reference) and Method() are separated by a dot; blank spaces are not permitted. A method must specify its open and close parentheses, even if there are no param arguments.

The following syntactic forms are supported: DO oref.Method(), DO (oref).Method(), DO ..Method(), DO ##class(cname).Method().

Also see “Using the DO Command with Dynamic Object Expressions” and “Using the DO Command with Dynamic Array Expressions” in Using JSON in Caché.

You cannot specify an offset when calling a CACHESYS % routine. If you attempt to do so, Caché issues a <NOLINE> error.

If you specify a nonexistent label, Caché issues a <NOLINE> error. If you specify a nonexistent routine, Caché issues a <NOROUTINE> error. If you specify a nonexistent method, Caché issues a <METHOD DOES NOT EXIST> error. If you use extended reference (for example, DO ^|"%SYS"|MyProg) and specify a nonexistent namespace, Caché issues a <NAMESPACE> error. If you use extended reference and specify a namespace for which you do not have privileges, Caché issues a <PROTECT> error, followed by the database path, such as the following: <PROTECT> \*^|^^c:\\intersystems\\cache\\mgr\\|MyRoutine. For further details on these errors, refer to the [$ZERROR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzerror) special variable.

If you specify an offset that points to the middle of a multi-line statement, Caché starts execution at the beginning of the next statement.

param

Parameter values to be passed to the subroutine, procedure, extrinsic function or object method. You can specify a single param value, or a comma-separated list of param values. A param list is enclosed in parentheses. When no param is specified, the enclosing parentheses are required when calling a procedure or extrinsic function, optional when calling a subroutine. Parameters can be passed by value or passed by reference. The same call can mix parameters passed by value and parameters passed by reference. When passing by value, you can specify a parameter as a value constant, expression, or unsubscripted local variable name. (See [Passing By Value](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_usercode#GCOS_usercode_args_byval).) When passing by reference, the parameters must reference the name of a local variable or unsubscripted array in the form .name (See [Passing By Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_usercode#GCOS_usercode_args_byref).)

The maximum total param values for a DO entrypoint is 382; the maximum total param values for a DO method or DO with indirection is 380. This total can include up to 254 [actual parameters](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_usercode#GCOS_ucode_parampassing) and 128 [postconditional parameters](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_commands#GCOS_commands_pc).

you can specify a [variable number of parameters](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_usercode#GCOS_usercode_args_variable) using ... syntax.

The Argumentless DO Command

The argumentless DO command executes the block of code that immediately follows it in the same program. Each line of this block of code is indicated by a period (.) prefix. Caché then executes the next command after that block of code. Argumentless DO blocks can be nested. A postconditional expression can be appended to the argumentless DO command keyword to specify whether or not the block of code that immediately follows the DO command should be executed or skipped.

The block structures provided by the IF, FOR, DO WHILE, and WHILE commands are a preferable means to perform the same operations. The argumentless DO command continues to be supported, but its use in new coding is discouraged. Note that DO establishes a new execution level; DO WHILE and the other block structure commands do not change execution level. For further details, see [Argumentless DO (legacy version)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cdo_legacy).

The DO Command with Arguments

The DO command with entryref arguments invokes the execution of one or more blocks of code that are defined elsewhere. Each block of code to execute is specified by its entryref. The DO command can specify multiple blocks of code to execute as a comma-separated list. The execution of the DO command, and the execution of each entryref in a comma-separated list can be governed by optional postconditional expressions.

DO can invoke the execution of a subroutine (with or without parameter passing), a procedure, or an extrinsic function. Upon completion of the execution of the block of code, execution resumes at the next command after the DO command. A block of code invoked by the DO command cannot return a value to the DO command; any value returned is ignored. Thus DO can execute an extrinsic function, but cannot receive the return value of that function.

Note:

DO cannot invoke most Caché [intrinsic (system-supplied) functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_intro#GCOS_intro_functions). Attempting to do so results in a <SYNTAX> error. A few intrinsic functions can be invoked by DO. These include [$CASE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fcase), [$CLASSMETHOD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fclassmethod), [$METHOD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fmethod), and the deprecated [$ZUTIL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_replacements) functions. DO cannot receive the return value of the function.

You can specify a $CASE function as a DO command argument. For an example program, refer to the [$CASE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fcase) function.

The DO Command without Parameter Passing

The DO command without parameter passing is only used with subroutines. Use of DO entryref without parameter passing (that is, without specifying the param option) takes advantage of the fact that a calling routine and its called subroutine share the same variable environment. Any variable updates made by the subroutine are automatically available to the code following the DO command.

When using DO without parameter passing, you must make sure that both the calling routine and the called subroutine reference the same variables.

Note:

Procedures handle variables entirely differently. Refer to [Procedures](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_usercode) in Using Caché ObjectScript.

In the following example, Start (the calling routine) and Exponent (the called subroutine) share access to three variables: num, powr, and result. Start sets num and powr to the user-supplied values. These values are automatically available to Exponent when it is called by the DO command. Exponent references num and powr, and places the calculated value in result. When Exponent executes the RETURN command, control returns to the WRITE command immediately after the DO. The WRITE command outputs the calculated value by referencing result:

Start  ; Raise an integer to a specified power.
  READ !,"Integer= ",num QUIT:num\="" 
  READ !,"Power= ",powr QUIT:powr\=""
  DO Exponent()
  WRITE !,"Result= ",result,!
  RETURN
Exponent()
  SET result\=num
  FOR i\=1:1:powr\-1 { SET result\=result\*num }
  RETURN

In the following example, DO invokes the Admit() method on the object referred to by pat. The method does not receive parameters or return a value.

   DO pat.Admit()

In the following example, DO calls, in succession, the subroutines Init and Read1 in the current routine and the subroutine Convert in routine Test.

   DO Init,Read1,Convert^Test

In the following example, DO uses an extended reference to call the routine fibonacci in a different namespace (the SAMPLES namespace):

   ZNSPACE "USER"
   DO ^|"SAMPLES"|fibonacci

DO and GOTO

The DO command can be used to invoke a subroutine (with or without parameter passing), a procedure, or an extrinsic function. At the completion of the call, Caché executes the next command following the DO command.

The GOTO command can only be used to invoke a subroutine without parameter passing. At the completion of the call, Caché issues a QUIT, ending execution.

DO with Parameter Passing

When used with parameter passing, DO entryref explicitly passes one or more values to the called subroutine, procedure, extrinsic function or object method. The passed values are specified as a comma-separated list with the param option. With parameter passing, you must make sure that the called subroutine is defined with a parameter list. The subroutine definition takes the form:

\>label( param)

where label is the [label name](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_syntax#GCOS_syntax_labels) of the subroutine, procedure, extrinsic function or object method, and param is a comma separated list of one or more unsubscripted local variable names. For example,

Main
  SET x\=1,y\=2,z\=3
  WRITE !,"In Main ",x,y,z
  DO Sub1(x,y,z)
  WRITE !,"Back in Main ",x,y,z
  QUIT
Sub1(a,b,c)
  WRITE !,"In Sub1 ",a,b,c
  QUIT

 

The list of parameters passed by the DO command is known as the actual parameter list. The list of parameter variables defined as part of the label of the coded routine is known as the formal parameter list. When DO calls the routine, the parameters in the actual parameter list are mapped, by position, to the corresponding variables in the formal parameter list. In the above example, the value of the first actual parameter (x) is placed in the first variable (a) of the subroutine’s formal parameter list; the value of the second actual parameter (y) is placed in the second variable (b); and so on. The subroutine can then access the passed values by using the appropriate variables in its formal parameter list.

If there are more variables in the actual parameter list than there are parameters in the formal parameter list, Caché issues a <PARAMETER> error.

If there are more variables in the formal parameter list than there are parameters in the actual parameter list, the extra variables are left undefined. In the following example, the formal parameter c is left undefined:

Main
  SET x\=1,y\=2,z\=3
  WRITE !,"In Main ",x,y,z
  DO Sub1(x,y)
  WRITE !,"Back in Main ",x,y,z
  QUIT
Sub1(a,b,c)
  WRITE !,"In Sub1 "
  IF $DATA(a) {WRITE !,"a=",a}
     ELSE {WRITE !,"a is undefined"}
  IF $DATA(b) {WRITE !,"b=",b}
     ELSE {WRITE !,"b is undefined"}
  IF $DATA(c) {WRITE !,"c=",c}
     ELSE {WRITE !,"c is undefined"}
  QUIT

 

You can specify a default value for a formal parameter, to be used when no actual parameter value is specified.

You can leave any variable undefined by omitting the corresponding parameter from the DO command’s actual parameter list. However, you must include a comma as a place holder for each omitted actual parameter. In the following example, the formal parameter b is left undefined:

Main
  SET x\=1,y\=2,z\=3
  WRITE !,"In Main ",x,y,z
  DO Sub1(x,,z)
  WRITE !,"Back in Main ",x,y,z
  QUIT
Sub1(a,b,c)
  WRITE !,"In Sub1 "
  IF $DATA(a) {WRITE !,"a=",a}
     ELSE {WRITE !,"a is undefined"}
  IF $DATA(b) {WRITE !,"b=",b}
     ELSE {WRITE !,"b is undefined"}
  IF $DATA(c) {WRITE !,"c=",c}
     ELSE {WRITE !,"c is undefined"}
  QUIT

 

You can specify a variable number of parameters using ... syntax:

Main
  SET x\=3,x(1)\=10,x(2)\=20,x(3)\=30
  DO Sub1(x...)
  QUIT
Sub1(a,b,c)
  WRITE a," ",b," ",c
  QUIT 

 

The DO command can pass parameters either by value (for example, DO Sub1(x,y,z)) or by reference (for example, DO Sub1(.x,.y,.z)). You can mix passing by value and passing by reference within the same DO command. For further details, refer to [Parameter Passing](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_usercode#GCOS_ucode_parampassing) in Using Caché ObjectScript.

The following examples show the difference between passing by value and passing by reference:

Main /\* Passing by Value \*/
  SET x\=1,y\=2,z\=3
  WRITE !,"In Main ",x,y,z
  DO Sub1(x,y,z)
  WRITE !,"Back in Main ",x,y,z
  QUIT
Sub1(a,b,c)
  SET a\=a+1,b\=b+1,c\=c+1
  WRITE !,"In Sub1 ",a,b,c
  QUIT

 

Main /\* Passing by Reference \*/
  SET x\=1,y\=2,z\=3
  WRITE !,"In Main ",x,y,z
  DO Sub1(.x,.y,.z)
  WRITE !,"Back in Main ",x,y,z
  QUIT
Sub1(&a,&b,&c)   /\* The & prefix is an optional by-reference marker \*/
  SET a\=a+1,b\=b+1,c\=c+1
  WRITE !,"In Sub1 ",a,b,c
  QUIT

 

DO with Indirection

You can use indirection to supply a target subroutine location for DO. For example, you might implement a generalized menu program by storing the various menu functions at different locations in a separate routine. In your main program code, you could use name indirection to provide the DO command with the location of the function corresponding to each menu choice.

You cannot use indirection with Caché object dot syntax. This is because dot syntax is parsed at compile time, not at runtime.

In name indirection, the value of the expression to the right of the indirection operator (@) must be a name (that is, a [line label](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_syntax#GCOS_syntax_labels) or a routine name). In the following code segment, name indirection supplies the DO with the location of a target function in the routine Menu.

  READ !,"Enter the number for your choice: ",num QUIT:num\=""
  DO @("Item"\_num)^Menu

The DO command invokes the subroutine in Menu whose label is Item concatenated with the user-supplied num value (for example, Item1, Item2, and so on).

You can also use the argument form of indirection to substitute the value of an expression for a complete DO argument. For example, consider the following DO command:

   DO @(eref\_":fstr>0")

This command calls the subroutine specified by the value of eref if the value of fstr is greater than 0.

For more information, refer to [Indirection](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_operators#GCOS_operators_indirection) in Using Caché ObjectScript.

DO with Argument Postconditionals

You can use argument postconditional expressions to select a target subroutine for a DO command. If the postconditional expression evaluates to FALSE (0), Caché ignores the associated subroutine call. If the postconditional expression evaluates to TRUE (1), Caché executes the associated subroutine call, then returns to the DO command. You can use postconditionals on both the DO command and on its arguments.

For example, consider the command:

   DO:F\>0 A:F\=1,B:F\=2,C

The DO command has a postconditional expression; if F is not greater than 0, no part of the DO is executed. The DO command’s arguments also have postconditional expressions. DO uses these argument postconditionals to select which subroutine(s) (A, B, or C) to execute. All subroutines that fulfill the truth condition are executed, in the order presented. Thus, in the above example, C, with no postconditional, is always executed: if F=1 both A and C are executed; if F=2, B and C are executed; if F=3 (or any other number) C is executed. To establish C as a true default, do the following:

   DO:F\>0 A:F\=1,B:F\=2,C:((F'=1)&&(F'=2))

In this example, one and only one subroutine is executed.

In the following example, the DO command takes a postconditional, and each of its arguments also takes a postconditional. In this case, the first argument is not executed, because its postconditional is 0. The second argument is executed because its postconditional is 1.

Main
  SET x\=1,y\=2,z\=3
  WRITE !,"In Main ",x,y,z
  DO:1 Sub1(x,y,z):0,Sub2(x,y,z):1
  WRITE !,"Back in Main ",x,y,z
  QUIT
Sub1(a,b,c)
  WRITE !,"In Sub1 ",a,b,c
  QUIT
Sub2(d,e,f)
  WRITE !,"In Sub2 ",d,e,f
  QUIT

 

Most Object (oref) methods invoked by DO can take an argument postconditional. However, $SYSTEM object methods cannot take an argument postconditional. Attempted to do so generates a <SYNTAX> error.

Note that because Caché evaluates expressions in strict left-to-right order, an argument that contains expressions is evaluated (and can generate an error) before Caché evaluates the argument postconditional.

When using argument postconditionals, make sure there are no unwanted side effects. For example, consider the following command:

   DO @^Control(i):z\=1

In this case, ^Control(i) contains the name of the subroutine to be called if the postconditional z=1 tests TRUE. Whether or not z=1, Caché evaluates the value of ^Control(i) and resets the current global naked indicator accordingly. If z=1 is FALSE, Caché does not execute the DO. However, it does reset the global naked indicator just as if it had executed the DO. For more details on the naked indicator, see [Naked Global Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GGBL_using) in Using Caché Globals.

For more information on how postconditional expressions are evaluated, see [Command Postconditional Expressions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_commands#GCOS_commands_pc) in Using Caché ObjectScript.

$TEST Behavior with DO

Argumentless DO always preserves the value of the $TEST special variable.

When you use DO to call a procedure, Caché preserves the value of $TEST by restoring it to its state at the time of the call upon quitting the procedure. However, when you use DO to call a subroutine (either with or without parameter passing), Caché does not preserve the value of $TEST across the call.

To save the $TEST value across a DO call, you can explicitly assign it to a variable before the call. You can then reference the variable in code that follows the call.

The following code illustrates some unexpected $TEST behavior that can result when using DO with the legacy IF command (which sets $TEST). This behavior does not occur with the standard (code block) IF command, because the standard IF does not set $TEST.

Start ; This routine uses the legacy IF command syntax
  SET x\=1
  IF x\=1 DO Sub1(x) ; sets $TEST to TRUE (1)
  ELSE  DO Sub2(x)
  QUIT
Sub1(y)  ; a subroutine that evaluates a FALSE IF
  WRITE !,"hello from subroutine 1"
  IF y\=2 WRITE " - IF in Sub1 was TRUE" ; Set $TEST to FALSE (0)
  ELSE  WRITE " - IF in Sub1 was FALSE"
  QUIT
Sub2(z)  ; another subroutine
  WRITE !,"hello from subroutine 2"
  QUIT

At first glance, you might expect that Start will call only Sub1 and then exit.

In fact, execution of this code produces the following:

USER>DO ^Start
hello from subroutine 1 - IF in Sub1 was FALSE
hello from subroutine 2

This unexpected behavior results from the fact the $TEST value is reset in Sub1, that causes Caché to execute the ELSE command in Start. The processing sequence is as follows:

1.  Caché evaluates the IF command expression in Start as TRUE. It sets $TEST to TRUE and calls Sub1.
    
2.  Caché evaluates the IF command expression in Sub1 as FALSE. It sets $TEST to FALSE and then executes the following ELSE command.
    
3.  Caché returns to Start when it encounters the QUIT.
    
4.  Caché executes the ELSE in Start and performs the DO call to Sub2. It executes the ELSE because $TEST was set to FALSE in Sub1, replacing the TRUE value set by the IF command in Start.
    
    To produce the expected behavior, you could replace the ELSE in either Start or Sub1 with an additional IF. For example, you might recast Start as follows:
    
    Start
      SET x\=1
      IF x\=1 DO Sub1(x)
      IF x'=1 DO Sub2(x)
      QUIT
    

See Also

*   [GOTO](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cgoto) command
    
*   [XECUTE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cxecute) command
    
*   [NEW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cnew) command
    
*   [QUIT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cquit) command
    
*   [Argumentless DO (legacy version)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cdo_legacy) command
    
*   [IF (legacy version)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cif_legacy) command
    
*   [$CASE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fcase) function
    
*   [$ESTACK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vestack) special variable
    
*   [$STACK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vstack) special variable
    
*   [Subroutines](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_usercode) in Using Caché ObjectScript
    
*   [Procedures](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_usercode) in Using Caché ObjectScript
    
*   [Parameter Passing](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_usercode) in Using Caché ObjectScript
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ccontinue "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cdowhile "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_cdo.xml**