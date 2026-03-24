# ZTRAP

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_cztrap

---

 

Caché ObjectScript Reference

ZTRAP

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cznspace "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czwrite "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_COMMANDS "Caché ObjectScript Commands") \]  >  \[ [ZTRAP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cztrap "ZTRAP") \]

Go to: Description Arguments Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Forces an error with a specified error code.

Synopsis

ZTRAP:pc ztraparg

ZTRAP:pc $ZERROR
ZTRAP:pc $ZE

Arguments

pc

Optional — A postconditional expression.

ztraparg

Optional — An error code string. An error code string is specified as a string literal or an expression that evaluates to a string; only the first four characters of the string are used.

$ZERROR

The special variable $ZERROR, which can be abbreviated $ZE.

Description

The ZTRAP command accepts both a command postconditional and argument indirection. ZTRAP has three forms:

*   Without an argument
    
*   With a string argument
    
*   With $ZERROR
    

ZTRAP without an argument forces an error with the error code <ZTRAP>.

ZTRAP ztraparg forces an error with the error code <Zxxxx>, where xxxx is the first four characters of the string specified by ztraparg. If you specify an expression, rather than a quoted string literal, the compiler evaluates the expression and uses the first four characters of the resulting string. When evaluating an expression, Caché strips the plus sign and leading and trailing zeros from numbers. All remaining characters of ztraparg are ignored.

ZTRAP $ZERROR does not force a new error. It stops execution at the current program stack level and pops stack levels until another error handler is found. Execution then continues in that error handler with the current error code.

Arguments

pc

An optional postconditional expression. Caché executes the command if the postconditional expression is true (evaluates to a nonzero numeric value). Caché does not execute the command if the postconditional expression is false (evaluates to zero). For further details, refer to [Command Postconditional Expressions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_commands#GCOS_commands_pc) in Using Caché ObjectScript.

ztraparg

A string literal or an expression that evaluates to a string. Any of the following values can be specified for ztraparg:

*   A quoted string of any length containing any characters. ZTRAP uses only the first four characters to generate an error code; if there are fewer than four characters, it uses the characters provided. Unlike system error codes, which are always uppercase, case is preserved. Thus:
    
      ZTRAP "FRED"  ; generates <ZFRED>
      ZTRAP "Fred"  ; generates <ZFred>
      ZTRAP "Freddy"  ; generates <ZFred>
      ZTRAP "foo"  ; generates <Zfoo>
      ZTRAP " foo"  ; generates <Z foo>
      ZTRAP "@#$%"  ; generates <Z@#$%>
      ZTRAP ""  ; generates <Z>
      ZTRAP """"  ; generates <Z">
    
*   An expression that evaluates to a string.
    
      ZTRAP 1234  ; generates <Z1234>
      ZTRAP 2+2  ; generates <Z4>
      ZTRAP 10/3  ; generates <Z3.33>
      ZTRAP +0.700  ; generates <Z.7>
      ZTRAP $ZPI  ; generates <Z3.14>
      ZTRAP $CHAR(64)\_$CHAR(37)  ; generates <Z@%>
      ZTRAP ""  ; generates <Z>
      ZTRAP """"  ; generates <Z">
    

The ZTRAP command accepts argument indirection. For more information, refer to [Indirection](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_operators#GCOS_operators_indirection) in Using Caché ObjectScript.

Passing Control to an Error Handler with $ZERROR

When the ZTRAP argument is the special variable $ZERROR, special processing is performed which is useful in $ZTRAP error handlers. ZTRAP $ZERROR does not force a new error. It stops execution at the current program stack level and pops stack levels until another error handler is found. Execution then continues in that error handler with the current error code. This error handler may be located in a different namespace.

This way of passing control to an error handler is preferable to use of the legacy ZQUIT command.

Examples

This example shows how you use the ZTRAP command with an expression to produce an error code:

   ; at this point the routine discovers an error ...
   ZTRAP "ER23"
   ...

When the routine is run and it discovers the anticipated error condition, the output appears as follows:

<ZER23>label+offset^routine

This example shows how the use of a postconditional affects the ZTRAP command:

   ;
   ZTRAP:y<0 "yNEG"
   ;

When the routine is run and y is negative, the output is:

<ZyNEG>label+offset^routine

This example shows how you use argument indirection in the ZTRAP command:

   ;
   SET ERPTR\="ERMSG"
   SET ERMSG\="WXYZ"
   ;
   ;
   ZTRAP @ERPTR

The output is:

<ZWXYZ>label+offset^routine

The following example shows a ZTRAP command that invokes a $ZTRAP error trap handler defined at a previous context level.

Main
   NEW $ESTACK
   SET $ZTRAP\="OnErr"
   WRITE !,"$ZTRAP set to: ",$ZTRAP
   WRITE !,"Main $ESTACK= ",$ESTACK   // 0
   WRITE !,"Main $ECODE= ",$ECODE," $ZERROR=",$ZERROR
   DO SubA
   WRITE !,"Returned from SubA"   // not executed
   WRITE !,"MainReturn $ECODE= ",$ECODE," $ZERROR=",$ZERROR
   QUIT
SubA
   WRITE !,"SubA $ESTACK= ",$ESTACK   // 1
   ZTRAP 
   WRITE !,"SubA $ECODE= ",$ECODE," $ZERROR=",$ZERROR
   QUIT
OnErr
   WRITE !,"OnErr $ESTACK= ",$ESTACK   // 0
   WRITE !,"OnErr $ECODE= ",$ECODE," $ZERROR=",$ZERROR
   QUIT

 

See Also

*   [$ZERROR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzerror) special variable
    
*   [$ZTRAP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztrap) special variable
    
*   [Error Handling](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_errors) in Using Caché ObjectScript
    
*   [Labels](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_syntax#GCOS_syntax_labels) in Using Caché ObjectScript
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cznspace "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czwrite "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_cztrap.xml**