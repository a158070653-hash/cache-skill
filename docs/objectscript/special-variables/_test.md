# $TEST

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_vtest

---

 

Caché ObjectScript Reference

$TEST

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vsystem "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vthis "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Special Variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_VARIABLES "Caché ObjectScript Special Variables") \]  >  \[ [$TEST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vtest "$TEST") \]

Go to: Description Example Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Contains the truth value resulting from the last command using the timeout option.

Synopsis

$TEST
$T

Description

$TEST contains the truth value (1 or 0) resulting from the last command with a timeout. $TEST is set by the following commands, regardless of whether they are entered from programmer mode or encountered in routine code:

*   A timed JOB sets $TEST to 1 if the attempt to start the new job succeeds before the timeout expires. If the timeout expires, $TEST is set to 0.
    
*   A timed LOCK sets $TEST to 1 if the lock attempt succeeds before the timeout expires. If the timeout expires, $TEST is set to 0.
    
*   A timed OPEN sets $TEST to 1 if the open attempt succeeds before the timeout expires. If the timeout expires, $TEST is set to 0.
    
*   A timed READ sets $TEST to 1 if the read completes before the timeout expires. If the timeout expires, $TEST is set to 0.
    

Issuing these commands without a timeout does not set $TEST.

Note:

$TEST is also set by the legacy version of the IF command. It is neither set nor checked by the current block-structured IF command. When the test expression of a legacy IF command is evaluated, $TEST is set equal to the resulting truth value. In other words, if the IF expression tests true, $TEST is set to 1. If it tests false, $TEST is set to 0 (zero).

Setting $TEST

You can use the SET command to set $TEST to a boolean value. A value of 1, or any non-zero numeric value, sets $TEST\=1. A value of 0, or a non-numeric string value, sets $TEST\=0.

$TEST can be set by any command or function that can return a logical condition.

Maintaining $TEST

A successful JOB, LOCK, OPEN, or READ command that did not specify a timeout does not change the existing value of $TEST.

The DO command maintains the value of $TEST when calling a procedure, but not when calling a subroutine. For details, refer to the [DO](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cdo) command.

The ZBREAK command maintains the value of $TEST when calling execute\_code. For details, refer to the [ZBREAK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czbreak) command.

Example

The following code performs a timed read and uses $TEST to test for completion of the read.

   READ !,"Type a letter: ",a#1:10
   IF $TEST { DO Success(a) }
   ELSE { DO TimedOut }
Success(val)
   WRITE !,"Received data: ",val
TimedOut()
   WRITE !,"Timed out"

Notes

Operations That Do Not Set $TEST

JOB, LOCK, OPEN, and READ commands without a timeout have no effect on $TEST. Postconditional expressions also have no effect on $TEST.

The block-oriented IF command (which defines a block of code by enclosing it in curly braces) does not use $TEST in any way. The following invocations of the legacy IF command also do not use $TEST: legacy IF without an argument and the ELSE command have no effect on $TEST.

Unsuccessful Timed Operations

Caché does not produce an error message after an unsuccessful timed operation. Your application must check $TEST and then produce an appropriate message.

See Also

*   [JOB](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cjob) command
    
*   [LOCK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_clock) command
    
*   [OPEN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_copen) command
    
*   [READ](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cread) command
    
*   [IF (legacy version)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cif_legacy) command
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vsystem "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vthis "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_vtest.xml**