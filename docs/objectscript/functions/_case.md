# $CASE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fcase

---

 

Caché ObjectScript Reference

$CASE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fbitlogic "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fchar "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$CASE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fcase "$CASE") \]

Go to: Description Parameters Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Compares expressions and returns the value of the first matching case.

Synopsis

$CASE(target,case:value,case:value,...,:default)

Parameters

target

A literal or expression the value of which is to be matched against cases.

case

A literal or expression the value of which is to be matched with the results of the evaluation of target.

value

The value to be returned upon a successful match of the corresponding case.

default

Optional — The value to be returned if no case matches target.

Description

The $CASE function compares target to a list of cases (literals or expressions), and returns the value of the first matching case. An unlimited number of case:value pairs can be specified. Cases are matched in the order specified (left-to-right); matching stops when the first exact match is encountered.

If there is no matching case, default is returned. If there is no matching case and no default is specified, Caché issues an <ILLEGAL VALUE> error.

Caché permits specifying $CASE with no case:value pairs. It always returns the default value, regardless of the target value.

Parameters

target

$CASE evaluates this expression once, then matches the result to each case in left-to-right order.

case

A case can be a literal or an expression; matching of literals is substantially more efficient than matching expressions, because literals can be evaluated at compile time. Each case must be paired with a value. An unlimited number of case and value pairs may be specified.

value

A value can be a literal or an expression. Using $CASE as an argument of a GOTO command or a DO command restricts value as follows:

*   When using a $CASE statement with a GOTO command, each value must be a valid [line label](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_syntax#GCOS_syntax_labels). It cannot be an expression.
    
*   When using a $CASE statement with a DO command, each value must be a valid DO argument. These DO arguments can include parameters.
    

default

The default is specified like a case:value pair, except that there is no case specified between the comma (used to separate pairs) and the colon (used to pair items). The default is optional. If specified, it is always the final parameter in a $CASE function. The default value follows the same GOTO and DO restrictions as the value parameter.

If there is no matching case and no default is specified, Caché issues an <ILLEGAL VALUE> error.

Examples

The following example takes a day-of-week number and returns the corresponding day name. Note that a default value “entry error” is provided:

  SET daynum\=$ZDATE($HOROLOG,10)
  WRITE $CASE(daynum,
              1:"Monday",2:"Tuesday",3:"Wednesday",
              4:"Thursday",5:"Friday",
              6:"Saturday",7:"Sunday",:"entry error")

 

The following example takes as input the number of bases achieved by a baseball batter and writes out the appropriate baseball term:

  SET hit\=$RANDOM(5)
  SET atbat\=$CASE(hit,1:"single",2:"double",3:"triple",4:"home run",:"strike out")
  WRITE hit," = ",atbat

 

The following example uses $CASE as the [DO](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cdo) command argument. It calls the routine appropriate for the exp exponent value:

Start  ; Raise an integer to a randomly-selected power.
  SET exp\=$RANDOM(6)
  SET num\=4
  DO $CASE(exp,0:NoMul(),2:Square(num),3:Cube(num),:Exponent(num,exp))
  WRITE !,num," ",result,!
  RETURN
Square(n)
  SET result\=n\*n
  SET result\="Squared = "\_result
  RETURN
Cube(n)
  SET result\=n\*n\*n
  SET result\="Cubed = "\_result
  RETURN
Exponent(n,x)
  SET result\=n
  FOR i\=1:1:x\-1 { SET result\=result\*n }
  SET result\="exponent "\_x\_" = "\_result
  RETURN
NoMul()
  SET result\="multiply by zero"
  RETURN

 

The following example tests whether the character input is a letter or some other character:

  READ "Input a letter: ",x
  SET chartype\=$CASE(x?1A,1:"letter",:"other")
  WRITE chartype

The following example uses $CASE to determine which subscripted variable to return:

   SET dabbrv\="W"
   SET wday(1)\="Sunday",wday(2)\="Monday",wday(3)\="Tuesday",
       wday(4)\="Wednesday",wday(5)\="Thursday",wday(6)\="Friday",wday(7)\="Saturday"
   WRITE wday($CASE(dabbrv,"Su":1,"M":2,"Tu":3,"W":4,"Th":5,"F":6,"Sa":7))

 

The following example specifies no case:value pairs. It return the default string “not defined”:

  SET dummy\=3
  WRITE $CASE(dummy,:"not defined")

 

See Also

*   [DO](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cdo) command
    
*   [GOTO](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cgoto) command
    
*   [IF](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cif) command
    
*   [$SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fselect) function
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fbitlogic "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fchar "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fcase.xml**