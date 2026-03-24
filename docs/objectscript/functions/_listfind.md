# $LISTFIND

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_flistfind

---

 

Caché ObjectScript Reference

$LISTFIND

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistdata "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistfromstring "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$LISTFIND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistfind "$LISTFIND") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Searches a specified list for the requested value.

Synopsis

$LISTFIND(list,value,startafter)
$LF(list,value,startafter)

Parameters

list

An expression that evaluates to a valid list. A list is an encoded string containing one or more elements. A list must be created using [$LISTBUILD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistbuild) or [$LISTFROMSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistfromstring), or extracted from another list using $LIST.

value

An expression containing the desired element value.

startafter

Optional — An integer expression interpreted as a list position. The search starts with the element after this position; thus 0 means to start with position 1, 1 means to start with position 2. startafter\=-1 is a valid value, but always returns no match. Only the integer portion of the startafter value is used.

Description

$LISTFIND searches the specified list for the first instance of the requested value. A match must be exact and consist of the full element value. Letter comparisons are case-sensitive. Numbers are compared in canonical form. If an exact match is found, $LISTFIND returns the position of the matching element. If value is not found, $LISTFIND returns a 0.

The search begins with the element after the position indicated by the startafter parameter. If you omit the startafter parameter, $LISTFIND assumes a startafter value of 0 and starts the search with the first element (element 1).

If no match is found, $LISTFIND returns a 0. $LISTFIND will also return a 0 if the value of the startafter parameter refers to a nonexistent list member.

You can use the [$LISTVALID](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistvalid) function to determine if list is a valid list. If list is not a valid list, Caché generates a <LIST> error.

If the value of the startafter parameter is less than -1, invoking the $LISTFIND function generates a <RANGE> error.

Empty Strings and Empty Lists

The $LISTFIND function can be used to locate an empty string value, as shown in the following example:

   SET x\=$LISTBUILD("A","","C","D")
   WRITE $LISTFIND(x,"")   ; returns 2

 

$LISTFIND can be used with lists containing omitted elements, but cannot be used to locate an omitted element. The following example finds a value in a list with omitted elements:

   SET x\=$LISTBUILD("A",,"C","D")
  WRITE $LISTFIND(x,"C")   ; returns 3

 

The following $LISTFIND example returns 1:

  WRITE $LISTFIND($LB(""),"")   ; returns 1

 

The following $LISTFIND examples returns 0:

  WRITE $LISTFIND("",""),!     ; returns 0
  WRITE $LISTFIND($LB(),""),!  ; returns 0

 

The following examples list consists of an empty list concatenated to a list containing data. Appending the empty list changes the list position of elements in the resulting concatenated list:

   SET x\=$LISTBUILD("A","B","C","D")
   WRITE $LISTFIND(x,"B"),!         ; returns 2
   WRITE $LISTFIND(""\_x,"B"),!      ; returns 2
   WRITE $LISTFIND($LB()\_x,"B"),!   ; returns 3
   WRITE $LISTFIND($LB(,,,)\_x,"B")  ; returns 6

 

However, concatenating a null string to value has no effect on $LISTFIND:

   SET x\=$LISTBUILD("A","B","C","D")
   WRITE $LISTFIND(x,"B"),!      ; returns 2
   WRITE $LISTFIND(x,"B"\_""),!   ; returns 2
   WRITE $LISTFIND(x,""\_"B"),!   ; returns 2

 

Examples

The following example returns 2, the position of the first occurrence of the requested string:

   SET x\=$LISTBUILD("A","B","C","D")
   WRITE $LISTFIND(x,"B")

 

The following example returns 0, indicating the requested string was not found:

   SET x\=$LISTBUILD("A","B","C","D")
   WRITE $LISTFIND(x,"E")

 

The following examples show the effect of using the startafter parameter. The first example does not find the requested string and returns 0 because the string occurs at the startafter position:

   SET x\=$LISTBUILD("A","B","C","D")
   WRITE $LISTFIND(x,"B",2)

 

The second example finds the second occurrence of the requested string and returns 4, because the first occurs before the startafter position:

   SET y\=$LISTBUILD("A","B","C","A")
   WRITE $LISTFIND(y,"A",2)

 

The $LISTFIND function only matches complete elements. Thus, the following example returns 0 because no element of the list is equal to the string “B”, though all of the elements contain “B”:

   SET mylist \= $LISTBUILD("ABC","BCD","BBB")
   WRITE $LISTFIND(mylist,"B")

 

The following numeric examples all return 0, because numbers are converted to canonical form before matching. In these cases, the string numeric value and the canonical form number do not match:

   SET y\=$LISTBUILD("1.0","+2","003","2\*2")
   WRITE $LISTFIND(y,1.0),!
   WRITE $LISTFIND(y,+2),!
   WRITE $LISTFIND(y,003),!
   WRITE $LISTFIND(y,4)

 

The following numeric examples match because numeric values are compared in their canonical forms:

   SET y\=$LISTBUILD(7.0,+6,005,2\*2)
   WRITE $LISTFIND(y,++7.000),!   ; returns 1
   WRITE $LISTFIND(y,0006),!      ; returns 2
   WRITE $LISTFIND(y,8\-3),!       ; returns 3
   WRITE $LISTFIND(y,\-\-4.0)       ; returns 4

 

The following examples all return 0, because the specified startafter value results in no match:

   SET y\=$LISTBUILD("A","B","C","D")
   WRITE $LISTFIND(y,"A",1),!
   WRITE $LISTFIND(y,"B",2),!
   WRITE $LISTFIND(y,"B",99),!
   WRITE $LISTFIND(y,"B",\-1)

 

The following example shows how $LISTFIND can be used to find a nested list. Note that Caché treats a multi-element nested list as a single list element with a list value:

   SET y\=$LISTBUILD("A",$LB("x","y"),"C","D")
   WRITE $LISTFIND(y,$LB("x","y"))

 

See Also

*   [$LIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flist) function
    
*   [$LISTBUILD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistbuild) function
    
*   [$LISTDATA](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistdata) function
    
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

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistdata "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistfromstring "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_flistfind.xml**