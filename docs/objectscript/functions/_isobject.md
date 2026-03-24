# $ISOBJECT

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fisobject

---

 

Caché ObjectScript Reference

$ISOBJECT

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_finumber "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fisvaliddouble "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$ISOBJECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fisobject "$ISOBJECT") \]

Go to: Description Parameter Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Returns whether an expression is an object reference.

Synopsis

$ISOBJECT(expr)

Parameter

expr

A Caché ObjectScript expression.

Description

$ISOBJECT returns 1 if expr is an object reference. $ISOBJECT returns 0 if expr is not an object reference.

$ISOBJECT returns –1 if expr is a reference to an invalid object. Invalid objects should not occur in normal operations; an invalid object could be caused, for example, by recompiling the class while instances of the class are active.

To remove an object reference, set the variable to the null string (""). The obsolete %Close() method cannot be used to remove an object reference. %Close() performs no operation and always returns successful completion. Do not use %Close() when writing new code.

Parameter

expr

Any Caché ObjectScript expression.

Examples

The following example shows the values returned by $ISOBJECT for an object reference and a non-object reference (in this case, a string reference):

  SET a\="certainly not an object"
  SET o\=##class(%SQL.Statement).%New()
  WRITE !,"non-object a: ",$ISOBJECT(a)
  WRITE !,"object ref o: ",$ISOBJECT(o)

 

The following example shows how to remove an object reference. The %Close() method does not change the object reference. Setting an object reference to the null string deletes the object reference:

  SET o\=##class(%SQL.Statement).%New()
  WRITE !,"objref o: ",$ISOBJECT(o)
  DO o.%Close()  ; this is a no-op
  WRITE !,"objref o: ",$ISOBJECT(o)
  SET o\=""
  WRITE !,"objref o: ",$ISOBJECT(o)

 

See Also

*   [$SYSTEM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vsystem) special variable
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_finumber "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fisvaliddouble "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fisobject.xml**