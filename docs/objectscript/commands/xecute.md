# XECUTE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_cxecute

---

 

Caché ObjectScript Reference

XECUTE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cwrite "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czkill "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_COMMANDS "Caché ObjectScript Commands") \]  >  \[ [XECUTE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cxecute "XECUTE") \]

Go to: Description Arguments Examples Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Executes the specified commands.

Synopsis

XECUTE:pc xecutearg,... 
X:pc xecutearg,...

where xecutearg can be either of the following

"cmdline":pc
("(fparams) cmdline",params):pc

Arguments

pc

Optional — A postconditional expression.

cmdline

An expression that resolves to a command line consisting of one or more valid Caché ObjectScript commands. Note that the cmdline or (fparams) cmdline must be specified as a quoted string.

fparams

Optional — A formal parameters list, specified as a comma-separated list enclosed in parentheses. Formal parameters are variables use by cmdline, the values of which are supplied by passing params. Note that the fparams are the first item within the quoted code string.

params

Optional — A parameters list, specified as a comma-separated list. These are the parameters passed to fparams. If params are specified, an equal or greater number of fparams must be specified.

Description

XECUTE executes one or more Caché ObjectScript command lines, each command line specified by an xecutearg. You can specify multiple xecuteargs, separated by commas. These xecutearg are executed in left-to-right sequence, the execution of each being governed by an optional postconditional expression. There are two syntactical forms of xecutearg:

*   Without parameter passing. This form uses no parentheses.
    
*   With parameter passing. This form requires enclosing parentheses.
    

An XECUTE can contain any combination of these two forms of xecutearg. You can use the [CheckSyntax()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25Library.Routine#CheckSyntax) method of the [%Library.Routine](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Library.Routine) class to perform syntax checking on an xecutearg command line string.

In effect, each xecutearg is like a one-line subroutine called by a DO command and terminated when the end of the argument is reached or a QUIT command is encountered. After Caché executes the argument, it returns control to the point immediately after the xecutearg.

Each invocation of XECUTE places a new context frame on the call stack for your process. The $STACK special variable contains the current number of context frames on the call stack.

The XECUTE command performs substantially the same operation as the $XECUTE function, with the following differences: The command can use postconditionals, the function cannot. The command can specify multiple xecuteargs, the function can specify only one xecutearg. The command does not require a QUIT to complete execution; the function requires an argumented QUIT for every execution path.

Arguments

pc

An optional postconditional expression. If a postconditional expression is appended to the command keyword, Caché only executes the XECUTE command if the postconditional expression is true (evaluates to a nonzero numeric value). Caché does not execute the XECUTE command if the postconditional expression is false (evaluates to zero).

If a postconditional expression is appended to an xecutearg, Caché evaluates the argument only if the postconditional expression is true (evaluates to a nonzero numeric value). If the postconditional expression is false, Caché skips that xecutearg and evaluates the next xecutearg (if one exists). For further details, refer to [Command Postconditional Expressions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_commands#GCOS_commands_pc) in Using Caché ObjectScript.

cmdline

Each cmdline must evaluate to a string containing one or more Caché ObjectScript commands. Note that in some cases two spaces must be inserted between a command and the command following it. The cmdline string must not contain a tab character at the beginning or a <Return> at the end. To specify quotation marks within the cmdline string, double the quotation marks. The following example shows a single cmdline containing two commands:

   XECUTE "WRITE ""hello "",!  WRITE ""world"",!"

 

Because a cmdline is a string, it cannot be simply broken across multiple code lines. You can divide a single cmdline argument into separate strings joined with the [concatenate operator](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_operators#GCOS_operators_string):

   XECUTE "WRITE ""hello "",!"\_
          " WRITE ""world"",!"

 

You can divide a single cmdline argument into multiple separate comma-separated cmdline arguments:

   XECUTE "WRITE ""hello "",!",
          "WRITE ""world"",!"

 

The maximum length for cmdline depends on the following considerations: Caché stores both the source cmdline string and its generated object code as a single string. This resulting string must not exceed the Caché maximum string length. For further details on maximum string length, refer to [Long Strings](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_strings_long) in Using Caché ObjectScript.

You can embed /\* text \*/ comments within a cmdline, between concatenated cmdline strings, or between comma-separated cmdline arguments:

   XECUTE "SET x=""hello "" /\* 1st val \*/ SET y=""world"" /\* 2nd val \*/ "\_
          " WRITE x,! /\* part of 1st cmdline \*/ ",
          "WRITE y,! /\* 2nd cmdline \*/ "

 

A cmdline can evaluate to a null string (""). In this case, Caché performs no action and continues execution with the next xecutearg (if one exists).

If you are passing parameters, the fparams formal parameter list must precede the cmdline commands, with both elements enclosed in the same quotation marks. While it is recommended that you separate fparams from the cmdline by one or more spaces, no space is required.

  SET x\=1
  XECUTE ("(in,out) SET out=in+3", x, .y)
  WRITE y
  QUIT

 

By default, all local variables used in cmdline are public variables. You can designate variables within the command line as private variables by enclosing the command setting them within curly braces. For example:

  SET x\=1
  XECUTE ("(in,out) { SET out=in+3 }", x, .y)
  WRITE y
  QUIT

 

You can override this designation of private variables for specific variables by specifying a public variable list, enclosed in square brackets, immediately after the fparams formal parameter list. The following example specifies a public variable list containing the variable x:

  SET x\=1
  XECUTE ("(in,out) \[x\] { SET out=in+3 }", x, .y)
  WRITE y
  QUIT

 

fparams

A list of formal parameters, separated by commas and enclosed by parentheses. Formal parameter names must be valid identifiers. Because these formal parameters are executed in another context, they must only be unique within their xecutearg; they have no effect on local variables with the same name in the program that issued the XECUTE, or in another xecutearg. You do not have to use any or all of the fparams in cmdline. However, the number of fparams must equal or exceed the number of params specified, or a <PARAMETER> error is generated.

params

The actual parameters to be passed from the invoking program to fparams, specified as a comma-separated list. The params must be defined variables within the calling program.

You can use a dot prefix to pass a parameter by reference. This is useful for passing a value out from a cmdline. An example is provided below. For further details, refer to [Passing by Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_usercode#GCOS_usercode_args_byref) in the “[User-defined Code](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_usercode)” chapter of Using Caché ObjectScript.

Examples

The following example passes a parameter to a command line that sets a global. Two command lines are provided. Execution of each depends upon their postconditional setting.

  SET bad\=0,good\=1
  SET val\="paid in full"
  XECUTE ("(pay) SET ^acct1=pay",val):bad,("(pay) SET ^acct2=pay",val):good

Here the first xecutearg is skipped because of the value of the bad postconditional. The second xecutearg is executed with val being passed in as a parameter, supplying a value to the pay formal parameter used in the command line.

The following example uses passing by reference (.y) to pass a local variable value from the cmdline to the invoking context.

CubeIt
  SET x\=5
  XECUTE ("(in,out) SET out=in\*in\*in",x,.y)
  WRITE !,x," cubed is ",y

 

In the following example, the XECUTE command references the local variables x and y. x and y each contain a string literal consisting of three separate Caché ObjectScript commands that XECUTE invokes.

   SET x\="SET id=ans QUIT:ans="""" DO Idcheck"
   SET y\="SET acct=num QUIT:acct="""" DO Actcheck"
   XECUTE x,y

The following example uses XECUTE with a $SELECT construction.

   XECUTE "SET A=$SELECT(A>100:B,1:D)"

The following example executes the subroutine that is the value of A.

   SET A\="WRITE ! FOR I=1:1:5 { WRITE ?I\*5,I+1 }"
   XECUTE A

 

Notes

XECUTE and Objects

You can use XECUTE to call object methods and properties and execute the returned value, as shown in the following examples:

   XECUTE patient.Name
   XECUTE "WRITE patient.Name"

XECUTE and FOR

If an XECUTE argument contains a FOR command, the scope of the FOR is the remainder of the argument. When the outermost FOR in an XECUTE argument is terminated, the XECUTE argument is also terminated.

XECUTE and DO

If an XECUTE command contains a DO command, Caché executes the routine or routines specified in the DO argument or arguments. When it encounters a QUIT, it returns control to the point immediately following the DO argument.

For example, in the following commands, Caché executes the routine ROUT and returns to the point immediately following the DO argument to write the string “DONE”.

   XECUTE "DO ^ROUT WRITE !,""DONE"""

XECUTE and GOTO

If an XECUTE argument contains a GOTO command, Caché transfers control to the point specified in the GOTO argument. When it encounters a QUIT, it does not return to the point immediately following the GOTO argument that caused the transfer. Instead, Caché returns control to the point immediately following the XECUTE argument that contained the GOTO.

In the following example, Caché transfers control to the routine ROUT and returns control to the point immediately following the XECUTE argument to write the string “FINISH”. It never writes the string “DONE”.

   XECUTE "GOTO ^ROUT WRITE !,""DONE""" WRITE !,"FINISH"

XECUTE and QUIT

There is an implied QUIT at the end of each XECUTE argument.

XECUTE with $TEXT

If you include a $TEXT function within a cmdline, it designates lines of code in the routine that contains the XECUTE. For example, in the following program, the $TEXT function retrieves and executes a line.

A
   SET H\="WRITE !!,$PIECE($TEXT(HELP+1),"","",3)"
   XECUTE H
   QUIT
HELP
   ;; ENTER A NUMBER FROM 1 TO 5

Running routine A extracts and writes “ENTER A NUMBER FROM 1 TO 5”.

Nested Invocation of XECUTE

Caché ObjectScript supports the use of XECUTE within an XECUTE argument. However, you should use nested invocation of XECUTE with caution because it can be difficult to determine the exact flow of processing at execution time.

Execution Time for Commands Called by XECUTE

The execution time for code called within XECUTE can be slower than the execution time for the same code encountered in the body of a routine. This is because Caché compiles source code that is specified with the XECUTE command or that is contained in a referenced global variable each time it processes the XECUTE.

Implementing Generalized Operations

A typical use for XECUTE is to implement generalized operations within an application. For example, assume that you want to implement an inline mathematical calculator that would allow the user to perform mathematical operations on any two numbers and/or variables. To make the calculator available from any point in the application, you might use a specific function key (say, F1) to trigger the calculator subroutine.

A simplified version of the code to implement such a calculator might appear as follows.

Start  SET ops\=$CHAR(27,21)
  READ !,"Total amount (or F1 for Calculator): ",amt
  IF $ZB\=ops { DO Calc 
    ; . . .
  }
Calc  READ !,"Calculator"
  READ !,"Math operation on two numbers and/or variables."
  READ !,"First number or variable name: ",inp1
  READ !,"Mathematical operator (+,-,\*,/): ",op
  READ !,"Second number or variable name: ",inp2
  SET doit\="SET ans="\_inp1\_op\_inp2
  XECUTE doit
  WRITE !,"Answer (ans) is: ",ans
  READ !,"Repeat? (Y or N) ",inp
  IF (inp\="Y")!(inp\="y") { GOTO Calc+2 }
  QUIT

When executed, the Calc routine accepts the user inputs for the numbers and/or variables and the desired operation and stores them as a string literal defining the appropriate SET command in variable doit. The XECUTE command references doit and executes the command string that it contains. This code sequence can be called from any number of points in the application, with the user supplying different inputs each time. The XECUTE performs the SET command each time, using the supplied inputs.

XECUTE and ZINSERT

You use the XECUTE command to define and insert a single line of executable code from within a routine. You can use the ZINSERT command from the programmer prompt to define and insert by line position a single line of executable code into the current routine. You can use the ZREMOVE command from the programmer prompt to delete by line position one or more lines of executable code from the current routine.

An XECUTE command cannot be used to define a new label. Therefore, XECUTE does not require an initial blank space before the first command in its code line. ZINSERT can be used to define a new label. Therefore, ZINSERT does require an initial blank space (or the name of a new label) before the first command in its command line.

See Also

*   [DO](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cdo) command
    
*   [GOTO](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cgoto) command
    
*   [QUIT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cquit) command
    
*   [ZINSERT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czinsert) command
    
*   [$TEXT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ftext) function
    
*   [$XECUTE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fxecute) function
    
*   [$STACK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vstack) special variable
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cwrite "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czkill "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_cxecute.xml**