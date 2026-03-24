# $GET

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fget

---

 

Caché ObjectScript Reference

$GET

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ffnumber "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fincrement "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$GET](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fget "$GET") \]

Go to: Description Parameters Examples Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Returns the data value of a specified variable.

Synopsis

$GET(variable,default)
$G(variable,default)

Parameters

variable

A local or global variable, subscripted or unsubscripted. The variable may be undefined. variable may be specified as a multidimensional object property with the syntax obj.property.

default

Optional — The value to be returned if the variable is undefined. If a variable, it must be defined.

Description

$GET returns the data value of a specified variable. The handling of undefined variables depends on whether you specify a default parameter.

*   $GET(variable) returns the value of the specified variable, or the null string if the variable is undefined. The variable parameter value can be the name of any variable, including a subscripted array element (either local or global).
    
*   $GET(variable,default) provides a default value to return if the variable is undefined. If the variable is defined, $GET returns its value.
    

Parameters

variable

The variable whose data value is to be returned.

*   variable can be a local variable, a global variable, or a process-private global (PPG) variable. It can be subscripted or unsubscripted.
    
    The variable does not need to be a defined variable. $GET returns the null string for an undefined variable; it does not define the variable. A variable can be defined and set to the null string (""). If a global variable, it can contain an [extended global reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GGBL_structure). If a subscripted global variable, it can be specified using a [naked global reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GGBL_using). Even when referencing an undefined subscripted global variable, variable resets the naked indicator, affecting future naked global references, as described below.
    
*   variable can be a [multidimensional object property](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ROBJ_property_multidimensional); it cannot be a non-multidimensional object property. Attempting to use $GET on a non-multidimensional object property results in an <OBJECT DISPATCH> error.
    
    $GET cannot return a property value for a Proxy Object property. Caché instead issues a message that the specified property does not exist. This property access limitation is unique to the class [%ZEN.proxyObject](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25ZEN.proxyObject), which is defined in the InterSystems Class Reference.
    

default

The data value to be returned if variable is undefined. It can any expression, including a local variable or a global variable, either subscripted or unsubscripted. If a global variable, it can contain an [extended global reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GGBL_structure). If a subscripted global variable, it can be specified using a [naked global reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GGBL_using). If present, default resets the naked indicator, affecting future naked global references, as described below.

If default is an undefined variable, by default $GET issues an <UNDEFINED> error, even when variable is defined. You can change Caché behavior to not generate an <UNDEFINED> error when referencing an undefined variable by setting the [%SYSTEM.Process.Undefined()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process#Undefined) method. If the Undefined() method is set to not generate an <UNDEFINED> error, $GET returns variable when default is undefined.

Examples

In the following example, the variable test is defined and the variable xtest is undefined. (The ZWRITE command is used because it explicitly returns a null string value.)

   KILL xtest
   SET test\="banana"
   SET tdef\=$GET(test),tundef\=$GET(xtest)
   ZWRITE tdef    ; $GET returned value of test
   ZWRITE tundef  ; $GET returned null string for xtest
   WRITE !,$GET(xtest,"none") 
     ; $GET returns default of "none" for undefined variable 

 

In the following example, a multidimensional property is used as the variable value. This example returns the names of all defined namespaces:

  SET obj \= ##class(%ResultSet).%New("%SYS.Namespace:List")
  DO obj.Execute()
  WRITE !,$GET(obj.Data,"none") // returns "none"
  SET x\=1
  WHILE x'="" {
     DO obj.Next()
     SET x\=$GET(obj.Data("Nsp"))
     IF x'="" {
     WRITE !,"Namespace: ",x }
   }
   WRITE !,"Done!"

 

A similar program returns the same information using the [$DATA](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fdata) function.

Notes

$GET Compared to $DATA

$GET provides an alternative to $DATA tests for both undefined variables ($DATA=0) and array nodes that are downward pointers without data ($DATA=10). If the variable is either undefined or a pointer array node without data, $GET returns a null string ("") without an undefined error. For example, you can recode the following lines:

   IF $DATA(^client(i))\=10 {
      WRITE !!,"Name: No Data" 
      GOTO Level1+3
   }

as:

   IF $GET(^client(i))\="" {
      WRITE !!,"Name: No Data" 
      GOTO Level1+3
   }

Note that $DATA tests are more specific than $GET tests because they allow you to distinguish between undefined elements and elements that are downward pointers only. For example, the lines:

  IF $DATA(^client(i))\=0 { QUIT }
  ELSEIF $DATA(^client(i))\=10 {
    WRITE !!,"Name: No Data" 
    GOTO Level1+3 
  }

could not be re-coded as:

   IF $GET(^client(i))\="" { QUIT }
   ELSEIF $GET(^client(i))\="" {
      WRITE !!,"Name: No Data" 
      GOTO Level1+3
   }

The two lines perform different actions depending on whether the array element is undefined or a downward pointer without data. If $GET were used here, only the first action (QUIT) would ever be performed. You could use $DATA for the first test and $GET for the second, but not the reverse ($GET for the first test and $DATA for the second).

Defaults with $GET and $SELECT

$GET(variable,default) allows you to return a default value when a specified variable is undefined. The same operation can be performed using a $SELECT function.

However, unlike $SELECT, the second argument in $GET is always evaluated.

The fact that $GET always evaluates both of its arguments is significant if variable and default both make subscripted global references and thus both modify the naked indicator. Because the arguments are evaluated in left-to-right sequence, the naked indicator is set to the default global reference, regardless of the whether $GET returns the default value. For further details on using $GET with global variables and the naked indicator, see [Using Multidimensional Storage (Globals)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GGBL_using) in Using Caché Globals.

Handling Undefined Variables

$GET defines handling behavior if a specified variable is undefined. The basic form of $GET returns a null string ("") if the specified variable is undefined.

$DATA tests if a specified variable is defined. It returns 0 if the variable is undefined.

You can define handling behavior for all undefined variables on a per-process basis using the [Undefined()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process#Undefined) method of the [%SYSTEM.Process](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process) class. The system-wide default behavior can be established by setting the [Undefined](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=Config.Miscellaneous#Undefined) property of the Config.Miscellaneous class. Setting Undefined has no effect on $GET or $DATA handling of specified variables.

See Also

*   [$DATA](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fdata) function
    
*   [$SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fselect) function
    
*   [Using Multidimensional Storage (Globals)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GGBL_using) in Using Caché Globals
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ffnumber "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fincrement "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fget.xml**