# IF

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_cif

---

 

Caché ObjectScript Reference

IF

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_chang "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cjob "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_COMMANDS "Caché ObjectScript Commands") \]  >  \[ [IF](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cif "IF") \]

Go to: Description Arguments IF with QUIT IF with GOTO Example See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Evaluates an expression, then selects which block of code to execute based on the truth value of the expression.

Synopsis

IF expression1,... {
  code
}
ELSEIF expression2,... {
    code
}
ELSE {
    code
}

or

I expression1,... {
  code
}
ELSEIF expression2,... {
    code
}
ELSE {
    code
}

Arguments

expression1

A boolean test condition for the IF clause. A single condition or a comma-separated list of conditions.

expression2

A boolean test condition for an ELSEIF clause. A single condition or a comma-separated list of conditions.

code

A block of Caché ObjectScript commands enclosed in curly braces.

Description

This page describes the IF, ELSEIF, and ELSE command keywords, all of which are considered to be component clauses of the IF command.

An IF command consist of one IF clause, followed by any number of ELSEIF clauses, followed by one ELSE clause. The ELSEIF and ELSE clauses are optional, but it is a good programming practice to always specify an ELSE clause.

The IF command first evaluates the IF clause expression1 and, if expression1 is TRUE, it executes the code block within the curly braces that follow it and the IF command completes.

If expression1 is FALSE, execution jumps to the next clause of the IF statement. It evaluates the first ELSEIF clause (if present). If expression2 in the ELSEIF clause is TRUE, it executes the ELSEIF code block within the curly braces that follow it and the IF command completes. If expression2 is FALSE, the next ELSEIF clause (if present) is evaluated in the same way. Each successive ELSEIF clause is tested in the order listed until one of them evaluates TRUE, or all of them evaluate FALSE.

If the IF clause and all ELSEIF clauses evaluate to FALSE, execution continues with the ELSE clause. It executes the ELSE code block within the curly braces that follow it and the IF command completes. If the ELSE clause is omitted, the IF command completes.

IF is a block-oriented command. Each command keyword is followed by a block of code enclosed in curly braces. IF, ELSEIF, and ELSE clauses may use white space (line returns, indents, and blank spaces) freely. However, each IF and ELSEIF keyword and the first character of its boolean test expression must be on the same line, separated by one blank space. A boolean test expression can span multiple lines and contain multiple blank spaces.

An opening or closing curly brace may appear on its own code line or on the same line as a command. An opening or closing curly brace may even appear in column 1 (though this is not recommended). It is a recommended programming practice to indent curly braces to indicate the beginning and end of a nested block of code. No whitespace is required before or after an opening curly brace. No whitespace is required before or after a closing curly brace, including a curly brace that follows an argumentless command. There is only one whitespace requirement for curly braces: the final closing curly brace of the last clause of the IF command must be separated from the command that follows it by a space, tab, or line return.

Note:

An earlier, [line-oriented IF command syntax](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cif_legacy) may exist in legacy applications. This line-oriented form of IF does not use curly braces or support the ELSEIF clause. These two forms of IF are syntactically different and should not be combined. Thus, an IF of one type should not be paired with an ELSE of the other type. The earlier line-oriented ELSE could be abbreviated as E; the block-oriented ELSE keyword cannot be abbreviated.

In a program where a block-oriented IF is nested within a line-oriented IF, the block-oriented IF requires an ELSE clause. The following is valid syntax:

  IF x\=1 WRITE "x is 1"
     IF y\=1 { WRITE "y is 1" }
     ELSE { WRITE "y is not 1" }
  ELSE  WRITE "x is not 1"

The following is not valid syntax:

  IF x=1 WRITE "x is 1"
     IF y=1 { WRITE "y is 1" }
  ELSE  WRITE "x is not 1"

The block-oriented IF command does not read or set the value of the $TEST special variable. If a boolean test expression evaluates to TRUE, it executes the block of code within the curly braces, regardless of the value of $TEST.

Arguments

expression1

A test condition for the IF clause. It can take the form of a single expression or a comma-separated list of expressions. For an expression list, Caché evaluates the individual expressions in left to right order. It stops evaluation if it encounters an expression in the comma-separated list that evaluates to FALSE. If all expressions in the comma-separated list evaluate to TRUE, Caché executes the block of code associated with the IF clause. If any expression in the list evaluates to FALSE, Caché ignores any remaining expressions, and does not execute the block of code associated with the IF clause.

Commonly, expression1 is a boolean expression that evaluates to TRUE or FALSE (for example, x=7). Refer to the [Operators and Expressions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_operators) chapter of Using Caché ObjectScript. IF interprets a literal value as a boolean TRUE and FALSE as follows:

*   TRUE: any non-zero numeric value, or a numeric string that evaluates to a non-zero numeric value. For example, 1, 7, -.007, "7-7", and "7dwarves".
    
*   FALSE: a zero numeric value, or a string that evaluates to a zero numeric value. A non-numeric string evaluates to a zero numeric value. For example, 0, -0.00, 7-7, "0", "TRUE", "FALSE", "strike3", and the empty string ("").
    

For further details, refer to [Strings as Numbers](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_nonnumasnum) in the “Data Types and Values” chapter of Using Caché ObjectScript.

expression2

A test condition for an ELSEIF clause. It can take the form of a single expression or a comma-separated list of expressions. It is evaluated the same way as expression1.

IF with QUIT

If a QUIT command is encountered within an IF code block (or an ELSEIF code block or an ELSE code block) the QUIT behaves as a regular QUIT command, as if the code block did not exist. This behavior differs from a QUIT within any other type of curly brace code block (FOR, WHILE, DO...WHILE, TRY, or CATCH).

*   If the IF code block is nested within a loop structure (such as a FOR code block), the QUIT exits the loop structure block and continues execution with the command that follows the loop structure code block.
    
*   If the IF code block is within a TRY block or a CATCH block, the QUIT exits the TRY or CATCH block and continues execution with the command that follows the TRY or CATCH block.
    
*   If the IF code block is not nested within a loop structure, or within a TRY or CATCH block, the QUIT exits the current routine.
    

Issuing a [RETURN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_creturn) exits the current routine, whether or not it is issued from within a block structure.

The following example demonstrates the behavior of QUIT when the IF is not in a loop structure. The QUIT exits the routine:

  SET y\=$RANDOM(10)
  IF y#2\=0 {
    WRITE y," is even",!
    QUIT
    WRITE "never written"
    }
  ELSE {
    WRITE y," is odd",!
    QUIT
    WRITE "never written"
    }
  WRITE "QUIT out of the IF (never written)"

 

The following example demonstrates the behavior of QUIT when the IF is in a loop structure. The QUIT exits the FOR loop, then execution of the routine continues:

  FOR x\=1:1:8 {
    IF x#2\=0 {
      WRITE x," is even",!
      QUIT:x\=4
    }
    ELSE {
      WRITE x," is odd",!
    }
  }
  WRITE "QUIT out of the FOR loop (written)"

 

The following example demonstrates the behavior of QUIT when the IF is in a TRY block. The QUIT exits the TRY block, then execution of the routine continues with the next code after the CATCH block:

  TRY {
  SET y\=$RANDOM(10)
    IF y#2\=0 {
      WRITE y," is even",!
      QUIT
      WRITE "never written"
    }
    ELSE {
      WRITE y," is odd",!
      QUIT
      WRITE "never written"
    }
  WRITE "QUIT out of the IF (never written)" 
  }
  CATCH exp1 {
    WRITE "only written if an error occurred",!
    WRITE "Error Name: ",$ZCVT(exp1.Name,"O","HTML"),!
  }
  TRY {
    WRITE "on to the next TRY block"
  }
  CATCH exp2 {
    WRITE "only written if an error occurred",!
    WRITE "Error Name: ",$ZCVT(exp2.Name,"O","HTML"),!
  }

 

IF with GOTO

If a GOTO is encountered within an IF code block, program execution obeys that statement, with certain restrictions:

A GOTO statement can jump to a location outside of the IF command, or within the code block of the current clause. A GOTO statement cannot jump into another code block: neither a code block that belongs to another clause of the current IF command, nor a code block that belongs to another IF, FOR, DO WHILE, or WHILE command.

Example

In the following example, the IF command is used to categorize responders into one of three groups and invokes the appropriate subroutine. The three groups are females aged 44 or less, males aged 44 or less, and either females or males from age 45 through 120. In this example, the sex test expressions use the Contains operator ( \[ ). (See [Operators](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_operators) in Using Caché ObjectScript.)

Mainloop
  NEW sex,age
  READ !,"What is your sex? (M or F): ",!,sex QUIT:sex\=""
  READ !,"What is your age? ",!,age QUIT:age\=""
  IF "Ff"\[sex,age<45 {
    DO SubA(age)
  }
  ELSEIF "Mm"\[sex,age<45 {
    DO SubB(age)
  }
  ELSEIF "FfMm"\[sex,age\>44,age<125 {
    DO SubC(age)
  }
  ELSE {
    WRITE !,"Invalid data value input"
  }
SubA(y)
  WRITE !,"Young woman ",y," years old"
SubB(y)
  WRITE !,"Young man ",y," years old"
SubC(y)
  WRITE !,"Older person ",y," years old"

See Also

*   [DO WHILE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cdowhile) command
    
*   [FOR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cfor) command
    
*   [WHILE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cwhile) command
    
*   [GOTO](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cgoto) command
    
*   [QUIT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cquit) command
    
*   [$CASE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fcase) function
    
*   [IF (legacy version)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cif_legacy) command
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_chang "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cjob "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_cif.xml**