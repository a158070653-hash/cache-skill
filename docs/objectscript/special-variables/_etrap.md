# $ETRAP

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_vetrap

---

 

Caché ObjectScript Reference

$ETRAP

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vestack "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhalt "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Special Variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_VARIABLES "Caché ObjectScript Special Variables") \]  >  \[ [$ETRAP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vetrap "$ETRAP") \]

Go to: Description Example Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Contains a string of Caché ObjectScript commands to be executed when an error occurs.

Synopsis

$ETRAP
$ET

Description

$ETRAP contains a string that specifies one or more Caché ObjectScript commands that are executed when an error occurs.

Note:

$ETRAP is the least desirable of the available Caché ObjectScript error handling facilities. Its use is discouraged. (See “[Caché Error Handling Facilities](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vetrap#RCOS_vetrap54)” below.)

You use the SET command to give $ETRAP the value of a string that contains one or more Caché ObjectScript commands. Then, when an error occurs, Caché executes the commands you entered into $ETRAP. For example, suppose you set $ETRAP to a string that contains a GOTO command to transfer control to an error-handling routine:

   SET $ETRAP\="GOTO LOGERR^ERRROU"

Caché then executes this command in $ETRAP immediately following any Caché ObjectScript command that generates an error condition. Caché executes the $ETRAP command at the same context level in which the error condition occurs. However, Caché resets [$ROLES](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vroles) to the value that was in effect for the execution level at which $ETRAP was set; this prevents the $ETRAP error handler from using elevated privileges that were granted to the routine after establishing the error handler.

When setting $ETRAP to execute an error handler (for example, with a GOTO command) you can specify the error handler as label (a [label](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_syntax#GCOS_syntax_labels) in the current routine), ^routine (the beginning of a specified external routine), or label^routine (a specified label in a specified external routine).

$ETRAP supports label+offset in some contexts (but not in procedures). This optional +offset is an integer specifying the number of lines to offset from label. InterSystems recommends that you avoid the use of a line offset when specifying an error handler location.

$ETRAP Commands Compared with XECUTE Commands

The commands in a $ETRAP string are not executed in a new context level, unlike the commands in an XECUTE string. In addition, the $ETRAP command string is always terminated by an implicit QUIT command. The implicit QUIT command quits with a null-string argument when the $ETRAP error-handling commands are invoked in a user-defined function context where an argumented QUIT command is required.

Setting $ETRAP Values in Different Context Levels

By default, Caché carries the value of the $ETRAP special variable forward into new DO, XECUTE, and user-defined function contexts. However, you can create a new copy of $ETRAP in a context by issuing the NEW command, as follows:

   NEW $ETRAP

Whenever you issue a NEW for $ETRAP, Caché performs the following actions:

1.  Saves the copy of $ETRAP that was in use at that point.
    
2.  Creates a new copy of $ETRAP.
    
3.  Assigns the new copy of $ETRAP the same value as the old, saved copy of $ETRAP.
    

You then use the SET command to assign a different value to the new copy of $ETRAP. In this way, you can establish new $ETRAP error-handling commands for the current context.

You can also clear $ETRAP by setting it to the null string. Caché then executes no $ETRAP commands at the context level in the event of an error.

When a QUIT command causes the current context to be exited, Caché restores the old, saved value of $ETRAP.

Example

The following example demonstrates how the value of $ETRAP is carried forward into new contexts and how you can invoke $ETRAP error-handling commands again in each context after an error occurs. The $ETRAP commands in this example make no attempt to dismiss the error. Rather, control by default is passed back to $ETRAP error-handling commands at each previous context level.

The sample code is as follows:

ETR    
  NEW $ETRAP
  SET $ETRAP\="WRITE !,""$ETRAP invoked at Context Level "",$STACK"
    ; Initiate an XECUTE context that initiates a DO context
  XECUTE "DO A"
  QUIT
    ; Initiate a user-defined function context
A
  SET A\=$$B
  QUIT
    ; A User-defined function that generates an error
B()    
  QUIT 1

A sample session using this code might run as follows:

 >DO ^ETR 
$ETRAP invoked at context level 4 
$ETRAP invoked at context level 3 
$ETRAP invoked at context level 2 
$ETRAP invoked at context level 1 
<COMMAND>

Notes

Use NEW Before Setting $ETRAP to a New Value

If you assign a new value to $ETRAP in a context without first creating a new copy of $ETRAP with the NEW command, Caché establishes that new value as the value of $ETRAP not only for the current context but also for all previous contexts. Therefore, InterSystems strongly recommends that you use the NEW $ETRAP command to create a new copy of $ETRAP before you set $ETRAP with a new value.

$ETRAP Value is a Line of ObjectScript Code

Because the string value of $ETRAP is executable Caché ObjectScript commands, the length of the string cannot be longer than the maximum length of a Caché ObjectScript routine line. See [Using Caché ObjectScript](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS) for more information.

Caché Error Handling Facilities

The $ETRAP special variable is one of several Caché ObjectScript language facilities that enable you to control the handling and logging of errors that occur in your applications.

*   The preferred Caché features for error handling are the block-structured [TRY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ctry) and [CATCH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ccatch) commands.
    
*   The [$ZTRAP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztrap) special variable is preferable to $ETRAP.
    
*   $ETRAP will continue to be a supported feature of Caché. However, use of $ETRAP in new code should generally be avoided in preference to the other error handling facilities.
    

See [Error Handling](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_errors) in Using Caché ObjectScript for more information about error handling.

$ETRAP and $ZTRAP

When you set an error handler using $ZTRAP, this handler takes precedence over any existing $ETRAP error handler. Caché implicitly performs a NEW $ETRAP command and sets $ETRAP to the null string ("").

$ETRAP and TRY / CATCH

The TRY and CATCH commands perform error handling within an execution level. When an exception occurs within a TRY block, Caché normally executes the CATCH block of exception handler code that immediately follows the TRY block.

Note:

Use of $ETRAP within a program structured with TRY blocks is strongly discouraged.

You cannot set $ETRAP within a TRY block. Attempting to do so generates a compilation error. You can set $ETRAP prior to the TRY block, or within the CATCH block.

If $ETRAP was previously set and an exception occurs in a TRY block, Caché may take $ETRAP rather than CATCH unless you forestall this possibility. If both $ETRAP and CATCH are present when an exception occurs, Caché executes the error code (CATCH or $ETRAP) that applies to the current execution level. Because $ETRAP is intrinsically not associated with an execution level, Caché assumes that it is associated with the current execution level unless you specify otherwise. You must NEW $ETRAP before setting $ETRAP to establish a level marker for $ETRAP, so that Caché will correctly take CATCH as the current level exception handler, rather than $ETRAP. Otherwise, a system error (including a system error thrown by the THROW command) may take the $ETRAP exception handler.

An exception that occurs within a CATCH block is handled by the current error trap handler.

See Also

*   [NEW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cnew) command
    
*   [SET](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cset) command
    
*   [THROW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cthrow) command
    
*   [TRY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ctry) command
    
*   [$ECODE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vecode) special variable
    
*   [$ZEOF](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzeof) special variable
    
*   [$ZTRAP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztrap) special variable
    
*   [Error Handling](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_errors) in Using Caché ObjectScript
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vestack "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vhalt "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_vetrap.xml**