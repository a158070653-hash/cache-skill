# $PARAMETER

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fparameter

---

 

Caché ObjectScript Reference

$PARAMETER

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_forder "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fpiece "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$PARAMETER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fparameter "$PARAMETER") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Returns the value of the specified class parameter.

Synopsis

$PARAMETER(class,parameter)

Parameters

class

Optional — Either a class name or an object reference (oref) to a class instance. If omitted, uses the object reference of the current class instance. When omitted, you must specify the placeholder comma.

parameter

The name of a parameter. An expression which evaluates to a string. The value of the string must match the name of an existing parameter of the class identified by class.

Description

$PARAMETER returns the value of a specified class parameter. $PARAMETER can look up this parameter in the current class context or in a specified class context. You can specify a class name as a quoted string, specify an oref, or omit the class parameter and take as default the current instance (see [$THIS](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vthis)). Specifying class is optional; specifying the comma separator is mandatory.

For example:

  WRITE $PARAMETER("%Library.Boolean","XSDTYPE")

 

There are several ways to return the value of a parameter using object syntax, as shown in the following example:

  ZNSPACE "Samples"
  WRITE "ObjectScript function:",!
  WRITE $PARAMETER("Sample.Person","EXTENTQUERYSPEC")
  WRITE !,"class parameter:",!
  WRITE ##class(Sample.Person).#EXTENTQUERYSPEC
  WRITE !,"instance parameter:",!
  SET myinst\=##class(Sample.Person).%New()
  WRITE myinst.%GetParameter("EXTENTQUERYSPEC")
  WRITE !,"instance parameter:",!
  WRITE myinst.#EXTENTQUERYSPEC

 

Invalid Values

*   $PARAMETER("","XMLTYPE"): attempting to invoke an invalid oref (such as the empty string, an integer, or a fractional number) results in an <INVALID OREF> error.
    
*   $PARAMETER("bogus","XMLTYPE"): attempting to invoke a nonexistent class results in a <CLASS DOES NOT EXIST> error, followed by the specified class name. If a package name is not specified, Caché assumes the default. For example, attempting to invoke the nonexistent class “bogus” results in the error <CLASS DOES NOT EXIST> \*User.bogus.
    
*   $PARAMETER(,"XMLTYPE"): attempting to default to the current object instance when none has been established results in a <NO CURRENT OBJECT> error.
    
*   $PARAMETER("%SYSTEM.Task",""): attempting to reference an invalid parameter name (for example, an empty string) or to reference a parameter by number generates an <ILLEGAL VALUE> error.
    
*   $PARAMETER("%SYSTEM.Task","MakeCoffee"): attempting to reference a nonexistent parameter name returns the empty string ("").
    

Examples

The following example specifies class names and returns the class default values for the XMLTYPE and XSDTYPE parameters:

  WRITE $PARAMETER("%SYSTEM.Task","XMLTYPE"),!
  WRITE $PARAMETER("%Date","XSDTYPE")

 

The following example specifies an oref and returns the value of the XMLTYPE parameter for this instance:

  SET oref\=##class(%SYSTEM.Task).%New()
  WRITE $PARAMETER(oref,"XMLTYPE")

 

The following example returns a system parameter using $PARAMETER syntax and using class syntax:

  WRITE $PARAMETER("%SYSTEM.SQL","%RandomSig"),!
  WRITE ##class(%SYSTEM.SQL).#%RandomSig

 

See Also

*   [$CLASSMETHOD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fclassmethod) function
    
*   [$CLASSNAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fclassname) function
    
*   [$METHOD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fmethod) function
    
*   [$PROPERTY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fproperty) function
    
*   [$THIS](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vthis) special variable
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_forder "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fpiece "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fparameter.xml**