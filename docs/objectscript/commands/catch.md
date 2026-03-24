# CATCH

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_ccatch

---

 

Caché ObjectScript Reference

CATCH

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cbreak "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cclose "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_COMMANDS "Caché ObjectScript Commands") \]  >  \[ [CATCH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ccatch "CATCH") \]

Go to: Description Argument Examples: System Exceptions Examples: Thrown Exceptions Example: Nested TRY/CATCH See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Identifies a block of code to execute when an exception occurs.

Synopsis

CATCH exceptionvar
{
   . . .
}

Argument

exceptionvar

Optional — An exception variable. Specified as a local variable, with or without subscripts, that receives a reference to a Caché Object (an oref). This argument can, optionally, be enclosed with parentheses.

Description

The CATCH command defines an exception handler, a block of code to execute when an exception occurs in a TRY block of code. The CATCH command is followed by a block of code statements, enclosed in curly braces.

If you specify a TRY block, a CATCH block is required; every TRY block must have a corresponding CATCH block. Only one CATCH block is permitted for each TRY block. The CATCH block must immediately follow its TRY block. No lines of executable code are permitted between a TRY block and its CATCH block. No label is permitted between a TRY block and its CATCH block, or on the same line as the CATCH command. You can, however, include comments between a TRY block and its CATCH block.

A CATCH block is entered when an exception occurs. If no exception occurs, the CATCH block should not be executed. You should never use a GOTO statement to enter a CATCH block.

You can exit a CATCH block using [QUIT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cquit) or [RETURN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_creturn). QUIT exits the current block structure and continues execution with the next command outside of that block structure. For example, if you are within a nested CATCH block, issuing a QUIT exits that CATCH block to the enclosing block structure. You cannot use an argumented QUIT to exit a CATCH block; attempted to do so results in a compile error. To exit a routine completely from within a CATCH block, issue a RETURN statement.

The CATCH command has two forms:

*   Without an argument
    
*   With an argument
    

The argumented form is preferred.

CATCH Exception Handling

CATCH exceptionvar receives an object instance reference (oref) from the TRY block, either explicitly passed by the THROW command, or implicitly from the system runtime environment in the event of a system error. This Object provides properties that contain information about the exception.

An exception can pass four exception properties to CATCH. These are, in order: Name, Code, Location, and Data. A thrown exception cannot pass a Location parameter. You can use the %IsA() instance method to determine what type of exception passed in these properties.

In the following example, the TRY block can generate a [system exception](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RERR_system) (undefined local variable), throw an [SQL exception](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RERR_sql), throw a [status exception](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RERR_gen), or throw a [general exception](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RERR_system). This general-purpose CATCH exception handler determines which type of exception occurred and displays the appropriate properties. It displays all four properties for a system exception (the Data property is the empty string for some types of system errors). It displays two properties for an SQL exception (Code and Data). It supplies two properties to $SYSTEM.Status.Error() to generate an error message string for a Status exception. It displays three properties for a general ObjectScript exception (Name, Code, and Data). It uses the $ZCVT function to format a Name value containing angle brackets for browser display:

  TRY {
    SET x\=$RANDOM(4)
    IF x\=0 { KILL undefvar
             WRITE undefvar }
    ELSEIF x\=1 { 
       SET oref\=##class(%Exception.SQL).%New(,"-999",,"SQL error message")
       THROW oref }
    ELSEIF x\=2 { 
       SET oref\=##class(%Exception.StatusException).%New(,"5002",,$LISTBUILD("My Status Error"))
       THROW oref }
    ELSE {
       SET oref\=##class(%Exception.General).%New("<MY BAD>","999",,"General error message") 
       THROW oref }
    WRITE "this should not display",!
  }
  CATCH exp { WRITE "In the CATCH block",!
                IF 1\=exp.%IsA("%Exception.SystemException") {
                  WRITE "System exception",!
                  WRITE "Name: ",$ZCVT(exp.Name,"O","HTML"),!
                  WRITE "Location: ",exp.Location,!
                  WRITE "Code: "
                }
                ELSEIF 1\=exp.%IsA("%Exception.SQL") {
                  WRITE "SQL exception",!
                  WRITE "SQLCODE: "
                }
                ELSEIF 1\=exp.%IsA("%Exception.StatusException") {
                  WRITE "Status exception",!
                  DO $SYSTEM.Status.DisplayError($SYSTEM.Status.Error(exp.Code,exp.Data))
                  RETURN
                }
                ELSEIF 1\=exp.%IsA("%Exception.General") {
                  WRITE "General ObjectScript exception",!
                  WRITE "Name: ",$ZCVT(exp.Name,"O","HTML"),!
                  WRITE "Code: "
                }
                ELSE { WRITE "Some other type of exception",! RETURN }
              WRITE exp.Code,!
              WRITE "Data: ",exp.Data,! 
              RETURN
  }

 

Nested TRY/CATCH Blocks

Only one CATCH block is permitted for each TRY block. However, it is possible to nest paired TRY/CATCH blocks.

You can nest an inner TRY/CATCH pair within an outer CATCH block, such as the following:

  TRY {
       /\* TRY code \*/
  }
  CATCH exvar1 {
      /\* CATCH code \*/
       TRY {
           /\* nested TRY code \*/
       }
       CATCH exvar2 {
          /\* nested CATCH code \*/
       }
  }

You can nest an inner TRY/CATCH pair within an outer TRY block, such as the following:

  TRY {
       /\* TRY code \*/
       TRY {
           /\* nested TRY code \*/
       }
       CATCH exvar2 {
          /\* nested CATCH code \*/
       }
  }
  CATCH exvar1 {
      /\* CATCH code \*/
  }

Execution Stack

The %Exception object contains the execution stack at the time the object was created. You can access this execution stack using the [StackAsArray()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25Exception.AbstractException#StackAsArray) method. The following example shows this execution stack:

  TRY {
      WRITE "In the TRY block",!
      WRITE 7/0
  }
  CATCH exobj {
       WRITE "In the CATCH block",!
       WRITE $ZCVT($ZERROR,"O","HTML"),!
       TRY {
            WRITE "In the nested TRY block",!
            KILL fred
            WRITE fred
       }
       CATCH exobj2 {
          WRITE "In the nested CATCH block",!
          WRITE $ZCVT($ZERROR,"O","HTML"),!!
          WRITE "The Execution Stack",!
          DO exobj2.StackAsArray(.stk)
          ZWRITE stk
       }
  }

 

CATCH and $ZTRAP, $ETRAP

You cannot set $ZTRAP or $ETRAP within a TRY block. However, you can set $ZTRAP or $ETRAP within a CATCH block. You can also set $ZTRAP or $ETRAP before entering the TRY block.

If an exception occurs within the CATCH block, the specified $ZTRAP or $ETRAP exception handler is taken.

TRY / CATCH Loop

A loop where a TRY block invokes a CATCH block that loops back to the TRY block does not loop infinitely. It eventually issues a <FRAMESTACK> error.

Disabling CATCH

Issuing a ZBREAK /ERRORTRAP:OFF command disables CATCH exception handling.

Argument

exceptionvar

A local variable, used to receive the exception object reference from the THROW command or from the system runtime environment in the event of a system error. When a system error occurs, exceptionvar receives a reference to an object of type [%Exception.SystemException](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Exception.SystemException). When a user-specified error occurs, exceptionvar receives a reference to an object of type [%Exception.General](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Exception.General), [%Exception.StatusException](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Exception.StatusException), or [%Exception.SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Exception.SQL). For further details, refer to the [%Exception.AbstractException](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Exception.AbstractException) abstract class in the InterSystems Class Reference.

The exceptionvar argument can optionally be enclosed with parentheses, thus: CATCH(var) { code block }. This parentheses syntax is provided for compatibility, and has no effect on functionality.

Examples: System Exceptions

The following example shows an argumentless CATCH invoked by a divide-by-zero runtime error. It displays the $ZERROR and $ECODE error values. Argumentless CATCH is not recommended because it is less reliable than passing an exceptionvar. If an error occurs in the CATCH block, $ZERROR will contain this most recent error, not the error that invoked the CATCH. In this example the QUIT command exits the CATCH block, but does not prevent “fall-through” to the next line outside the block structure:

  TRY {
    WRITE !,"TRY block about to divide by zero",!!
    SET a\=7/0
    WRITE !,"this should not display"
  }
  CATCH {
      WRITE "CATCH block exception handler",!!
      WRITE "$ZERROR is: ",$ZERROR,!
      WRITE "$ECODE is :",$ECODE,!
      QUIT
      WRITE !,"this should not display"
  }
  WRITE !,"this is where the code falls through"

 

The following example shows a CATCH invoked by a divide-by-zero runtime error and receiving an argument. This is the preferred coding practice. The myexp oref argument receives a system-generated exception object. It displays the Name, Code, and Location properties of this exception instance. In this example the RETURN command exits the program, so no “fall-through” occurs:

  TRY {
    WRITE !,"TRY block about to divide by zero",!!
    SET a\=7/0
    WRITE !,"this should not display"
  }
  CATCH myexp {
      WRITE "CATCH block exception handler",!!
      WRITE "Name: ",$ZCVT(myexp.Name,"O","HTML"),!
      WRITE "Code: ",myexp.Code,!
      WRITE "Location: ",myexp.Location,!
      RETURN
  }
  WRITE !,"this is where the code falls through"

 

The following example shows a CATCH receiving a system exception object. The CATCH block code displays the system exception as a $ZERROR\-formatted string using the [AsSystemError()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25Exception.SystemException#AsSystemError) method of the [%Exception.SystemException](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Exception.SystemException) class. ($ZERROR is also displayed, for comparison purposes.) This CATCH block then displays the error name, error code, error data, and error location as separate properties:

  TRY {
    WRITE !,"this global is not defined",!
    SET a\=^badglobal(1)
    WRITE !,"this should not display"
  }
  CATCH myvar {
      WRITE !,"this is the exception handler",!
      WRITE "AsSystemError is: ",myvar.AsSystemError(),!
      WRITE "$ZERROR is:       ",$ZERROR,!!
      WRITE "Error name=",$ZCVT(myvar.Name,"O","HTML"),!
      WRITE "Error code=",myvar.Code,!
      WRITE "Error data=",myvar.Data,!
      WRITE "Error location=",myvar.Location,!
      RETURN
  }

 

Examples: Thrown Exceptions

The following example shows a CATCH invoked by the THROW command. The myvar argument receives a user-defined exception object with four properties. Note that in this example the THROW does not supply a value for the omitted Location property of the [%Exception.General](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Exception.General) class:

  TRY {
    SET total\=1234
    WRITE !,"Throw an exception!"
    THROW ##class(%Exception.General).%New("Example Error",999,,"MyThrow")
    WRITE !,"this should not display"
  }
  CATCH myvar {
      WRITE !!,"this is the exception handler"
      WRITE !,"Error data=",myvar.Data
      WRITE !,"Error code=",myvar.Code
      WRITE !,"Error name=",myvar.Name
      WRITE !,"Error location=",myvar.Location,!
      RETURN
  }

 

The following two examples generate birth dates in the TRY block. If they generate a birth date that is in the future, they use THROW to issue a general exception, passing the user-defined exception to the CATCH block. (You may have to run these examples more than once to generate a date that throws an exception.)

The first of these examples does not specify a CATCH exceptionvar. It uses the oref name defined in the TRY block to specify the exception properties:

  TRY {
      WRITE "In the TRY block",!
      SET badDOB\=##class(%Exception.General).%New("BadDOB","999",,"Birth date is in the future")
      FOR x\=1:1:20 { SET rndDOB \= $RANDOM(7)\_$RANDOM(10000)
        IF rndDOB \> $HOROLOG { THROW badDOB }
        ELSE { WRITE "Birthdate ",$ZDATE(rndDOB,1,,4)," is valid",! }
     }
  }
  CATCH {
        WRITE !,"In the CATCH block"
        WRITE !,"Birthdate ",$ZDATE(rndDOB,1,,4)," is invalid"
        WRITE !,"Error code=",badDOB.Code
        WRITE !,"Error name=",badDOB.Name
        WRITE !,"Error data=",badDOB.Data
        RETURN
  }

 

The second of these examples specifies a CATCH exceptionvar. It uses this renamed oref to specify the exception properties. This is the preferred usage:

  TRY {
      WRITE "In the TRY block",!
      SET badDOB\=##class(%Exception.General).%New("BadDOB","999",,"Birth date is in the future")
      FOR x\=1:1:20 { SET rndDOB \= $RANDOM(7)\_$RANDOM(10000)
        IF rndDOB \> $HOROLOG { THROW badDOB }
        ELSE { WRITE "Birthdate ",$ZDATE(rndDOB,1,,4)," is valid",! }
     }
  }
  CATCH err {
        WRITE !,"In the CATCH block"
        WRITE !,"Birthdate ",$ZDATE(rndDOB,1,,4)," is invalid"
        WRITE !,"Error code=",err.Code
        WRITE !,"Error name=",err.Name
        WRITE !,"Error data=",err.Data
        RETURN
  }

 

Example: Nested TRY/CATCH

The following example shows a CATCH invoked by a divide-by-zero runtime error. The CATCH block contains an inner TRY block paired with an inner CATCH block. This inner CATCH block is invoked by a thrown exception. For the purposes of demonstration, this THROW is invoked randomly in this program. In a real-world program, the inner CATCH block would be invoked by an exception test, such as a mismatch between AsSystemError() (the caught error) and $ZERROR (the most-recent error):

  TRY {
    WRITE !,"Outer TRY block",!!
    SET a\=7/0
    WRITE !,"this should not display"
  }
  CATCH myexp {
      WRITE "Outer CATCH block",!
      WRITE "Name: ",$ZCVT(myexp.Name,"O","HTML"),!
      WRITE "Code: ",myexp.Code,!
      WRITE "Location: ",myexp.Location,!
      SET rndm\=$RANDOM(2)
      IF rndm\=1 {RETURN }
    TRY {
      WRITE !,"Inner TRY block",!
      SET oref\=##class(%Exception.General).%New("<MY BAD>","999",,"General error message") 
         THROW oref
      RETURN
    }
    CATCH myexp2 {
                  WRITE !,"Inner CATCH block",!
                  IF 1\=myexp2.%IsA("%Exception.General") {
                    WRITE "General ObjectScript exception",!
                    WRITE "Name: ",$ZCVT(myexp2.Name,"O","HTML"),!
                    WRITE "Code: ",myexp2.Code,!
                  }
                  ELSE { WRITE "Some other type of exception",! }
                  QUIT
     }
     WRITE !,"back to Outer CATCH block",!
     RETURN
  }

 

See Also

*   [THROW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cthrow) command
    
*   [TRY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ctry) command
    
*   [ZBREAK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czbreak) command
    
*   [Error Processing](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_errors) in Using Caché ObjectScript
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cbreak "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cclose "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_ccatch.xml**