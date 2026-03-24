# $LISTLENGTH

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_flistlength

---

 

Caché ObjectScript Reference

$LISTLENGTH

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistget "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistnext "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$LISTLENGTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistlength "$LISTLENGTH") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Returns the number of elements in a specified list.

Synopsis

$LISTLENGTH(list)
$LL(list)

Parameter

list

Any expression that evaluates to a list. A list can be created using $LISTBUILD or $LISTFROMSTRING, or extracted from another list using $LIST.

Description

$LISTLENGTH returns the number of elements in list. $LISTLENGTH counts as a list element every designated list position, whether or not that position contains data.

You can use the [$LISTVALID](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistvalid) function to determine if list is a valid list. If list is not a valid list, Caché generates a <LIST> error.

An “empty” list created by $LISTBUILD defines an encoded list element, although that list element contains no data. Because $LISTLENGTH counts list elements (not elements containing data), an “empty” list has a $LISTLENGTH count of 1.

The null string ("") is used to represent a null list, a list containing no elements. Because it contains no list elements, it has a $LISTLENGTH count of 0.

Examples

The following example returns 3, because there are 3 elements in the list:

   WRITE $LISTLENGTH($LISTBUILD("Red","Blue","Green"))

 

The following example also returns 3, even though the second element in the list contains no data:

   WRITE $LISTLENGTH($LISTBUILD("Red",,"Green"))

 

The following examples all return 1. $LISTLENGTH makes no distinction between an empty list element and a list element containing data:

  WRITE $LISTLENGTH($LB()),!
  WRITE $LISTLENGTH($LB(NULL)),!
  WRITE $LISTLENGTH($LB("")),!
  WRITE $LISTLENGTH($LB($CHAR(0))),!
  WRITE $LISTLENGTH($LB("John Smith"))

 

The following example returns 0. [$LISTVALID](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistvalid) considers the null string a valid list, but it contains no list elements:

   WRITE $LISTLENGTH("")

 

The following example returns 3, because the two placeholder commas represent 3 empty list elements:

   WRITE $LISTLENGTH($LB(,,))

 

$LISTLENGTH and Concatenation

Concatenating two lists always results in a $LISTLENGTH equal to the sum of the lengths of the lists. This is true even when concatenating empty lists, or concatenating a null string to a list.

The following example all return a list length of 3:

  WRITE $LISTLENGTH($LB()\_$LB("a","b")),!
  WRITE $LISTLENGTH($LB("a")\_$LB(NULL)\_$LB("c")),!
  WRITE $LISTLENGTH($LB("")\_$LB()\_$LB(NULL)),!
  WRITE $LISTLENGTH(""\_$LB("a","b","c")),!
  WRITE $LISTLENGTH($LB("a","b")\_""\_$LB("c"))

 

$LISTLENGTH and Nested Lists

The following example returns 3, because $LISTLENGTH does not recognize the individual elements in a nested list, and treats it as a single list element:

   WRITE $LISTLENGTH($LB("Apple","Pear",$LB("Walnut","Pecan")))

 

The following examples all return 1, because $LISTLENGTH counts only the outermost nested list:

   WRITE $LISTLENGTH($LB($LB($LB()))),!
   WRITE $LISTLENGTH($LB($LB($LB("Fred")))),!
   WRITE $LISTLENGTH($LB($LB("Barney"\_$LB("Fred")))),!
   WRITE $LISTLENGTH($LB("Fred"\_$LB("Barney"\_$LB("Wilma"))))

 

See Also

*   [$LIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flist) function
    
*   [$LISTBUILD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistbuild) function
    
*   [$LISTDATA](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistdata) function
    
*   [$LISTFIND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistfind) function
    
*   [$LISTFROMSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistfromstring) function
    
*   [$LISTGET](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistget) function
    
*   [$LISTNEXT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistnext)
    
*   [$LISTSAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistsame) function
    
*   [$LISTTOSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flisttostring) function
    
*   [$LISTUPDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistupdate) function
    
*   [$LISTVALID](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistvalid) function
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistget "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistnext "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_flistlength.xml**