# $METHOD

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fmethod

---

 

Caché ObjectScript Reference

$METHOD

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fmatch "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fname "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$METHOD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fmethod "$METHOD") \]

Go to: Description Example See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Supports calls to an instance method.

Synopsis

$METHOD(instance, methodname, arg1, arg2, arg3, ... )

Parameters

instance

An expression that evaluates to an object reference. The value of the expression must be that of an in-memory instance of a class.

methodname

An expression that evaluates to a string. The value of the string must exactly match the name of an existing method in the instance of the class given as the first argument.

arg1, arg2, arg3, ...

A series of expressions to be substituted for the arguments to the designated method. The values of the expressions can be of any type. It is the responsibility of the implementer to make sure that the supplied expressions both match in type and have values with the bounds that the method expects. (If the specified method expects no arguments then nothing beyond classname and methodname need be used in the function invocation. If the method requires arguments, the rules that govern what must be supplied are those of the target method.)

Description

$METHOD executes a named instance method for a specified instance of a designated class.

This function permits a Caché ObjectScript program to call an arbitrary method in an existing instance of some class. Since the first argument must be a reference to an object, it is computed at execution time. The method name may be computed at runtime or supplied as a string literal. If the method takes arguments, they are supplied from the list of arguments that follow the method name. A maximum of 255 argument values may be passed to the method. If the method requires arguments, the rules that govern what must be supplied are those of the target method. To invoke a class method rather than an instance method, use the $CLASSMETHOD function.

The invocation of $METHOD as a function or a procedure determines the invocation of the target method. You can invoke $METHOD using the DO command, discarding the return value.

When used within one method of a class instance to refer to another method of that instance, the $METHOD may omit instance. The comma that would normally follow Instance is still required, however.

If there is an attempt to invoke a method that is nonexistent or that is declared to be a class method, this results in a <METHOD DOES NOT EXIST> error.

Example

The following example shows $METHOD used as a function:

 SET ListOfStuff \= ##class(%Library.ListOfDataTypes).%New()
 FOR i \= "First", "Second", "Third", "Fourth"
    {
        DO ListOfStuff.Insert((i \_ "-Element"))
    }
 SET methodname \= "Count"
 SET elements \= $METHOD(ListOfStuff,methodname)
 WRITE "Elements: ",elements,!
 SET i \= $RANDOM(elements) + 1
 WRITE "Element #", i , " = " , $METHOD(ListOfStuff,"GetAt", i), !

 

See Also

*   [$CLASSMETHOD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fclassmethod) function
    
*   [$CLASSNAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fclassname) function
    
*   [$PARAMETER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fparameter) function
    
*   [$PROPERTY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fproperty) function
    
*   [$THIS](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vthis) special variable
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fmatch "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fname "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fmethod.xml**