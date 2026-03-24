# $LISTFIND

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_d_listfind

---

 

Caché SQL Reference

$LISTFIND

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listdata "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listfromstring "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [$LISTFIND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listfind "$LISTFIND") \]

Go to: Description Examples Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A list function that searches a specified list for the requested value.

Synopsis

$LISTFIND(list,value\[,startafter\])

Arguments

list

An expression that evaluates to a valid list. A list is an encoded character string containing one or more elements. You can create a list using the SQL or Caché ObjectScript $LISTBUILD or $LISTFROMSTRING functions. You can extract a list from an existing list using the SQL or Caché ObjectScript $LIST function.

value

An expression containing the search element. A character string.

startafter

Optional — An integer expression interpreted as a list position. The search starts with the element after this position. Zero and –1 are valid values; –1 never returns an element. Zero is the default.

Description

$LISTFIND searches the specified list for the first instance of the requested value. The search begins with the element after the position indicated by the startafter argument. If you omit the startafter argument, $LISTFIND assumes a startafter value of 0 and starts the search with the first element (element 1). If the value is found, $LISTFIND returns the position of the matching element. If the value is not found, $LISTFIND returns a 0. The $LISTFIND function will also return a 0 if the value of the startafter argument refers to a nonexistent list member.

This function returns data of type SMALLINT.

Examples

The following Embedded SQL example returns 2, the position of the first occurrence of the requested string:

   SET a\=$LISTBUILD("Red","Blue","Green")
   &sql(SELECT $LISTFIND(:a,'Blue') INTO :b)
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"The position is ",b }

 

The following Embedded SQL example returns 0, indicating the requested string was not found:

   SET a\=$LISTBUILD("Red","Blue","Green")
   &sql(SELECT $LISTFIND(:a,'Orange') INTO :b)
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"The position is ",b }

 

The following three Embedded SQL examples show the effect of using the startafter argument. The first example does not find the requested string and returns 0 because the requested string occurs at the startafter position:

   SET a\=$LISTBUILD("Red","Blue","Green")
   &sql(SELECT $LISTFIND(:a,'Blue',2) INTO :b)
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"The position is ",b }

 

The second example finds the requested string at the first position by setting startafter to zero (the default value):

   SET a\=$LISTBUILD("Red","Blue","Green")
   &sql(SELECT $LISTFIND(:a,'Red',0) INTO :b)
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"The position is ",b }

 

The third example finds the second occurrence of the requested string and returns 5, because the first occurs before the startafter position:

   SET a\=$LISTBUILD("Red","Blue","Green","Yellow","Blue")
   &sql(SELECT $LISTFIND(:a,'Blue',3) INTO :b)
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"The position is ",b }

 

The $LISTFIND function only matches complete elements. Thus, the following example returns 0 because no element of the list is equal to the string “B”, though all of the elements contain “B”:

   SET a\=$LISTBUILD("ABC","BCD","BBB")
   &sql(SELECT $LISTFIND(:a,'B') INTO :b)
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"The position is ",b }

 

Notes

Invalid Argument Values

If the expression in the list argument does not evaluate to a valid list, the $LISTFIND function generates an SQLCODE -400 fatal error.

   SET a\="Blue"
   &sql(SELECT $LISTFIND(:a,'Blue') INTO :b)
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
    WRITE !,"The position is ",b }

 

If the value of the startafter argument is -1, $LISTFIND always returns zero (0).

   SET a\=$LISTBUILD("Red","Blue","Green")
   &sql(SELECT $LISTFIND(:a,'Blue',\-1) INTO :b)
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"The position is ",b }

 

If the value of the startafter argument is less than -1, invoking the $LISTFIND function generates an SQLCODE -400 fatal error.

   SET a\=$LISTBUILD("Red","Blue","Green")
   &sql(SELECT $LISTFIND(:a,'Blue',\-3) INTO :b)
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"The position is ",b }

 

See Also

*   SQL functions: [$LIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_list), [$LISTBUILD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listbuild), [$LISTDATA](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listdata), [$LISTFROMSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listfromstring), [$LISTGET](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listget), [$LISTLENGTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listlength), [$LISTSAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listsame), [$LISTTOSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listtostring), [$PIECE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_piece)
    
*   Caché ObjectScript functions: [$LIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flist), [$LISTBUILD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistbuild), [$LISTDATA](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistdata), [$LISTFIND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistfind), [$LISTFROMSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistfromstring), [$LISTGET](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistget), [$LISTLENGTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistlength), [$LISTNEXT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistnext), [$LISTSAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistsame), [$LISTTOSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flisttostring), [$LISTVALID](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistvalid)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listdata "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listfromstring "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_d\_listfind.xml**