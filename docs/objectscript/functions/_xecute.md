# $XECUTE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fxecute

---

 

Caché ObjectScript Reference

$XECUTE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwreverse "Go back to the previous section.") 

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$XECUTE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fxecute "$XECUTE") \]

Go to: Description Parameters Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Executes a specified command line.

Synopsis

$XECUTE(code,paramlist)

Parameters

code

An expression that resolves to a valid Caché ObjectScript command line, specified as a quoted string. A command line can contain one or more Caché ObjectScript commands. The final command must be an argumented QUIT.

paramlist

Optional — A list of parameters to be passed to code. Multiple parameters are separated by commas.

Description

The $XECUTE function allows you to execute user-written code as a function, supplying passed parameters and returning a value. The code parameter must evaluate to a quoted string containing one or more Caché ObjectScript commands. The code execution must conclude with a QUIT command that returns an argument. Caché then returns this QUIT argument as the $XECUTE return code value.

You can use the paramlist argument to pass parameters to code. If you are passing parameters, there must be a formal parameter list at the beginning of code. Parameters are specified positionally. There must be at least as many formal parameters listed in code as there are actual parameters specified in paramlist.

You can use the [CheckSyntax()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25Library.Routine#CheckSyntax) method of the [%Library.Routine](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Library.Routine) class to perform syntax checking on code.

Each invocation of $XECUTE places a new context frame on the call stack for your process. The $STACK special variable contains the current number of context frames on the call stack.

The $XECUTE function performs substantially the same operation as the XECUTE command, with the following differences: The $XECUTE function does not support postconditionals or the use of multiple command line arguments. The $XECUTE function requires every execution path to end with an argumented QUIT; the XECUTE command neither requires a QUIT nor permits an argumented QUIT.

Parameters

code

An expression that evaluates to a valid Caché ObjectScript command line, specified as a quoted string. The code string must not contain a tab character at the beginning or a <Return> at the end. The string can be no longer than a valid Caché ObjectScript program line. The code string must contain a QUIT command that returns an argument at the conclusion of each possible execution path.

If $XECUTE passes parameters to code, the code string must begin with a formal parameter list. A formal parameter list is enclosed in parentheses; within the parentheses, parameters are separated by commas.

paramlist

A list of parameters to pass to code, specified as a comma-separated list. Each parameter in paramlist must correspond to a formal parameter within the code string. The number of parameters in paramlist may be less than or equal to the number of formal parameters listed in code.

You can use a dot prefix to pass a parameter by reference. This is useful for passing a value out from code. An example is provided below. For further details, refer to [Passing by Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_usercode#GCOS_usercode_args_byref) in the “[User-defined Code](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_usercode)” chapter of Using Caché ObjectScript.

Examples

In the following example, the $XECUTE function executes the command line specified in cmdline. It passes two parameters, num1 and num2 to this command line.

   SET cmd\="(dvnd,dvsr) IF dvsr=0 {QUIT 99} ELSE {SET ^testnum=dvnd/dvsr QUIT 0}"
   SET rtn\=$XECUTE(cmd,num1,num2)
   IF rtn\=99
     {WRITE !,"Division by zero. ^testnum not set"}
   ELSE
     {WRITE !,"global ^testnum set to",^testnum}

The following example uses passing by reference (.y) to pass a local variable value from the code to the invoking context.

CubeIt
  SET x\=7
  SET rtn\=$XECUTE("(in,out) SET out=in\*in\*in QUIT 0",x,.y)
  IF rtn\=0 {WRITE !,x," cubed is ",y}
  ELSE {WRITE !,"Error code=",SQLCODE}

 

The following example shows how $XECUTE increments the $STACK special variable. This example either writes the $STACK value from within $XECUTE, or has $XECUTE invoke the XECUTE command, which writes the $STACK value:

StackIt
  SET stackit\=$RANDOM(3)
  IF stackit\=0 {GOTO StackIt}
  WRITE "initial stack level ",$STACK,!
  SET cmd\="(stackit) IF stackit=1 {WRITE ""stack is "",$STACK,!  QUIT 1} "\_
               "ELSEIF stackit=2 {WRITE ""stack is "",$STACK  XECUTE ""WRITE """" stack is """",$STACK,!""  QUIT 1} "\_
               "ELSE { QUIT 0}"
  SET rtn\=$XECUTE(cmd,stackit)
  IF rtn\=1 { WRITE "return stack level ",$STACK }
  ELSE {WRITE "unexpected value: rtn=",rtn}

 

See Also

*   [DO](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cdo) command
    
*   [XECUTE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cxecute) command
    
*   [QUIT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cquit) command
    
*   [$STACK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vstack) special variable
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fwreverse "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fxecute.xml**