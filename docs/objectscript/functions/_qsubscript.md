# $QSUBSCRIPT

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fqsubscript

---

 

Caché ObjectScript Reference

$QSUBSCRIPT

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fqlength "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fquery "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$QSUBSCRIPT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fqsubscript "$QSUBSCRIPT") \]

Go to: Description Parameters Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Returns a variable name or a subscript name.

Synopsis

$QSUBSCRIPT(namevalue,intexpr)
$QS(namevalue,intexpr)

Parameters

namevalue

A string, or an expression that evaluates to a string, which is the name of a local variable, process-private global, or global variable, with or without subscripts.

intexpr

An integer code that specifies which name to return: variable name, subscript name, or namespace name.

Description

$QSUBSCRIPT returns the variable name, or the name of a specified subscript of namevalue, depending on the value of intexpr. If namevalue is a global variable, you can also return the namespace name, if it was explicitly specified. $QSUBSCRIPT does not return a default namespace name.

Parameters

namevalue

A quoted string, or expression that evaluates to a string, which is a local or global reference. It can have the form: Name(s1,s2,...,sn).

If the string is a global reference, it can contain a namespace reference. Because namevalue is a quoted string, the quotes around a namespace reference must be doubled to be parsed correctly as literal quotation marks.

A namevalue must reference a variable name in canonical form (a fully expanded reference). To use $QSUBSCRIPT with a naked global reference, or with indirection, you can use the $NAME function to return the corresponding fully expanded reference.

intexpr

An integer expression code that indicates which value to return. Assume that the namevalue parameter has the form NAME(s1,s2,...,sn), where n is the ordinal number of the last subscript. The intexpr parameter can have any of the following values:

Code

Return Value

< -1

Generates a <FUNCTION> error; these numbers are reserved for future extensions.

\-1

Returns the namespace name if a global variable namevalue includes one; otherwise, returns the null string ("").

0

Returns the variable name. Returns ^NAME for a global variable, and ^||NAME for a process-private global variable. Does not return a namespace name.

<=n

Returns the subscript name for the level of subscription specified by the integer n, with 1 being the first subscript level and n being the highest defined subscript level.

\>n

An integer > n returns the null string (""), where n is the highest defined subscript level.

Examples

The following example returns $QSUBSCRIPT values when namevalue is a subscripted global with one subscript level and a specified namespace:

   SET global\="^|""account""|%test(""customer"")"
   WRITE !,$QSUBSCRIPT(global,\-1)  ; account
   WRITE !,$QSUBSCRIPT(global,0)   ; ^%test
   WRITE !,$QSUBSCRIPT(global,1)   ; customer
   WRITE !,$QSUBSCRIPT(global,2)   ; null string

 

The following example returns $QSUBSCRIPT values when namevalue is a process-private global with two subscript levels. The $ZREFERENCE special variable contains the name of the most recently referenced variable.

   SET ^||myppg(1,3)\="apples"
   WRITE !,$QSUBSCRIPT($ZREFERENCE,\-1)  ; null string
   WRITE !,$QSUBSCRIPT($ZREFERENCE,0)   ; ^||myppg
   WRITE !,$QSUBSCRIPT($ZREFERENCE,1)   ; 1
   WRITE !,$QSUBSCRIPT($ZREFERENCE,2)   ; 3

 

The following example returns the $QSUBSCRIPT value for a global variable specified as a naked global indicator. The $NAME function is used to expand the naked global indicator to canonical form:

   SET ^grocerylist("food","fruit",1)\="apples"
   SET ^(2)\="bananas"
   WRITE !,$QSUBSCRIPT($NAME(^(2)),2)   ; returns "fruit"

 

See Also

*   [$QUERY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fquery) function
    
*   [$QLENGTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fqlength) function
    
*   [$NAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fname) function
    
*   [$ZREFERENCE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzreference) special variable
    
*   [Global Structure](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GGBL_structure) in Using Caché Globals
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fqlength "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fquery "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fqsubscript.xml**