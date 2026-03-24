# $QUERY

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fquery

---

 

Caché ObjectScript Reference

$QUERY

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fqsubscript "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_frandom "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$QUERY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fquery "$QUERY") \]

Go to: Description Parameters Example Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Performs a physical scan of a local or global array.

Synopsis

$QUERY(reference,direction,target)
$Q(reference,direction,target)

Parameters

reference

A reference that evaluates to the name (and optionally subscripts) of a public local or global variable.

direction

Optional — The direction (forwards or backwards) to traverse the array.

target

Optional — Returns the current data value of the local or global variable specified in reference.

Description

$QUERY performs a physical scan of a public local or global array; it returns the full reference, name and subscripts, of the defined node next in sequence to the specified array node. If no such node exists, $QUERY returns the null string.

Parameters

reference

This parameter must evaluate to a public variable or a global. $QUERY cannot scan a private variable.

The returned global reference can be at the same level, a lower level, or a higher level as the level specified in the reference parameter. If you specify reference without specifying subscripts, $QUERY returns the first defined node in the array.

direction

If no direction is specified, the default direction is forward. If you wish to specify a direction, a parameter value of 1 will traverse the array forward, a value of -1 will traverse the array backward.

target

If you wish to specify a target, you must specify a direction. If the variable identified in the reference parameter is undefined, the target value remains unchanged.

The ZBREAK command cannot specify the target parameter as a watchpoint.

Example

This example presents a generalized routine for outputting the data values for all the nodes in a user-specified array. It can accommodate arrays with any number of levels. The code performs the same operation as the code shown in the example under the $ORDER function. Instead of requiring 23 lines, however, it requires only six and is not restricted as to the number of levels it can handle.

Start  READ !,"Array name: ",ary QUIT:ary\=""
  SET queryary\=$QUERY(@ary@(""))
  WRITE !,@queryary
  FOR   {
    SET queryary\=$QUERY(@queryary) 
        QUIT:queryary\=""  
        WRITE !,@queryary
  }
  WRITE !,"Finished."
  QUIT

The first SET command uses $QUERY with subscript indirection to initialize the global reference to the first existing node that contains data. For more information, refer to [Indirection](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_operators#GCOS_operators_indirection) in Using Caché ObjectScript. (Like $ORDER, $QUERY accepts a null string to designate the first subscript in an array.) The first WRITE command outputs the value of the first node found. If it were omitted, the first node would be skipped.

In the FOR loop, $QUERY is used to retrieve the next node and update the global reference, whose contents are then output by the WRITE command. The postconditional QUIT terminates the loop when it finds a null string (""), indicating that $QUERY has reached the end of the array.

No $DATA tests are required, unless you wish to distinguish between pointer nodes ($DATA=11) and terminal nodes ($DATA=1).

Notes

Using $QUERY to Traverse an Array

Used repetitively, $QUERY can traverse an entire array in left-to-right, top-to-bottom fashion, returning the entire sequence of defined nodes. $QUERY can start at the point determined by the subscript specified for reference. It proceeds along both the horizontal and vertical axes. For example:

   SET exam\=$QUERY(^client(4,1,2))

Based on this example, $QUERY might return any of the following values, assuming a three-level array:

Value

Returned by the $QUERY Function If...

^client(4,1,3)

If ^client(4,1,3) exists and contains data.

^client(4,2)

If ^client(4,1,3) does not exist or does not contain data and if ^client(4,2) does exist and contains data.

^client(5)

If ^client(4,1,3) and ^client(4,2) do not exist or do not contain data and if ^client(5) does exist and contains data.

null string ("")

If none of the previous global references exist or contain data; $QUERY has reached the end of the array.

With a direction value of -1, $QUERY can traverse an entire array in reverse order in right-to-left, bottom-to-top fashion.

$QUERY Compared to $ORDER

$QUERY differs from the [$ORDER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_forder) function in that $QUERY returns a full global reference, while $ORDER returns only the subscript of the next node. $ORDER proceeds along only the horizontal axis, across nodes at one level.

$QUERY also differs from $ORDER in that it selects only those existing nodes that contain data. $ORDER selects existing nodes, regardless of whether or not they contain data. Where $ORDER performs an implicit test for existence ($DATA'=0), $QUERY performs an implicit test for both existence and data ($DATA'=0 and $DATA'=10). Note, however, that $QUERY does not distinguish between pointer nodes ($DATA=11) and terminal nodes ($DATA=1) that contain data. To make this distinction, you must include appropriate $DATA tests in your code.

Like $ORDER, $QUERY is typically used with loop processing to traverse the nodes in an array that doesn’t use consecutive integer subscripts. $QUERY simply returns the global reference of the next node with a value. $QUERY provides very compact code for accessing global arrays.

Like the [$NAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fname) and $ORDER functions, $QUERY can be used with a [naked global reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GGBL_using), which is specified without the array name and designates the most recently executed global reference. For example:

   SET a\=^client(1)
   SET x\=2
   SET z\=$QUERY(^(x))

The first SET command establishes the current global reference, including the level for the reference. The second SET command sets up a variable for use with subscripts. The $QUERY function uses a naked reference to return the full global reference for the next node following ^client(2). For example, the returned value might be ^client(2,1) or ^client(3).

$QUERY and Extended Global References

You can control whether $QUERY returns global references in Extended Global Reference form on a per-process basis using the [RefInKind()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process#RefInKind) method of the [%SYSTEM.Process](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process) class. The system-wide default behavior can be established by setting the [RefInKind](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=Config.Miscellaneous#RefInKind) property of the Config.Miscellaneous class.

For further details on extended global references, see the [Global Structure](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GGBL_structure) chapter in Using Caché Globals.

See Also

*   [$DATA](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fdata) function
    
*   [$NAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fname) function
    
*   [$ORDER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_forder) function
    
*   [$QLENGTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fqlength) function
    
*   [$QSUBSCRIPT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fqsubscript) function
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fqsubscript "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_frandom "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fquery.xml**