# $CLASSNAME

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fclassname

---

 

Caché ObjectScript Reference

$CLASSNAME

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fclassmethod "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fcompile "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$CLASSNAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fclassname "$CLASSNAME") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Returns the name of a class.

Synopsis

$CLASSNAME(n)

Parameter

n

Optional — An object reference (oref) to an class instance. If omitted, the class name of the current class is returned.

Description

$CLASSNAME returns the name of a class. Commonly, it takes an object reference (oref) and returns the corresponding class name. $CLASSNAME with no argument returns the name of the current class. $CLASSNAME always returns the full class name (for example, %SQL.Statement), not the short version of the class name omitting the package name (for example, Statement).

$CLASSNAME is functionally equivalent to the [%ClassName(1)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25Library.Base#%ClassName) method of the [%Library.Base](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Library.Base) superclass. The $CLASSNAME function gives better performance than the %ClassName(1) method for returning the full class name. To return the short version of the class name, you can use either %ClassName() or %ClassName(0).

Examples

The following example creates an instance of a class. $CLASSNAME takes the instance oref and returns the corresponding class name:

   SET dynoref \= ##class(%SQL.Statement).%New()
   WRITE "instance class name: ",$CLASSNAME(dynoref)

 

In the following example, $CLASSNAME with no parameter returns the class name of the current class context. In this case, it is the DocBook.Utils class. This is the same class name contained in the $THIS special variable:

   WRITE "class context: ",$CLASSNAME(),!
   WRITE "class context: ",$THIS

 

The following example shows that the $CLASSNAME function and the %ClassName(1) method return the same values. It also shows use of the %ClassName() method (with no argument or with a 0 argument) to return the short version of the class name:

CurrentClass
   WRITE "current full class name: ",$CLASSNAME(),!
   WRITE "current full class name: ",..%ClassName(1),!
   WRITE "current short class name: ",..%ClassName(0),!
   WRITE "current short class name: ",..%ClassName(),!!
ClassInstance
   SET x \= ##class(%SQL.Statement).%New()
   WRITE "oref full class name: ",$CLASSNAME(x),!
   WRITE "oref full class name: ",x.%ClassName(1),!
   WRITE "oref short class name: ",x.%ClassName(0),!
   WRITE "oref short class name: ",x.%ClassName()

 

See Also

*   [$CLASSMETHOD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fclassmethod) function
    
*   [$METHOD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fmethod) function
    
*   [$PARAMETER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fparameter) function
    
*   [$PROPERTY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fproperty) function
    
*   [$THIS](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vthis) special variable
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fclassmethod "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fcompile "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fclassname.xml**