# $FACTOR

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_ffactor

---

 

Caché ObjectScript Reference

$FACTOR

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fextract "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ffind "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$FACTOR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ffactor "$FACTOR") \]

Go to: Description Parameters Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Converts an integer to a $BIT bitstring.

Synopsis

$FACTOR(num,scale)

Parameters

num

An expression that evaluates to a number. num is converted to a positive integer before bitstring conversion. A negative number is converted to a positive number (its absolute value). A fractional number is rounded to an integer.

scale

Optional — An integer used as a power-of-ten exponent (scientific notation) multiplier for num. The default is 0.

Description

$FACTOR returns the [$BIT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fbit) format bitstring that corresponds to the binary representation of the supplied integer. It performs the following operations:

*   If you specify a negative number, $FACTOR takes the absolute value of the number.
    
*   If you specify a scale $FACTOR multiplies the integer by 10\*\*scale.
    
*   If you specify a fractional number $FACTOR rounds this number to an integer. When rounding numbers, Caché rounds the fraction .5 up to the next highest integer.
    
*   $FACTOR converts the integer to its binary representation.
    
*   $FACTOR converts this binary number to $BIT encoded binary format.
    

The binary string returned specifies bit positions starting from the least significant bit at position 1 (one's place at position 1). This corresponds to the bitstrings used by the various [$BIT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fbit) functions.

Parameters

num

A number (or an expression that evaluates to a number). $FACTOR applies the scale parameter (if supplied), converts this number to an integer by rounding, and then returns the corresponding bitstring. num can be positive or negative. If num is a mixed numeric string (for example “7dwarves” or “5.6.8”) $FACTOR converts the numeric part of the string (in our example, 7 and 5.6) until it encounters a nonnumeric character. If num is zero, or rounds to zero, or is the null string (""), or a nonnumeric string, $FACTOR returns an empty string. The $DOUBLE values INF, –INF, and NAN return the empty string.

scale

An integer that specifies the scientific notation exponent to apply to num. For example, if scale is 2, then scale represents 10 exponent 2, or 100. This scale value is multiplied by num. For example, $FACTOR(7,2) returns the bitstring that corresponds to the integer 700. This multiplication is done before rounding num to an integer. By default, scale is 0.

Examples

The following example show the conversion of the integers 1 through 9 to bitstrings:

   SET x\=1
   WHILE x<10 {
   WRITE !,x,"="
   FOR i\=1:1:8 {
     WRITE $BIT($FACTOR(x),i) }
   SET x\=x+1 }

 

The following example show $FACTOR conversion of negative numbers and fractions to positive integers:

  FOR i\=1:1:8 {WRITE $BIT($FACTOR(17),i)}
  WRITE " Positive integer",!
  FOR i\=1:1:8 {WRITE $BIT($FACTOR(\-17),i)}
  WRITE " Negative integer (absolute value)",!
  FOR i\=1:1:8 {WRITE $BIT($FACTOR(16.5),i)}
  WRITE " Positive fraction (rounded up)",!
  FOR i\=1:1:8 {WRITE $BIT($FACTOR(\-16.5),i)}
  WRITE " Negative fraction (rounded up)"

 

The following example show the bitstring returned when the scale parameter is specified:

 SET x\=2.7
   WRITE !,x," scaled then rounded to an integer:",!!
   FOR i\=1:1:12 {
     WRITE $BIT($FACTOR(x),i) }
   WRITE " binary = ",$NORMALIZE(x,0)," decimal",!
 SET scale\=1
   SET y\=x\*(10\*\*scale)
   FOR i\=1:1:12 {
     WRITE $BIT($FACTOR(x,scale),i) }
   WRITE " binary = ",$NORMALIZE(y,0)," decimal",!
 SET scale\=2
   SET y\=x\*(10\*\*scale)
   FOR i\=1:1:12 {
     WRITE $BIT($FACTOR(x,scale),i) }
   WRITE " binary = ",$NORMALIZE(y,0)," decimal"

 

See Also

*   [$BIT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fbit) function
    
*   [$BITCOUNT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fbitcount) function
    
*   [$BITFIND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fbitfind) function
    
*   [$BITLOGIC](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fbitlogic) function
    
*   [$DOUBLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fdouble) function
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fextract "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ffind "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_ffactor.xml**