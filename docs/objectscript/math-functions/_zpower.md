# $ZPOWER

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fzpower

---

 

Caché ObjectScript Reference

$ZPOWER

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzlog "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzsec "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Additional Math and Time Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_MATHFUNCTIONS "Additional Math and Time Functions") \]  >  \[ [$ZPOWER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzpower "$ZPOWER") \]

Go to: Description Parameters Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Returns the value of a number raised to the selected power.

Synopsis

$ZPOWER(num,exponent)

Parameters

num

The number to be raised to a power.

exponent

The exponent.

Description

$ZPOWER returns the value of the num parameter raised to the nth power.

This function performs the same operation as the Exponentiation operator (\*\*). For details on valid parameter values and the value returned for specific combinations of parameter values, see [Exponentiation Operator](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_operators#GCOS_operators_expon) in the “Operators and Expressions” chapter of Using Caché ObjectScript.

Parameters

num

The number to be raised to a power. It can be integer or floating point, negative, positive, or zero. It can be a standard Caché number or an IEEE double-precision number (a [$DOUBLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fdouble) number). It can be specified as a value, a variable, or an expression. If specified as a quoted string, evaluation terminates at the first nonnumeric character. The null string ("") and nonnumeric strings evaluate to zero.

exponent

The exponent is a number that can be integer or floating point, negative, positive, or zero. It can be a standard Caché number or an IEEE double-precision number (a [$DOUBLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fdouble) number). It can be specified as a numeric or string value, a variable, or an expression. The null string ("") and nonnumeric strings evaluate to zero.

The following combinations of num and exponent result in an error:

*   If num is negative, exponent must be an integer. Otherwise an <ILLEGAL VALUE> error is generated.
    
*   If num is 0, exponent must be a positive number or zero. Otherwise an <ILLEGAL VALUE> or <DIVIDE> error is generated.
    
*   Large exponent values, such as $ZPOWER(9,153) may result in an overflow, generating a <MAXNUMBER> error, or may result in an underflow, returning 0. Which result occurs depends on whether num is greater than 1 (or -1), and whether exponent is positive or negative.
    

For further details on valid parameter values and the value returned for specific combinations of parameter values, see [Exponentiation Operator](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_operators#GCOS_operators_expon) in the “Operators and Expressions” chapter of Using Caché ObjectScript.

Examples

The following example raises 2 to the first ten powers:

  SET x\=0
  WHILE x < 10 {
    SET rtn\=$ZPOWER(2,x)
    WRITE !,"The ",x," power of 2=",rtn
    SET x\=x+1 }

 

See Also

*   [$ZSQR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzsqr) function
    
*   [$ZEXP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzexp) function
    
*   [$ZLN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzln) function
    
*   [$ZLOG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzlog) function
    
*   [Operators](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_operators) in Using Caché ObjectScript
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzlog "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzsec "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fzpower.xml**