# $LISTGET

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_flistget

---

 

Caché ObjectScript Reference

$LISTGET

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistfromstring "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistlength "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$LISTGET](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistget "$LISTGET") \]

Go to: Description Parameters Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Returns an element in a list, or a specified default value if the requested element is undefined.

Synopsis

$LISTGET(list,position,default)
$LG(list,position,default)

Parameters

list

An expression that evaluates to a valid list.

position

Optional — An integer code specifying the starting position in list. Permitted values are n (count from beginning of list), \* (last element in list), and \*-n (relative offset count backwards from end of list). Thus, the first element in the list is 1, the second element is 2, the last element in the list is \*, and the next-to-last element is \*-1. If position is a fractional number, it is truncated to its integer part. If position is omitted, it defaults to 1.

\-1 may be used in older code to specify the last element in the list. This deprecated use of -1 should not be combined with \* or \*-n relative offset syntax.

default

Optional — An expression that provides the value to return if the list element has an undefined value. If default is omitted, it defaults to the null string (““). You must specify a position parameter value to specify a default value.

Description

$LISTGET returns the requested element in the specified list. If the value of position refers to a nonexistent element or identifies an element with an undefined value, the default value is returned.

The $LISTGET function is identical to the one- and two-argument forms of the $LIST function except that, under conditions that would cause $LIST to produce a <NULL VALUE> error, $LISTGET returns a default value. See the description of the $LIST function for more information on conditions that generate <NULL VALUE> errors.

Parameters

list

A list can be created using $LISTBUILD or $LISTFROMSTRING, or extracted from another list using $LIST. The null string ("") is also treated as a valid list. You can use [$LISTVALID](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistvalid) to determine if list is a valid list. An invalid list causes $LISTGET to generate a <LIST> error.

position

The position (element count) of the list element to return. An element is returned as a string. List elements are counted from 1. If position is omitted, $LISTGET returns the first element.

*   If position is n (a positive integer), $LISTGET counts elements from the beginning of list. If position is greater than the number of elements in list, $LISTGET returns the default value.
    
*   If position is \*, $LIST returns the last element in list.
    
*   If position is \*-n (an asterisk followed by a negative integer), $LIST counts elements by relative offset from the end of list. Thus, \*-0 is the last element in the list, \*-1 is the next-to-last list element (an offset of 1 from the end). If the \*-n offset specifies the position before the first element of list (for example, \*-3 for a 3–element list), $LISTGET returns the default value. If the \*-n offset specifies a position prior to that (for example, \*-4 for a 3–element list), Caché issues a <RANGE> error.
    
*   If position is 0 or -0, $LISTGET returns the default value.
    

The numeric portion of the position parameter evaluates to an integer. Caché truncates a fractional number to its integer portion. Specifying position as -1 (indicating last element of the list) is deprecated and should not be used in new code; a position negative number less than -1 generates a <RANGE> error.

When using a variable to specify \*-n you must always specify the asterisk and a sign character in the parameter itself.

The following are valid specifications of \*-n:

  SET count\=2
  SET alph\=$LISTBUILD("a","b","c","d")
  WRITE $LISTGET(alph,\*\-count,"blank")

 

  SET count\=\-2
  SET alph\=$LISTBUILD("a","b","c","d")
  WRITE $LISTGET(alph,\*+count,"blank")

 

default

An expression that evaluates to a string or numeric value. $LISTGET returns default if the element specified by position does not exist. This may occur if position is beyond the end of the list, if position specifies an element that has no value, if position is 0, or if list contains no elements. However, if a position of \*-n specifies a position before the 0th element of list, Caché issues a <RANGE> error.

If you omit the default parameter, the null string ("") is returned as the default value.

Examples

The $LISTGET functions in the following example return the value of the list element specified by position (the position default is 1):

   SET list\=$LISTBUILD("A","B","C")
   WRITE !,$LISTGET(list)     ; returns "A"
   WRITE !,$LISTGET(list,1)   ; returns "A"
   WRITE !,$LISTGET(list,3)   ; returns "C"
   WRITE !,$LISTGET(list,\*)   ; returns "C"
   WRITE !,$LISTGET(list,\*\-1) ; returns "B"

 

The $LISTGET functions in the following example return a value upon encountering the undefined 2nd element in the list. The first two returns a question mark (?), which the user has defined as the default value. The second two returns a null string because the user has not specified a default value:

   WRITE "returns:",$LISTGET($LISTBUILD("A",,"C"),2,"?"),!
   WRITE "returns:",$LISTGET($LISTBUILD("A",,"C"),\*\-1,"?"),!
   WRITE "returns:",$LISTGET($LISTBUILD("A",,"C"),2),!
   WRITE "returns:",$LISTGET($LISTBUILD("A",,"C"),\*\-1)

 

The following example returns all of the element values in the list. It also lists the positions before and after the ends of the list. Where a value is non-existent, it returns the default value:

   SET list\=$LISTBUILD("a","b",,"d",,,"g")
   SET llen\=$LISTLENGTH(list)
      FOR x\=0:1:llen+1 { 
      WRITE "position ",x,"=",$LISTGET(list,x," no value"),!
      }
      WRITE "end of the list"

 

The following example returns all of the element values in the list in reverse order. Where a value is omitted, it returns the default value:

   SET list\=$LISTBUILD("a","b",,"d",,,"g")
   SET llen\=$LISTLENGTH(list)
      FOR x\=0:1:llen { 
      WRITE "position \*-",x,"=",$LISTGET(list,\*\-x," no value"),!
      }
      WRITE "beginning of the list"

 

The $LISTGET functions in the following example return the list element value of the null string; they do not return the default value:

   WRITE "returns:",$LISTGET($LB(""),1,"no value"),!
   WRITE "returns:",$LISTGET($LB(""),\*,"no value"),!
   WRITE "returns:",$LISTGET($LB(""),\*\-0,"no value")

 

The $LISTGET functions in the following example all return the default value:

   WRITE $LISTGET("",1,"no value"),!
   WRITE $LISTGET($LB(),1,"no value"),!
   WRITE $LISTGET($LB(NULL),1,"no value"),!
   WRITE $LISTGET($LB(,),1,"no value"),!
   WRITE $LISTGET($LB(,),\*,"no value"),!
   WRITE $LISTGET($LB(,),\*\-1,"no value"),!
   WRITE $LISTGET($LB(""),2,"no value"),!
   WRITE $LISTGET($LB(""),\*\-1,"no value")

 

See Also

*   [$LIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flist) function
    
*   [$LISTBUILD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistbuild) function
    
*   [$LISTDATA](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistdata) function
    
*   [$LISTFIND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistfind) function
    
*   [$LISTFROMSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistfromstring) function
    
*   [$LISTLENGTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistlength) function
    
*   [$LISTNEXT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistnext)
    
*   [$LISTSAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistsame) function
    
*   [$LISTTOSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flisttostring) function
    
*   [$LISTUPDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistupdate) function
    
*   [$LISTVALID](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistvalid) function
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistfromstring "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistlength "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_flistget.xml**