# $LISTGET

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_d_listget

---

 

Caché SQL Reference

$LISTGET

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listfromstring "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listlength "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [$LISTGET](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listget "$LISTGET") \]

Go to: Description Arguments Examples Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A list function that returns an element in a list or a specified default value.

Synopsis

$LISTGET(list\[,position\[,default\]\])

Arguments

list

An expression that evaluates to a valid list. A list is an encoded character string containing one or more elements. You can create a list using the SQL or Caché ObjectScript $LISTBUILD or $LISTFROMSTRING functions. You can extract a list from an existing list using the SQL or Caché ObjectScript $LIST function.

position

Optional — An expression interpreted as a position in the specified list.

default

Optional — An expression that provides the value to return if the list element has an undefined value.

Description

$LISTGET returns the requested element in the specified list as a standard character string. If the value of the position argument refers to a nonexistent member or identifies an element with an undefined value, the specified default value is returned.

The $LISTGET function is identical to the one- and two-argument forms of the $LIST function except that, under conditions that would cause $LIST to return a null string, $LISTGET returns a default value.

This function returns data of type VARCHAR.

You can use $LISTGET to retrieve a field value from a serial container field. In the following example, Home is a serial container field, the third element of which is Home\_State:

SELECT Name,$LISTGET(Home,3) AS HomeState
FROM Sample.Person

 

Arguments

list

An encoded character string containing one or more elements. You can create a list using the SQL [$LISTBUILD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listbuild) function or the Caché ObjectScript [$LISTBUILD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistbuild) function. You can convert a delimited string into a list using the SQL [$LISTFROMSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listfromstring) function or the Caché ObjectScript [$LISTFROMSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistfromstring) function. You can extract a list from an existing list using the SQL [$LIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_list) function or the Caché ObjectScript [$LIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flist) function.

position

The position argument must evaluate to an integer. If it is omitted, by default, the function examines the first element of the list. If the value of the position argument is -1, it is equivalent to specifying the last element of the list.

default

A character string. If you omit the default argument, a zero-length string is assumed for the default value.

Examples

The $LISTGET functions in the following Embedded SQL example both return “Red”, the first element in the list:

   SET a\=$LISTBUILD("Red","Blue","Green")
   &sql(SELECT $LISTGET(:a),$LISTGET(:a,1)
   INTO :b,:c)
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"The one-arg element returned is ",b
     WRITE !,"The two-arg element returned is ",c }

 

The $LISTGET functions in the following Embedded SQL example both return “Green”, the third and last element in the list:

   SET a\=$LISTBUILD("Red","Blue","Green")
   &sql(SELECT $LISTGET(:a,3),$LISTGET(:a,\-1)
   INTO :b,:c)
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"The third element is ",b
     WRITE !,"The last element is ",c }

 

The $LISTGET functions in the following Embedded SQL example both return a value upon encountering the undefined 2nd element in the list. The first returns a question mark (?), which the user defined as the default value. The second returns a null string because a default value is not specified:

   SET a\=$LISTBUILD("Red",,"Green")
   &sql(SELECT $LISTGET(:a,2,'?'),$LISTGET(:a,2)
   INTO :b,:c)
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"The default value is ",b
     WRITE !,"The no-default value is ",c }

 

The $LISTGET functions in the following Embedded SQL example both specify a position greater than the last element in the three-element list. The first returns a null string because the default value is not specified. The second returns the user-specified default value, “ERR”:

   SET a\=$LISTBUILD("Red","Blue","Green")
   &sql(SELECT $LISTGET(:a,4),$LISTGET(:a,4,'ERR')
   INTO :b,:c)
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"The no-default 4th element is ",b
     WRITE !,"The default for 4th element is ",c }

 

The $LISTGET functions in the following Embedded SQL example both return a null string:

   SET a\=$LISTBUILD("Red","Blue","Green")
   &sql(SELECT $LISTGET(:a,0),$LISTGET(NULL)
   INTO :b,:c)
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"The zero element is ",b
     WRITE !,"The NULL element is ",c }

 

Notes

Invalid Argument Values

If the expression in the list argument does not evaluate to a valid list, an SQLCODE -400 fatal error occurs because the $LISTGET return variable remains undefined. This occurs even when a default value is supplied, as in the following Embedded SQL example:

   &sql(SELECT $LISTGET('fred',1,'failsafe') INTO :b)
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"The non-list element is ",b  ; Variable not set
   }

 

If the value of the position argument is less than -1, an SQLCODE -400 fatal error occurs because the $LISTGET return variable remains undefined. This occurs even when a default value is supplied, as in the following Embedded SQL example:

   SET a\=$LISTBUILD("Red","Blue","Green")
   &sql(SELECT $LISTGET(:a,\-3,'failsafe') INTO :c)
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"A neg-num position returns ",c  ; Variable not set
   }

 

This does not occur when position is a nonnumeric value:

   SET a\=$LISTBUILD("Red","Blue","Green")
   &sql(SELECT $LISTGET(:a,'g','failsafe') INTO :c)
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"A nonnumeric position returns ",c }

 

See Also

*   SQL functions: [$LIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_list) [$LISTBUILD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listbuild) [$LISTDATA](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listdata) [$LISTFIND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listfind) [$LISTFROMSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listfromstring) [$LISTLENGTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listlength) [$LISTSAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listsame) [$LISTTOSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listtostring) [$PIECE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_piece)
    
*   Caché ObjectScript functions: [$LIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flist) [$LISTBUILD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistbuild) [$LISTDATA](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistdata) [$LISTFIND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistfind) [$LISTFROMSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistfromstring) [$LISTGET](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistget) [$LISTLENGTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistlength) [$LISTNEXT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistnext) [$LISTSAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistsame) [$LISTTOSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flisttostring) [$LISTVALID](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistvalid)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listfromstring "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listlength "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_d\_listget.xml**