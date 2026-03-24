# $LISTSAME

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_d_listsame

---

 

Caché SQL Reference

$LISTSAME

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listlength "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listtostring "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [$LISTSAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listsame "$LISTSAME") \]

Go to: Description Arguments Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A list function that compares two lists and returns a boolean value.

Synopsis

$LISTSAME(list1,list2)

Arguments

list1

An expression that evaluates to a valid list.

list2

An expression that evaluates to a valid list.

Description

$LISTSAME compares the contents of two lists and returns 1 if the lists are the same. If the lists are not the same, $LISTSAME returns 0. $LISTSAME compares the two lists element-by-element. For two lists to be the same, they must contain the same number of elements and each element in list1 must match the corresponding element in list2.

$LISTSAME compares list elements using their string representations. $LISTSAME comparisons are case-sensitive. $LISTSAME compares the two lists element-by-element in left-to-right order. Therefore $LISTSAME returns a value of 0 when it encounters the first non-matching pair of list elements; it does not check subsequent items to determine if they are valid list elements.

This function returns data of type SMALLINT.

Arguments

list (list1 and list2)

A list is an encoded character string containing one or more elements. You can create a list using the SQL [$LISTBUILD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listbuild) function or the Caché ObjectScript [$LISTBUILD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistbuild) function. You can convert a delimited string into a list using the SQL [$LISTFROMSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listfromstring) function or the Caché ObjectScript [$LISTFROMSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistfromstring) function. You can extract a list from an existing list using the SQL [$LIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_list) function or the Caché ObjectScript [$LIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flist) function.

The following are examples of valid lists:

*   $LISTBUILD('a','b','c'): a three-element list.
    
*   $LISTBUILD('a','','c'): a three-element list, the second element of which has a null string value.
    
*   $LISTBUILD('a',,'c') or $LISTBUILD('a',NULL,'c'): a three-element list, the second element of which has no value.
    
*   $LISTBUILD(NULL,NULL) or $LISTBUILD(,NULL): a two-element list, the elements of which have no values.
    
*   $LISTBUILD(NULL) or $LISTBUILD(): a one-element list, the element has no value.
    

If a list argument is NULL, $LISTSAME returns NULL. If a list argument is not a valid list (and is not NULL), Caché SQL generates an SQLCODE -400 fatal error.

Examples

The following embedded SQL example uses $LISTSAME to compare two list arguments:

  SET a\=$LISTBUILD("Red",,"Yellow","Green","","Violet")
  SET b\=$LISTBUILD("Red",,"Yellow","Green","","Violet")
 &sql(SELECT $LISTSAME(:a,:b)
      INTO :c )
  IF SQLCODE'=0 {
    WRITE !,"Error code ",SQLCODE }
  ELSEIF c\=1 { WRITE "lists a and b are the same",! }
  ELSE { WRITE "lists a and b are not the same",! }

 

The following SQL example compares lists with NULL, absent, or empty string elements:

  SELECT $LISTSAME($LISTBUILD('Red',NULL,'Blue'),$LISTBUILD('Red',,'Blue')) AS NullAbsent,
         $LISTSAME($LISTBUILD('Red',NULL,'Blue'),$LISTBUILD('Red','','Blue')) AS NullEmpty,
         $LISTSAME($LISTBUILD('Red',,'Blue'),$LISTBUILD('Red','','Blue')) AS AbsentEmpty

 

$LISTSAME comparison is not the same equivalence test as the one used by the Caché equal sign. An equal sign compares the two lists as encoded strings (character-by-character); $LISTSAME compares the two lists element-by-element. This distinction is easily seen when comparing a number and a numeric string, as in the following example:

   SET a \= $LISTBUILD("365")
   SET b \= $LISTBUILD(365)
   IF a\=b 
     { WRITE "Equal sign: lists a and b are the same",! }
   ELSE { WRITE "Equal sign: lists a and b are not the same",! }
  &sql(SELECT $LISTSAME(:a,:b)
      INTO :c )
  IF SQLCODE'=0 {
    WRITE !,"Error code ",SQLCODE }
  ELSEIF c\=1 { WRITE "$LISTSAME: lists a and b are the same",! }
  ELSE { WRITE "$LISTSAME: lists a and b are not the same",! }

 

The following SQL example compares lists containing numbers and numeric strings in canonical and non-canonical forms. When comparing a numeric list element and a string list element, the string list element must represent the numeric in canonical form; this is because Caché always reduces numbers to canonical form before performing a comparison. In the following example, $LISTSAME compares a string and a number. The first three $LISTSAME functions return 1 (identical); the fourth $LISTSAME function returns 0 (not identical) because the string representation is not in canonical form:

  SELECT $LISTSAME($LISTBUILD('365'),$LISTBUILD(365)),
         $LISTSAME($LISTBUILD('365'),$LISTBUILD(365.0)),
         $LISTSAME($LISTBUILD('365.5'),$LISTBUILD(365.5)),
         $LISTSAME($LISTBUILD('365.0'),$LISTBUILD(365.0))

 

See Also

*   SQL functions: [$LIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_list) [$LISTBUILD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listbuild) [$LISTDATA](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listdata) [$LISTFIND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listfind) [$LISTFROMSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listfromstring) [$LISTGET](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listget) [$LISTLENGTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listlength) [$LISTTOSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listtostring) [$PIECE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_piece)
    
*   Caché ObjectScript functions: [$LIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flist) [$LISTBUILD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistbuild) [$LISTDATA](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistdata) [$LISTFIND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistfind) [$LISTFROMSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistfromstring) [$LISTGET](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistget) [$LISTLENGTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistlength) [$LISTNEXT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistnext) [$LISTSAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistsame) [$LISTTOSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flisttostring) [$LISTVALID](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistvalid)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listlength "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listtostring "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_d\_listsame.xml**