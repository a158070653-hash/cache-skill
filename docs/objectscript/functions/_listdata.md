# $LISTDATA

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_flistdata

---

 

Caché ObjectScript Reference

$LISTDATA

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistbuild "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistfind "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$LISTDATA](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistdata "$LISTDATA") \]

Go to: Description Parameters Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Indicates whether the specified element exists and has a data value.

Synopsis

$LISTDATA(list,position,var)
$LD(list,position,var)

Parameters

list

An expression that evaluates to a valid list.

position

Optional — An expression interpreted as a position in the specified list. Either a positive, non-zero integer or -1.

var

Optional — A variable that contains the element value at the specified list position. If $LISTDATA returns a value of a 1, var is written; if $LISTDATA returns a value of a 0, var is unchanged.

Description

$LISTDATA checks for data in the requested element in a list and returns a boolean value. $LISTDATA returns a value of 1 if the element indicated by the position parameter is in the list and has a data value. $LISTDATA returns a value of a 0 if the element is not in the list or does not have a data value.

Optionally, $LISTDATA can write the element value to the var variable.

Note:

$LISTDATA should not be used in a loop structure to return multiple successive element values. While this will work, it is highly inefficient, because $LISTDATA must evaluate the list from the beginning with each iteration. The [$LISTNEXT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistnext) function is a far more efficient way to return multiple successive element values.

Parameters

list

A list is an encoded string containing multiple elements. A list must have been created using [$LISTBUILD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistbuild) or [$LISTFROMSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistfromstring), or extracted from another list using $LIST.

You can use the [$LISTVALID](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistvalid) function to determine if an expression is a valid list. If the expression in the list parameter does not evaluate to a valid list, a <LIST> error occurs. If a valid list contains no data at the specified position, $LISTDATA returns 0.

position

The integer position of the element in the list, counting from 1. If you omit the position parameter, $LISTDATA evaluates the first element. If the value of the position parameter is -1, it is equivalent to specifying the final element of the list.

$LISTDATA returns 0 if position refers to a nonexistent list member. A position of 0 always returns 0. If the value of position is less than -1, invoking the $LISTDATA function generates a <RANGE> error.

var

If $LISTDATA returns a value of a 1, Caché writes the value of the requested element to var. If $LISTDATA returns a value of a 0, var is unchanged. The var parameter can be a local, global, or process-private variable, with or without subscripts. It does not need to be defined; the first call to $LISTDATA that returns 1 defines and sets var. If the first call to $LISTDATA returns 0, var remains undefined.

The var parameter cannot be a non-multidimensional object property. Attempting to write a value to a non-multidimensional object property results in an <OBJECT DISPATCH> error.

The var parameter cannot be a special variable. Attempting to write a value to a special variable results in a <SYNTAX> error.

Examples

The following two examples show the results of the various values of the position parameter.

The following $LISTDATA statements return a value of 0:

   KILL y
   SET x\=$LISTBUILD("Red",,y,"","Green",)
   WRITE !,$LISTDATA(x,2)  ; second element is undefined
   WRITE !,$LISTDATA(x,3)  ; third element is a killed variable
   WRITE !,$LISTDATA(x,\-1) ; the last element is undefined
   WRITE !,$LISTDATA(x,0)  ; the 0th position
   WRITE !,$LISTDATA(x,6)  ; 6th position in 5-element list

 

The following $LISTDATA statements return a value of 1:

   SET x\=$LISTBUILD("Red",,y,"","Green",)
   WRITE !,$LISTDATA(x)    ; first element (by default)
   WRITE !,$LISTDATA(x,1)  ; first element specified
   WRITE !,$LISTDATA(x,4)  ; fourth element, value=null string
   WRITE !,$LISTDATA(x,5)  ; fifth element

 

The following 3-parameter $LISTDATA statement tests for the presence of an element value and updates the evalue variable with that value. Note that when $LISTDATA returns 0, evalue remains unchanged:

   SET x\=$LISTBUILD("Red",,y,"","Green",)
   FOR i\=1:1:$LISTLENGTH(x) {
      WRITE "element ",i," data? ",$LISTDATA(x,i,evalue)," value ",evalue,! 
   }
   WRITE i," list elements"

 

All of the following $LISTDATA statements return a value of 0:

   WRITE !,$LISTDATA($LB())     ; null list
   WRITE !,$LISTDATA($LB(NULL)) ; null list
   WRITE !,$LISTDATA("")        ; null string is a valid list
                                ; but contain no data
   WRITE !,$LISTDATA($LB(,))    ; two-element null list

 

The following $LISTDATA statements return a value of 1:

  WRITE !,$LISTDATA($LB(""))       ; data is null string
  WRITE !,$LISTDATA($LB($CHAR(0))) ; data is non-display character

 

See Also

*   [$LIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flist) function
    
*   [$LISTBUILD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistbuild) function
    
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

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistbuild "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistfind "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_flistdata.xml**