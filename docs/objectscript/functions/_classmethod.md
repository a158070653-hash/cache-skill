# $CLASSMETHOD

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fclassmethod

---

 

Caché ObjectScript Reference

$CLASSMETHOD

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fchar "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fclassname "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$CLASSMETHOD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fclassmethod "$CLASSMETHOD") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Executes a named class method in the designated class.

Synopsis

$CLASSMETHOD(classname, methodname, arg1, arg2, arg3, ... )

Parameters

classname

Optional — An expression that evaluates to a string. The content of the string must match exactly the name of an existing, accessible, previously compiled class. In the case of references to Caché classes, the name may be either in its canonical form ([%Library.String](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Library.String)), or its abbreviated form ([%String](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Library.String)).

If classname is omitted, the current class context is used. (You can use [$THIS](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vthis) to determine the current class context.) Note that when classname is omitted the placeholder comma must be specified.

methodname

An expression which evaluates to a string. The value of the string must match the name of an existing class method in the class identified by classname.

arg1, arg2, arg3, ...

Optional — A series of expressions to be substituted sequentially for the arguments to the designated method. The values of the expressions can be of any type. It is the responsibility of the implementor to make sure that the type of the supplied expressions match what the method expects, and have values within the bounds declared. (If the specified method expects no arguments then no arguments beyond the methodname need be given in the function invocation. If the method requires arguments, the rules that govern what must be supplied are those of the target method.)

Description

$CLASSMETHOD permits a Caché ObjectScript program to invoke an arbitrary class method in an arbitrary class. Both the class name and the method name may be computed at runtime or supplied as string constants. To invoke an instance method rather than a class method, use the $METHOD function.

If the method takes arguments, they are supplied by the list of arguments that follow the method name. A maximum of 255 argument values may be passed to the method.

The invocation of $CLASSMETHOD as a function or a procedure determines the invocation of the target method. You can invoke $CLASSMETHOD using the JOB command or the DO command, discarding the return value.

An attempt to invoke a nonexistent class results in a <CLASS DOES NOT EXIST> error, followed by the current namespace name and the specified class name. For example, attempting to invoke the nonexistent classname “Fred” results in the error <CLASS DOES NOT EXIST> \*User.Fred. Specifying the empty string for classname results in <CLASS DOES NOT EXIST> \*(No name).

An attempt to invoke a nonexistent class method results in a <METHOD DOES NOT EXIST> error.

Examples

The following example shows $CLASSMETHOD used as a function:

 SET classname \= "%Dictionary.ClassDefinition"
 SET classmethodname \= "NormalizeClassname"
 SET singleargument \= "%String"
 WRITE $CLASSMETHOD(classname,classmethodname,singleargument),!

 

It returns %Library.String.

The following example shows $CLASSMETHOD with two parameters:

 WRITE $CLASSMETHOD("%Library.Persistent","%PackageName"),!
 WRITE $CLASSMETHOD("%Library.Persistent","%ClassName")

 

These calls return %Library and %Persistent.

The following example uses $CLASSMETHOD to execute a Dynamic SQL query:

  SET q1\="SELECT Age,Name FROM Sample.Person "
  SET q2\="WHERE Age > ? AND Age < ? "
  SET q3\="ORDER by Age"
  SET myquery\=q1\_q2\_q3
  SET rset\=$CLASSMETHOD("%SQL.Statement","%ExecDirect",,myquery,12,20)
  DO rset.%Display()
  WRITE !,"Teenagers in Sample.Person"

 

See Also

*   [$CLASSNAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fclassname) function
    
*   [$METHOD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fmethod) function
    
*   [$PARAMETER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fparameter) function
    
*   [$PROPERTY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fproperty) function
    
*   [$THIS](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vthis) special variable
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fchar "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fclassname "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fclassmethod.xml**