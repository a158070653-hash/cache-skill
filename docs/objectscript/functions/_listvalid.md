# $LISTVALID

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_flistvalid

---

 

Caché ObjectScript Reference

$LISTVALID

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistupdate "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flocate "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$LISTVALID](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistvalid "$LISTVALID") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Determines if an expression is a list.

Synopsis

$LISTVALID(exp)
$LV(exp)

Parameter

exp

Any valid expression.

Description

$LISTVALID determines whether exp is a list, and returns a Boolean value: If exp is a list, $LISTVALID returns 1; if exp is not a list, $LISTVALID returns 0.

A list can be created using $LISTBUILD or $LISTFROMSTRING, or extracted from another list using $LIST. A list containing the empty string ("") as its sole element is a valid list. The empty string ("") by itself is also considered a valid list.

Examples

The following examples all return 1, indicating a valid list:

   SET w \= $LISTBUILD("Red","Blue","Green")
   SET x \= $LISTBUILD("Red")
   SET y \= $LISTBUILD(365)
   SET z \= $LISTBUILD("")
   WRITE !,$LISTVALID(w)
   WRITE !,$LISTVALID(x)
   WRITE !,$LISTVALID(y)
   WRITE !,$LISTVALID(z)

 

The following examples all return 0. Numbers and strings (with the exception of the null string) are not valid lists:

   SET x \= "Red"
   SET y \= 44
   WRITE !,$LISTVALID(x)
   WRITE !,$LISTVALID(y)

 

The following examples all return 1. Concatenated, nested, and omitted value lists are all valid lists:

   SET w\=$LISTBUILD("Apple","Pear")
   SET x\=$LISTBUILD("Walnut","Pecan")
   SET y\=$LISTBUILD("Apple","Pear",$LISTBUILD("Walnut","Pecan"))
   SET z\=$LISTBUILD("Apple","Pear",,"Pecan")
   WRITE !,$LISTVALID(w\_x) ; concatenated
   WRITE !,$LISTVALID(y)   ; nested
   WRITE !,$LISTVALID(z)   ; omitted element

 

The following examples all return 1. $LISTVALID considers all of the following “empty” lists as valid lists:

  WRITE $LISTVALID(""),!
  WRITE $LISTVALID($LB()),!
  WRITE $LISTVALID($LB(NULL)),!
  WRITE $LISTVALID($LB("")),!
  WRITE $LISTVALID($LB($CHAR(0))),!
  WRITE $LISTVALID($LB(,))

 

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
    
*   [$LISTUPDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistupdate) function
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistupdate "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flocate "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_flistvalid.xml**