# $LISTTOSTRING

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_flisttostring

---

 

Caché ObjectScript Reference

$LISTTOSTRING

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistsame "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistupdate "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$LISTTOSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flisttostring "$LISTTOSTRING") \]

Go to: Description Parameters Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Creates a string from a list.

Synopsis

$LISTTOSTRING(list,delimiter,flag)
$LTS(list,delimiter,flag)

Parameters

list

A Caché list, created using $LISTBUILD or $LISTFROMSTRING, or extracted from another list using $LIST.

delimiter

Optional — A delimiter used to separate substrings. Specify delimiter as a quoted string. If no delimiter is specified, the default is the comma (,) character.

flag

Optional — A boolean value that specifies how to handle an omitted list element. 0 issues a <NULL VALUE> error. 1 inserts an empty string for the element. The default is 0.

Description

$LISTTOSTRING takes a Caché list and converts it to a string. In the resulting string, the elements of the list are separated by the delimiter.

A list represents data in an encoded format which does not use delimiter characters. Thus a list can contain all possible characters, and is ideally suited for bitstring data. $LISTTOSTRING converts this list to a string with delimited elements. It sets aside a specified character (or character string) to serve as a delimiter. These delimited elements can be handled using the $PIECE function.

Note:

The delimiter specified here must not occur in the source data. Caché makes no distinction between a character serving as a delimiter and the same character as a data character.

Parameters

list

A Caché list, which contains one or more elements. A list is created using $LISTBUILD or extracted from another list using $LIST.

If the expression in the list parameter does not evaluate to a valid list, a <LIST> error occurs.

   SET x\=$CHAR(0,0,0,1,16,27,134,240)
   SET a\=$LISTTOSTRING(x,",")   // generates a <LIST> error

delimiter

A character (or string of characters) used to delimit substrings within the output string. It can be a numeric or string literal (enclosed in quotation marks), the name of a variable, or an expression that evaluates to a string.

Commonly, a delimiter is a designated character which is never used within string data, but is set aside solely for use as a delimiter separating substrings. A delimiter can also be a multi-character string, the individual characters of which can be used within string data.

If you specify no delimiter, the default delimiter is the comma (,) character. You can specify a null string ("") as a delimiter; in this case, substrings are concatenated with no delimiter. To specify a quote character as a delimiter, specify the quote character twice ("""") or use $CHAR(34).

flag

A boolean flag used to specify how to handle omitted elements in list. In the following example, the $LISTBUILD creates a list with an omitted element, and the flag\=1 option is specified to handle this list element:

  SET colorlist\=$LISTBUILD("Red",,"Blue")
  WRITE $LISTTOSTRING(colorlist,,1)

 

If the flag option was omitted or set to 0, the $LISTTOSTRING would generate a <NULL VALUE> error.

Note that if flag\=1, an element with an empty string value is indistinguishable from an omitted element. Thus $LISTBUILD("Red","","Blue") and $LISTBUILD("Red",,"Blue") would return the same $LISTTOSTRING value. This flag\=1 behavior is compatible with the implementation of [$LISTTOSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_listtostring) in Caché SQL, as described in the Caché SQL Reference.

Examples

The following example creates a list of four elements, then converts it to a string with the elements delimited by the colon (:) character:

   SET namelist\=$LISTBUILD("Deborah","Noah","Martha","Bowie")
   WRITE $LISTTOSTRING(namelist,":")

 

returns "Deborah:Noah:Martha:Bowie"

The following example creates a list of four elements, then converts it to a string with the elements delimited by the \*sp\* string:

   SET namelist\=$LISTBUILD("Deborah","Noah","Martha","Bowie")
   WRITE $LISTTOSTRING(namelist,"\*sp\*")

 

returns "Deborah\*sp\*Noah\*sp\*Martha\*sp\*Bowie"

The following example creates a list with one omitted element and one element with an empty string value. $LISTTOSTRING converts it to a string with the elements delimited by the colon (:) character. Because of the omitted element, flag\=1 is required to avoid a <NULL VALUE> error. However, when flag\=1, the omitted element and the empty string value are indistinguishable:

   SET namelist\=$LISTBUILD("Deborah",,"","Bowie")
   WRITE $LISTTOSTRING(namelist,":",1)

 

returns "Deborah:::Bowie"

$LISTVALID considers all of the following valid lists. With flag\=1, $LISTTOSTRING returns the null string ("") for all of them:

  WRITE "1",$LISTTOSTRING("",,1),!
  WRITE "2",$LISTTOSTRING($LB(),,1),!
  WRITE "3",$LISTTOSTRING($LB(NULL),,1),!
  WRITE "4",$LISTTOSTRING($LB(""),,1)

 

With flag\=0, $LISTTOSTRING returns the null string ("") for only the following:

  WRITE "1",$LISTTOSTRING("",,0),!
  WRITE "4",$LISTTOSTRING($LB(""),,0)

 

The others generate a <NULL VALUE> error.

See Also

*   [$LISTFROMSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistfromstring) function
    
*   [$LISTBUILD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistbuild) function
    
*   [$LIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flist) function
    
*   [$PIECE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fpiece) function
    
*   [$LISTDATA](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistdata) function
    
*   [$LISTFIND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistfind) function
    
*   [$LISTGET](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistget) function
    
*   [$LISTLENGTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistlength) function
    
*   [$LISTNEXT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistnext) function
    
*   [$LISTSAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistsame) function
    
*   [$LISTUPDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistupdate) function
    
*   [$LISTVALID](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistvalid) function
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistsame "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistupdate "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_flisttostring.xml**