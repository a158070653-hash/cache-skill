# $LISTBUILD

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_flistbuild

---

 

Caché ObjectScript Reference

$LISTBUILD

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flist "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistdata "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$LISTBUILD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistbuild "$LISTBUILD") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Builds a list of elements from the specified expressions.

Synopsis

$LISTBUILD(element,...) 
$LB(element,...)

SET $LISTBUILD(var1,var2,...)=list
SET $LB(var1,var2,...)=list

Parameters

element

An expression that specifies a list element value. Can be a single expression or an expression in a comma-separated list of expressions. A placeholder comma can be specified for an omitted element.

var

A variable, specified as a single variable or as a variable in a comma-separated list of variables. A placeholder comma can be specified for an omitted variable. A var may be a variable of any type: local, process-private, or global, unsubscripted or subscripted.

list

An expression that evaluates to a valid list. Because lists contain encoding, list must be created using $LISTBUILD or [$LISTFROMSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistfromstring), or extracted from another list using [$LIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flist).

Description

$LISTBUILD takes one or more expressions and returns an encoded list structure with one element for each expression. Elements are placed in the list in the order specified. Elements are counted from 1.

The following functions can be used to create a list:

*   $LISTBUILD, which creates a list from multiple data items (strings or numerics), one list element per data item. $LISTBUILD can also be used to create list elements containing no data.
    
*   $LISTFROMSTRING, which creates a list from a single string containing multiple delimited elements.
    
*   $LIST, which extracts a sublist from an existing list.
    
*   The null string ("") is also considered to be a valid list. The null string ("") is used to represent a null list, a list containing no elements. Because it contains no list elements, $LISTLENGTH("") returns an element count of 0.
    

You can use the [$LISTVALID](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistvalid) function to determine if an expression is a valid list.

$LISTBUILD is used with the other $LIST functions: $LISTDATA, $LISTFIND, $LISTGET, $LISTNEXT, $LISTLENGTH, $LISTSAME, and $LISTTOSTRING.

Note:

$LISTBUILD and the other $LIST functions use an optimized binary representation to store data elements. For this reason, equivalency tests may not work as expected when comparing encoded lists. Data that might, in other contexts, be considered equivalent, may have a different internal representation. For example, $LISTBUILD(1) is not equal to $LISTBUILD(“1”) and $LISTBUILD(1.0) is not equal to $LISTBUILD(1). However, list display functions, such as $LIST and $LISTTOSTRING return numeric list element values in [canonical form](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_numcanonical). Therefore $LIST($LISTBUILD(1),1)=$LIST($LISTBUILD("1"),1).

For the same reason, an encoded list value returned by $LISTBUILD should not be used in character search and parse functions that use a delimiter character, such as $PIECE and the two-argument form of $LENGTH. Elements in a list created by $LISTBUILD are not marked by a character delimiter, and thus can contain any character.

SET $LISTBUILD

When used on the left side of the equal sign in a SET command, the $LISTBUILD function extracts multiple elements from a list as a single operation. The syntax is as follows:

SET $LISTBUILD(var1,var2,...)=list

The var arguments of $LISTBUILD are a comma-separated list of variables, each of which receives the element of list that corresponds to its position in the $LISTBUILD parameter list. Variable names may be omitted (leaving the placeholder commas) for positions that are not of interest. Note that var arguments do not have to be existing variables, and that the number of var arguments (or placeholder commas) may be less than or greater than the number of list elements.

The maximum number of var arguments is 128. Attempting to exceed this number results in a compile error.

If a var argument is an object property (object.property) the property must be multidimensional. Any property may be referenced as an [i%property](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GOBJ_specialcos#GOBJ_specialcos_ipcpropname) instance variable within an object method.

In the following example, $LISTBUILD (on the right side of the equal sign) is first used to return a list. Then $LISTBUILD (on the left side of the equal sign) is used to extract two items from that list and set the appropriate variables:

   SET colorlist\=$LISTBUILD("red","blue","green","white")
   SET $LISTBUILD(a,,c)\=colorlist
   WRITE "a=",a,!,"c=",c

 

In this example, a="red" and c="green".

In the following example, $LISTBUILD (on the left side of the equal sign) attempts to extract the 2nd and 5th element from a list. Because the specified list does not have a 5th element, the corresponding var variable contains its initial default value:

   SET (a,b,c,d,e)\=0
   SET colorlist\=$LISTBUILD("red","blue","green","white")
   SET $LISTBUILD(,b,,,e)\=colorlist
   WRITE "b=",b,!,"e=",e

 

Unicode

If one or more characters in a list element is a wide (Unicode) character, all characters in that element are represented as wide characters. To ensure compatibility across systems, $LISTBUILD always stores these bytes the same way, regardless of the hardware platform. Wide characters are represented as byte strings.

Examples

Many of the examples shown here use the [$LISTTOSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flisttostring) function to convert the $LISTBUILD return value for display; $LISTBUILD returns an encoded string that cannot be displayed directly.

The following example produces the three-element list "Red,Blue,Green":

   SET colorlist\=$LISTBUILD("Red","Blue","Green")
   WRITE $LISTTOSTRING(colorlist,"^")

 

The following example creates a list of six numeric elements that display as "3^0^44^5.6^33^400". Note that $LISTBUILD encodes numeric element values based on an optimized binary representation, which may not be the same as canonical form. List display functions such as $LIST and $LISTTOSTRING return numeric element values in canonical form:

   SET numlist\=$LISTBUILD(003,0.00,44.0000000,5.6,+33,4E2)
   WRITE $LISTTOSTRING(numlist,"^")

 

Omitting Elements

Omitting an element expression defines an encoded element, but the data value of that element is undefined.

In the following example, the $LISTBUILD statements both produce a valid three-element list whose second element has an undefined value. Omitting an element and specifying an undefined variable for an element produces exactly the same result. In either case, referencing the second element with any list function (such as $LIST or $LISTTOSTRING) generates a <NULL VALUE> error:

   KILL a
   SET list1\=$LISTBUILD("Red",,"Green")
   SET list2\=$LISTBUILD("Red",a,"Green")
   WRITE "List lengths:",$LISTLENGTH(list1)," ",$LISTLENGTH(list2),!
   IF $LISTVALID(list1)\=1,$LISTVALID(list2)\=1 {
      WRITE "These are valid lists",! }
   IF list1\=list2 {WRITE "and they're identical"}
   ELSE {WRITE "They're not identical"}

 

The following example shows that an undefined element can be specified at the end of a list, as well as within a list. A list with trailing undefined elements is a valid list. However, referencing this undefined element with any list function generates a <NULL VALUE> error:

   KILL z
   SET list3\=$LISTBUILD("Red",)
   SET list4\=$LISTBUILD("Red",z)
   WRITE "List lengths:",$LISTLENGTH(list3)," ",$LISTLENGTH(list4),!
   IF $LISTVALID(list3)\=1,$LISTVALID(list4)\=1 {
      WRITE "These are valid lists",! }
   IF list3\=list4 {WRITE "and they're identical"}
   ELSE {WRITE "They're not identical"}

 

However, the following example produces a three-element list whose second element has a data value: the empty string. No error condition occurs when referencing the second element:

   SET list5\=$LISTBUILD("Red","","Green")
   SET list5len\=$LISTLENGTH(list5)
   WRITE "List length: ",list5len,!
   FOR i\=1:1:list5len {
      WRITE "Element ",i," value: ",$LIST(list5,i),! }

 

Lists with No Data or Null String Data

Any list created using $LISTBUILD contains at least one encoded list element. That element may or may not contain data. Because $LISTLENGTH counts elements, not data, any list created using $LISTBUILD has a list length of at least 1.

Referencing a $LISTBUILD element whose data value is undefined generates a <NULL VALUE> error. The following are all valid $LISTBUILD statements that create “empty” lists. However, attempting to reference an element is such a list results in a <NULL VALUE> error:

   TRY {
     SET x\=$LISTBUILD(NULL)
     SET y\=$LISTBUILD(,)
     SET z\=$LISTBUILD()
     IF $LISTVALID(x)\=1,$LISTVALID(y)\=1,$LISTVALID(z)\=1 {
        WRITE "These are valid lists",! }
     WRITE "$LB(NULL) contains ",$LISTLENGTH(x)," elements",!
     WRITE "$LB(,) contains ",$LISTLENGTH(y)," elements",!
     WRITE "$LB() contains ",$LISTLENGTH(z)," elements",!
     /\* Attempt to use null lists \*/
     WRITE "$LB(NULL) list value ",$LISTTOSTRING(x,"^"),!
     WRITE "$LB(,) list value ",$LISTTOSTRING(y,"^"),!
     WRITE "$LB() list value ",$LISTTOSTRING(z,"^"),!
   }
     CATCH exp { WRITE !!,"In the CATCH block",!
           IF 1\=exp.%IsA("%Exception.SystemException") {
              WRITE "System exception",!
              WRITE "Name: ",$ZCVT(exp.Name,"O","HTML"),!
              WRITE "Location: ",exp.Location,!
              WRITE "Code: "
            }
            ELSE { WRITE "Some other type of exception",! RETURN }
              WRITE exp.Code,!
              WRITE "Data: ",exp.Data,! 
              RETURN
     }

 

The following are valid $LISTBUILD statements that create a list element that contains data, though this data has a null string value:

  SET x\=$LISTBUILD("")
    WRITE "list contains ",$LISTLENGTH(x)," elements",!
    WRITE "list value is ",$LISTTOSTRING(x,"^"),!
  SET y\=$LISTBUILD($CHAR(0))
    WRITE "list contains ",$LISTLENGTH(y)," elements",!
    WRITE "list value is ",$LISTTOSTRING(y,"^")

 

Nesting Lists

An element of a list may itself be a list. For example, the following statement produces a three-element list whose third element is the two-element list, "Walnut,Pecan":

  SET nlist\=$LISTBUILD("Apple","Pear",$LISTBUILD("Walnut","Pecan"))
    WRITE "Nested list length is ",$LISTLENGTH($LIST(nlist,3)),!
    WRITE "Full list length is ",$LISTLENGTH(nlist),!
    WRITE "List is ",$LISTTOSTRING(nlist,"^"),!

 

Concatenating Lists

The result of concatenating two lists with the Concatenate operator (\_) is a list that combines the two lists.

In the following example, concatenating two lists creates a list that is identical to a list with the same elements created using LISTBUILD:

  SET list1\=$LISTBUILD("A","B")
  SET list2\=$LISTBUILD("C","D","E")
  SET clist\=list1\_list2
  SET list\=$LISTBUILD("A","B","C","D","E")
    IF clist\=list {WRITE "they're identical",!}
    ELSE {WRITE "they're not identical",!}
  WRITE "concatenated ",$LISTTOSTRING(clist,"^"),!
  WRITE "same list as ",$LISTTOSTRING(list,"^")

 

You cannot concatenate a string to a list. Attempting to do so generates a <LIST> error the first time you attempt to access the result:

   TRY {
   SET list\=$LISTBUILD("A","B")\_"C"
     WRITE "$LISTBUILD completed without error",!
   SET listlen\=$LISTLENGTH(list)
     WRITE "$LISTLENGTH completed without error",!
   SET listval\=$LISTTOSTRING(list,"^")
      WRITE "$LISTTOSTRING completed without error",!
   }
   CATCH exp { WRITE !!,"In the CATCH block",!
        IF 1\=exp.%IsA("%Exception.SystemException") {
           WRITE "System exception",!
           WRITE "Name: ",$ZCVT(exp.Name,"O","HTML"),!
           WRITE "Location: ",exp.Location,!
           WRITE "Code: "
        }
        ELSE { WRITE "Some other type of exception",! RETURN }
        WRITE exp.Code,!
        WRITE "Data: ",exp.Data,! 
        RETURN
   }

 

For further details on concatenation, see [Operators](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_operators) in Using Caché ObjectScript.

See Also

*   [SET](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cset) command
    
*   [ZZDUMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czzdump) command
    
*   [$LIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flist) function
    
*   [$LISTDATA](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistdata) function
    
*   [$LISTFIND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistfind) function
    
*   [$LISTFROMSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistfromstring) function
    
*   [$LISTGET](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistget) function
    
*   [$LISTLENGTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistlength) function
    
*   [$LISTNEXT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistnext)
    
*   [$LISTSAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistsame) function
    
*   [$LISTTOSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flisttostring) function
    
*   [$LISTUPDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistupdate) function
    
*   [$LISTVALID](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistvalid) function
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flist "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistdata "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_flistbuild.xml**