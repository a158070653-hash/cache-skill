# $ZTRAP

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_vztrap

---

 

Caché ObjectScript Reference

$ZTRAP

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztimezone "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzversion "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Special Variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_VARIABLES "Caché ObjectScript Special Variables") \]  >  \[ [$ZTRAP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztrap "$ZTRAP") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Contains the name of the current error trap handler.

Synopsis

$ZTRAP
$ZT

Description

$ZTRAP contains the [line label](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_syntax#GCOS_syntax_labels) name and/or routine name of the current error trap handler. There are three ways to set $ZTRAP:

*   SET $ZTRAP=“location”
    
*   SET $ZTRAP=“\*location”
    
*   SET $ZTRAP=“^%ET” or “^%ETN”
    

Here location can be specified as label (a [line label](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_syntax#GCOS_syntax_labels) in the current routine), ^routine (the beginning of a specified external routine), or label^routine (a specified label in a specified external routine).

However, $ZTRAP\=label^routine cannot be used in a procedure block. $ZTRAP in a procedure block cannot be used to go to a location that is outside the body of the procedure; $ZTRAP in a procedure block can only reference a location within that procedure block.

Note:

$ZTRAP provides legacy support for label+offset in some contexts (but not in procedures). This optional +offset is an integer specifying the number of lines to offset from label. The use of +offset is deprecated, and may result in a compilation warning error. InterSystems recommends that you avoid the use of a line offset when specifying location.

You cannot specify an +offset when calling a procedure or a CACHESYS % routine. If you attempt to do so, Caché issues a <NOLINE> error.

The $ZTRAP location must be in the current namespace. $ZTRAP does not support [extended routine reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_syntax#GCOS_syntax_namespaces_extendedrefs).

If you specify a nonexistent location in the current routine, Caché issues a <NOLINE> error message. Also, private labels are not supported.

Each stack level can have its own $ZTRAP value. When you set $ZTRAP, Caché saves the value of $ZTRAP for the previous stack level. Caché restores this value when the current stack level ends. To enable error trapping at the current stack level, set $ZTRAP to the error trap handler by specifying its location. For example:

   IF $ZTRAP\="" {WRITE !,"$ZTRAP not set" }
   ELSE {WRITE !,"$ZTRAP already set: ",$ZTRAP
         SET oldtrap\=$ZTRAP }
   SET $ZTRAP\="Etrap1^Handler"
   WRITE !,"$ZTRAP set to: ",$ZTRAP
   //  program code
   SET $ZTRAP\=oldtrap
   WRITE !,"$ZTRAP restored to: ",$ZTRAP

 

When an error occurs, this format unwinds the call stack and transfers control to the specified error trap handler.

In [SqlComputeCode](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ROBJ_property_sqlcomputecode), do not set $ZTRAP=$ZTRAP. This can result in significant problems with transactional processing and error reporting.

You can choose to leave the call stack as it is after the error has occurred. To do so, place an asterisk (\*) before location. This form is not valid for use within procedures. It can only be used in subroutines that are not procedures, as in this example:

Main
   SET $ZTRAP\="\*OnError"
   WRITE !,"$ZTRAP set to: ",$ZTRAP
  // program code
OnError
   // Error handling code
   QUIT

 

This format simply causes a GOTO to the [line label](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_syntax#GCOS_syntax_labels) specified in $ZTRAP; $STACK and $ESTACK are unchanged. The context frame of the $ZTRAP error handling routine is the same as the context frame where the error occurred. However, Caché resets [$ROLES](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vroles) to the value that was in effect for the execution level at which $ZTRAP was set; this prevents the $ZTRAP error handler from using elevated privileges that were granted to the routine after establishing the error handler. Upon completion of the $ZTRAP error handling routine, Caché unwinds the stack to the previous context level. This form of $ZTRAP is especially useful for analyzing unexpected errors.

Note that the asterisk sets a $ZTRAP option; it is not part of the location. For this reason, this asterisk does not display when performing a WRITE or ZZDUMP on $ZTRAP.

The error handlers ^%ETN and ^%ET always behave as if they were preceded by an asterisk (\*).

To disable error trapping, set $ZTRAP to the null string (""). This clears any error trap set at the current DO stack level.

When you set an error handler using $ZTRAP, this handler takes precedence over any existing $ETRAP error handler. Caché implicitly performs a NEW $ETRAP command and sets $ETRAP to the null string ("").

Note:

Use of $ZTRAP from the Caché Terminal prompt is limited to the current line of code. The SET $ZTRAP command and the command generating the error must be in the same line of code. Caché Terminal restores $ZTRAP to the system default at the beginning of each command line.

TRY / CATCH and $ZTRAP

You cannot set $ZTRAP within a TRY block. Attempting to do so generates a compilation error. You can set $ZTRAP prior to the TRY block, or within the CATCH block.

An error that occurs within a TRY block is handled by the CATCH block, regardless of whether a $ZTRAP has previously been set. An error that occurs within a CATCH block is handled by the current error trap handler.

The first of the following examples shows an error occurring in a TRY block. The second of the following examples shows an exception being thrown in a TRY block. In both cases, the CATCH block, not the $ZTRAP, is taken:

  SET $ZTRAP\="Ztrap"
  TRY { WRITE 1/0 }    /\* divide-by-zero error \*/
  CATCH { WRITE "Catch taken" }
  QUIT
Ztrap
  WRITE "$ZTRAP taken"
  SET $ZTRAP\=""
  QUIT

 

  SET $ZTRAP\="Ztrap"
  TRY { SET myvar\=##class(Sample.MyException).%New("Example Error",999,,errdatazero)
        WRITE !,"Throwing an exception!",!
        THROW myvar
        QUIT  }
  CATCH { WRITE "Catch taken" }
  QUIT
Ztrap
  WRITE "$ZTRAP taken"
  SET $ZTRAP\=""
  QUIT

 

However, a TRY block can call code that sets and uses $ZTRAP. In the following example, the divide-by-zero error is caught by $ZTRAP, rather than the CATCH block:

  TRY { DO Errsub } 
  CATCH { WRITE "Catch taken" }
  QUIT
Errsub
  SET $ZTRAP\="Ztrap"
  WRITE 1/0   /\* divide-by-zero error \*/
  QUIT
Ztrap
  WRITE "$ZTRAP taken"
  SET $ZTRAP\=""
  QUIT

 

A THROW command from a CATCH block can also invoke a $ZTRAP error handler.

Examples

The following example sets $ZTRAP to the OnError routine in this program. It then calls SubA in which an error occurs (attempting to divide a number by 0). When the error occurs, Caché calls the OnError routine specified in $ZTRAP. OnError is invoked at the context level at which $ZTRAP was set. Because OnError is at the same context level as Main, execution does not return to Main.

Main
   NEW $ESTACK
   SET $ZTRAP\="OnError"
   WRITE !,"$ZTRAP set to: ",$ZTRAP
   WRITE !,"Main $ESTACK= ",$ESTACK   // 0
   WRITE !,"Main $ECODE= ",$ECODE
   DO SubA
   WRITE !,"Returned from SubA"   // not executed
   WRITE !,"MainReturn $ECODE= ",$ECODE
   QUIT
SubA
   WRITE !,"SubA $ESTACK= ",$ESTACK   // 1
   WRITE !,6/0    // Error: division by zero
   WRITE !,"fine with me"
   QUIT
OnError
   WRITE !,"OnError $ESTACK= ",$ESTACK   // 0
   WRITE !,"$ECODE= ",$ECODE
   QUIT

 

The following example is identical to the previous example, with one exception: The $ZTRAP location is prefaced by an asterisk (\*). When the error occurs in SubA, this asterisk causes Caché to call the OnError routine at the context level of SubA (where the error occurred), not at the context level of Main (where $ZTRAP was set). Therefore, when OnError completes, execution returns to Main at the line following the DO command.

Main
   NEW $ESTACK
   SET $ZTRAP\="\*OnError"
   WRITE !,"$ZTRAP set to: ",$ZTRAP
   WRITE !,"Main $ESTACK= ",$ESTACK   // 0
   WRITE !,"Main $ECODE= ",$ECODE
   DO SubA
   WRITE !,"Returned from SubA"   // executed
   WRITE !,"MainReturn $ECODE= ",$ECODE
   QUIT
SubA
   WRITE !,"SubA $ESTACK= ",$ESTACK   // 1
   WRITE !,6/0    // Error: division by zero
   WRITE !,"fine with me"
   QUIT
OnError
   WRITE !,"OnError $ESTACK= ",$ESTACK   // 1
   WRITE !,"$ECODE= ",$ECODE
   QUIT

 

See Also

*   [THROW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cthrow) command
    
*   [ZQUIT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czquit) command
    
*   [ZINSERT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czinsert) command
    
*   [$ETRAP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vetrap) special variable
    
*   [$ECODE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vecode) special variable
    
*   [$ESTACK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vestack) special variable
    
*   [$STACK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vstack) special variable
    
*   [Error Handling](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_errors) in Using Caché ObjectScript
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztimezone "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzversion "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_vztrap.xml**