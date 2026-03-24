# $LISTLENGTH

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_d_listlength

---

 

Caché SQL Reference

$LISTLENGTH

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listget "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listsame "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [$LISTLENGTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listlength "$LISTLENGTH") \]

Go to: Description Examples Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A list function that returns the number of elements in a specified list.

Synopsis

$LISTLENGTH(list)

Argument

list

An expression that evaluates to a valid list. A list is an encoded character string containing one or more elements. You can create a list using the SQL or Caché ObjectScript $LISTBUILD or $LISTFROMSTRING functions. You can extract a list from an existing list using the SQL or Caché ObjectScript $LIST function.

Description

$LISTLENGTH returns the number of elements in list.

This function returns data of type SMALLINT.

Examples

The following Embedded SQL example returns 3, because there are 3 elements in the list:

   SET a\=$LISTBUILD("Red","Blue","Green")
   &sql(SELECT $LISTLENGTH(:a) INTO :b)
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"The number of elements is ",b }

 

The following SQL example also returns 3, because there are 3 elements in the list:

SELECT $LISTLENGTH($LISTBUILD('Red','Blue','Green'))

 

The following Embedded SQL example also returns 3. There are 3 elements in the list, though the second element contains no data:

   SET a\=$LISTBUILD("Red",,"Green")
   &sql(SELECT $LISTLENGTH(:a) INTO :b)
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"The number of elements is ",b }

 

In the following SQL example, each $LISTLENGTH returns 3, because there are 3 elements in the list, though the second element contains no data:

SELECT $LISTLENGTH($LISTBUILD('Red','','Green')),
       $LISTLENGTH($LISTBUILD('Red',NULL,'Green')),
       $LISTLENGTH($LISTBUILD('Red',,'Green'))

 

Notes

Invalid Lists

If list is not a valid list, an SQLCODE -400 fatal error is generated:

   SET a\="fred"
   &sql(SELECT $LISTLENGTH(:a) INTO :b)
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"The number of elements is ",b ; Variable not set
   }

 

If the Caché ObjectScript $LISTBUILD function is used to build a list that contains only the null string, this is a valid list, containing one element:

   SET a\=$LISTBUILD("")
   &sql(SELECT $LISTLENGTH(:a) INTO :b)
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
   WRITE !,"The number of elements is ",b }

 

Null Lists

The SQL $LISTLENGTH function and the Caché ObjectScript $LISTLENGTH function differ in how they handle a null list (a list containing no elements).

The following three Embedded SQL examples show how the $LISTLENGTH SQL function handles a null list. In the first two examples, list is the null string, and a null string is returned:

   SET a\=""
   &sql(SELECT $LISTLENGTH(:a)
   INTO :b)
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"The number of elements is ",b }

 

   &sql(SELECT $LISTLENGTH(NULL)
   INTO :b)
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"The number of elements is ",b }

 

In the third example, list is the value $CHAR(0), which is an invalid list; an SQLCODE -400 fatal error is generated:

   &sql(SELECT $LISTLENGTH('')
   INTO :b)
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"The number of elements is ",b }

 

Note that this differs from how the Caché ObjectScript $LISTLENGTH function handles a null list. In Caché ObjectScript, the null string ("") is used to represent a null list, a list containing no elements. Because it contains no list elements, it has a $LISTLENGTH count of 0, as shown in the following example:

   WRITE $LISTLENGTH("")

 

$LISTLENGTH and Nested Lists

The following Embedded SQL example returns 3, because $LISTLENGTH does not recognize the individual elements in nested lists:

   SET a\=$LISTBUILD("Apple","Pear",$LISTBUILD("Walnut","Pecan"))
   &sql(SELECT $LISTLENGTH(:a)
   INTO :b)
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"The number of elements is ",b }

 

See Also

*   SQL list functions: [$LIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_list), [$LISTBUILD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listbuild), [$LISTDATA](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listdata), [$LISTFIND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listfind), [$LISTFROMSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listfromstring), [$LISTGET](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listget), [$LISTSAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listsame), [$LISTTOSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listtostring)
    
*   Other SQL functions: [$PIECE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_piece)
    
*   Caché ObjectScript list functions: [$LIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flist), [$LISTBUILD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistbuild), [$LISTDATA](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistdata), [$LISTFIND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistfind), [$LISTFROMSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistfromstring), [$LISTGET](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistget), [$LISTLENGTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistlength), [$LISTNEXT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistnext), [$LISTSAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistsame), [$LISTTOSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flisttostring), [$LISTVALID](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistvalid)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listget "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listsame "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_d\_listlength.xml**