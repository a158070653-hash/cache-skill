# $ZORDER

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_vzorder

---

 

Caché ObjectScript Reference

$ZORDER

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vznspace "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzparent "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Special Variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_VARIABLES "Caché ObjectScript Special Variables") \]  >  \[ [$ZORDER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzorder "$ZORDER") \]

Go to: Description Example See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Contains the value of the next global node.

Synopsis

$ZORDER
$ZO

Description

$ZORDER contains the value of the next global node (in $QUERY sequence, not $ORDER sequence), after the current global reference. If there is no next global node, accessing $ZORDER results in an <UNDEFINED> error, indicating the last global successfully accessed by $ZORDER. For further details on <UNDEFINED> errors, refer to $ZERROR.

This special variable cannot be modified using the SET command. Attempting to do so results in a <SYNTAX> error.

Example

The following example uses a WHILE loop to repeatedly call $ZORDER to traverse a series of subscript nodes:

  SET ^||a\="groceries"
  SET ^||a(1)\="fruit"
  SET ^||a(1,1)\="apples"
  SET ^||a(1,2)\="oranges"
  SET ^||a(3)\="nuts"
  SET ^||a(3,1)\="peanuts"
  SET ^||a(2)\="vegetables"
  SET ^||a(2,1)\="lettuce"
  SET ^||a(2,2)\="tomatoes"
  SET ^||a(2,1,1)\="iceberg"
  SET ^||a(2,1,2)\="romaine"
  SET $ZERROR\="unset"
  WRITE !,"last referenced: ",^||a(1,1)
  WHILE $ZERROR\="unset" {
      WRITE !,$ZORDER }
  QUIT

 

The above example starts with the last-referenced global (in this case, process-private globals): ^||a(1,1). $ZORDER does not contain the value of ^||a(1,1), but works forward from that point. Calls to $ZORDER traverse the subscript tree nodes in the following order: (1,2), (2), (2,1), (2,1,1), (2,1,2), (2,2), (3), (3,1). Each WRITE $ZORDER displays the data value in each successive node. It then runs out of nodes and generates the following error: <UNDEFINED> ^||a(3,1). Note that ^||a(3,1) is not undefined; it is specified because $ZORDER could not find another global after this one.

See Also

*   [$ORDER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_forder) function
    
*   [$QUERY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fquery) function
    
*   [$ZORDER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzorder) function
    
*   [$ZERROR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzerror) special variable
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vznspace "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzparent "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_vzorder.xml**