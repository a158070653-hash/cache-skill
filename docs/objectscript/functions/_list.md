# $LIST

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_flist

---

 

Caché ObjectScript Reference

$LIST

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flength "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistbuild "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$LIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flist "$LIST") \]

Go to: Description Parameters $LIST Errors Replacing Elements Using SET $LIST Examples Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Returns or replaces elements in a list.

Synopsis

$LIST(list,position,end) 
$LI(list,position,end)  

SET $LIST(list,position,end)=value 
SET $LI(list,position,end)=value

Parameters

list

An expression that evaluates to a valid list. Because lists contain encoding, list must be created using [$LISTBUILD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistbuild) or [$LISTFROMSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistfromstring), or extracted from another list using $LIST. In SET $LIST syntax, list must be a variable or a multi-dimensional property.

position

Optional — An integer code specifying the starting position in list. Permitted values are n (count from beginning of list), \* (last element in list), and \*-n (relative offset count backwards from end of list). SET $LIST syntax also supports \*+n (relative offset integer count of elements to append beyond the end of list). Thus, the first element in the list is 1, the second element is 2, the last element in the list is \*, and the next-to-last element is \*-1. If position is a fractional number, it is truncated to its integer part. If position is omitted, it defaults to 1.

\-1 may be used in older code to specify the last element in the list. This deprecated use of -1 should not be combined with \*, \*-n, or \*+n relative offset syntax.

end

Optional — An integer code specifying the ending position of a sublist of list. Used with position. Uses the same values as position.

Description

$LIST can be used in two ways:

*   [To return an element (or elements)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flist#RCOS_flist_return) from list. This uses the $LIST(list,position,end) syntax.
    
*   [To replace an element (or elements)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flist#RCOS_flist_set) within list. The replacement element may be the same length, longer, or shorter than the original element. This uses the SET $LIST(list,position,end)=value syntax.
    

Returning List Elements

$LIST returns list elements. The elements returned depend on the parameters used.

*   $LIST(list) returns the first element in the list as a string.
    
*   $LIST(list,position) returns the element specified by position as a string.
    
*   $LIST(list,position,end) returns a “sublist” (an encoded list string) containing the elements of the list from the specified start position through the specified end position (inclusive). If position and end specify the same element, $LIST returns this element as an encoded list.
    

Note:

$LIST should not be used in a loop structure to return multiple successive element values. While this will work, it is highly inefficient, because $LIST must evaluate the list from the beginning with each iteration. The [$LISTNEXT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistnext) function is a far more efficient way to return multiple successive element values.

Parameters

list

An encoded list string containing one or more elements. Lists can be created using $LISTBUILD or $LISTFROMSTRING, or extracted from another list by using the $LIST function.

When [returning an element (or elements)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flist#RCOS_flist_return), list can be a variable or an object property.

When $LIST is used with SET on the left hand side of the equals sign to [replace an element (or elements)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flist#RCOS_flist_set), list can be a variable or a [multidimensional property](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ROBJ_property_multidimensional) reference; it cannot be a non-multidimensional object property.

The following are valid list arguments:

   SET myList \= $LISTBUILD("Red","Blue","Green","Yellow")
   WRITE !,$LIST(myList,2)   ; prints Blue
   SET subList \= $LIST(myList,2,4)
   WRITE !,$LIST(subList,2)  ; prints Green

 

In the following example, subList is not a valid list argument, because it is a single element returned as an ordinary string, not an encoded list string:

   SET myList \= $LISTBUILD("Red","Blue","Green","Yellow")
   SET subList \= $LIST(myList,2)
   WRITE $LIST(subList,1)

In SET $LIST syntax form, list cannot be a non-multidimensional object property.

position

The position (element count) of the list element to return (or replace). A single element is returned as a string. List elements are counted from 1. If position is omitted, $LIST returns the first element.

*   If position is a positive integer, $LIST counts elements from the beginning of list. If position is greater than the number of elements in list, Caché issues a <NULL VALUE> error.
    
*   If position is \* (asterisk), $LIST returns the last element in list.
    
*   If position is \*-n (an asterisk followed by a negative number), $LIST counts elements by offset backwards from the end of list. Thus, \*-0 is the last element in the list, \*-1 is the next-to-last list element (an offset of 1 from the end). If the position relative offset count is equal to the number of elements in list (and thus specifies the 0th element), Caché issues a <NULL VALUE> error. If the position relative offset count is greater than the number of elements in list, Caché issues a <RANGE> error.
    
*   For SET $LIST syntax only — If position is \*+n (an asterisk followed by a positive number), SET $LIST appends elements by offset beyond the end of list. Thus, \*+1 appends an element beyond the end of list, \*+2 appends an element two positions beyond the end of list, padding with a null string element.
    
*   If position is 0 or -0, Caché issues a <NULL VALUE> error.
    

If the end parameter is specified, position specifies the first element in a range of elements. A range of elements is always returned as an encoded list string. Even when only one element is returned (when position and end are the same number) this value is returned as an encoded list string. Thus, $LIST(x,2) is the same element, but not the same data value as $LIST(x,2,2).

end

The position of the last element in a range of elements, specified as an integer. You must specify position to specify end. If end is a fractional number, it is truncated to its integer part.

When end is specified, the value returned is an encoded list string. Because of this encoding, such strings should only be processed by other $LIST functions.

*   position < end: If end and position are positive integers, and position < end, $LIST returns an encoded sublist containing the specified list of elements, inclusive of the position and end elements. If position is 0 or 1, the sublist begins with the first element in list. If end is greater than the number of elements in list, $LIST returns an encoded sublist containing all of the elements from position through the end of the list. If end is \*-n, position can be a positive integer or a \*-n value greater than or equal to this end position. Thus, $LIST(fourlist,\*-1,\*), $LIST(fourlist,\*-3,\*-2), $LIST(fourlist,2,\*-1) are all valid sublists.
    
*   position = end: If end and position evaluate to the same element, $LIST returns an encoded sublist containing that single element. For example, in a list with four elements, end and position may be identical ($LIST(fourlist,2,2), $LIST(fourlist,\*,\*), or $LIST(fourlist,\*-2,\*-2)) or they may specify the same element ($LIST(fourlist,4,\*), $LIST(fourlist,3,\*-1)).
    
*   position > end: If position > end, $LIST returns the null string (""). For example, in a list with four elements, $LIST(fourlist,3,2), $LIST(fourlist,7,\*), or $LIST(fourlist,\*-1,\*-2) all return the null string.
    
*   position\=0, with end: If end is specified, and position is zero (0) or a negative offset that evaluates to position zero, position 0 is equivalent to 1. Therefore, if end evaluates to an element position greater than zero, $LIST returns an encoded sublist containing the elements from position 1 through the end position. If end also evaluates to element position zero, $LIST returns the null string (""), because position > end.
    
*   position or end < 0: If either end or position evaluate to an element position less than zero, Caché issues a <RANGE> error.
    
*   For SET $LIST syntax only — If end is \*+n (an asterisk followed by a positive number), SET $LIST appends a range of elements by offset beyond the end of list. If position is \*+n, SET $LIST appends a range of values. If position is a positive integer, or \*-n SET $LIST both replaces and appends values. To replace the last element and append elements, specify SET $LIST(mylist,\*+0,\*+n). If the start of the specified range is beyond the end of list, the list is padded with a null string elements as needed. If end is larger than the supplied range of values, trailing padding is not performed.
    

Deprecated –1 Values

In older code, a position or end value of -1 represents the last element in the list. A value of -1 cannot be used with \*, \*+n, or \*-n syntax.

Specifying \*-n and \*+n Parameter Values

When using a variable to specify \*-n or \*+n, you must always specify the asterisk and a sign character in the parameter itself.

The following are valid specifications of \*-n:

  SET count\=2
  SET alph\=$LISTBUILD("a","b","c","d")
  WRITE $LIST(alph,\*\-count)

 

  SET count\=\-2
  SET alph\=$LISTBUILD("a","b","c","d")
  WRITE $LIST(alph,\*+count)

 

The following is a valid specification of \*+n:

  SET count\=2
  SET alph\=$LISTBUILD("a","b","c","d")
  SET $LIST(alph,\*+count)\="F"
  WRITE $LISTTOSTRING(alph,"^",1)

 

Whitespace is permitted within these parameter values.

$LIST Errors

The following $LIST parameter values generate an error:

*   If the list parameter does not evaluate to a valid list, $LIST generates a <LIST> error. You can use the [$LISTVALID](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistvalid) function to determine if a list is valid.
    
*   If the list parameter evaluate to a valid list that contains a null value, or concatenates a list and a null value, $LIST generates a <NULL VALUE> error. All of the following are valid lists (according to [$LISTVALID](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistvalid)) for which $LIST generate a <NULL VALUE> error:
    
      WRITE $LIST(""),!
      WRITE $LIST($LB()),!
      WRITE $LIST($LB(NULL)),!
      WRITE $LIST($LB(,))
      WRITE $LIST($LB()\_$LB("a","b","c"))
    
*   If the position parameter is 0, or a negative relative offset that specifies the 0th element, and no end parameter is used, $LIST generates a <NULL VALUE> error.
    
*   If the value of the position parameter refers to a nonexistent list member and no end parameter is used, $LIST generates a <NULL VALUE> error.
    
       SET list2\=$LISTBUILD("Brown","Black")
       WRITE $LIST(list2,3) ; generates a <NULL VALUE> error
    
*   If the value of the position parameter identifies an element with an undefined value, and no end parameter is used, $LIST generates a <NULL VALUE> error.
    
       SET list3\=$LISTBUILD("A",,"C")
       ZZDUMP $LIST(list3,2)  ; generates a <NULL VALUE> error
    
*   If the \*-n value of the position parameter specifies an n value larger than the number of element positions in list, $LIST generates a <RANGE> error.
    
       SET list2\=$LISTBUILD("Brown","Black")
       WRITE $LIST(list2,\*\-2) ; generates a <NULL VALUE> error
       WRITE $LIST(list2,\*\-3) ; generates a <RANGE> error
    
*   If the value of the position parameter or the end parameter is less than -1, $LIST generates a <RANGE> error.
    

Replacing Elements Using SET $LIST

*   You can use SET $LIST(list,position) to remove an element, replace an element’s value, or append an element to the list. In this two-parameter form, you specify the new element value as a string.
    
*   You can use SET $LIST(list,position,end) to remove one or more elements, replace one or more element values, or append one or more elements to the list. In this three-parameter form, you must specify the new element value(s) as an encoded list.
    

When $LIST is used with SET on the left hand side of the equals sign, list can be a valid variable name. If the variable does not exist, SET $LIST defines it. The list parameter can also be a [multidimensional property](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ROBJ_property_multidimensional) reference; it cannot be a non-multidimensional object property. Attempting to use SET $LIST on a non-multidimensional object property results in an <OBJECT DISPATCH> error.

You cannot use SET (a,b,c,...)=value syntax with $LIST (or $PIECE or $EXTRACT) on the left of the equals sign, if the function uses relative offset syntax: \* representing the end of a string and \*-n or \*+n representing relative offset from the end of the string. You must instead use SET a=value,b=value,c=value,... syntax.

You can also use [$LISTUPDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistupdate) to replace one or more elements in a list or append element to a list by element position. $LISTUPDATE replaces list elements, performing a boolean test for each element replacement. Unlike SET $LIST, $LISTUPDATE does not modify the initial list, but returns a copy of that list with the specified element replacements.

Two Parameter Operations

You can perform the following two-parameter operations. Note that two-parameter operations specify an element value as a string. Specifying an element value as a list creates a sublist within the list.

*   Replace one element value with a new value:
    
      SET $LIST(fruit,2)\="orange"   ; count from beginning of list
      SET $LIST(fruit,\*)\="pear"     ; element at end of list
      SET $LIST(fruit,\*\-2)\="peach"  ; offset from end of list
    
*   Remove an element value (this sets the value to the null string; it does not remove the element position):
    
      SET $LIST(fruit,2)\=""
    
*   Append an element to a list. You can append to the end of the list, or to a location past the end of the list, by using \*+n syntax. SET $LIST inserts null value elements as needed to pad to the specified position:
    
      SET $LIST(fruit,\*+1)\="plum"
    
*   Replace one element with a sublist of elements:
    
      SET $LIST(fruit,3)\=$LISTBUILD("orange","banana")
    

Three Parameter Operations

You can perform the following three-parameter (range) operations. Note that range operations specify an element values as a list, even when specifying a single element value.

*   Replace one element with several elements:
    
      SET $LIST(fruit,3,3)\=$LISTBUILD("orange","banana")
    
*   Replace a range of element values with the same number of new values:
    
      SET $LIST(fruit,2,3)\=$LISTBUILD("orange","banana")
    
*   Replace a range of element values with a larger or smaller number of new values:
    
      SET $LIST(fruit,2,3)\=$LISTBUILD("orange","banana","peach")
    
*   Remove a range of element values (this sets the element values to the null string; it does not remove the element positions):
    
      SET $LIST(fruit,2,3)\=$LISTBUILD("","")
    
*   Remove a range of element values and their positions:
    
      SET $LIST(fruit,2,3)\=""
    
*   Append a range of element to a list. You can append to the end of the list, or to a location past the end of the list, by using \*+n syntax. SET $LIST inserts null value elements as needed to pad to the specified position:
    
      SET $LIST(fruit,\*+1,\*+2)\=$LISTBUILD("plum","pear")
    
    SET $LIST only appends the specified element values. If the end position is larger than the specified elements, empty trailing element positions are not created.
    

Examples

Examples of Returning Elements with $LIST

The following examples use the 2-parameter form of $LIST to return a list element as a string:

The following two $LIST statements return “Red”, the first element in the list. The first returns the first element by default, the second returns the first element because the position parameter is set to 1. The value is returned as a string:

   SET colorlist\=$LISTBUILD("Red","Orange","Yellow","Green","Blue","Violet")
   WRITE $LIST(colorlist),!
   WRITE $LIST(colorlist,1)

 

The following two $LIST statements return “Orange”, the second element in the list. The first counts from the beginning of the list, the second counts backwards from the end of the list. The value is returned as a string:

   SET colorlist\=$LISTBUILD("Red","Orange","Yellow","Green","Blue","Violet")
   WRITE $LIST(colorlist,2),!
   WRITE $LIST(colorlist,\*\-4)

 

The following examples use the 3-parameter form of $LIST to return one or more elements as an encoded list string. Because a list contains non-printing encoding character, you must use $LISTTOSTRING to convert the sublist to a printable string.

The following two $LIST statements return “Blue”, the fifth element in the list as an encoded list string. The first counts from the beginning of the list, the second counts backwards from the end of the list. Because the element is specified as a range, it is retrieved as a list consisting of one element:

   SET colorlist\=$LISTBUILD("Red","Orange","Yellow","Green","Blue","Violet")
   WRITE $LISTTOSTRING($LIST(colorlist,5,5))
   WRITE $LISTTOSTRING($LIST(colorlist,\*\-1,\*\-1))

 

The following example returns “Red Orange Yellow”, a three-element list string beginning with the first element and ending with the third element in the list:

   SET colorlist\=$LISTBUILD("Red","Orange","Yellow","Green","Blue","Violet")
   WRITE $LISTTOSTRING($LIST(colorlist,1,3))

 

The following example returns “Green Blue Violet”, a three-element list string beginning with the fourth element and ending with the last element in the list:

   SET colorlist\=$LISTBUILD("Red","Orange","Yellow","Green","Blue","Violet")
   WRITE $LISTTOSTRING($LIST(colorlist,4,\*))

 

The following example returns a list element from a property:

  SET cfg\=##class(%iKnow.Configuration).%New("Trilingual",1,$LB("en","fr","es"))
  WRITE $LIST(cfg.Languages,2)

 

Examples of Replacing, Removing, or Appending Elements with SET $LIST

The following example shows SET $LIST replacing the second element:

   SET fruit\=$LISTBUILD("apple","onion","banana","pear")
   WRITE !,$LISTTOSTRING(fruit,"/")
   SET $LIST(fruit,2)\="orange"
   WRITE !,$LISTTOSTRING(fruit,"/")

 

The following example shows SET $LIST replacing the second and third elements:

   SET fruit\=$LISTBUILD("apple","potato","onion","pear")
   WRITE !,$LISTTOSTRING(fruit,"/")
   SET $LIST(fruit,2,3)\=$LISTBUILD("orange","banana")
   WRITE !,$LISTTOSTRING(fruit,"/")

 

The following example shows SET $LIST replacing the second and third elements with four elements:

   SET fruit\=$LISTBUILD("apple","potato","onion","pear")
   WRITE !,$LISTTOSTRING(fruit,"/")
   SET $LIST(fruit,2,3)\=$LISTBUILD("orange","banana","peach","tangerine")
   WRITE !,$LISTTOSTRING(fruit,"/")

 

The following example shows SET $LIST appending an element to the end of the list:

   SET fruit\=$LISTBUILD("apple","orange","banana","peach")
   WRITE $LL(fruit)," ",$LISTTOSTRING(fruit,"/",1),!
   SET $LIST(fruit,\*+1)\="pear"
   WRITE $LL(fruit)," ",$LISTTOSTRING(fruit,"/",1)

 

The following example shows SET $LIST appending an element three positions past the end of the list:

   SET fruit\=$LISTBUILD("apple","orange","banana","peach")
   WRITE $LL(fruit)," ",$LISTTOSTRING(fruit,"/",1),!
   SET $LIST(fruit,\*+3)\="tangerine"
   WRITE $LL(fruit)," ",$LISTTOSTRING(fruit,"/",1)

 

The following four examples show SET $LIST using \*-n syntax to replace elements by offset from the end of the list. Note that SET $LIST(x,\*-n) and SET $LIST(x,n,\*-n perform different operations: SET $LIST(x,\*-n) replaces the value of the specified element; SET $LIST(x,n,\*-n) deletes the specified range of elements, then appends the specified list.

To replace the next-to-last element with a single value, use SET $LIST(x,\*-1):

   SET fruit\=$LISTBUILD("apple","banana","orange","potato","pear")
   WRITE !,"list length is ",$LISTLENGTH(fruit)," "
   WRITE $LISTTOSTRING(fruit,"/")
   SET $LIST(fruit,\*\-1)\="peach"
   WRITE !,"list length is ",$LISTLENGTH(fruit)," "
   WRITE $LISTTOSTRING(fruit,"/")

 

To remove a single element by offset from the end of the list, use SET $LIST(x,\*-n,\*-n)="":

   SET fruit\=$LISTBUILD("apple","banana","orange","potato","pear")
   WRITE !,"list length is ",$LISTLENGTH(fruit)," "
   WRITE $LISTTOSTRING(fruit,"/")
   SET $LIST(fruit,\*\-1,\*\-1)\=""
   WRITE !,"list length is ",$LISTLENGTH(fruit)," "
   WRITE $LISTTOSTRING(fruit,"/")

 

To replace a single element by offset from the end of the list with a list of elements, use SET $LIST(x,\*-n,\*-n)=list:

   SET fruit\=$LISTBUILD("apple","banana","potato","orange","pear")
   WRITE !,"list length is ",$LISTLENGTH(fruit)," "
   WRITE $LISTTOSTRING(fruit,"/")
   SET $LIST(fruit,\*\-2,\*\-2)\=$LISTBUILD("peach","plum","quince")
   WRITE !,"list length is ",$LISTLENGTH(fruit)," "
   WRITE $LISTTOSTRING(fruit,"/")

 

To replace a single element by offset from the end of the list with a sublist, use SET $LIST(x,\*-n)=list:

   SET fruit\=$LISTBUILD("apple","banana","potato","orange","pear")
   WRITE !,"list length is ",$LISTLENGTH(fruit)," "
   WRITE $LISTTOSTRING(fruit,"/")
   SET $LIST(fruit,\*\-2)\=$LISTBUILD("peach","plum","quince")
   WRITE !,"list length is ",$LISTLENGTH(fruit)," "
   WRITE $LISTTOSTRING(fruit,"/")

 

The following example shows SET $LIST removing elements from the list, beginning with the third element through the end of the list:

   SET fruit\=$LISTBUILD("apple","orange","onion","peanut","potato")
   WRITE !,"list length is ",$LISTLENGTH(fruit)," "
   WRITE $LISTTOSTRING(fruit,"/")
   SET $LIST(fruit,3,\*)\=""
   WRITE !,"list length is ",$LISTLENGTH(fruit)," "
   WRITE $LISTTOSTRING(fruit,"/")

 

Notes

Unicode

If one Unicode character appears in a list element, that entire list element is represented as Unicode (wide) characters. Other elements in the list are not affected.

The following example shows two lists. The y list consists of two elements which contain only ASCII characters. The z list consists of two elements: the first element contains a Unicode character ($CHAR(960) = the pi symbol); the second element contains only ASCII characters.

   IF $SYSTEM.Version.IsUnicode()  {
   SET y\=$LISTBUILD("ABC"\_$CHAR(68),"XYZ")
   SET z\=$LISTBUILD("ABC"\_$CHAR(960),"XYZ")
   WRITE !,"The ASCII list y elements: "
   ZZDUMP $LIST(y,1)
   ZZDUMP $LIST(y,2)
   WRITE !,"The Unicode list z elements: "
   ZZDUMP $LIST(z,1)
   ZZDUMP $LIST(z,2)
   }
   ELSE {WRITE "This example requires a Unicode installation of Caché"}

 

Note that Caché encodes the first element of z entirely in wide Unicode characters. The second element of z contains no Unicode characters, and thus Caché encodes it using narrow ASCII characters.

$LIST Compared with $EXTRACT and $PIECE

$LIST determines an element from an encoded list by counting elements (not characters) from the beginning (or end) of the list.

[$EXTRACT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fextract) determines a substring by counting characters from the beginning (or end) of a string. $EXTRACT takes as input an ordinary character string.

[$PIECE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fpiece) determines a substring by counting user-defined delimiter characters within the string. $PIECE takes as input an ordinary character string containing multiple instances of a character (or string) intended for use as a delimiter.

$LIST cannot be used on ordinary strings. $PIECE and $EXTRACT cannot be used on encoded lists.

See Also

*   [$LISTBUILD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistbuild) function
    
*   [$LISTDATA](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistdata) function
    
*   [$LISTFIND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistfind) function
    
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

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flength "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistbuild "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_flist.xml**