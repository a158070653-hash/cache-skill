# $ZLOG

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fzlog

---

 

Caché ObjectScript Reference

$ZLOG

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzln "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzpower "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Additional Math and Time Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_MATHFUNCTIONS "Additional Math and Time Functions") \]  >  \[ [$ZLOG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzlog "$ZLOG") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Returns the base 10 logarithm value of the specified positive numeric expression.

Synopsis

$ZLOG(n)

Parameter

n

Any positive, nonzero number, which can be specified as a value, a variable, or an expression.

Description

$ZLOG returns the base 10 logarithm value of n.

Specifying zero or a negative number results in an <ILLEGAL VALUE> error.

The corresponding natural log (base e) function is [$ZLN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzln).

Examples

The following example writes the base 10 logarithms of the integers 1 through 10:

   FOR x\=1:1:10 {
     WRITE !,"The log of ",x," = ",$ZLOG(x) 
     }
   QUIT

 

returns:

The log of 1 = 0
The log of 2 = .301029995663981195
The log of 3 = .477121254719662437
The log of 4 = .60205999132796239
The log of 5 = .698970004336018805
The log of 6 = .778151250383643633
The log of 7 = .845098040014256831
The log of 8 = .903089986991943586
The log of 9 = .954242509439324875
The log of 10 = 1

   WRITE $ZLOG($ZPI)

 

returns .4971498726941338541.

   WRITE $ZLOG(.5)

 

returns -.301029995663981195.

   WRITE $ZLOG(0)

issues an <ILLEGAL VALUE> error.

See Also

*   [$ZEXP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzexp) function
    
*   [$ZLN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzln) function
    
*   [$ZPI](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzpi) special variable
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzln "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzpower "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fzlog.xml**