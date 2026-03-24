# $ZERROR

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_vzerror

---

 

Caché ObjectScript Reference

$ZERROR

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzeos "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzhorolog "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Special Variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_VARIABLES "Caché ObjectScript Special Variables") \]  >  \[ [$ZERROR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzerror "$ZERROR") \]

Go to: Description Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Contains the name and location of the last error.

Synopsis

$ZERROR
$ZE 

Description

$ZERROR contains the name of the most recent error, the location of the most recent error (where applicable), and (for certain error codes) additional information about what caused the error. $ZERROR always contains the most recent error for the appropriate language mode.

The string contained in $ZERROR can be in any of the following forms:

<error\>
<error\>entryref
<error\> info
<error\>entryref info

<error\>

The error name. The error name is always returned in all capital letters, enclosed in angle brackets. It may contain blank spaces.

entryref

A reference to the line of code in which the error occurred. This consists of the label name and line offset from that label, followed by a ^ and the program name. This entryref follows immediately after the closing angle bracket of the error name. When invoking $ZERROR from Caché Terminal, this entryref information is not meaningful and is therefore not returned.

A reference to the routine most recently loaded into the routine buffer using ZLOAD.

info

Additional information specific to certain error types (see table below). This information is separated from <error\> or <error\>entryref by a blank space. If there are multiple components to info, they are separated by a comma.

For example, a program (named zerrortest) contains the following routine (named ZerrorMain) which attempts to write the contents of fred, an undefined local variable:

ZerrorMain
  TRY {
  SET $ZERROR\=""
  WRITE "$ZERROR = ",$ZERROR,!
  WRITE fred }
  CATCH {
  WRITE "$ZERROR = ",$ZCVT($ZERROR,"O","HTML")
  }

 

In the above example, the first $ZERROR contains a null string (""), because no errors have occurred since $ZERROR was reset to the null string. The attempt to write an undefined variable sets $ZERROR and throws it to the CATCH block. This $ZERROR contains <UNDEFINED>ZerrorMain+4^zerrortest \*fred, specifying the name of the error, the location, and additional information specific to that type of error. In this case, the additional information is the name of the undefined local variable fred; the asterisk prefix indicates that it is a local variable. (Note that $ZCVT($ZERROR,"O","HTML") is used in this example because Caché error names are enclosed in angle brackets and this example is run from a web browser.)

An entryref can appear as follows:

ZerrorMain+4^zerrortest -- 4 line offset from label ZerrorMain in program zerrortest
ZerrorMain^zerrortest -- no offset from label ZerrorMain in program zerrortest; error occurred in the label line
+3^zerrortest -- 3 line offset from beginning of program zerrortest; no label precedes the error line

The maximum length of the $ZERROR value is 512 characters. A value exceeding that length is truncated to 512 characters.

AsSystemError() Method

The [AsSystemError()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25Exception.SystemException#AsSystemError) method of the [%Exception.SystemException](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Exception.SystemException) class returns the same value as $ZERROR. This is shown in the following example:

  TRY {
       KILL mylocal
       WRITE mylocal
      }
  CATCH myerr {
       WRITE "AsSystemError is: ",myerr.AsSystemError(),!
       WRITE "$ZERROR is:       ",$ZERROR
      }

 

AsSystemError() is preferable to $ZERROR in a TRY/CATCH exception handling block structure, because $ZERROR could be overwritten by an error occurring during exception handling.

Additional Information For Some Errors

When certain types of errors occurs, $ZERROR returns the error in the following format:

<ERRORCODE>entryref info

The info component contains additional information about what caused the error. The following table gives a list of errors that include additional info and the format of that information. The error code is separated from the info component by a space character.

Error Code

Info Component

<UNDEFINED>

The name of the undefined variable (including any subscripts used). This may be a local variable, a process-private global, a global, or a multidimensional class property. Local variable names are prefixed by an asterisk. Multidimensional property names start with a period to distinguish them from local variable names.

You can change Caché behavior to not generate an <UNDEFINED> error when referencing an undefined variable by setting the [%SYSTEM.Process.Undefined()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process#Undefined) method.

<SUBSCRIPT>

The subscript reference in error: the line reference (routine and line offset) that generated the error, the subscripted variable, and which subscript level is in error. For a [Structured System Variable (SSVN)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_SVARIABLES), only the the line reference (routine and line offset) is provided.

<NOROUTINE>

Prefixed by an asterisk, the referenced routine name.

<CLASS DOES NOT EXIST>

Prefixed by an asterisk, the referenced class name.

<PROPERTY DOES NOT EXIST>

Prefixed by an asterisk, the name of the referenced property, followed by a comma separator and the class name it is supposed to be in.

<METHOD DOES NOT EXIST>

Prefixed by an asterisk, the name of the method invoked, followed by a comma separator and the class name it is supposed to be in.

<PROTECT>

The name of the global referenced and the name of the directory containing it, separated by a comma.

<THROW>

Prefixed by an asterisk, the object name, followed by the value returned by the DisplayString() method.

<COMMAND>

When invoking TCOMMIT when not in a transaction, the info component is \*NoTransaction.

When invoking a user-defined function that does not return a value, the info component is a message that includes the location of the command that should have returned the value.

<DIRECTORY>

Prefixed by an asterisk, the full pathname of the invalid directory.

<FRAMESTACK>

When a <FRAMESTACK> error terminates a process, the <FRAMESTACK> error with additional information is written as a message to mgr/cconsole.log. The informational message shows the process id (pid) of the terminated process and the line reference (routine and line offset) that generated the error. For example: (pid) 0 <FRAMESTACK> at +13^|"USER"|mytest

The names of variables local to routines (or methods), as well as the names of undefined routines, classes, properties, and methods, are prefixed with an asterisk (\*). Process-private globals are identified by their ^|| prefix. Global variables are identified by their ^ (caret) prefix. Class names are presented in their %-prefix form.

The following examples show additional error information specifying the cause of the error. In each case, the specified item does not exist. Note that the info component of the generated error is separated from the error name by a blank space. The asterisk (\*) indicates a local variable, a class, a property, or a method. The caret (^) indicates a global, and ^|| indicates a process-private global.

Examples of <UNDEFINED> errors:

UndefTest ;
   ZNSPACE "SAMPLES"
   KILL x,abc(2)
   KILL ^xyz(1,1),^|"USER"|xyz(1,2)
   KILL ^||ppg(1),^||ppg(2)
   TRY {WRITE x }             // undefined local variable
     CATCH {WRITE $ZERROR,! }                           
   TRY {WRITE abc(2)}         // undefined subscripted local variable
     CATCH {WRITE $ZERROR,! }
   TRY {WRITE ^xyz(1,1) }          // undefined global
     CATCH {WRITE $ZERROR,! }  
   TRY {WRITE ^|"USER"|xyz(1,2) }  // undefined global in another namespace
     CATCH {WRITE $ZERROR,! }
   TRY {WRITE ^||ppg(1) }     // undefined process-private global
     CATCH {WRITE $ZERROR,! }
   TRY {WRITE ^|"^"|ppg(2) }  // undefined process-private global
     CATCH {WRITE $ZERROR,! }
     
<UNDEFINED>UndefTest+5^MyProg \*x
<UNDEFINED>UndefTest+7^MyProg \*abc(2)
<UNDEFINED>UndefTest+9^MyProg ^xyz(1,1)
<UNDEFINED>UndefTest+11^MyProg ^xyz(1,2)
<UNDEFINED>UndefTest+13^MyProg ^||ppg(1)
<UNDEFINED>UndefTest+15^MyProg ^||ppg(2)

Examples of <SUBSCRIPT> errors:

SubscriptTest ;
   DO $SYSTEM.Process.NullSubscripts(0)
   KILL abc
   TRY {SET x=abc("")}
   CATCH {WRITE $ZERROR,! }
   TRY {SET xyz($JUSTIFY("",1000))=1}
   CATCH {WRITE $ZERROR,! }

<SUBSCRIPT>SubscriptTest+3^MyProg \*abc() Subscript 1 is ""
<SUBSCRIPT>SubscriptTest+5^MyProg \*xyz() Encoded subscript 1 > 511 chars

Examples of <NOROUTINE> errors:

NoRoutineTest ;
   KILL ^NotThere
   TRY {DO ^NotThere }
     CATCH {WRITE $ZERROR,! }
   TRY {JOB ^NotThere }
     CATCH {WRITE $ZERROR,! }
   TRY {GOTO ^NotThere }
     CATCH {WRITE $ZERROR,! }

<NOROUTINE>NoRoutineTest+2^MyProg \*NotThere
<NOROUTINE>NoRoutineTest+4^MyProg \*NotThere
<NOROUTINE>NoRoutineTest+6^MyProg \*NotThere 

Examples of object errors:

WRITE $SYSTEM.XXQL.MyMethod()
<CLASS DOES NOT EXIST> \*%SYSTEM.XXQL

DO $SYSTEM.SQL.MyMethod()
<METHOD DOES NOT EXIST> \*MyMethod,%SYSTEM.SQL

SET x=##class(%SQL.Statement).%New()
WRITE x.MyProp
<PROPERTY DOES NOT EXIST> \*MyProp,%SQL.Statement

Example of <PROTECT> error (on Windows):

   // user does not have access privileges for %SYS namespace
   SET x=^|"%SYS"|var
  <PROTECT> ^var,c:\\intersystems\\cache\\mgr\\

Example of a <COMMAND> error when invoking a user-defined function. In this example, the MyFunc QUIT command does not return a value. This generates a <COMMAND> error with the entryref specifying the location of the call to $$MyFunc, and the info message specifying the location of the QUIT command:

Main 
   TRY {
     KILL x
     SET x\=$$MyFunc(7,10)
     WRITE "returned value is ",x,!
     RETURN
   }
   CATCH {  WRITE "$ZERROR = ",$ZCVT($ZERROR,"O","HTML"),!
   }
MyFunc(a,b)
   SET c\=a+b
   QUIT

 

The same <COMMAND> error when invoking the function as a procedure with the PUBLIC keyword:

Main 
   TRY {
     KILL x
     SET x\=$$MyFunc(7,10)
     WRITE "returned value is ",x,!
     RETURN
   }
   CATCH {  WRITE "$ZERROR = ",$ZCVT($ZERROR,"O","HTML"),! 
   }
MyFunc(a,b) PUBLIC {
   SET c\=a+b
   QUIT }

 

Example of <DIRECTORY> error (on Windows):

  TRY { SET prev\=$SYSTEM.Process.CurrentDirectory("bogusdir")
        WRITE "previous directory: ",prev,!
        RETURN }
  CATCH { WRITE "$ZERROR = ",$ZCVT($ZERROR,"O","HTML"),! 
          QUIT }

 

Pre-5.1 Error Handling Code

A consequence of adding an info component to these error codes for Caché 5.1 and subsequent releases is that pre-5.1 error handling routines that made assumptions about the format of the string in $ZERROR may require redesign to work as before. For example, the following will no longer work in version 5.1:

  WRITE "Error line: ", $PIECE($ZERROR, ">", 2)

and should be changed to be something like:

  WRITE "Error line: ", $PIECE($PIECE($ZERROR, ">", 2), " ", 1)

Notes

ZLOAD and Error Messages

Following a ZLOAD operation, the name of the routine loaded into the routine buffer appears in the entryref portion of subsequent error messages. This persists for the duration of the process, or until removed using ZREMOVE, or removed or replaced by another ZLOAD. The following example shows this display of the contents of the routine buffer:

SAMPLES>ZLOAD Sample.Person.1
SAMPLES>WRITE 6/0
<DIVIDE>^Sample.Person.1
SAMPLES>WRITE fred
<UNDEFINED>^Sample.Person.1 \*fred
SAMPLES>WRITE ^fred
<UNDEFINED>^Sample.Person.1 ^fred
SAMPLES>ZNAME "USER"
USER>WRITE 7/0
<DIVIDE>^Sample.Person.1
USER>ZREMOVE
USER>WRITE ^fred
<UNDEFINED> ^fred

$ZERROR and the Program Stack

The <error> portion of the $ZERROR string contains the most recent error message. The contents of the entryref portion of the $ZERROR string reflect the stack level of the most recent error. The following terminal session attempts to call the nonsense command GOBBLEDEGOOK, resulting in a <SYNTAX> error. It also runs ZerrorMain (specified above), resulting in the $ZERROR value <UNDEFINED>. Subsequent $ZERROR values during this terminal session reflect this program call, as shown in the following:

SAMPLES>gobbledegook
SAMPLES>WRITE $ZERROR
<SYNTAX>
SAMPLES>DO ^zerrortest
SAMPLES>WRITE $ZERROR
<UNDEFINED>ZerrorMain+2^zerrortest \*FRED
SAMPLES 2d0>gobbledegook
SAMPLES 2d0>WRITE $ZERROR
<SYNTAX>^zerrortest
SAMPLES 2d0>QUIT
SAMPLES>WRITE $ZERROR
<SYNTAX>^zerrortest
SAMPLES>gobbledegook
SAMPLES>WRITE $ZERROR
<SYNTAX>

$ZERROR Actions when $ZTRAP is Set

When an error occurs and $ZTRAP is set, Caché returns the error message in $ZERROR and branches to the error-trap handler specified for $ZTRAP. (For a list of the possible error texts, refer to [System Error Messages](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RERR_system) in Caché Error Reference.)

Setting $ZERROR

You can set $ZERROR with the SET command to a value of up to 512 characters only in Caché mode. Values longer than 512 characters are truncated to 512.

Setting $ZERROR to the null string ("") is a useful for resetting it following an error.

See Also

*   [CATCH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ccatch) command
    
*   [ZTRAP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cztrap) command
    
*   [$ECODE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vecode) special variable
    
*   [$ZTRAP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztrap) special variable
    
*   [Error Handling](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_errors) chapter in Using Caché ObjectScript
    
*   [System Error Messages](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RERR_system) in Caché Error Reference
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzeos "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzhorolog "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_vzerror.xml**