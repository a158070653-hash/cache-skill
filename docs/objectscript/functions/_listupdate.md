# $LISTUPDATE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_flistupdate

---

 

Caché ObjectScript Reference

$LISTUPDATE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flisttostring "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistvalid "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$LISTUPDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistupdate "$LISTUPDATE") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Updates a list by optionally replacing a specified list element or sequence of elements.

Synopsis

$LISTUPDATE(list,position,bool:val...)
$LU(list,position,bool:val...)

Parameters

list

Any expression that evaluates to a list. A list can be created using $LISTBUILD or $LISTFROMSTRING, or extracted from another list using $LIST. The null string ("") is also treated as a valid list.

position

A positive integer specifying the position in list to update, counting from 1. If position is larger than the number of elements in list, $LISTUPDATE appends the element, padding if necessary.

bool:

Optional — A boolean variable specifying whether or not to update the specified list element. If omitted, bool defaults to 1, causing this element to be updated.

value

The value used to update the list at the specified position. You can specify a comma-separated list of value parameters or bool:value pair parameters in any combination.

Description

$LISTUPDATE returns a copy of a list updated by replacing or adding one or more list elements by position. The position specifies the position in the list at which to begin updating elements. Elements are updated sequentially, starting from this position. The position can be larger than the number of elements in the list. If so, additional null elements are added to the list (if necessary) to insert a new element at the specified position. Note that position must be a positive integer; $LISTUPDATE cannot insert a new element at the beginning of the list. Setting position\=0 performs no operation. $LISTUPDATE also cannot specify end-of-list, except by position count from the beginning of the list.

$LISTUPDATE can specify or more than one bool:value pairs. Multiple bool:value pairs are separated by commas. These bool:value pairs update sequential elements, beginning with the position element. If bool\=1, Caché updates that element with value; if bool\=0, Caché does not update that element and proceeds to the next element.

Sequential elements that are not to be updated in a sequence of bool:value pairs do not have to be specified; they can be represented by placeholder commas. The bool: parameter is optional for each value; if omitted, it defaults to 1. If you omit bool also omit the colon (:) separator character. Thus the following are permitted ways to specify a bool:value pair:

*   Element to update: either 1:"newval", or "newval" (bool defaults to 1).
    
*   Element to not update: either 0:"newval", or a placeholder comma.
    

$LISTUPDATE is commonly used to return an updated version of an existing list. You can use $LISTUPDATE to create a list by specifying list\="".

You can also use [SET $LIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flist#RCOS_flist_set) to update one or more elements in an existing list.

Examples

The following example replaces the list element at position 2 with the specified value:

   SET caps\=1
   SET mylist \= $LISTBUILD("Red","White","Blue")
   SET newlist \= $LISTUPDATE(mylist,2,caps:"WHITE")
   ZWRITE newlist

 

The following example does not replace the list element at position 2 with the specified value:

   SET caps\=0
   SET mylist \= $LISTBUILD("Red","White","Blue")
   SET newlist \= $LISTUPDATE(mylist,2,caps:"WHITE")
   ZWRITE newlist

 

The following example replaces the list element at position 2 with null:

   SET caps\=1
   SET mylist \= $LISTBUILD("Red","White","Blue")
   SET newlist \= $LISTUPDATE(mylist,2,caps:"")
   ZWRITE newlist

 

The following example appends the list at position 7 with the specified value, padding null elements at positions 5 and 6:

   SET bool\=1
   SET mylist \= $LISTBUILD("Red","Orange","Yellow","Green")
   SET newlist \= $LISTUPDATE(mylist,7,bool:"Purple")
   ZWRITE newlist

 

The following example does not append the list at position 7 with the specified value. The unchanged list is returned with no null element padding:

   SET bool\=0
   SET mylist \= $LISTBUILD("Red","Orange","Yellow","Green")
   SET newlist \= $LISTUPDATE(mylist,7,bool:"Purple")
   ZWRITE newlist

 

The following three examples are all functionally identical. Each replaces the elements at positions 2 and 4, and appends a new element at position 7. It does not replace elements 3 and 5. Element 6 is created as a null element:

   SET mylist \= $LISTBUILD("Red","Orange","Yellow","Green","Blue")
   SET newlist \= $LISTUPDATE(mylist,2,1:"ORANGE",0:"YELLOW",
                                      1:"GREEN",0:"BLUE",
                                      0:"INDIGO",1:"VIOLET")
   ZWRITE newlist

 

Here the bool parameter is only specified when it is 0:

   SET mylist \= $LISTBUILD("Red","Orange","Yellow","Green","Blue")
   SET newlist \= $LISTUPDATE(mylist,2,"ORANGE",0:"YELLOW",
                                      "GREEN",0:"BLUE",
                                      0:"INDIGO","VIOLET")
   ZWRITE newlist

 

Here placeholder commas are used to skip elements that are not updated:

   SET mylist \= $LISTBUILD("Red","Orange","Yellow","Green","Blue")
   SET newlist \= $LISTUPDATE(mylist,2,"ORANGE",,"GREEN",,,"VIOLET")
   ZWRITE newlist

 

See Also

*   [$LIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flist) function
    
*   [$LISTBUILD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistbuild) function
    
*   [$LISTDATA](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistdata) function
    
*   [$LISTFIND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistfind) function
    
*   [$LISTFROMSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistfromstring) function
    
*   [$LISTGET](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistget) function
    
*   [$LISTLENGTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistlength) function
    
*   [$LISTNEXT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistnext) function
    
*   [$LISTSAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistsame) function
    
*   [$LISTTOSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flisttostring) function
    
*   [$LISTVALID](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistvalid) function
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flisttostring "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistvalid "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_flistupdate.xml**