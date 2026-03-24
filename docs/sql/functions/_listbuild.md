# $LISTBUILD

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_d_listbuild

---

 

Caché SQL Reference

$LISTBUILD

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_list "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listdata "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [$LISTBUILD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listbuild "$LISTBUILD") \]

Go to: Description Examples Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A list function that builds a list from strings.

Synopsis

$LISTBUILD(element,...)

Argument

element

Any expression, or comma-separated list of expressions.

Description

$LISTBUILD takes one or more expressions and returns a list with one element for each expression.

The following functions can be used to create a list:

*   $LISTBUILD, which creates a list from multiple strings, one string per element.
    
*   $LISTFROMSTRING, which creates a list from a single string containing multiple delimited elements.
    
*   $LIST, which extracts a sublist from an existing list.
    

$LISTBUILD is used with the other Caché SQL list functions: $LIST, $LISTDATA, $LISTFIND, $LISTFROMSTRING, $LISTGET, $LISTLENGTH, and $LISTTOSTRING.

Note:

$LISTBUILD and the other $LIST functions use an optimized binary representation to store data elements. For this reason, equivalency tests may not work as expected with some $LIST data. Data that might, in other contexts, be considered equivalent, may have a different internal representation. For example, $LISTBUILD(1) is not equal to $LISTBUILD('1').

For the same reason, a list string value returned by $LISTBUILD should not be used in character search and parse functions that use a delimiter character, such as $PIECE and the two-argument form of $LENGTH. Elements in a list created by $LISTBUILD are not marked by a character delimiter, and thus can contain any character.

Examples

The following Embedded SQL example takes three strings and produces a three-element list:

  SET x\="Red"
  SET y\="White"
  SET z\="Blue"
  &sql(SELECT $LISTBUILD(:x,:y,:z)
       INTO :listout)
  IF SQLCODE\=0 {WRITE listout," length ",$LISTLENGTH(listout)}
  ELSE {WRITE "Error code:",SQLCODE}

 

Notes

Omitting Arguments

Omitting an element expression yields an element whose value is NULL. For example, the following Embedded SQL contains two $LISTBUILD statements that both produce a three-element list whose second element has an undefined (NULL) value:

  SET x\="Red"
  SET y\="White"
  SET z\="Blue"
  &sql(SELECT $LISTBUILD(:x,,:z),
              $LISTBUILD(:x,'',:z)
       INTO :list1,list2)
  IF SQLCODE\=0 {WRITE list1," length ",$LISTLENGTH(list1),!
                WRITE list2," length ",$LISTLENGTH(list2)}
  ELSE {WRITE "Error code:",SQLCODE}

 

Additionally, if a $LISTBUILD expression is undefined, the corresponding list element has an undefined value. The following Embedded SQL example produces a two-element list whose first element is "Red" and whose second element has an undefined value:

  &sql(SELECT $LISTBUILD('Red',:z)
       INTO :list1)
  IF SQLCODE\=0 {WRITE list1," length ",$LISTLENGTH(list1)}
  ELSE {WRITE "Error code:",SQLCODE}

 

The following Embedded SQL example produces a two-element list. The trailing comma indicates the second element has an undefined value:

  &sql(SELECT $LISTBUILD('Red',)
       INTO :list1)
  IF SQLCODE\=0 {WRITE list1," length ",$LISTLENGTH(list1)}
  ELSE {WRITE "Error code:",SQLCODE}

 

Providing No Arguments

Invoking the $LISTBUILD function with no arguments returns a list with one element whose data value is undefined. This is not the same as NULL. The following are valid $LISTBUILD statements that create “empty” lists:

  &sql(SELECT $LISTBUILD(),
              $LISTBUILD(NULL)
       INTO :list1,:list2)
  IF SQLCODE\=0 {
     ZZDUMP list1
     WRITE !,"length ",$LISTLENGTH(list1),!
     ZZDUMP list2
     WRITE !,"length ",$LISTLENGTH(list2),!
  }
  ELSE {WRITE "Error code:",SQLCODE}

 

The following are valid $LISTBUILD statements that create a list element that contains an empty string:

  &sql(SELECT $LISTBUILD(''),
              $LISTBUILD(CHAR(0))
       INTO :list1,:list2)
  IF SQLCODE\=0 {
     ZZDUMP list1
     WRITE !,"length ",$LISTLENGTH(list1),!
     ZZDUMP list2
     WRITE !,"length ",$LISTLENGTH(list2),!
  }
  ELSE {WRITE "Error code:",SQLCODE}

 

Nesting Lists

An element of a list may itself be a list. For example, the following statement produces a three-element list whose third element is the two-element list, "Walnut,Pecan":

SELECT $LISTBUILD('Apple','Pear',$LISTBUILD('Walnut','Pecan'))

 

Concatenating Lists

The result of concatenating two lists with the SQL Concatenate operator (||) is another list. For example, the following SELECT items produce the same list, "A,B,C":

SELECT $LISTBUILD('A','B','C') AS List,
  $LISTBUILD('A','B')||$LISTBUILD('C') AS CatList

 

In the following example, the first two select items result in the same two-element list; the third select item results in NULL (because concatenating NULL to anything results in NULL); the fourth and fifth select items result in the same three-element list:

SELECT
  $LISTBUILD('A','B') AS List,
  $LISTBUILD('A','B')||'' AS CatEStr,
  $LISTBUILD('A','B')||NULL AS CatNull,
  $LISTBUILD('A','B')||$LISTBUILD('') AS CatEList,
  $LISTBUILD('A','B')||$LISTBUILD(NULL) AS CatNList

 

Unicode

If one or more characters in a list element is a wide (Unicode) character, all characters in that element are represented as wide characters. To ensure compatibility across systems, $LISTBUILD always stores these bytes in the same order, regardless of the hardware platform. Wide characters are represented as byte strings. For further details, refer to the Caché ObjectScript [$LISTBUILD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistbuild) function in the Caché ObjectScript Reference.

See Also

*   SQL functions: [$LIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_list), [$LISTDATA](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listdata), [$LISTFIND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listfind), [$LISTFROMSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listfromstring), [$LISTGET](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listget), [$LISTLENGTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listlength), [$LISTSAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listsame), [$LISTTOSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listtostring), [$PIECE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_piece)
    
*   Caché ObjectScript functions: [$LIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flist), [$LISTBUILD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistbuild), [$LISTDATA](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistdata), [$LISTFIND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistfind), [$LISTFROMSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistfromstring), [$LISTGET](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistget), [$LISTLENGTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistlength), [$LISTNEXT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistnext), [$LISTSAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistsame), [$LISTTOSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flisttostring), [$LISTVALID](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistvalid)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_list "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listdata "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_d\_listbuild.xml**