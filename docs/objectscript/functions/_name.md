# $NAME

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fname

---

 

Caché ObjectScript Reference

$NAME

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fmethod "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fnconvert "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$NAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fname "$NAME") \]

Go to: Description Parameters Examples Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Returns the name value of a variable or a portion of a subscript reference.

Synopsis

$NAME(variable,integer)
$NA(variable,integer)

Parameters

variable

The variable whose name value is to be returned. It can specify a local or global variable, which can be either subscripted or unsubscripted. It does not need to be a defined variable. However, it may not be a defined private variable. If variable is a subscripted global, you can specify a [naked global reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GGBL_using).

integer

Optional — A numeric value that specifies which portion (level) of a subscript reference to return. It can be a positive integer, the name of a variable, or an expression. When used, variable must be a subscripted reference.

Description

$NAME returns a formatted form of the variable name reference supplied as variable. It does not check whether this variable is defined or has a data value. The value $NAME returns depends on the parameters used.

*   $NAME(variable) returns the name value of the specified variable in canonical form; that is, as a fully expanded reference.
    
*   $NAME(variable,integer) returns a portion of a subscript reference. Specifically, integer controls the number of subscripts returned by the function.
    

Execution of this function does not affect the naked indicator.

Parameters

variable

variable can be a local variable, a process-private global variable, or a global variable. It can be unsubscripted or subscripted. If variable is a global it can use [extended global reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GGBL_structure). $NAME returns the extended global reference as specified, without checking whether the specified namespace exists or whether the user has access privileges for the namespace. It does not capitalize the namespace name. If variable is a [naked global reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GGBL_using), $NAME returns the full global reference. If variable is a private variable, a compile error occurs.

It can be a [multidimensional object property](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ROBJ_property_multidimensional); it cannot be a non-multidimensional object property. Attempting to use $NAME on a non-multidimensional object property results in an <OBJECT DISPATCH> error.

$NAME cannot return a [special variable](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_variables#GCOS_variables_special), even those that can be modified using SET. Attempting to return a special variable results in a <SYNTAX> error.

integer

The integer parameter is used when variable is a subscripted reference. If the value of integer is 0, $NAME returns only the name of the variable. If the value of integer is less than the number of subscripts in variable, $NAME returns the number of subscripts indicated by the value of integer. If integer is greater than the number of subscripts in variable, $NAME returns the full subscripted reference.

If variable is an unsubscripted variable, the value of integer is ignored; $NAME returns the variable name. If integer is the null string ("") or a nonnumeric string, $NAME returns the variable name with no subscripts.

The value of integer receives standard integer parsing. For example, leading zeros and the plus sign are ignored. Fractional digits are truncated and ignored. A negative integer value results in a <FUNCTION> error.

Examples

In this example, the integer parameter specifies the level to return. If the specified number of subscripts in integer matches or exceeds the number of subscript levels (in this case, 3), then $NAME returns all defined levels, behaving as if you specified the one-parameter form. If you specify an integer level of zero (0), the null string (""), or any nonnumeric string (such as "A"), $NAME returns the name of the array (in this case “^client”)

   SET ^client(4)\="Vermont"
   SET ^client(4,1)\="Brattleboro"
   SET ^client(4,1,1)\="Yankee Ingenuity"
   SET ^client(4,1,2)\="Vermonster Systems"
   WRITE !,$NAME(^client(4,1,1),1) ; returns 1 level
   WRITE !,$NAME(^client(4,1,1),2) ; returns 2 levels
   WRITE !,$NAME(^client(4,1,1),3) ; returns 3 levels
   WRITE !,$NAME(^client(4,1,1),4) ; returns all (3) levels
   WRITE !,$NAME(^client(4,1,1),0) ; returns array name
   WRITE !,$NAME(^client(4,1,1),"") ; returns array name
   WRITE !,$NAME(^client(4,1,1))    ; returns all (3) levels

 

In the following example, $NAME is used with a naked reference in a loop to output the values for all elements in the current (user-supplied) array level.

   READ !,"Array element: ",ary
   SET x\=@ary ; dummy operation to set current array and level
   SET y\=$ORDER(^("")) ; null string to find beginning of level
   FOR i\=0:0 {
     WRITE !,@$NAME(^(y)) 
     SET y\=$ORDER(^(y)) 
     QUIT:y\=""
   }

The first SET command performs a dummy assignment to establish the user-supplied array and level as the basis for the subsequent naked references. The $ORDER function is used with a naked reference to return the number of the first subscript (whether negative or positive) at the current level.

The WRITE command in the FOR loop uses $NAME with a naked global reference and argument indirection to output the value of the current element. The SET command uses $ORDER with a naked global reference to return the subscript of the next existing element that contains data. Finally, the postconditional QUIT checks the value returned by $ORDER to detect the end of the current level and terminate the loop processing.

You can use the returned $NAME string value for name or subscript indirection or pass it as a parameter to a routine or user-defined function. For more information, refer to [Indirection](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_operators#GCOS_operators_indirection) in Using Caché ObjectScript. Consider the routine ^DESCEND that lists descendant nodes of the specified node.

DESCEND(ROOT)  ;List descendant nodes
   NEW REF 
   SET REF\=ROOT
   IF ROOT'\["(" {
       FOR {
       SET REF\=$QUERY(@REF) 
       QUIT:REF\="" 
       WRITE REF,! }
   }
   ELSE {
     SET $EXTRACT(ROOT,$LENGTH(ROOT))\=","
     FOR {
       SET REF\=$QUERY(@REF)
       QUIT:REF'\[ROOT 
       WRITE REF,!   }
   }

The following example demonstrates how you can use $NAME to pass a parameter to the ^DESCEND routine defined in the previous example.

   FOR var1\="ONE","TWO","THREE" { 
     DO ^DESCEND($NAME(^X(var1))) }  

^X("ONE",2,3)

Notes

Uses for $NAME

You typically use $NAME to return the name values of array elements for use with the $DATA and $ORDER functions.

If a reference to an array element contains expressions, $NAME evaluates the expressions before returning the canonical form of the name. For example:

   SET x\=1+2
   SET y\=$NAME(^client(4,1,x))
   WRITE y

 

$NAME evaluates the variable x and returns the value ^client(4,1,3).

Naked Global References

$NAME also accepts a naked global reference and returns the name value in its canonical form (that is, a full (non-naked) reference). A naked reference is specified without the array name and designates the most recently executed global reference. In the following example, the first SET command establishes the global reference and the second SET command uses the $NAME function with a naked global reference.

   SET ^client(5,1,2)\="Savings/27564/3270.00"
   SET y\=$NAME(^(3))
   WRITE y

 

In this case, $NAME returns the value ^client(5,1,3). The supplied subscript value (3) replaces the existing subscript value (2), at the current level.

For more details, see [Naked Global Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GGBL_using) in Using Caché Globals.

Extended Global References

You can control whether $NAME returns name values in extended global reference form on a per-process basis using the [RefInKind()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process#RefInKind) method of the [%SYSTEM.Process](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process) class. The system-wide default behavior can be established by setting the [RefInKind](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=Config.Miscellaneous#RefInKind) property of the Config.Miscellaneous class.

With extended reference mode in effect, the following example returns the defined namespace and name ^\["PAYROLL"\]MyRoutine (as shown in the first example), and not just ^MyRoutine (as shown in the second example):

   DO ##class(%SYSTEM.Process).RefInKind(0)
   WRITE $NAME(^\["PAYROLL"\]MyRoutine)

 

   DO ##class(%SYSTEM.Process).RefInKind(1)
   WRITE $NAME(^\["PAYROLL"\]MyRoutine)

 

For a description of extended global reference syntax, see the [Global Structure](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GGBL_structure) chapter in Using Caché Globals.

See Also

*   [$DATA](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fdata) function
    
*   [$ORDER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_forder) function
    
*   [$GET](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fget) function
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fmethod "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fnconvert "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fname.xml**