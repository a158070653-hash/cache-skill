# ZWRITE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_czwrite

---

 

Caché ObjectScript Reference

ZWRITE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cztrap "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czzdump "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_COMMANDS "Caché ObjectScript Commands") \]  >  \[ [ZWRITE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czwrite "ZWRITE") \]

Go to: Description ZWRITE without an Argument ZWRITE with Arguments Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Displays variable names and their values and/or expression values.

Synopsis

ZWRITE:pc arg,...
ZW:pc arg,...

Arguments

pc

Optional — A postconditional expression.

arg

Optional — A variable or expression to display, or a comma-separated list of variables and/or expressions to display. A comma-separated list can contain any combination of variables and expressions.

Description

The ZWRITE command lists names of variables and their values. It lists these variables and their descendents in the format varname\=value in canonical order, one variable per line, on the current device. ZWRITE also lists the values of expressions. Expressions are listed as value, one per line, in the order specified. The ZWRITE command has two forms:

*   [Without an argument](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czwrite#RCOS_czwrite_noarg)
    
*   [With arguments](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czwrite#RCOS_czwrite_args)
    

ZWRITE can take an optional postconditional expression. Caché executes the command if the postconditional expression is true (evaluates to a nonzero numeric value). Caché does not execute the command if the postconditional expression is false (evaluates to zero). For further details, refer to [Command Postconditional Expressions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_commands#GCOS_commands_pc) in Using Caché ObjectScript.

ZWRITE listing can be interrupted by issuing a CTRL-C, generating an <INTERRUPT> error.

ZWRITE without an Argument

ZWRITE without an argument is functionally identical to [WRITE without an argument](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cwrite#RCOS_cwrite_noarg). It displays the names and values of all variables in the local variable environment ([local variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_variables#GCOS_variables_local)), including private variables. It does not display process-private globals or special variables. It lists variables by name in ASCII order. It lists subscripted variables in subscript tree order.

For further details, refer to the [WRITE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cwrite) command.

ZWRITE with Arguments

ZWRITE with an argument can specify one argument or a comma-separated list of arguments. These arguments are evaluated in left-to-right order. Each argument can specify a variable or an expression. If arg is a comma-separated list, each variable or expression is displayed on a separate line.

ZWRITE displays variables as varname\=value. ZWRITE displays expressions as value. ZWRITE displays [special variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_variables#GCOS_variables_special) as expressions (value).

ZWRITE displays a non-numeric string value as a quoted string. ZWRITE displays a numeric value as a [canonical number](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_numcanonical). A numeric string in canonical form is displayed as an unquoted canonical number. A numeric string not in canonical form is displayed as a quoted string.

ZWRITE truncates the display of very long strings and appends ... to indicate that the string display was truncated.

ZWRITE displays values containing control characters (including those created with [$LISTBUILD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistbuild) and [$BIT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fbit)) in a readable format. If this formatting causes very long string values to exceed the [maximum string length](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_strings_long), ZWRITE truncates the displayed string and appends ... to indicate that the string was truncated.

For a comparison of ZWRITE with the [WRITE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cwrite), [ZZDUMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czzdump), and [ZZWRITE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czzwrite) commands, refer to the [Write Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_commands#GCOS_cmds_iowrite) features tables in the “Commands” chapter of Using Caché ObjectScript.

Variables

If arg is a variable, ZWRITE writes varname\=value on a separate line. Variables can be subscripted. If the variable has defined subnodes, ZWRITE writes a separate varname\=value line for each subnode in subscript tree order. The variable can be a [local variable](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_variables#GCOS_variables_local), [process-private global](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_variables#GCOS_variables_procprivglbls), [global variable](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_variables#GCOS_variables_global), or [object reference (oref)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czwrite#RCOS_czwrite_oref).

ZWRITE ignores undefined variables. It does not issue an error. If you specify one or more undefined variables in a comma-separated list of variables, ZWRITE ignores the undefined variables and returns the defined variables. This behavior allows you to display multiple variables without checking to determine if all of them are defined. If you specify an undefined variable to [WRITE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cwrite), [ZZDUMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czzdump), or [ZZWRITE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czzwrite) Caché issues an <UNDEFINED> error.

You can use extended global reference to specify a global variable not mapped to the current namespace. ZWRITE displays extended global references even when the [RefInKind()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process#RefInKind) method of the [%SYSTEM.Process](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process) class or the [RefInKind](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=Config.Miscellaneous#RefInKind) property of the Config.Miscellaneous class has been set to strip extended global references. If you specify a nonexistent namespace, Caché issues a <NAMESPACE> error. If you specify a namespace for which you do not have privileges, Caché issues a <PROTECT> error, followed by the global name and database path, such as the following: <PROTECT> ^myglobal,c:\\intersystems\\cache\\mgr\\. For further information on subscripted variables and extended global reference, refer to [Global Structure](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GGBL_structure) in Using Caché Globals.

Expressions

If arg is an expression, ZWRITE evaluates the expression and writes the resulting value on a separate line. If the expression contains an undefined variable, Caché issues an <UNDEFINED> error.

If the expression is a multidimensional property, ZWRITE will not display the property descendents. To display an entire multidimensional property with ZWRITE, either [MERGE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cmerge) it into a local array and display the array, or display the entire object.

$ZWRITE with an Object Reference

You can specify an object reference to ZWRITE as either a variable or an expression. If the ZWRITE argument is an object reference, ZWRITE displays General Information, Attribute Values, and Swizzled References for the properties of the object, one attribute per line. If the ZWRITE argument is an embedded object property, ZWRITE displays General Information and Attribute Values for the array elements of the container property, one attribute per line. The display format is the same as the [%SYSTEM.OBJ.Dump()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.OBJ#Dump) method.

The following example displays an object instance and its properties:

  SET tStatement \= ##class(%SQL.Statement).%New()
  ZWRITE tStatement

 

The following example returns information about an object reference:

  ZNSPACE "Samples"
  SET pobj\=##class(Sample.Person).%OpenId(1)
  ZWRITE pobj

 

The following example returns information about an embedded object property:

  ZNSPACE "Samples"
  SET pobj=##class(Sample.Person).%OpenId(1)
  ZWRITE pobj.Home

$ZWRITE with a Bitstring

You can specify a bitstring to ZWRITE as either a variable or an expression. If the ZWRITE argument is a Caché compressed bitstring (created using the [$BIT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fbit) function), ZWRITE displays the decimal representation of the compressed binary string as $ZWCHAR ($zwc) two-byte (wide) characters.

ZWRITE also displays a comment that lists the uncompressed “1” bits in left-to-right order as a comma-separated list. If there are three or more consecutive “1” bits, it lists them as a range (inclusive) with two dot syntax (n..m). For example, the bitstring \[1,0,1,1,1,1,0,1\] is shown as /\*$bit(1,3..6,8)\*/. The bitstring \[1,1,1,1,1,1,1,1\] is shown as /\*$bit(1..8)\*/. The bitstring \[0,0,0,0,0,0,0,0\] is shown as /\*$bit()\*/. The following example shows ZWRITE bitstring output:

  SET $BIT(a,1) \= 0
  SET $BIT(a,2) \= 0
  SET $BIT(a,3) \= 1
  SET $BIT(a,4) \= 0
  SET $BIT(a,5) \= 1
  SET $BIT(a,6) \= 1
  SET $BIT(a,7) \= 1
  SET $BIT(a,8) \= 0
  ZWRITE a

 

Examples

ZWRITE Without an Argument

In following example, ZWRITE without an argument lists all defined local variables in ASCII name order.

   SET A\="A",a\="a",AA\="AA",aA\="aA",aa\="aa",B\="B",b\="b"
   ZWRITE

returns:

A="A"
AA="AA"
B="B"
a="a"
aA="aA"
aa="aa"
b="b"

In the following example ZWRITE without an argument lists canonical and non-canonical numeric values:

   SET w\=10
   SET x\=++0012.00
   SET y\="6.5"
   SET z\="007"
   SET a\=w+x+y+z
   ZWRITE

returns:

a=35.5
w=10
x=12
y=6.5
z="007"

ZWRITE with Arguments

In the following example, ZWRITE displays three variables as varname\=value, each on its own line:

   SET alpha\="abc"
   SET x\=100
   SET y\=80
   SET sum\=x+y
   ZWRITE x,sum,alpha

 

In the following example, ZWRITE evaluates an expression in the first argument. It returns the expression as value, and the variable as varname\=value:

   SET x=100
   SET y=80
   ZWRITE x+y,y

The following example compares ZWRITE and WRITE when displaying different variable values. ZWRITE returns quotation marks delimiting strings, WRITE does not:

   SET a\=+007.00
   SET b\=9E3
   SET c\="+007.00"
   SET d\=""
   SET e\="Rhode Island"
   SET f\="Rhode"\_"Island"
   ZWRITE a,b,c,d,e,f
   WRITE !,a,!,b,!,c,!,d,!,e,!,f

 

ZWRITE Displaying Subscripted Globals

The following example shows ZWRITE displaying the contents of subscripted process-private global variables. (ZWRITE displays process-private globals and conventional globals in the same way.) ZWRITE displays the subscripts of the variable in hierarchical order:

   SET ^||fruit(1)\="apple",^||fruit(4)\="banana",^||fruit(8)\="cherry"
   SET ^||fruit(1,1)\="Macintosh",^||fruit(1,2)\="Delicious",^||fruit(1,3)\="Granny Smith"
   SET ^||fruit(1,2,1)\="Red Delicious",^||fruit(1,2,2)\="Golden Delicious"
   SET ^||fruit\="Fruits"
   WRITE "global arg ZWRITE:",!
   ZWRITE ^||fruit

 

ZWRITE Displaying a Global and its Descendants

The following example shows ZWRITE displaying the contents of a subscripted global variable and all its descendent nodes. Note that the descendent nodes contain list structures, which are displayed as $LISTBUILD ($lb) constructions:

   ZNSPACE "SAMPLES"
   ZWRITE ^CinemaooFilmCategoryD

 

Additional non-printing characters used in lists are also displayed.

ZWRITE Displaying a Global in Another Namespace

The following example shows ZWRITE using extended global reference to display the contents of a subscripted global variable located in another namespace:

   ZNSPACE "SAMPLES"
   ZWRITE ^CinemaooFilmD(17)
   WRITE !,"Changing namespace",!!
   ZNSPACE "USER"
   ZWRITE ^\["SAMPLES"\]CinemaooFilmD(17)

 

ZWRITE always displays the extended global reference, regardless of the setting of the RefInKind method or property, which can be set to strip extended global references from globals returned by $QUERY or $NAME.

See Also

*   [WRITE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cwrite) command
    
*   [ZZDUMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czzdump) command
    
*   [ZZWRITE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czzwrite) command
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cztrap "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czzdump "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_czwrite.xml**