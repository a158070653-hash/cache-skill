# $SELECT

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fselect

---

 

Caché ObjectScript Reference

$SELECT

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fsconvert "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fsequence "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fselect "$SELECT") \]

Go to: Description Parameters Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Returns the value associated with the first true expression.

Synopsis

$SELECT(expression:value,...)
$S(expression:value,...)

Parameters

expression

The select test for the associated value parameter.

value

The value to be returned if the associated expression evaluates to true.

Description

The $SELECT function returns the value associated with the first expression that evaluates to true (1). Each $SELECT argument is a pair of expressions separated by a colon. The left half is a truth-valued expression. The right half can be any expression.

The specified list of expression:value pairs can be of any length. $SELECT evaluates the parameters from left to right. When $SELECT discovers a truth-valued expression with the value of true (1), it returns the matching expression to the right of the colon. $SELECT stops evaluation after it discovers the left-most true truth-valued expression. It never evaluates later pairs on the parameter list.

Parameters

expression

The select test for the associated value parameter. It can be any valid Caché relational or logical expression. If no expression evaluates to true, Caché generates a <SELECT> error. To prevent an error from disrupting an executing routine, the final expression can be the value 1, which always evaluates to true.

When expression is a string or numeric, any non-zero numeric value evaluates to true. A zero numeric value or a non-numeric string evaluates to false.

value

The value to be returned if the associated expression evaluates to true. It can be a numeric value, a string literal, a variable name, or any valid Caché ObjectScript expression. If you specify an expression for value, it is evaluated only after the associated expression evaluates to true. If value contains a subscripted global reference, it changes the naked indicator when it is evaluated. For this reason, be careful when using naked global references either within or immediately after a $SELECT function. For more details on the naked indicator, see [Naked Global Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GGBL_using) in Using Caché Globals.

Examples

To ensure that a <SELECT> error never results, you should always include the value 1 as the last expression with an appropriate default value. This is shown in the following example:

Start
   READ !,"Which level?: ",a 
   QUIT:a\=""
   SET x\=$SELECT(a\=1:"Level1",a\=2:"Level2",a\=3:"Level3",1:"Start")
   DO @x
Level1()
   WRITE !,"This is Level 1"
Level2()
   WRITE !,"This is Level 2"
Level3()
   WRITE !,"This is Level 3"

If the user enters a value other then 1, 2, 3, or the null string, control is passed back to the top of the routine.

You can use $SELECT to replace multiple IF clauses. The following example uses IF, ELSEIF, and ELSE clauses to determine whether a number is odd or even:

OddEven()
   READ !,"Enter an Integer: ",x
   QUIT:x\=""
   WRITE !,"The input value is "
   IF 0\=$ISVALIDNUM(x) { WRITE "not a number" }
   ELSEIF x\=0 { WRITE "zero" }
   ELSEIF ""\=$NUMBER(x,"I") { WRITE "not an integer" }
   ELSEIF x#2\=1 { WRITE "odd" }
   ELSE  { WRITE "even" }
   DO OddEven

The following example also accepts a number and determines if the number is odd or even. It uses $SELECT to replace the IF command in the previous example:

OddEven()
   READ !,"Enter an Integer: ",x
   QUIT:x\=""
   WRITE !,"The input value is "
   WRITE $SELECT(0\=$ISVALIDNUM(x):"not a number",x\=0:"zero",
                 ""\=$NUMBER(x,"I"):"not an integer",x#2\=1:"odd",1:"even")
   DO OddEven

See Also

*   [$CASE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fcase) function
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fsconvert "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fsequence "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fselect.xml**