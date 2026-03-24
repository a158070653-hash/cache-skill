# $LISTDATA

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_d_listdata

---

 

Caché SQL Reference

$LISTDATA

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listbuild "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listfind "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [$LISTDATA](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listdata "$LISTDATA") \]

Go to: Description Arguments Examples Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A list function that indicates whether the specified element exists and has a data value.

Synopsis

$LISTDATA(list\[,position\])

Arguments

list

An expression that evaluates to a valid list. A list is an encoded character string containing one or more elements. You can create a list using the SQL or Caché ObjectScript $LISTBUILD or $LISTFROMSTRING functions. You can extract a list from an existing list using the SQL or Caché ObjectScript $LIST function.

position

Optional — An integer expression specifying an element in list.

Description

$LISTDATA checks for data in the requested element in a list. $LISTDATA returns a value of 1 if the element indicated by the position argument is in the list and has a data value. $LISTDATA returns a value of a 0 if the element is not in the list or does not have a data value.

This function returns data of type SMALLINT.

Arguments

list

An encoded character string containing one or more elements. You can create a list using the SQL [$LISTBUILD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listbuild) function or the Caché ObjectScript [$LISTBUILD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistbuild) function. You can convert a delimited string into a list using the SQL [$LISTFROMSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listfromstring) function or the Caché ObjectScript [$LISTFROMSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistfromstring) function. You can extract a list from an existing list using the SQL [$LIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_list) function or the Caché ObjectScript [$LIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flist) function.

position

If you omit the position argument, $LISTDATA evaluates the first element. If the value of the position argument is -1, it is equivalent to specifying the final element of the list. If the value of the position argument refers to a nonexistent list member, $LISTDATA returns 0.

Examples

The following Embedded SQL examples show the results of the various values of the position argument.

All of the following $LISTDATA statements return a value of 1:

  KILL Y
  SET a\=$LISTBUILD("Red",,Y,"","Green")
  &sql(SELECT $LISTDATA(:a), $LISTDATA(:a,1), 
       $LISTDATA(:a,4), $LISTDATA(:a,5), $LISTDATA(:a,\-1)
       INTO :b,:c, :d, :e, :f)
  IF SQLCODE'=0 {
    WRITE !,"Error code ",SQLCODE }
  ELSE {
    WRITE !,"1st element status  ",b  ; 1st element default
    WRITE !,"1st element status  ",c  ; 1st element specified
    WRITE !,"4th element status  ",d  ; 4th element null string
    WRITE !,"5th element status  ",e  ; 5th element in 5-element list
    WRITE !,"last element status ",f  ; last element in 5-element list
  }

 

The following $LISTDATA statements return a value of 0 for the same five-element list:

  KILL Y
  SET a\=$LISTBUILD("Red",,Y,"","Green")
  &sql(SELECT $LISTDATA(:a,2), $LISTDATA(:a,3), 
       $LISTDATA(:a,0), $LISTDATA(:a,6)
       INTO :b,:c, :d, :e)
  IF SQLCODE'=0 {
    WRITE !,"Error code ",SQLCODE }
  ELSE {
    WRITE !,"2nd element status ",b  ; 2nd element is undefined
    WRITE !,"3rd element status ",c  ; 3rd element is killed variable
    WRITE !,"0th element status ",d  ; zero position nonexistent
    WRITE !,"6th element status ",e  ; 6th element in 5-element list
  }

 

Notes

Invalid Argument Values

If the expression in the list argument does not evaluate to a valid list, an SQLCODE -400 fatal error occurs:

   &sql(SELECT $LISTDATA('fred') INTO :b)
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
   WRITE !,"The the element is ",b }

 

If the value of the position argument is less than -1, an SQLCODE -400 fatal error occurs:

   SET a\=$LISTBUILD("Red","Blue","Green")
   &sql(SELECT $LISTDATA(:a,\-3) INTO :c)
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
   WRITE !,"A neg-num position status ",c }

 

This does not occur when position is a nonnumeric value:

   SET a\=$LISTBUILD("Red","Blue","Green")
   &sql(SELECT $LISTDATA(:a,'g') INTO :c)
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"Error code ",SQLCODE
     WRITE !,"A nonnumeric position status ",c }

 

See Also

*   SQL functions: [$LIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_list), [$LISTBUILD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listbuild), [$LISTFIND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listfind), [$LISTFROMSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listfromstring), [$LISTGET](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listget), [$LISTLENGTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listlength), [$LISTSAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listsame), [$LISTTOSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listtostring), [$PIECE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_piece)
    
*   Caché ObjectScript functions: [$LIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flist), [$LISTBUILD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistbuild), [$LISTDATA](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistdata), [$LISTFIND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistfind), [$LISTFROMSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistfromstring), [$LISTGET](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistget), [$LISTLENGTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistlength), [$LISTNEXT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistnext), [$LISTSAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistsame), [$LISTTOSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flisttostring), [$LISTVALID](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistvalid)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listbuild "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listfind "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_d\_listdata.xml**