# $DATA

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fdata

---

 

Caché ObjectScript Reference

$DATA

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fcompile "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fdecimal "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$DATA](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fdata "$DATA") \]

Go to: Description Parameters Examples Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Checks if a variable contains data.

Synopsis

$DATA(variable,target)
$D(variable,target)

Parameters

variable

The variable whose status is to be checked. A local or global variable, subscripted or unsubscripted. The variable may be undefined. You cannot specify a simple object property reference as variable; you can specify a multidimensional property reference as variable with the syntax obj.property.

target

Optional — A variable into which $DATA returns the current value of variable.

Description

You can use $DATA to test whether a variable contains data before attempting an operation on it. $DATA returns status information about the specified variable. The variable parameter can be the name of any variable (local variable, process-private global, or global), and can include a subscripted array element. It can be a [multidimensional object property](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ROBJ_property_multidimensional); it cannot be a non-multidimensional object property.

The possible status values that may be returned are as follows:

Status Value

Meaning

0

The variable is undefined. Any reference would cause an <UNDEFINED> error.

1

The variable exists and contains data, but has no descendants. Note that the null string ("") qualifies as data.

10

The variable identifies an array element that has descendants (contains a downward pointer to another array element) but does not contain data. Any direct reference to such a variable will result in an <UNDEFINED> error. For example, if y(1) is defined, but y is not, $DATA(y) returns 10, set x=y will produce an <UNDEFINED> error.

11

The variable identifies an array element that has descendants (contains a downward pointer to another array element) and contains data. Variables of this type can be referenced in expressions.

You can use modulo 2 (#2) arithmetic to return a boolean value from $DATA: $DATA(var)#2 returns 0 for the undefined status codes (0 and 10), and returns 1 for the defined status codes (1 and 11).

Status values 1 and 11 indicate only the presence of data, not the type of data.

You can use the [Undefined()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process#Undefined) method of the [%SYSTEM.Process](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process) class to set behavior when encountering an undefined variable. For more information on <UNDEFINED> errors, refer to the [$ZERROR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzerror) special variable.

Parameters

variable

The variable that is being tested for the presence of data:

*   variable can be a local variable, a global variable, or a process-private global (PPG) variable. It can be subscripted or unsubscripted.
    
    If a global variable, it can contain an [extended global reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GGBL_structure). If a subscripted global variable, it can be specified using a [naked global reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GGBL_using). Even when referencing an undefined subscripted global variable, variable resets the naked indicator, affecting future naked global references, as described below.
    
*   variable can be a [multidimensional object property](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ROBJ_property_multidimensional); for example, $DATA(..Count). It cannot be a non-multidimensional object property. Attempting to use $DATA on a non-multidimensional object property results in an <OBJECT DISPATCH> error.
    
    $DATA cannot return a data status value for a Proxy Object property. Caché instead issues a message that the specified property does not exist. This property access limitation is unique to the class [%ZEN.proxyObject](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25ZEN.proxyObject), which is defined in the InterSystems Class Reference.
    
*   If variable is the [^$ROUTINE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_sroutine) structured system variable, the possible returned status values are 1 or 0.
    

target

An optional parameter. Specify the name of a local variable, a process-private global, or a global. This target variable does not need to be defined. If target is specified, $DATA writes the current data value of variable into target. If variable is undefined, the target value remains unchanged.

The ZBREAK command cannot specify the target parameter as a watchpoint.

Examples

This example writes a selected range of records from the ^client array, a sparse array consisting of three levels. The first level contains the client’s name, the second the client’s address, and the third the client’s accounts, account numbers, and balances. A client can have up to four separate accounts. Because ^client is a sparse array there may be undefined elements at any of the three levels. The contents for a typical record might appear as follows:

^client(5) John Jones
^client(5,1) 23 Bay Rd./Boston/MA 02049
^client(5,1,1) Checking/45673/1248.00
^client(5,1,2) Savings/27564/3270.00
^client(5,1,3) Reserve Credit/32456/125.00
^client(5,1,4) Loan/81263/460.00

The code below provides a separate subroutine to handle the output for each of the three array levels. It uses the $DATA function at the start of each subroutine to test the current array element.

The $DATA\=0 test in Level1, Level2, and Level3 tests whether the current array element is undefined. If TRUE, it causes the code to QUIT and revert to the previous level.

The $DATA\=10 test in Level1 and Level2 tests whether the current array element contains a pointer to a subordinate element, but no data. If TRUE, it causes the code to write out a “No Data” message. The code then skips to the FOR loop processing for the next lower level. There is no $DATA\=10 test in Level3 because there are no elements subordinate to this level.

The WRITE commands in Level2 and Level3 use the [$PIECE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fpiece) function to extract the appropriate information from the current array element.

Start  Read !,"Output how many records: ",n
  Read !,"Start with record number: ",s
  For i\=s:1:s+(n1) {
    If $Data(^client(i)) {
      If $Data(^client(i))\=10 {
        Write !," Name: No Data"
      }
      Else {
        Write !," Name: " ,^client(i)
      }
      If $Data(^client(i,1)) {
        If $Data(^client(i,1))\=10 {
                            Write !,"Address: No Data"
        }
        Else {
            Write !,"Address: ",$Piece(^client(i,1),"/",1)
            Write " , ",$Piece(^client(i,1),"/",2)
          Write " , ",$Piece(^client(i,1),"/",3)
        }
      }
      For j\=1:1:4 {
        If $Data(^client(i,1,j)) {
             Write !,"Account: ",$Piece(^client(i,1,j),"/",1)
          Write " #: ",$Piece(^client(i,1,j),"/",2)
          Write " Balance: ",$Piece(^client(i,1,j),"/",3)
        }
      }
    }
  }
  Write !,"Finished."
  Quit

When executed, this code might produce output similar to the following:

Output how many records: 3
Start with record number: 10
Name: Jane Smith
Address: 74 Hilltop Dr., Beverly, MA 01965
Account: Checking #: 34218 Balance: 876.72
Account: Reserve Credit #: 47821 Balance: 1200.00
Name: Thomas Brown
Address: 46 Huron Ave., Medford, MA 02019
Account: Checking #: 59363 Balance: 205.45
Account: Savings #: 41792 Balance: 1560.80
Account: Reserve Credit #: 64218 Balance: 125.52
Name: Sarah Copley
Address: No Data
Account: Checking #: 30021 Balance: 762.28

In the following example, a multidimensional property is used as the variable value. This example returns the names of all defined namespaces to the target parameter:

  SET obj \= ##class(%ResultSet).%New("%SYS.Namespace:List")
  DO obj.Execute()
  WRITE $DATA(obj.Data,targ),!                // returns 0
  SET targ\="blank"
  WHILE targ'="" {
     DO obj.Next()
     WRITE $DATA(obj.Data,targ)               // returns 10
     WRITE " ",$DATA(obj.Data("Nsp"),targ),!  // returns 1
     IF targ'="" {
     WRITE "Namespace: ",targ,! }
     }
   WRITE !,"Done!"

 

A similar program returns the same information using the [$GET](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fget) function.

Notes

Naked Global References

$DATA sets the naked indicator when used with a global variable. The naked indicator is set even if the specified global variable in not defined (Status Value = 0).

Subsequent references to the same global variable can use a naked global reference, as shown in the following example:

   IF $DATA(^A(1,2,3))#2 {
     SET x\=^(3)  }

For further details on using $DATA with global variables and naked global references, see [Using Multidimensional Storage (Globals)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GGBL_using) in Using Caché Globals.

Global References in a Networked Environment

Using $DATA to repeatedly reference a global variable that is not defined (for example, $DATA(^x(1)) where ^x is not defined) always requires a network operation to test if the global is defined on the ECP server.

Using $DATA to repeatedly reference undefined nodes within a defined global variable (for example, $DATA(^x(1)) where any other node in ^x is defined) does not require a network operation once the relevant portion of the global (^x) is in the client cache.

For further details, refer to [Developing Distributed Applications](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GDDM_develop) in the Caché Distributed Data Management Guide.

Functions Related to $DATA

For related information, see $GET and $ORDER. Since $ORDER selects the next element in an array that contains data, it avoids the need to perform $DATA tests when looping through array subscripts.

See Also

*   [KILL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ckill) command
    
*   [SET](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cset) command
    
*   [$GET](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fget) function
    
*   [$ORDER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_forder) function
    
*   [Using Multidimensional Storage (Globals)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GGBL_using) in Using Caché Globals
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fcompile "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fdecimal "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fdata.xml**