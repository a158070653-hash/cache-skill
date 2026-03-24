# FOR

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_cfor

---

 

Caché ObjectScript Reference

FOR

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_celseif "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cgoto "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_COMMANDS "Caché ObjectScript Commands") \]  >  \[ [FOR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cfor "FOR") \]

Go to: Description Arguments Exiting a FOR Loop Examples Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Executes a block of code repeatedly, testing at the beginning of each loop.

Synopsis

FOR var=forparameter { code }
F var=forparameter { code }

FOR var=forparameter1,forparameter2,... { code }
F var=forparameter1,forparameter2,... { code }

where forparameter can be:

expr
start:increment
start:increment:end

Arguments

var

Optional — A local variable or instance variable initialized by the FOR command. Commonly, this a numeric counter that is incremented each time the code block is executed.

expr

Optional — The value assigned to var before executing the code block. Can be a single value or a comma-separated list of values.

start

Optional — The numeric value assigned to var before the first execution of the code block. Used with increment and (optionally) end to govern multiple iterations of the FOR loop.

increment

Optional — The numeric value used to increment (or decrement) var after each iteration of the FOR loop.

end

Optional — The numeric value used to terminate the FOR loop. Looping ends when var is incremented to a value equal to or greater than end.

code

A block of Caché ObjectScript commands enclosed in curly braces.

Description

FOR is a block-oriented command. Commonly, it consists of a counter and an executable block of code enclosed in curly braces. The number of times this block of code is executed is determined by the counter, which is tested at the top of each loop. Less commonly, a FOR command does not specify an incrementing counter. It can be argumentless (looping infinitely until exited), or can specify an expression as its argument (looping once).

The FOR command has two basic forms:

*   [Without an argument](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cfor#RCOS_cfor_noarg)
    
*   [With an argument](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cfor#RCOS_cfor_witharg)
    

FOR Without an Argument

FOR without an argument executes the loop code block infinitely until exited by a command within the code block. Caché repeats the commands within the curly braces until it encounters a QUIT, RETURN, or GOTO command that breaks out of the loop. The following example exits the loop when x\=3:

  SET x\=8
  FOR { WRITE "Running loop x=",x,!
        SET x\=x\-1
        QUIT:x\=3
      }
  WRITE "Next command after FOR code block"

 

An error, of course, also breaks out of a FOR loop, as shown in the following example. This FOR loop is exited by a divide-by-zero error, caught by the CATCH block:

  TRY {
  SET x\=8
  FOR { SET y\=4/x
        WRITE "Running loop 4/",x,"=",y,!
        SET x\=x\-1
      }
  WRITE "Next command after FOR code block"
  }
 CATCH exp {
      WRITE !,"this is the exception handler",!
      IF 1\=exp.%IsA("%Exception.SystemException") {
         WRITE "Name: ",$ZCVT(exp.Name,"O","HTML"),!
         WRITE "Location: ",exp.Location,!
         WRITE "Code: ",exp.Code
      }
      ELSE {WRITE "Unexpected exception type",! }
      RETURN
  }

 

FOR With an Argument

The action FOR performs depends on the argument form you use:

FOR var\=expr executes the code block once, setting var to the value of expr. FOR var\=expr1,expr2...,exprN executes the code block N times, setting var for each loop to each successive value of expr.

FOR var\=start:increment executes the code block infinitely, unless exited. On the first iteration, Caché sets var to the value of start. Each execution of the FOR command increments the var value by the specified increment value. Caché repeats until it encounters a QUIT, RETURN, or GOTO command in the code block.

FOR var\=start:increment:end sets var to the value of start. Caché then executes the code block based on the conditions described in this table:

increment is positive

increment is negative

If start > end, do not execute the code block. Stop looping when var is equal to or greater than end, or if Caché encounters a QUIT, RETURN, or GOTO command.

If start < end, do not execute the code block. Stop looping when var is equal to or less than end, or if Caché encounters a QUIT, RETURN, or GOTO command.

Caché evaluates the start, increment, and end values when it begins execution of the loop. Any changes made to these values within the loop are ignored and have no effect on the number of loop executions.

When the loop terminates, var contains a value that reflects the increment resulting from the last execution of the loop. However, var is never incremented beyond the maximum value specified in end.

A FOR loop can include multiple comma-separated forparameter arguments, but only one var argument. Valid syntax is as follows:

FOR var\=start1:increment1:end1,start2:increment2:end2

Arguments

var

The var argument can be a simple local variable, a subscripted local variable, or an [instance variable](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GOBJ_objapi#GOBJ_objapi_instancevar) (such as i%property). The FOR command initializes (or sets) this variable; var does not need to be defined prior to the FOR command.

*   When using loop counter syntax, var is a local variable that holds the current counter value for the FOR loop. It initially contains the numeric value specified by start. It is then recalculated using increment for each repetition of the FOR loop.
    
*   When using var\=expr syntax, a local variable initialized to the expr value.
    

expr

The value Caché assigns to var before executing the loop commands. The value of expr can be specified as a literal or any valid expression. An expr can be a single value or a comma-separated list of values. If a single value, Caché executes the FOR loop once, supplying this var value to the code block. If a comma-separated list of values, Caché executes the FOR loop as many times as there are values, each loop equating var to the value, and supplying this var value to the code block

start

The numeric value Caché assigns to var on the first iteration of the FOR loop. The value of start can be specified as a literal or any valid expression; Caché evaluates start for its numeric value. A non-numeric start value is evaluated as 0.

increment

The numeric value Caché uses to increment var after each iteration of the FOR loop. It is required if you specify start. The value of increment can be specified as a literal or any valid expression; Caché evaluates increment for its numeric value. increment can be an integer or a fractional number; it can be a positive number (to increment) or a negative number (to decrement).

A value of 0 or a non-numeric increment value causes the FOR loop to repeat infinitely, unless exited. This is true even if end\=0. However, if start is greater than end, the loop does not execute and an increment of 0 is ignored.

end

The numeric value Caché uses to terminate a FOR loop. When var equals (or exceeds) this value, the FOR loop is executed one last time and then terminated. The value of end can be specified as a literal or any valid expression; Caché evaluates end for its numeric value.

If start\=end the FOR loop executes once. If start is greater than end (and increment is a positive number) the FOR loop does not execute.

code

A block of one or more Caché ObjectScript commands enclosed in curly braces. This executable block of code can contain multiple commands, labels, comments, line returns, indents, and blank spaces as needed. When the FOR command concludes, execution continues with next command after the closing curly brace.

An opening or closing curly brace may appear on its own code line or on the same line as a command. An opening or closing curly brace may even appear in column 1 (though this is not recommended). It is a recommended programming practice to indent curly braces to indicate the beginning and end of a nested block of code. No whitespace is required before or after an opening curly brace. No whitespace is required before a closing curly brace, including a curly brace that follows an argumentless command. There is only one whitespace requirement for curly braces: a closing curly brace must be separated from the command that follows it by a space, tab, or line return.

Exiting a FOR Loop

You can exit a FOR loop by issuing a QUIT, RETURN, CONTINUE, or GOTO:

*   [QUIT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cquit) exits the current FOR block structure. Therefore, a QUIT in a FOR block causes Caché to begin execution at the next line after the FOR block. A QUIT only exits the current FOR block; if a FOR block is nested in another FOR block (or in any other block structure), issuing a QUIT exits the inner FOR block to the outer block structure.
    
    QUIT behavior within a FOR block (and some other block structures) differs from QUIT behavior when not in a block structure. A QUIT outside of one of these block structures exits the current routine, not just the current code block. For further details, refer to the [QUIT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cquit) command reference page.
    
    A QUIT exits a FOR block only if the QUIT appears within the FOR block. If the FOR loop invokes a subroutine, issuing a QUIT in the subroutine terminates the subroutine, not the FOR loop that invoked it.
    
*   [RETURN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_creturn) exits the current routine, whether or not it is issued from within a FOR block structure.
    
*   [CONTINUE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ccontinue) exits the current FOR loop. It causes execution to immediately jump back to the FOR command. The FOR command then increments and evaluates its arguments, and, based on that evaluation, determines whether to re-execute the code block loop. Thus, the CONTINUE command has exactly the same effect on execution as reaching the closing curly brace of the code block.
    
*   [GOTO](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cgoto) can exit the current FOR block structure by transferring control outside of the FOR code block. A FOR loop is not terminated by a GOTO that transfers control within the FOR code block.
    

Examples

Argumentless FOR

In the following example, demonstrating argumentless FOR, the user is prompted repeatedly for a number that is then passed to the Calc subroutine by the DO command. The FOR loop terminates when the user enters a null string (presses ENTER without inputting a number), which causes the QUIT command to execute.

Mainloop
  FOR {
    READ !,"Number: ",num 
    QUIT:num\="" 
    DO Calc(num) 
  }
Calc(a)
   WRITE !,"The number squared is ",a\*a
   QUIT

Using FOR var\=expr

When you specify var\=expr, Caché executes the FOR loop as many times as there are comma-separated values in expr. The value(s) in expr can be a literal or any valid expression. If you specify an expression, it must evaluate to a single value.

In the following example, the FOR command executes the code block once, with num having the value 4. It writes the number 12:

Loop
  SET val\=4
  FOR num\=val {
    WRITE num\*3,! 
  }
  WRITE "Next command after FOR code block"

 

In the following example, the FOR command executes the code block once, with alpha(7) having the value “abcdefg”:

Loop
  SET val\="abc"
  FOR alpha(7)\=val\_"defg" {
    WRITE alpha(7),!
  }
  WRITE "Next command after FOR code block"

 

In the following example, the FOR command executes the code block eight times, supplying each successive perfect number to the code block:

  FOR pnum\=6,28,496,8128,33550336,8589869056,137438691328,2305843008139952128 {
    WRITE "Perfect number ",pnum
    SET rp\=$REVERSE(pnum)
    IF 54\=$ASCII(rp,1) {
       WRITE " ends in 6",! }
    ELSEIF 56\=$ASCII(rp,1),50\=$ASCII(rp,2) {
       WRITE " ends in 28",! }
    ELSE {WRITE " is something unknown to mathematics",! }
  }

 

Using FOR var\=start:increment:end

The arguments start, increment, and end specify a start, increment, and end value, respectively. All three are evaluated as numbers. They can be integer or real, positive or negative. If you supply string values, they are converted to their numeric equivalents at the start of the loop.

When Caché first enters the loop, it assigns the start value to var and compares the var value to the end value. If the var value is less than the end value (or greater than it, in the case of a negative increment value), Caché executes the loop commands. It then updates the var value using the increment value. (The var value is decremented if a negative increment is used.)

Execution of the loop continues until the incrementing of the var value would exceed the end value (or until Caché encounters a QUIT, RETURN, or GOTO). At that point, to prevent var from exceeding end, Caché suppresses variable assignment and loop execution ends. If the increment causes the var value to equal the end value, Caché executes the FOR loop one last time and then terminates the loop.

The following code executes the WRITE command repetitively to output, in sequence, all of the characters in string1, except for the last character. Because the end value is specified as len\-1, the last character is not output. This is because the test is performed at the top of the loop, and the loop is terminated when the variable value (index) exceeds (not just matches) the end value (len\-1).

Stringwriteloop
  SET string1\="123 Primrose Path"
  SET len\=$LENGTH(string1)
  FOR index\=1:1:len\-1 {
    WRITE $EXTRACT(string1,index)
  }

 

Using FOR var\=start:increment

In this form of the FOR command there is no end value; the loop must contain a QUIT, RETURN, or GOTO command to terminate the loop.

The start and increment values are evaluated as numbers. They can be integer or real, positive or negative. If string values are supplied, they are converted to their numeric equivalents at the start of the loop. Caché evaluates the start and increment values when it begins execution of the loop. Any changes made to these values within the loop are ignored.

When Caché first enters the loop, it assigns the start value to var and executes the loop commands. It then updates the var value using the increment value. (The var value is decremented if a negative increment is used.) Execution of the loop continues until Caché encounters a QUIT, RETURN, or GOTO within the loop.

The following example uses start:increment syntax to return all of the multiples of 7 that are less than three digits in length:

  FOR i(1)\=0:7 {
      QUIT:$LENGTH(i(1))\=3
      WRITE "multiple of 7 = ",i(1),! }

 

The following example uses start:increment syntax to compute an average for a series of user supplied numbers. The postconditional QUIT is included to terminate execution of the loop when the user enters a null string (that is, presses ENTER without inputting a value). When the postconditional expression (num\="") tests TRUE, Caché executes the QUIT and terminates the loop.

The loop counter (the i variable) is used to keep track of how many numbers have been entered. i is initialized to 0 because the counter increment occurs after the user inputs a number. Caché terminates the loop when the user enters a null. After the loop is terminated, the SET command references i (as a local variable) to calculate the average.

Averageloop
  SET sum\=0
  FOR i\=0:1 {
    READ !,"Number: ",num 
    QUIT:num\="" 
    SET sum\=sum+num 
  }
  SET average\=sum/i

Using FOR with Multiple forparameters

A FOR command can contain only one var\= argument, but can contain multiple forparameter arguments, specified as a comma-separated list. For example, the syntax var\=expr1,expr2,expr3 would cause the code block to be executed three times, with a different var value for each execution.

These forparameter arguments are evaluated and executed in strict left-to-right order. Therefore an error in one forparameter does not prevent the execution of the forparameters that precede it.

A single FOR command can contain both types of parameter syntax: expr syntax and start:increment:end syntax.

The following example combines expr syntax with start:increment:end syntax. The two forparameters are separated by a comma. The first time through the FOR, Caché uses the expr syntax, and invokes the Test subroutine with x equal to the value of y. In the second (and subsequent) iterations, Caché uses the start:increment:end syntax. It sets x to 1, then 2, etc. On the final iteration, x\=10.

Mainloop
  SET y\="beta"
  FOR x\=y,1:1:10 {
    DO Test
  }
  QUIT
Test
  WRITE !,"Running test number ",x
  QUIT

 

The following example is a sampling program that includes three forparameter arguments with start:increment:end syntax. It sets i to 1, then increments single-digit numbers by 1 for 1 through 10; the second forparameter takes the i value of 10 and increments it by 10s through 100; the third forparameter takes the i value of 100 and increments it by 100s through 1000. Note that this example repeats the 10 and 100 values:

   FOR i\=1:1:10,i:10:100,i:100:1000 {WRITE i,!}

 

The following example performs the same operation as the previous example, without repeating the 10 and 100 values:

   FOR i\=1:1:9,i+1:10:99,i+10:100:1000 {WRITE i,!}

 

Incrementing with Argumentless FOR

The argumentless FOR operates the same as the FOR var\=start:increment form. The only difference is that it does not provide a way to keep track of the number of loop executions.

The following example shows how the previous loop counter example might be rewritten using the argumentless FOR. The assignment i\=i+1 replaces the loop counter.

Average2loop
  SET sum\=0
  SET i\=0
  FOR {
    READ !,"Number: ",num QUIT:num\="" 
    SET sum\=sum+num,i\=i+1
  }
  SET average\=sum/i
  WRITE !!,"Average is: ",average
  QUIT

Notes

FOR and NEW

A [NEW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cnew) command can affect var. Issuing an argumentless NEW command or an exclusive NEW command (that does not specifically exclude var) in the body of the FOR loop can result in var being undefined in the new frame context.

A NEW command that does not include var has no effect on FOR loop execution, as shown in the following example:

   SET a\=1,b\=1,c\=8
   FOR i\=a:b:c {
     WRITE !,"count is ",i
     NEW a,c
     WRITE " loop"
     NEW (i)
     WRITE " again"
   }

 

FOR and Watchpoints

You have limited use of watchpoints with FOR. If you establish a watchpoint for the control (index) variable of a FOR command, Caché triggers the specific watchpoint action only on the initial evaluation of each FOR command argument. This restriction is motivated by performance considerations.

The following example contains three kinds of FOR command arguments for the watched variable x: a range, with initial value, increment, and limit (final value); a single value; and a range with initial value, increment, and no limit. Breaks occur when x has the initial values 1, 20, and 50.

USER>ZBREAK \*x
USER>FOR x=1:1:10,20,50:2 {SET t=x QUIT:x>69}
<BREAK>
USER 2f0>WRITE
x=1
USER 2f0>g
USER>FOR x=1:1:10,20,50:2 {SET t=x QUIT:x>69}
<BREAK>
USER 2f0>WRITE
t=10
x=20
USER> 2f0>g
USER>FOR x=1:1:10,20,50:2 {SET t=x QUIT:x>69}
<BREAK>
USER 2f0>WRITE
t=20
x=50
USER 2f0>g
USER>WRITE
t=70
x=70

See Also

*   [FOR (legacy version)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cfor_legacy) command
    
*   [DO WHILE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cdowhile) command
    
*   [WHILE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cwhile) command
    
*   [IF](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cif) command
    
*   [CONTINUE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ccontinue) command
    
*   [DO](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cdo) command
    
*   [QUIT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cquit) command
    
*   [RETURN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_creturn) command
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_celseif "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cgoto "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_cfor.xml**