# $BITLOGIC

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fbitlogic

---

 

Caché ObjectScript Reference

$BITLOGIC

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fbitfind "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fcase "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$BITLOGIC](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fbitlogic "$BITLOGIC") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Performs bit-wise operations on bitstrings.

Synopsis

$BITLOGIC(bitstring\_expression,length)

Parameters

bitstring\_expression

A logical expression consisting of one or more bitstring variables and the logical operators &, |, ^, and ~. A bitstring can be specified as a local variable, a process-private global, a global, an object property, or the constant "". The null string ("") has a bitstring length of 0. A bitstring cannot be specified using a function (such as $FACTOR) that returns a bitstring.

length

Optional — The length, in bits, of the resulting bitstring. If length is not specified it defaults to the length of the longest bitstring in bitstring\_expression.

Description

$BITLOGIC evaluates a bit-wise operation on one or more bitstring values, as specified by bitstring\_expression, and returns the resulting bitstring.

A bitstring is an encoded (compressed) string which is interpreted as a series of bits. Only bitstrings created using $BIT, $FACTOR, or $BITLOGIC, or the null string (""), should be supplied to the $BITLOGIC function. Typically, bitstrings are used for index operations. Refer to [general information on bitstring functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fbit#RCOS_fbit_genl) in $BIT for further details.

$BITLOGIC is incompatible with any of the legacy $ZBIT bitstring functions, and should not be used in combination with them. $BITLOGIC and $ZBOOLEAN use different data formats. The results of one cannot be used as input to the other.

Bitstring Optimization

The most basic $BITLOGIC operation is $BITLOGIC(a). Seemingly, this operation does not do anything: bitstring a is input and the same bitstring a is output. However, $BITLOGIC performs bitstring compression which it optimizes by selecting from several compression algorithms. Therefore, if bitstring a has undergone substantial changes since its creation, passing it through $BITLOGIC can result in re-optimization of the bitstring. Refer to [$BIT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fbit#RCOS_fbit_genl) for further details.

For example, following a large number of delete operations an index bitstring may have become a sparse bitstring, consisting wholly or mainly of zeros. Passing this index bitstring through $BITLOGIC may result in substantial performance improvements.

Bitstring Logical Operators

$BITLOGIC can evaluate only the bitstring operators listed in the following table:

Operator

Meaning

&

AND

|

OR

^

XOR (exclusive OR)

~

NOT (one’s complement)

The bitstring\_expression can contain a single bitstring (~A), two bitstrings (A&B), or more than two bitstrings (A&B|C), up to the current maximum of 31 bitstrings. Evaluation is performed left-to-right. Logical operations may be grouped by parentheses within the bitstring\_expression, following standard Caché ObjectScript order of operations. If a variable used within $BITLOGIC is undefined, it is treated as a null string ("").

$BITLOGIC treats a null string as a bitstring of indefinite length, in which all bits are set to 0’s.

Note:

When $BITLOGIC is supplied more than two bitstring operands, it must create bitstring temporaries to hold the intermediate results. Under some extreme circumstances (many bitstrings and/or extremely large bitstrings), it can exhaust the space allocated to hold such temporaries. Bitstring pair operations do not have this limitation, and are thus preferable for large bitstring operations.

The NOT (~) operator can be used as a unary operator (for example, ~A), or can be used in combination with other operators (for example, A&~B). It performs the one’s complement operation on a string, turning all 1’s to 0’s and all 0’s to 1’s. Multiple NOT operators can be used (for example, ~~~A).

The length Argument

If length is not specified, it defaults to the length of the longest bitstring in bitstring\_expression.

If length is specified, it specifies the logical length of the resulting bitstring.

*   If length is larger than one or more of the bitstrings in bitstring\_expression, those bitstrings are zero-filled to that length before bitstring logic operations are performed.
    
*   If length is smaller than one or more of the bitstrings in bitstring\_expression, those bitstrings are truncated to that length before bitstring logic operations are performed.
    
*   If length is 0, a bitstring of length 0 (a null string) is returned.
    

Examples

The following example creates some simple bitstrings and demonstrates the use of $BITLOGIC on them:

   // Set a to \[1,1\]
   SET $BIT(a,1) \= 1
   SET $BIT(a,2) \= 1
   // Set b to \[0,1\]
   SET $BIT(b,1) \= 0
   SET $BIT(b,2) \= 1
   WRITE !,"bitstring a=",$BIT(a,1),$BIT(a,2)
   WRITE !,"bitstring b=",$BIT(b,1),$BIT(b,2)
   SET c \= $BITLOGIC(~b)
   WRITE !,"The one's complement of b=",$BIT(c,1),$BIT(c,2)
   // Find the intersection (AND) of a and b
   SET c \= $BITLOGIC(a&b)   // c should be \[0,1\]
   WRITE !,"The AND of a and b=",$BIT(c,1),$BIT(c,2)
   SET c \= $BITLOGIC(a&~b)   // c should be \[1,0\]
   WRITE !,"The AND of a and ~b=",$BIT(c,1),$BIT(c,2)
   // Find the union (OR) of a and b
   SET c \= $BITLOGIC(a|b)   // c should be \[1,1\]
   WRITE !,"The OR of a and b=",$BIT(c,1),$BIT(c,2)
   SET c \= $BITLOGIC(a^b)   // c should be \[1,0\]
   WRITE !,"The XOR of a and b=",$BIT(c,1),$BIT(c,2)
   QUIT

 

The following example shows the results of specifying a length greater than the input bitstring. The string is zero-filled before the logic operation is performed.

   // Set a to \[1,1\]
   SET $BIT(a,1) \= 1
   SET $BIT(a,2) \= 1
   WRITE !,"bitstring a=",$BIT(a,1),$BIT(a,2)
   SET c \= $BITLOGIC(~a,7)
   WRITE !,"~a (length 7)="
   WRITE $BIT(c,1),$BIT(c,2),$BIT(c,3),$BIT(c,4)
   WRITE $BIT(c,5),$BIT(c,6),$BIT(c,7),$BIT(c,8)

 

Here the one’s complement (~) of 11 is 0011111. Bits 3 through 7 were set to zero before the ~ operation was performed. This example also displays an eighth bit, which is beyond the specified string length and thus unaffected by the $BITLOGIC operation. It is, of course, displayed as 0.

The following example shows the results of specifying a length less than the input bitstring. The bitstring is truncated to the specified length before logical operations are performed. All bits beyond the specified length default to 0.

   // Set a to \[1,1,1\]
   SET $BIT(a,1) \= 1
   SET $BIT(a,2) \= 1
   SET $BIT(a,3) \= 1
   WRITE !,"bitstring a=",$BIT(a,1),$BIT(a,2),$BIT(a,3)
   SET c \= $BITLOGIC(a,2)
   WRITE !," a (length 2)="
   WRITE $BIT(c,1),$BIT(c,2),$BIT(c,3),$BIT(c,4)
   SET c \= $BITLOGIC(~a,2)
   WRITE !,"~a (length 2)="
   WRITE $BIT(c,1),$BIT(c,2),$BIT(c,3),$BIT(c,4)

 

The following example shows that when length is not specified, it defaults to the length of the longest bitstring. Shorter bitstrings are zero-filled before the logical operation is performed.

   // Set a to \[1,1,1\]
   SET $BIT(a,1) \= 1
   SET $BIT(a,2) \= 1
   SET $BIT(a,3) \= 1
   // Set b to \[1,1\]
   SET $BIT(b,1) \= 1
   SET $BIT(b,2) \= 1
   SET c \= $BITLOGIC(a&~b)
   WRITE !,"  a&~b="
   WRITE $BIT(c,1),$BIT(c,2),$BIT(c,3)
   SET c \= $BITLOGIC(a&~b,3)
   WRITE !,"a&~b,3="
   WRITE $BIT(c,1),$BIT(c,2),$BIT(c,3)

 

Here the two $BITLOGIC operations (with and without a length argument) both return the same value: 001.

See Also

*   [$BIT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fbit) function
    
*   [$BITCOUNT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fbitcount) function
    
*   [$BITFIND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fbitfind) function
    
*   [$ZBOOLEAN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzboolean) function
    
*   [Operators](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_operators) in Using Caché ObjectScript
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fbitfind "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fcase "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fbitlogic.xml**