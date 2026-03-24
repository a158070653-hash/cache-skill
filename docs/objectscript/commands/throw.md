# THROW

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_cthrow

---

 

Caché ObjectScript Reference

THROW

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ctcommit "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ctrollback "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_COMMANDS "Caché ObjectScript Commands") \]  >  \[ [THROW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cthrow "THROW") \]

Go to: Description Argument THROW from a TRY Block Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Explicitly throws an exception to the next exception handler.

Synopsis

THROW oref

Argument

oref

Optional — An object reference that is thrown to an exception handler. Optional, but highly recommended.

Description

The THROW command explicitly throws an exception. An exception can be a system error, a status exception, or a user-defined exception. It throws this exception as an object reference (oref) that inherits from the [%Exception.AbstractException](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Exception.AbstractException) object. The THROW command throws this exception to the next exception handler.

There are two ways to use THROW oref:

*   TRY/CATCH: Use THROW oref to explicitly signal an exception from within a TRY block of code, transferring execution from the TRY block to its corresponding CATCH block exception handler.
    
*   Other Exception Handlers: Use THROW oref to explicitly signal an exception when not in a TRY block. This triggers the current exception handler (for example, $ZTRAP), where the oref can be retrieved from the [$THROWOBJ](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vthrowobj) special variable.
    

Note:

Use of THROW [without an argument](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cthrow#RCOS_cthrow_noarg) is deprecated and not recommended for new code.

System Errors

Caché issues a system error when a runtime error occurs, such a referencing an undefined variable. A system error generates a [%Exception.SystemException](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Exception.SystemException) object reference, sets the oref properties Code, Name, Location, and Data, and also sets the $ZERROR and $ECODE special variables, and transfers control to the next error handler. This error handler can be a CATCH exception handler, or a $ZTRAP or $ETRAP error handler. A system error is an implicit error, which does not use a THROW.

You can use THROW within an error handler to throw a system error object to a further error handler. This is known as re-signalling a system error.

A THROW passes control up the execution stack to the next error handler. If the exception is an [%Exception.SystemException](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Exception.SystemException) object, the next error handler can be any type (CATCH, $ZTRAP, or $ETRAP). Otherwise, there must be a CATCH to handle the exception or Caché generates a <THROW> error.

Argument

oref

A reference to an exception object, which is an instance of any class that inherits from [%Exception.AbstractException](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Exception.AbstractException). A exception object for a system error is an instance of the class [%Exception.SystemException](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Exception.SystemException). A user-specified exception object can be a status exception object ([%Exception.StatusException](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Exception.StatusException)), a general exception object (%Exception.General), or SQL exception object ([%Exception.SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Exception.SQL)). The creation and population of a user exception object is the responsibility of the programmer.

THROW from a TRY Block

THROW oref can be issued from a TRY block to its corresponding CATCH block. This explicitly signals a user-defined exception. This transfers execution from a TRY block to its corresponding CATCH block. The thrown oref is set as the CATCH block’s exceptionvar argument.

To issue an argumented THROW from a CATCH exception handler, you can either throw to a non-CATCH exception handler, or you can nest a TRY block (and associated nested CATCH block) within the CATCH exception handler, and issue the THROW from this nested TRY block.

Status Exceptions and User-Defined Exceptions

To trap a status exception or a user-defined exception, specify an object based on the [%Exception.AbstractException](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Exception.AbstractException) object as the oref argument. Define an exception class, then create an instance of the class using %New() and supply the exception information. These types of exceptions must be handled by a CATCH exception handler. If no CATCH exists, Caché generates a <THROW> error.

A user-defined exception does not change the value of $ZERROR or $ECODE. In order to use either of these special variables, your program must explicitly set them using the SET command.

General Exception

Caché supplies a general exception that you can supply as the THROW argument. This is the [%Exception.General](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Exception.General) subclass of the [%Exception.AbstractException](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Exception.AbstractException) abstract class. Its use is shown in the following example:

  TRY {
       WRITE "In the TRY block",!!
       SET mygenex \= ##class(%Exception.General).%New("My exception","999",,
                             "My own special exception")
       THROW mygenex
       WRITE "This shouldn't display",!
      }
  CATCH stuff {
       WRITE "In the CATCH block",!
       WRITE stuff.Name,!
       WRITE stuff.Code,!
       WRITE stuff.Data,!
       WRITE "End of the CATCH block",!
       RETURN
      }

 

THROW when not in a TRY Block

If you issue a THROW outside of a TRY block, Cache generates a <THROW> error, such as the following: <THROW>+3^myprog \*%Exception.General MyErr 999 My user-definied error. This use of THROW is useful for re-signalling an error.

The object reference (oref) specified in the THROW is stored in the $THROWOBJ special variable. For example, 9@%Exception.General. The $THROWOBJ value is cleared by the next successful THROW operation, or by SET $THROWOBJ="".

In the following example, a THROW throws an exception to a $ZTRAP exception handler:

MainRou
       WRITE "In the Main Routine",!!
       SET $ZTRAP\=^ErrRou
       SET mygenex \= ##class(%Exception.General).%New("My exception","999",,
                             "My own special exception")
       THROW mygenex
       WRITE "This shouldn't display",!
       RETURN

ErrRou
       WRITE "In $ZTRAP",!
       SET oref\=$THROWOBJ
       SET $THROWOBJ\=""
       WRITE oref.Name,!
       WRITE oref.Code,!
       WRITE oref.Data,!
       WRITE "End of $ZTRAP",!
       RETURN

THROW without an Argument

Argumentless THROW re-signals the current system error, transferring control to the next exception handler. The current system error is the error referenced by the $ZERROR special variable. Thus, an argumentless THROW is equivalent to the command ZTRAP $ZERROR.

Use of argumentless THROW is not recommended, because which system error is the current system error may change. For instance, this would occur if the error handler changes the $ZERROR value, or if the error handler itself generates a system error. It is therefore preferable to explicitly specify the system error to be thrown to the next exception handler by using THROW oref.

Examples

Note that $ZCVT(myerr.Name,"O","HTML") is used in these examples because Caché error names are enclosed in angle brackets and these examples are run from a web browser. In most other contexts, myerr.Name will return the desired value.

The following example uses an instance of the [%Exception.General](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Exception.General) class to throw a user-defined exception. It generates birth dates in the TRY block; if it generates a birth date that is in the future, it uses THROW to issue a general exception, passing the oref of the user-defined exception to a general-purpose CATCH block. (You may have to run this example more than once to generate a date that throws an exception):

  TRY {
      WRITE "In the TRY block",!
      SET badDOB\=##class(%Exception.General).%New("<BAD DOB>","999",,"Birth date is in the future")
      FOR x\=1:1:20 { SET rndDOB \= $RANDOM(7)\_$RANDOM(10000)
        IF rndDOB \> $HOROLOG { WRITE !,"Birthdate ",$ZDATE(rndDOB,1,,4)," is invalid"
                               THROW badDOB }
        ELSE { WRITE "Birthdate ",$ZDATE(rndDOB,1,,4)," is valid",! }
     }
  }
  CATCH err {
        WRITE !,"In the CATCH block"
        WRITE !,"Error code=",err.Code
        WRITE !,"Error name=",$ZCVT(err.Name,"O","HTML")
        WRITE !,"Error data=",err.Data
        RETURN
  }

 

The following example can issue either of two THROW commands with a user-defined argument. $RANDOM picks which THROW to issue (random values 0 or 1) or to not issue a THROW (random value 2). Note that code execution continues after the TRY / CATCH block pair, unless block execution ends with a RETURN command:

  TRY {
     SET errdatazero\="this is the zero error"
     SET errdataone\="this is the one error"
     /\* Error Randomizer \*/
        SET test\=$RANDOM(3)
        WRITE "Error test is ",test,!
     IF test\=0 { 
       WRITE !,"Throwing exception 998",!
       THROW ##class(Sample.MyException).%New("TestZeroError",998,,errdatazero)
         THROW myvar
       }
     ELSEIF test\=1 { 
       WRITE !,"Throwing exception 999",!
       THROW ##class(Sample.MyException).%New("TestOneError",999,,errdataone)
       }
     ELSE { WRITE !,"No THROW error this time" }
  }
  CATCH exp {
      WRITE !,"This is the exception handler"
      WRITE !,"Error code=",exp.Code
      WRITE !,"Error name=",exp.Name
      WRITE !,"Error data=",exp.Data
      RETURN
  }
  WRITE !!,"Execution after TRY block continues here"

 

The following example shows the use of THROW with a system error. THROW is commonly used in a CATCH exception handler to forward the system error to another handler. This may occur when the system error received is an unexpected type of system error. Note that this requires nesting a TRY block (and corresponding CATCH block) within the CATCH block. It is used to THROW the system error to the nested CATCH block. This is shown in the following example, which calls Calculate to perform a division operation and return the answer. There are three possible outcomes: If y = any non-zero number, the division operation succeeds and no CATCH block code is executed. If y\=0 (or any nonnumeric string), the division operation attempts to divide by zero, throwing a system error to its CATCH block; this is caught by the calcerr exception handler, which “corrects” this error and returns a value of 0. If, however, y is not defined (NEW y), calcerr catches an unexpected system error, and throws this error to the myerr exception handler. To demonstrate these three possible outcomes, this sample program uses $RANDOM to set the divisor (y):

Randomizer
  SET test\=$RANDOM(3)
  IF test\=0 { SET y\=0 }
  ELSEIF test\=1 { SET y\=7 }
  ELSEIF test\=2 { NEW y }
  /\* Note: if test=2, y is undefined \*/
Main
  SET x\=4
  TRY {
    SET result\=$$Calculate(x,y)
    WRITE !,"Calculated value=",result
  }
  CATCH myerr {  
    WRITE !,"this is the exception handler"
    WRITE !,"Error code=",myerr.Code
    WRITE !,"Error name=",$ZCVT(myerr.Name,"O","HTML")
    WRITE !,"Error data=",myerr.Data
  }
  QUIT

Calculate(arg1,arg2) PUBLIC {
  TRY {
  SET answer\=arg1/arg2
  }
  CATCH calcerr {
      WRITE "In the CATCH Block",!
      TRY {
          IF calcerr.Name\="<DIVIDE>" {
          WRITE !,"handling zero divide error"
          SET answer\=0 }
          ELSE { THROW calcerr }
          RETURN
      }
      CATCH {
          WRITE "Unexpected error",!
          WRITE "Error name=",$ZCVT(myerr.Name,"O","HTML"),!
      }
  }
  QUIT answer
}

 

See Also

*   [CATCH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ccatch) command
    
*   [TRY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ctry) command
    
*   [ZTRAP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cztrap) command
    
*   [$ETRAP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vetrap) special variable
    
*   [$THROWOBJ](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vthrowobj) special variable
    
*   [$ZERROR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzerror) special variable
    
*   [$ZTRAP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vztrap) special variable
    
*   [Error Processing](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_errors) in Using Caché ObjectScript
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ctcommit "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ctrollback "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_cthrow.xml**