# $LISTNEXT

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_flistnext

---

 

Caché ObjectScript Reference

$LISTNEXT

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistlength "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistsame "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$LISTNEXT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistnext "$LISTNEXT") \]

Go to: Description Example See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Retrieves elements sequentially from a list.

Synopsis

$LISTNEXT(list,ptr,value)

Parameters

list

Any expression that evaluates to a list.

ptr

A pointer to the next element in the list. You must specify ptr as a local variable initialized to 0. This value points to the beginning of list. Caché increments ptr using an internal address value algorithm (not a predictable integer counter). Therefore, the only value you can use to set ptr is 0. ptr cannot be a global variable or a subscripted variable.

value

A local variable used to hold the data value of a list element. value does not have to be initialized before invoking $LISTNEXT. value cannot be a global variable or a subscripted variable.

Description

$LISTNEXT sequentially returns elements in a list. You initialize ptr to 0 before the first invocation of $LISTNEXT. This causes $LISTNEXT to begin returning elements from the beginning of the list. Each successive invocation of $LISTNEXT advances ptr and returns the next list element value to value. The $LISTNEXT function returns 1, indicating that a list element has been successfully retrieved.

When $LISTNEXT reaches the end of the list, it returns 0, resets ptr to 0, and leaves value unchanged from the previous invocation. Because ptr has been reset to 0, the next invocation of $LISTNEXT would start at the beginning of the list.

Note:

Because ptr is an index into the internal structure of list, the list should not be modified while $LISTNEXT is being used on it. Modifying list may make the ptr value invalid and cause the next invocation of $LISTNEXT to issue a <FUNCTION> error.

You can use $LISTVALID to determine if list is a valid list. An invalid list causes $LISTNEXT to generate a <LIST> error.

When $LISTNEXT encounters an omitted list element (an element with a null value), it returns 1 indicating that a list element has been successfully retrieved, advances ptr to the next element, and resets value to be an undefined variable. This can happen when encountering an omitted list element, such as the second invocation of $LISTNEXT on list\=$LB("a",,"b"), or with any of the following valid lists: list\=$LB(), list\=$LB(NULL), or list\=$LB(,).

$LISTNEXT("",ptr,value) returns 0, and does not advance the pointer or set value. $LISTNEXT($LB(""),ptr,value) returns 1, advances the pointer, and set value to the null string ("").

$LISTNEXT and Performance

A Caché list is the most efficient way to process large numbers of data values. Because [long strings](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_strings_long) are active by default, you can use lists to hold large numbers of values for processing, rather than using an array or other data structure. Using $LISTNEXT to return a large number of elements from a list is substantially more efficient than using $LIST to perform the same operation.

The following example rapidly accesses the elements in mylist:

    SET ptr\=0
    WHILE $LISTNEXT(mylist,ptr,value) {
          /\* perform some operation on value \*/
    }

It is substantially faster than the following equivalent example:

    FOR i\=1:1:$LISTLENGTH(mylist) {
        SET value\=$LIST(mylist,i)
        /\* perform some operation on value \*/
    }

$LISTNEXT and Nested Lists

The following example returns three elements, because $LISTNEXT does not recognize the individual elements in nested lists:

   SET list\=$LISTBUILD("Apple","Pear",$LISTBUILD("Walnut","Pecan"))
   SET ptr\=0,count\=0
   WHILE $LISTNEXT(list,ptr,value) {
      SET count\=count+1
      WRITE !,value 
   }
   WRITE !,"End of list: ",count," elements found"
   QUIT

 

Example

The following example sequentially returns all the elements in the list. When it encounters an omitted element, the [$SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fselect) returns the default value “omitted”:

   SET list\=$LISTBUILD("Red","Blue",,"Green")
   SET ptr\=0,count\=0
   WHILE $LISTNEXT(list,ptr,value) {
      SET count\=count+1
      WRITE !,count,": ",$SELECT($DATA(value):value,1:"omitted")
   }
   WRITE !,"End of list: ",count," elements found"
   QUIT

 

See Also

*   [$LIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flist) function
    
*   [$LISTBUILD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistbuild) function
    
*   [$LISTDATA](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistdata) function
    
*   [$LISTFIND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistfind) function
    
*   [$LISTFROMSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistfromstring) function
    
*   [$LISTGET](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistget) function
    
*   [$LISTLENGTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistlength) function
    
*   [$LISTSAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistsame) function
    
*   [$LISTTOSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flisttostring) function
    
*   [$LISTUPDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistupdate) function
    
*   [$LISTVALID](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistvalid) function
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistlength "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistsame "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_flistnext.xml**