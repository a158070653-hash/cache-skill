# $RANDOM

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_frandom

---

 

Caché ObjectScript Reference

$RANDOM

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fquery "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_freplace "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$RANDOM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_frandom "$RANDOM") \]

Go to: Description Parameters Examples

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Returns a pseudo-random integer value in the specified range.

Synopsis

$RANDOM(range)
$R(range)

Parameter

range

A nonzero positive integer used to specify the upper bound of the range of possible random numbers.

Description

$RANDOM returns a pseudo-random integer value between 0 and range\-1 (inclusive). Thus $RANDOM(3) returns 0, 1, 2, but not 3. Returned numbers are uniformly distributed across the specified range.

$RANDOM is sufficiently random for most purposes. Applications that require strictly random values should use the [GenCryptRand()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Encryption#GenCryptRand) method of the [%SYSTEM.Encryption](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Encryption) class.

Parameters

range

This value specifies the upper bound of the range of possible random numbers; the highest random number being range minus 1. The range value can be a nonzero positive integer value, the name of an integer variable, or any valid Caché ObjectScript expression that evaluates to a nonzero positive integer. The maximum range value is 1E17 (100000000000000000); specifying a value beyond this maximum results in a <FUNCTION> error. $RANDOM(1) is valid, but always returns 0. $RANDOM(0) results in a <FUNCTION> error.

Examples

The following example returns a random number from 0 through 24 (inclusive).

   WRITE $RANDOM(25)

 

To return a random number with a fractional portion, you can use the concatenation operator (\_) or the addition operator (+), as shown in the following example:

   SET x\=$RANDOM(10)\_$RANDOM(10)/10
   WRITE !,x
   SET y\=$RANDOM(10)+($RANDOM(10)/10)
   WRITE !,y

 

This program returns numbers with one fractional digit, ranging between .0 and 9.9 (inclusive). Using either operator, Caché deletes any leading and trailing zeros (and the decimal point, if the fractional portion is zero). However, if both $RANDOM functions return zero (0 and .0), Caché returns a zero (0).

The following example simulates the roll of two dice:

Dice
   FOR {
      READ "Roll dice? ",reply#1
      IF "Yy"\[reply,reply'="" {
         WRITE !,"Pair of dice: "
         WRITE $RANDOM(6)+1,"+",$RANDOM(6)+1,! }
      ELSE { QUIT }
   }

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fquery "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_freplace "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_frandom.xml**