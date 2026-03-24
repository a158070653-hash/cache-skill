# $LIST

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_d_list

---

 

Caché SQL Reference

$LIST

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_length "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listbuild "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [$LIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_list "$LIST") \]

Go to: Description Arguments Examples Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A list function that returns elements in a list.

Synopsis

$LIST(list\[,position\[,end\]\])

Arguments

list

An expression that evaluates to a valid list. A list is an encoded character string containing one or more elements. You can create a list using the SQL or Caché ObjectScript $LISTBUILD or $LISTFROMSTRING functions. You can extract a list from an existing list using the SQL or Caché ObjectScript $LIST function.

position

Optional — The starting position in the specified list. An expression that evaluates to an integer.

end

Optional — The ending position in the specified list. An expression that evaluates to an integer.

Description

$LIST returns elements from a list. The elements returned depend on the arguments used.

*   $LIST(list) returns the first element in the list as a text string.
    
*   $LIST(list,position) returns the element indicated by the specified position as a text string. The position argument must evaluate to an integer.
    
*   $LIST(list,position,end) returns a “sublist” (an encoded list string) containing the elements of the list from the specified start position through the specified end position.
    

This function returns data of type VARCHAR.

Arguments

list

An encoded character string containing one or more elements. You can create a list using the SQL [$LISTBUILD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listbuild) function or the Caché ObjectScript [$LISTBUILD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistbuild) function. You can convert a delimited string into a list using the SQL [$LISTFROMSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listfromstring) function or the Caché ObjectScript [$LISTFROMSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistfromstring) function. You can extract a list from an existing list using the SQL [$LIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_list) function or the Caché ObjectScript [$LIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flist) function.

A list can be supplied to the SQL $LIST function by using a host variable, or by specifying a $LISTBUILD within SQL. Both are shown in the following Embedded SQL example:

   SET mylist\=$LISTBUILD("Red","Blue","Green")
   &sql(SELECT $LIST(:mylist,2),$LIST($LISTBUILD('Red','Blue','Green'),3)
   INTO :a,:b )
   IF SQLCODE'=0 {
     WRITE "Error code ",SQLCODE,! }
   ELSE {
     WRITE !,"The host varable list element is ",a,!
     WRITE !,"The SQL $LISTBUILD list element is ",b,! }

 

A list can be extracted from another list by using the $LIST function:

   SET a\=$LISTBUILD("Red","Blue","Green")
   &sql(SELECT $LIST(:a,2,3)
   INTO :b )
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
   &sql(SELECT $LIST(:b,1)
   INTO :c )
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"The element returned is ",c }
   }

 

In the following Embedded SQL example, subList is not a valid list argument, because it is a single element returned as an ordinary string, not an encoded list string. Only the three-argument form of $LIST returns an encoded list string. In this case, an SQLCODE -400 fatal error is generated:

   SET a\=$LISTBUILD("Red","Blue","Green")
   &sql(SELECT $LIST(:a,2)
   INTO :sublist )
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
   &sql(SELECT $LIST(:sublist,1)
   INTO :c )
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"The sublist is"
     ZZDUMP c   ; Variable not set 
     }
   }

 

position

The position of a list element to return. List elements are counted from 1. If position is omitted, the first element is returned. If the value of position is 0 or greater than the number of elements in the list, Caché SQL does not return a value. If the value of position is negative one (–1), $LIST returns the final element in the list.

   SET a\=$LISTBUILD("Red","Blue","Green")
   &sql(SELECT $LIST(:a,\-1)
   INTO :b )
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"The last element is ",b }

 

If the end argument is specified, position specifies the first element in a range of elements. Even when only one element is returned (when position and end are the same number) this element is returned as an encoded list string. Thus, $LIST(x,2) (which returns the element as an ordinary string) is not identical to $LIST(x,2,2) (which returns the element as an encoded list string).

end

The position of the last element in a range of elements. You must specify position to specify end. When end is specified, the value returned is an encoded list string. Because of this encoding, such strings should only be processed by other $LIST functions.

If the value of end is:

*   greater than position, an encoded string containing a list of elements is returned.
    
*   equal to position, an encoded string containing the one element is returned.
    
*   less than position, no value is returned.
    
*   greater than the number of elements in list, it is equivalent to specifying the final element in the list.
    
*   negative one (–1), it is equivalent to specifying the final element in the list.
    

When specifying end, you can specify a position value of zero (0). In this case, 0 is equivalent to 1.

Examples

In the following Embedded SQL example, the two WRITE statements both return “Red”, the first element in the list. The first writes the first element by default, the second writes the first element because the position argument is set to 1:

   SET a\=$LISTBUILD("Red","Blue","Green")
   &sql(SELECT $LIST(:a),$LIST(:a,1)
   INTO :b,:c )
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"The one-arg sublist is ",b
     WRITE !,"The two-arg sublist is ",c }

 

The following Embedded SQL example returns “Blue”, the second element in the list:

   SET a\=$LISTBUILD("Red","Blue","Green")
   &sql(SELECT $LIST(:a,2)
   INTO :b )
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"The second element is ",b }

 

The following Embedded SQL example returns “Red Blue”, a two-element list string beginning with the first element and ending with the second element in the list. ZZDUMP is used rather than WRITE, because a list string contains special (non-printing) encoding characters:

   SET a\=$LISTBUILD("Red","Blue","Green")
   &sql(SELECT $LIST(:a,1,2)
   INTO :b )
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"The encoded sublist is"
     ZZDUMP b   ; Prints "Red Blue " 
     }

 

The following Embedded SQL example returns the last element in a list of unknown length. Here, the last element is returned first as an ordinary string, then as an encoded list string:

   SET a\=$LISTBUILD("Red","Blue","Green")
   &sql(SELECT $LISTLENGTH(:a),$LIST(:a,\-1)
   INTO :b,:plain )
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
   &sql(SELECT $LIST(:a,:b,\-1)
   INTO :encoded )
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"The final element as a string: ",plain
     WRITE !,"The final element as an encoded string: "
     ZZDUMP encoded }
   }

 

Notes

Invalid Argument Values

If the expression in the list argument does not evaluate to a valid list, an SQLCODE -400 fatal error is generated:

   SET a\="the quick brown fox"
   &sql(SELECT $LIST(:a,1)
   INTO :b )
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"The sublist is"
     ZZDUMP b   ; Variable not set 
     }

 

If the value of the position argument or the end argument is less than -1, an SQLCODE -400 fatal error is generated:

   SET a\=$LISTBUILD("Red","Blue","Green")
   &sql(SELECT $LIST(:a,\-2,3)
   INTO :b )
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"The sublist is"
     ZZDUMP b   ; Variable not set 
     }

 

If the value of the position argument refers to a nonexistent list member and no end argument is used, an SQLCODE -400 fatal error is generated:

   SET a\=$LISTBUILD("Red","Blue","Green")
   &sql(SELECT $LIST(:a,7)
   INTO :b )
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"The sublist is"
     ZZDUMP b   ; Variable not set
     }

 

However, if an end argument is used, no error occurs, and the null string is returned.

   SET a\=$LISTBUILD("Red","Blue","Green")
   &sql(SELECT $LIST(:a,7,\-1)
   INTO :b )
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"Error code ",SQLCODE
     WRITE !,"The sublist is"
     ZZDUMP b   ; Prints a null string 
     }

 

If the value of the position argument identifies an element with an undefined value, an SQLCODE –400 fatal error is generated:

   SET a\=$LISTBUILD("Red",,"Green")
   &sql(SELECT $LIST(:a,2)
   INTO :b )
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"The sublist is"
     ZZDUMP b   ; Variable not set
     }

 

Two-Argument and Three-Argument $LIST

$LIST(list,1) is not equivalent to $LIST(list,1,1) because the former returns a string, while the latter returns a single-element list string. If there are no elements to return, the two-argument form does not return a value; the three-argument form returns a null string.

Unicode

If one Unicode character appears in a list element, that entire list element is represented as Unicode (wide) characters. Other elements in the list are not affected.

The following Embedded SQL example shows two lists. The a list consists of two elements which contain only ASCII characters. The b list consists of two elements: the first element contains a Unicode character ($CHAR(960) = the pi symbol); the second element contains only ASCII characters.

  IF $SYSTEM.Version.IsUnicode() {
   SET a\=$LISTBUILD("ABC"\_$CHAR(68),"XYZ")
   SET b\=$LISTBUILD("ABC"\_$CHAR(960),"XYZ")
   &sql(SELECT $LIST(:a,1),$LIST(:a,2),$LIST(:b,1),$LIST(:b,2)
   INTO :a1,:a2,:b1,:b2 )
   IF SQLCODE'=0 {
     WRITE !,"Error code ",SQLCODE }
   ELSE {
     WRITE !,"The ASCII list a elements: "
     ZZDUMP a1
     ZZDUMP a2
     WRITE !,"The Unicode list b elements: "
     ZZDUMP b1
     ZZDUMP b2 }
  }
  ELSE {WRITE "This example requires a Unicode installation of Caché"}

 

Note that Caché encodes the first element of b entirely in wide Unicode characters. The second element of b contains no Unicode characters, and thus Caché encodes it using narrow ASCII characters.

See Also

*   SQL functions: [$LISTBUILD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listbuild), [$LISTDATA](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listdata), [$LISTFIND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listfind), [$LISTFROMSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listfromstring), [$LISTGET](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listget), [$LISTLENGTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listlength), [$LISTSAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listsame), [$LISTTOSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listtostring), [$PIECE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_piece)
    
*   Caché ObjectScript functions: [$LIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flist), [$LISTBUILD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistbuild), [$LISTDATA](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistdata), [$LISTFIND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistfind), [$LISTFROMSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistfromstring), [$LISTGET](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistget), [$LISTLENGTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistlength), [$LISTNEXT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistnext), [$LISTSAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistsame), [$LISTTOSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flisttostring), [$LISTVALID](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistvalid)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_length "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listbuild "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_d\_list.xml**