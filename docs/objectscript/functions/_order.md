# $ORDER

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_forder

---

 

Caché ObjectScript Reference

$ORDER

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fnumber "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fparameter "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$ORDER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_forder "$ORDER") \]

Go to: Description Examples Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Returns the next local variable or the subscript of a local or global variable.

Synopsis

$ORDER(variable,direction,target)
$O(variable,direction,target)

Parameters

variable

Either a local variable or a subscripted local, process-private global, or global variable. If an array, the subscript is required. You cannot specify just the array name. You cannot specify a simple object property reference as variable; you can specify a [multidimensional property](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ROBJ_property_multidimensional) reference as variable with the syntax obj.property.

direction

Optional — The subscript order in which to traverse the target array. Values for subscripted variables can be: 1 = ascending subscript order (the default) or –1 = descending subscript order. For unsubscripted local variables, 1 (the default) is the only permitted value.

target

Optional — Returns the current data value of the next or previous node of variable. Whether it is the next or previous depends on the setting of direction. You must specify a direction value to specify a target. For unsubscripted local variables, direction must be set to 1. If variable is undefined, the target value remains unchanged. The target parameter cannot be used with structured system variables (SSVNs) such as ^$ROUTINE.

Description

The value $ORDER returns depends on the parameters used.

*   $ORDER(variable) returns the number of the next defined subscript if variable is a subscripted variable. The returned subscript is at the same level as that specified for the variable. For example, $ORDER(^client(4,1,2)) returns the next subscript (3), assuming that ^client(4,1,3) exists.
    
    $ORDER(variable) returns the name of the next defined local variable in alphabetic collating sequence, if variable is an unsubscripted local variable. For example, $ORDER would return the following defined local variables in the following sequence: a, a0a, a1, a1a, aa, b, bb, c.
    
*   $ORDER(variable,direction) returns either the next or the previous subscript for the variable. You can specify direction as 1 (next, the default) or –1 (previous).
    
    For unsubscripted local variables, $ORDER returns variables in direction 1 (next) order only; you cannot specify a direction of –1 (previous).
    
*   $ORDER(variable,direction,target) returns the subscript for the variable, and sets target to its current data value. This can be either the next or the previous subscript for a subscripted variable, depending on the direction setting. For an unsubscripted local variable, direction must be set to 1 to return the current data value to target. The target parameter cannot be used with structured system variables (SSVNs) such as ^$ROUTINE. The ZBREAK command cannot specify the target parameter as a watchpoint.
    

Examples

The following example lists the name and value of the next defined local variables after variable a1:

   SET a\="best",a1\="prime",aa\="choice",b\="good",c\="utility grade"
   WRITE !,$ORDER(a1,1,target),!,target   

 

The following example returns 1, the first subscript in ^X. It sets the naked indicator to the first level.

   SET ^X(1,2,3)\="1", ^X(2)\="2"
   WRITE $ORDER(^X(\-1))

 

The following example returns 2, the next subscript on the single subscripted level. (The node you specify in the argument need not exist.) The naked indicator is still set to the first level.

   SET ^X(1,2,3)\="1",^X(2)\="2"
   WRITE $ORDER(^X(1))

 

The following example returns 2, the first subscript on the two-subscript level. The naked indicator is now set at the second level.

   SET ^X(1,2,3)\="1",^X(2)\="2"
   WRITE $ORDER(^X(1,\-1))

 

The following example uses $ORDER to list all of the primary subscripts in the ^data global:

   SET ^data(1)\="a",^data(3)\="c",^data(7)\="g"
   // Get first subscript
   SET key\=$ORDER(^data(""))
   WHILE (key'="") {
     WRITE key,!
     // Get next subscript 
     SET key \= $ORDER(^data(key))
   }

 

In the following example, a multidimensional property is used as the variable value. This example returns the names of all defined namespaces to the target parameter:

  SET obj \= ##class(%ResultSet).%New("%SYS.Namespace:List")
  DO obj.Execute()
    SET targ\="blank", x\=""
  WHILE targ'="" {
     DO obj.Next()
     WRITE $ORDER(obj.Data(x),1,targ),!       // returns "Nsp"
     IF targ'="" {
     WRITE "Namespace: ",targ,! }
     }
   WRITE !,"Done!"

 

Similar programs return the same information using the [$GET](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fget) and [$DATA](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fdata) functions.

Notes

Uses for $ORDER

$ORDER is typically used with loop processing to traverse the nodes in an array that doesn’t use consecutive integer subscripts. $ORDER simply returns the subscript of the next existing node. For example:

   SET struct\=""
   FOR  {
     SET struct\=$ORDER(^client(struct)) 
     QUIT:struct\=""
     WRITE !,^client(struct)
   }

The above routine writes the values for all the top-level nodes in the ^client global array.

$ORDER returns subscripts of existing nodes, but not all nodes contain a value. If you use $ORDER in a loop to feed a command (such as WRITE) that expects data, you must include a $DATA check for valueless nodes. For example, you could specify the WRITE command in the previous example with a postconditional test, as follows:

   WRITE:($DATA(^client(struct))#10) !,^client(struct)

This test covers the case of both a valueless pointer node and a valueless terminal node. If your code becomes too cumbersome for a simple FOR loop, you can relegate part of it to a block-structured DO.

Start and End for a Search

To start a search from the beginning of the current level, specify a null string ("") for the subscript. This technique is required if the level may contain negative as well as positive subscripts. The following example returns the first subscript on the array level:

   SET s\=$ORDER(^client(""))
   WRITE s

When $ORDER reaches the end of the subscripts for the given level, it returns a null string (""). If you use $ORDER in a loop, your code should always include a test for this value. In a previous example, QUIT:struct="" is used to detect the end of the level and terminate the loop.

Global References

If a variable is global variable, it can include an [extended global reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GGBL_structure), specifying a global in a different namespace. If you specify a nonexistent namespace, Caché issues a <NAMESPACE> error. If you specify a namespace for which you do not have privileges, Caché issues a <PROTECT> error, followed by the global name and database path, such as the following: <PROTECT> ^myglobal(1),c:\\intersystems\\cache\\mgr\\.

If variable is a subscripted global variable, it can be a [naked global reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GGBL_using). A naked global reference is specified without the array name and designates the most recently executed global reference. For example:

   SET var1\=^client(4,5)
   SET var2\=$ORDER(^(""))
   WRITE "var1=",var1,!,"var2=",var2

The first SET command establishes the current global reference, including the subscript level for the reference. The $ORDER function uses a naked global reference to return the first subscript for this level. For example, it would return the value 1, indicating ^client(4,1), if that subscripted global is defined. If ^client(4,1) is not defined, it would return the value 2, indicating ^client(4,2) if that subscripted global is defined, and so forth.

All three parameters of $ORDER can take a naked global reference, or specify the global reference. However, if direction specifies an explicit global reference, subsequent naked global references do not use the direction global reference. They continue to use the prior established global reference, as shown in the following example:

   SET ^client(4,3)\="Jones"
   SET ^client(4,5)\="Smith"
   SET ^dir(1)\=\-1
   SET rtn\=$ORDER(^client(4,5),\-1)
   WRITE $ZREFERENCE,!  
      /\* naked global ref is ^client(4,5) \*/
   SET rtn\=$ORDER(^client(4,5),^dir(1))
   WRITE $ZREFERENCE,!
      /\* NOTE: naked global ref is ^client(4,5) \*/
   SET rtn\=$ORDER(^client(4,5),^dir(1),^(1))
   WRITE $ZREFERENCE
      /\* NOTE: naked global ref is ^client(4,1) \*/
   WRITE ^client(4,1),!
   SET rtn\=$ORDER(^client(4,5),^dir(1),^targ(1))
   WRITE $ZREFERENCE
      /\* naked global ref is ^targ(1) \*/
   WRITE ^targ(1),!
   SET ^rtn(1)\=$ORDER(^client(4,5),^dir(1),^targ(2))
   WRITE $ZREFERENCE
      /\* naked global ref is ^rtn(1) \*/
   WRITE ^targ(2)

For more details, see [Naked Global Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GGBL_using) in Using Caché Globals.

$ORDER and $DOUBLE Subscripts

$DOUBLE floating point numbers can be used as subscript identifiers. However, when used as a subscript identifier, the $DOUBLE number is converted to a string. When $ORDER returns a subscript of this type, it returns it as a numeric string, not a $DOUBLE floating point number.

$ORDER and $NEXT

$ORDER is similar to $NEXT. Both functions return the subscripts of the next sibling in collating order to the specified node. However, $ORDER and $NEXT have different start and failure codes, as follows:

 

$NEXT

$ORDER

Starting point

\-1

Null string

Failure code

\-1

Null String

Because $ORDER starts and finishes on the null string, it correctly returns nodes having both negative and positive subscripts.

See Also

*   [$NEXT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fnext) function
    
*   [$QUERY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fquery) function
    
*   [$ZPREVIOUS](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzprevious) function
    
*   [Global Structure](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GGBL_structure) chapter in Using Caché Globals
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fnumber "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fparameter "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_forder.xml**