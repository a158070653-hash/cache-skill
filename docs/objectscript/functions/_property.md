# $PROPERTY

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fproperty

---

 

Caché ObjectScript Reference

$PROPERTY

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fprefetchon "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fqlength "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$PROPERTY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fproperty "$PROPERTY") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Supports reference to a particular property of an instance.

Synopsis

$PROPERTY(instance, propertyname, index1, index2, index3... )

Parameters

instance

An expression that evaluates to an OREF. The value of the expression must be that of an in-memory instance of the desired class.

propertyname

An expression that evaluates to a string. The value of the string must match the name of an existing property defined in the class identified by instance.

index1, index2, index3, ...

Optional — If propertyname is a multidimensional value, then this series of expressions is treated as indices into the array represented by the property. (If the specified property is not multidimensional, the presence of extra arguments causes an error at runtime.)

Description

$PROPERTY gets or sets the value of a property in an instance of the designated class. This function permits a Caché ObjectScript program to select the value of an arbitrary property in an existing instance of some class. Since the first argument must be an instance of a class, it is computed at execution time. The property name may be computed at runtime or supplied as a string literal. The contents of the string must match exactly the name of a property declared in the class. Property names are case sensitive.

If the property is declared to be [multidimensional](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ROBJ_property_multidimensional), then the arguments after the property name are treated as indices into a multidimensional array. A maximum of 255 argument values may be used for the index.

$PROPERTY may also appear on the left side of an assignment. When $PROPERTY appears to the left of an assignment operator, it provides the location to which a value is assigned. When it appears to the right, it is the value being used in the calculation.

If instance is not a valid in-memory OREF, an <INVALID OREF> error occurs. If propertyname is not a valid property, a <PROPERTY DOES NOT EXIST> error occurs. If you specify an index1 and propertyname is not multidimensional, an <OBJECT DISPATCH> error occurs.

Remarks

The $PROPERTY function calls the Get() or Set() methods of the property passed to it. It is functionally the same as using the “Instance.PropertyName” syntax, where “Instance” and “PropertyName” are equivalent to the arguments as listed in the function’s signature. Because of this, $PROPERTY should not be called within a property’s Get() or Set() method, if one exists. For more information on Get() and Set() methods, see the chapter “[Using and Overriding Property Methods](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GOBJ_propmethods)” in Using Caché Objects.

When used within a method to refer to a property of the current instance, $PROPERTY may omit instance. The comma that would normally follow instance is still required, however.

An attempt to get a multidimensional value from a property which is not declared to be multidimensional results in a <FUNCTION> error; likewise for attempting to set a multidimensional value into a property that is not multidimensional.

Examples

The following example returns the current NLS Language property value:

  SET nlsoref\=##class(%SYS.NLS.Locale).%New()
  WRITE $PROPERTY(nlsoref,"Language")

 

The following example shows $PROPERTY used as a function:

 SET TestName \= "%Library.File"
 SET ClassDef \= ##class(%Library.ClassDefinition).%OpenId(TestName)
 FOR i \= "Name", "Super", "Persistent", "Final" 
 {
    WRITE i, ": ", $PROPERTY(ClassDef, i), !
 }

 

The following example shows $PROPERTY used on both sides of an assignment operator:

 SET TestFile \= ##class(%Library.File).%New("AFile")
 WRITE "Initial file name: ",$PROPERTY(TestFile,"Name"),!
 SET $PROPERTY(TestFile,"Name") \= 
         $PROPERTY(TestFile,"Name") \_ "Renamed"
 WRITE "File name afterward: ",$PROPERTY(TestFile,"Name"),!

 

See Also

*   [$CLASSMETHOD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fclassmethod) function
    
*   [$CLASSNAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fclassname) function
    
*   [$METHOD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fmethod) function
    
*   [$PARAMETER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fparameter) function
    
*   [$THIS](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vthis) special variable
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fprefetchon "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fqlength "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fproperty.xml**