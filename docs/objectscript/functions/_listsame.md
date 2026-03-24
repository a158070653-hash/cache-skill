# $LISTSAME

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_flistsame

---

 

Caché ObjectScript Reference

$LISTSAME

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistnext "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flisttostring "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$LISTSAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistsame "$LISTSAME") \]

Go to: Description Examples Identical Lists See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Compares two lists and returns a boolean value.

Synopsis

$LISTSAME(list1,list2)
$LS(list1,list2)

Parameters

list1

Any expression that evaluates to a list. A list can be created using $LISTBUILD or $LISTFROMSTRING, or extracted from another list using $LIST. The null string ("") is also treated as a valid list.

list2

Any expression that evaluates to a list. A list can be created using $LISTBUILD or $LISTFROMSTRING, or extracted from another list using $LIST. The null string ("") is also treated as a valid list.

Description

$LISTSAME compares the contents of two lists and returns 1 if the lists are identical. If the lists are not identical, $LISTSAME returns 0. $LISTSAME compares list elements using their string representations. $LISTSAME comparisons are case-sensitive.

$LISTSAME compares the two lists element-by-element in left-to-right order. Therefore $LISTSAME returns a value of 0 when it encounters the first non-identical pair of list elements; it does not check subsequent items to determine if they are valid list elements. If a $LISTSAME comparison encounters an invalid item, it issues a <LIST> error.

Examples

The following example returns 1, because the two lists are identical:

   SET x \= $LISTBUILD("Red","Blue","Green")
   SET y \= $LISTBUILD("Red","Blue","Green")
   WRITE $LISTSAME(x,y)

 

The following example returns 0, because the two lists are not identical:

   SET x \= $LISTBUILD("Red","Blue","Yellow")
   SET y \= $LISTBUILD("Red","Yellow","Blue")
   WRITE $LISTSAME(x,y)

 

Identical Lists

$LISTSAME considers two lists to be identical if the string representations of the two lists are identical.

When comparing a numeric list element and a string list element, the string list element must represent the numeric in canonical form; this is because Caché always reduces numerics to canonical form before performing a comparison. In the following example, $LISTSAME compares a string and a numeric. The first three $LISTSAME functions return 1 (identical); the fourth $LISTSAME function returns 0 (not identical) because the string representation is not in canonical form:

  WRITE $LISTSAME($LISTBUILD("365"),$LISTBUILD(365)),!
  WRITE $LISTSAME($LISTBUILD("365"),$LISTBUILD(365.0)),!
  WRITE $LISTSAME($LISTBUILD("365.5"),$LISTBUILD(365.5)),!
  WRITE $LISTSAME($LISTBUILD("365.0"),$LISTBUILD(365.0))

 

$LISTSAME comparison is not the same equivalence test as the one used by other list operations, which test using the internal representation of a list. This distinction is easily seen when comparing a number and a numeric string, as in the following example:

   SET x \= $LISTBUILD("365")
   SET y \= $LISTBUILD(365)
   IF x\=y 
     { WRITE !,"Equal sign: number/numeric string identical" }
   ELSE { WRITE !,"Equal sign: number/numeric string differ" }
   IF 1\=$LISTSAME(x,y)
     { WRITE !,"$LISTSAME: number/numeric string identical" }
   ELSE { WRITE !,"$LISTSAME: number/numeric string differ" }  

 

The equality (=) comparison tests the internal representations of these lists (which are not identical). $LISTSAME performs a string conversion on both lists, compares them, and finds them identical.

The following example shows two lists with various representations of numeric elements. $LISTSAME considers these two lists to be identical:

   SET x \= $LISTBUILD("360","361","362","363","364","365","366")
   SET y \= $LISTBUILD(00360.000,(19\*19),+"362",363,364.0,+365,"3"\_"66")
   WRITE !,$LISTSAME(x,y)," lists are identical"

 

Numeric Maximum

A number larger than 2\*\*63 (9223372036854775810) or smaller than -2\*\*63 (–9223372036854775808) exceeds the maximum numeric range for $LISTSAME list comparison. $LISTSAME returns 0 when such extremely large numbers are compared, as shown in the following example:

  SET bignum\=$LISTBUILD(9223372036854775810)
  SET bigstr\=$LISTBUILD("9223372036854775810")  
  WRITE $LISTSAME(bignum,bigstr),!
  SET bignum\=$LISTBUILD(9223372036854775811)
  SET bigstr\=$LISTBUILD("9223372036854775811")
  WRITE $LISTSAME(bignum,bigstr)

 

Null String and Null List

A list containing the null string (an empty string) as its sole element is a valid list. The null string by itself is also considered a valid list. However these two (a null string and a null list) are not considered identical, as shown in the following example:

   WRITE !,$LISTSAME($LISTBUILD(""),$LISTBUILD(""))," null lists"
   WRITE !,$LISTSAME("","")," null strings"
   WRITE !,$LISTSAME($LISTBUILD(""),"")," null list and null string"

 

Normally, a string is not a valid $LISTSAME argument, and $LISTSAME issues a <LIST> error. However, the following $LISTSAME comparisons complete successfully and return 0 (values not identical). The null string and the string “abc” are compared and found not to be identical. These null string comparisons do not issue a <LIST> error:

   WRITE !,$LISTSAME("","abc")
   WRITE !,$LISTSAME("abc","")

 

The following $LISTSAME comparisons do issue a <LIST> error, because a list (even a null list) cannot be compared with a string:

   SET x \= $LISTBUILD("")
   WRITE !,$LISTSAME("abc",x)
   WRITE !,$LISTSAME(x,"abc")

 

Comparing “Empty” Lists

$LISTVALID considers all of the following as valid lists:

  WRITE $LISTVALID(""),!
  WRITE $LISTVALID($LB()),!
  WRITE $LISTVALID($LB(NULL)),!
  WRITE $LISTVALID($LB("")),!
  WRITE $LISTVALID($LB($CHAR(0))),!
  WRITE $LISTVALID($LB(,))

 

$LISTSAME considers only the following pairs as identical:

  WRITE $LISTSAME($LB(),$LB(NULL)),!
  WRITE $LISTSAME($LB(,),$LB(NULL,NULL)),!
  WRITE $LISTSAME($LB(,),$LB()\_$LB())

 

Empty Elements

A $LISTBUILD can create empty elements by including extra commas between elements or appending one or more commas to either end of the element list. $LISTSAME is aware of empty elements, and does not treat them as equivalent to null string elements.

The following $LISTSAME examples all return 0 (not identical):

  WRITE $LISTSAME($LISTBUILD(365,,367),$LISTBUILD(365,367)),!
  WRITE $LISTSAME($LISTBUILD(365,366,),$LISTBUILD(365,366)),!
  WRITE $LISTSAME($LISTBUILD(365,366,,),$LISTBUILD(365,366,)),!
  WRITE $LISTSAME($LISTBUILD(365,,367),$LISTBUILD(365,"",367)) 

 

$DOUBLE List Elements

$LISTSAME considers all forms of zero to be identical: 0, –0, $DOUBLE(0), and $DOUBLE(-0).

$LISTSAME considers a $DOUBLE(“NAN”) list element to be identical to another $DOUBLE(“NAN”) list element. However, because NAN (Not A Number) cannot be meaningfully compared using numerical operators, Caché operations (such as equal to, less than, or greater than) that attempt to compare $DOUBLE(“NAN”) to another $DOUBLE(“NAN”) fail, as shown in the following example:

   SET x \= $DOUBLE("NAN")
   SET a \= $LISTBUILD(1,2,x)
   SET b \= $LISTBUILD(1,2,x)
   WRITE !,$LISTSAME(a,b)    /\* 1 (NAN list elements same) \*/
   WRITE !,x\=x               /\* 0 (NAN values not equal)   \*/

 

Nested and Concatenated Lists

$LISTSAME does not support nested lists. It cannot compare two lists that contain lists, even if their contents are identical.

   SET x \= $LISTBUILD("365")
   SET y \= $LISTBUILD(365)
   WRITE !,$LISTSAME(x,y)," lists identical"
   WRITE !,$LISTSAME($LISTBUILD(x),$LISTBUILD(y))," nested lists not identical"

 

In the following example, both $LISTSAME comparisons returns 0, because these lists are not considered identical:

   SET x\=$LISTBUILD("Apple","Pear","Walnut","Pecan")
   SET y\=$LISTBUILD("Apple","Pear",$LISTBUILD("Walnut","Pecan"))
   SET z\=$LISTBUILD("Apple","Pear","Walnut","Pecan","")
   WRITE !,$LISTSAME(x,y)," nested list"
   WRITE !,$LISTSAME(x,z)," null string is list item"

 

$LISTSAME does support concatenated lists. The following example returns 1, because the lists are considered identical:

   SET x\=$LISTBUILD("Apple","Pear","Walnut","Pecan")
   SET y\=$LISTBUILD("Apple","Pear")\_$LISTBUILD("Walnut","Pecan")
   WRITE !,$LISTSAME(x,y)," concatenated list"

 

See Also

*   [$LIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flist) function
    
*   [$LISTBUILD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistbuild) function
    
*   [$LISTDATA](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistdata) function
    
*   [$LISTFIND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistfind) function
    
*   [$LISTFROMSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistfromstring) function
    
*   [$LISTGET](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistget) function
    
*   [$LISTLENGTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistlength) function
    
*   [$LISTNEXT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistnext) function
    
*   [$LISTTOSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flisttostring) function
    
*   [$LISTUPDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistupdate) function
    
*   [$LISTVALID](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistvalid) function
    
*   [$DOUBLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fdouble) function
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistnext "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flisttostring "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_flistsame.xml**