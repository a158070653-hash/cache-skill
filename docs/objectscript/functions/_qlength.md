# $QLENGTH

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fqlength

---

 

Caché ObjectScript Reference

$QLENGTH

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fproperty "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fqsubscript "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$QLENGTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fqlength "$QLENGTH") \]

Go to: Description Parameter Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Returns the number of subscript levels in a variable.

Synopsis

$QLENGTH(var)
$QL(var)

Parameter

var

A string, or expression that evaluates to a string, that contains the name of a variable. The variable name can specify no subscripts or one or more subscripts.

Description

$QLENGTH returns the number of subscript levels in var. $QLENGTH simply counts the number of subscript levels specified in var. The var variable does not have to be defined for $QLENGTH to return the number of subscript levels.

Parameter

var

A quoted string, or expression that evaluates to a string, which specifies a variable. It can be a local variable, a process-private variable, or a global variable.

If the string is a global reference, var can specify an [extended global reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GGBL_structure) by including a namespace name. Because var is a quoted string, the quotes around a namespace reference must be doubled to be parsed correctly as literal quotation marks. For example, "^|""SAMPLES""|myglobal(1,4,6)". The same applies to the quotes in a process-private global with "^" syntax. For example, "^|""^""|ppgname(3,6)". $QLENGTH does not check whether the specified namespace exists or whether the user has access privileges for the namespace.

A var must specify a variable name in canonical form (a fully expanded reference). To use $QLENGTH with a [naked global reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GGBL_using), or with indirection, you can use the $NAME function to return the corresponding fully expanded reference.

Examples

The following example show the results of $QLENGTH when used with subscripted and unsubscripted globals. The first $QLENGTH takes a global with no subscripts and returns 0. The second $QLENGTH takes a global with two subscript levels and returns 2. Note that quotes found in the variable name are doubled because var is specified as a quoted string.

   WRITE !,$QLENGTH("^|""USER""|test")
      ; returns 0
   SET name\="^|""USER""|test(1,""customer"")"
   WRITE !,$QLENGTH(name)
      ; returns 2

 

The following example returns the $QLENGTH value for a process-private global with three subscript levels. The $ZREFERENCE special variable contains the name of the most recently referenced variable.

   SET ^||myppg("food","fruit",1)\="apples"
   WRITE !,$QLENGTH($ZREFERENCE)   ; returns 3

 

The following example returns the $QLENGTH value for a process-private global specified as a naked global indicator. The $NAME function is used to expand the naked global indicator to canonical form:

   SET ^grocerylist("food","fruit",1)\="apples"
   SET ^(2)\="bananas"
   WRITE !,$QLENGTH($NAME(^(2)))   ; returns 3

 

See Also

*   [$QUERY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fquery) function
    
*   [$QSUBSCRIPT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fqsubscript) function
    
*   [$NAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fname) function
    
*   [$ZREFERENCE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzreference) special variable
    
*   [Global Structure](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GGBL_structure) in Using Caché Globals
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fproperty "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fqsubscript "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fqlength.xml**