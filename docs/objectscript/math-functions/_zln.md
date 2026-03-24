# $ZLN

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fzln

---

 

Caché ObjectScript Reference

$ZLN

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzhex "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzlog "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Additional Math and Time Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_MATHFUNCTIONS "Additional Math and Time Functions") \]  >  \[ [$ZLN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzln "$ZLN") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Returns the natural logarithm of the specified number.

Synopsis

$ZLN(n)

Parameter

n

Any positive nonzero number, which can be specified as a value, a variable, or an expression.

Description

$ZLN returns the natural logarithm (base e) value of n.

Specifying zero or a negative number results in an <ILLEGAL VALUE> error.

The corresponding natural logarithm power function is [$ZEXP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzexp).

Examples

The following example writes the natural log of the integers 1 through 10:

   FOR x\=1:1:10 {
     WRITE !,"The natural log of ",x," = ",$ZLN(x) 
     }
   QUIT

 

returns:

The natural log of 1 = 0
The natural log of 2 = .6931471805599453089
The natural log of 3 = 1.098612288668109691
The natural log of 4 = 1.386294361119890618
The natural log of 5 = 1.609437912434100375
The natural log of 6 = 1.791759469228055002
The natural log of 7 = 1.945910149055313306
The natural log of 8 = 2.079441541679835929
The natural log of 9 = 2.197224577336219384
The natural log of 10 = 2.302585092994045684

The following example shows the relationship between $ZLN and $ZEXP:

   SET x\=$ZEXP(1) ; x = 2.718281828459045236
   WRITE $ZLN(x)

 

returns 1.

   WRITE $ZLN(0)

issues an <ILLEGAL VALUE> error.

See Also

*   [$ZEXP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzexp) function
    
*   [$ZLOG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzlog) function
    
*   [$ZPI](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzpi) special variable
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzhex "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzlog "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fzln.xml**