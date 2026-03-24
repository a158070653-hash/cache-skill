# $BIT

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fbit

---

 

Caché ObjectScript Reference

$BIT

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fascii "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fbitcount "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$BIT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fbit "$BIT") \]

Go to: Description Examples General Information on Bitstring Functions $BIT Functions and $ZBIT Functions See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Returns and or sets the bit value of a specified position in a bitstring.

Synopsis

$BIT(bitstring,position)

SET $BIT(bitstring,position) = value

Parameters

bitstring

An expression that evaluates to a bitstring.

For $BIT, bitstring can be any expression that resolves to a bitstring, including a variable of any type, $FACTOR, a user-defined function, or an oref.prop, ..prop, or i%prop property reference.

For SET $BIT, bitstring can be a variable of any type, including an [i%Prop()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GOBJ_specialcos#GOBJ_specialcos_ipcpropname) property instance variable.

position

The bit position within bitstring. A literal or an expression that evaluates to a positive integer. Bit positions are counted from 1.

value

The bit value to set at position. A literal or an expression that evaluates to the integer 0 or 1.

Description

$BIT is used to return a bit value from a compressed bit string. $BIT(bitstring,position) returns the bit value (0 or 1) at the specified position, position, in the given bitstring expression bitstring. If no value has been defined for a position, $BIT returns 0 for that position. The position is counted from 1; if position is less than 1 (0 or a negative number) or is greater than the length of the bitstring, the function returns 0.

If bitstring is an undefined variable or the null string ("") the $BIT function returns 0. If bitstring is not a valid bitstring value an <INVALID BIT STRING> error occurs.

For general information on $BIT and other bitstring functions, see [below](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fbit#RCOS_fbit_genl).

SET $BIT

SET $BIT is used to set a specified bit value in a bitstring compressed bit string. If the bitstring is not defined, SET $BIT defines the bitstring variable as a compressed bit string and sets the specified bit value.

SET $BIT(bitstring,position) = value performs an atomic bit set on the bitstring specified by bitstring. If value is 1, then the bit at position position is set to 1. If value is 0, the bit is cleared (set to 0). Only an integer value of 0 or 1 should be used; Caché converts any non-numeric value, such as “true” or “false” to 0.

The bit position is counted from 1. If bitstring is shorter than the specified position, Caché pads the bitstring with 0 bits to the specified position. If you specify a position of 0, Caché generates a <VALUE OUT OF RANGE> error.

The bitstring variable must be either an undefined variable, a variable already set to a bitstring value, or a variable set to the empty string (""). Attempting to use SET $BIT on a variable already set to a non-bitstring value results in an <INVALID BIT STRING> error.

The SET $BIT bitstring parameter does not support oref.property or [.. property](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GOBJ_specialcos) syntax.

The SET $BIT bitstring parameter supports [i%property](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GOBJ_specialcos#GOBJ_specialcos_ipcpropname) syntax for both local (non-inherited) properties and properties inherited from a super class. If attempting to set inherited property in existing code is generating a <FUNCTION> error, recompiling the routine should resolve this error and allow setting of the inherited property.

Displaying a Bitstring

As shown in the examples, you can you can use WRITE to display the contents of an individual bit in a bitstring as the return value of $BIT.

Caché has several commands to display the contents of a variable. However, because a $BIT bitstring is a compressed binary string, [WRITE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cwrite) does not display a useful value. [ZZDUMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czzdump) displays the hexadecimal representation of the compressed binary string, which is also not a useful value for most purposes.

[ZWRITE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czwrite) and [ZZWRITE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czzwrite) display the decimal representation of the compressed binary string as $ZWCHAR ($zwc) two-byte (wide) characters. However, they also display a comment that lists the uncompressed “1” bits in left-to-right order as a comma-separated list. If there are three or more consecutive “1” bits, it lists them as a range (inclusive) with two dot syntax (n..m). For example, the bitstring \[1,0,1,1,1,1,0,1\] is shown as /\*$bit(1,3..6,8)\*/. The bitstring \[1,1,1,1,1,1,1,1\] is shown as /\*$bit(1..8)\*/. The bitstring \[0,0,0,0,0,0,0,0\] is shown as /\*$bit()\*/.

[$DATA](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fdata) returns 1 for a compressed binary string variable, including an all-zeros bitstring, such as \[0,0,0,0,0,0,0,0\]. [$GET](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fget) returns the empty string for a compressed binary string, regardless of its value; $GET also returns the empty string for an undefined variable.

Examples

Note in the following examples that a bit that has not been set always has a value of 0.

The following example uses SET $BIT to create a compressed bitstring. It then invokes $BIT repeatedly to display the bits in the bitstring:

  SET $BIT(a,1) \= 0
  SET $BIT(a,2) \= 0
  SET $BIT(a,3) \= 1
  SET $BIT(a,4) \= 0
  SET $BIT(a,5) \= 0
  SET $BIT(a,6) \= 1
  SET $BIT(a,7) \= 1
  SET $BIT(a,8) \= 0
  // Test single bits within the bitstring
  WRITE "bit #2 value: ",$BIT(a,2),!
  WRITE "bit #7 value: ",$BIT(a,7),!
  WRITE "bit #8 value: ",$BIT(a,8),!
  WRITE "bit #13 value: ",$BIT(a,13),!
  // Write the bitstring
  WRITE "bitstring value: "
  FOR x\=1:1:8 {WRITE $BIT(a,x) }
  WRITE !!,"compressed bitstring: "
  ZZDUMP a

 

Because Caché pads the bitstring with 0 bits to the specified position, the following example returns the exact same bitstring data value. However, note that because the #8 bit is not defined, the compressed bitstring a is not identical to the compressed bitstring b:

  SET $BIT(b,3) \= 1
  SET $BIT(b,6) \= 1
  SET $BIT(b,7) \= 1
  // Test single bits within the bitstring
  WRITE "bit #2 value: ",$BIT(b,2),!
  WRITE "bit #7 value: ",$BIT(b,7),!
  WRITE "bit #8 value: ",$BIT(b,8),!
  WRITE "bit #13 value: ",$BIT(b,13),!
  // Write the bitstring
  WRITE "bitstring value: "
  FOR x\=1:1:8 {WRITE $BIT(b,x) }
  WRITE !!,"compressed bitstring: "
  ZZDUMP b

 

For this reason, it is a recommended programming practice to always explicitly set the highest defined bit in the bitstring, even when its assigned value is 0.

You can use a [FOR expr](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cfor) comma-separated list to set multiple bits, as shown in the following example:

  FOR i\=3,6,7 { SET $BIT(b,i) \= 1 }
  // Test single bits within the bitstring
  WRITE "bit #2 value: ",$BIT(b,2),!
  WRITE "bit #7 value: ",$BIT(b,7),!
  WRITE "bit #8 value: ",$BIT(b,8),!
  WRITE "bit #13 value: ",$BIT(b,13),!
  // Write the bitstring
  WRITE "bitstring value: "
  FOR x\=1:1:8 {WRITE $BIT(b,x) }
  WRITE !!,"compressed bitstring: "
  ZZDUMP b

 

In the following example, successive invocations of $BIT return the bits of the bitstring generated by [$FACTOR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ffactor):

  FOR i\=1:1:32 {WRITE $BIT($FACTOR(2\*31\-1),i) }

 

The following example returns a random 16-bit bitstring:

   SET x\=$RANDOM(65536)
   FOR i\=1:1:16 {WRITE $BIT($FACTOR(x),i) }

 

General Information on Bitstring Functions

Bitstring functions manipulate encoded bit-based data. Although a bitstring can be used with any Caché ObjectScript command or function, it is generally meaningful only within the context of the bit functions.

The $BIT bitstring functions perform atomic operations. Therefore, no locking is required when performing bitstring operations.

The $BIT bitstring functions perform internal compression of bitstrings. Therefore, the actual data length of a bitstring and its physical space allocation may differ. $BIT bitstring functions use the data length of bitstrings. In most circumstances, the physical space allocation should be invisible to the user. bitstring compression is invisible to users of the $BIT functions.

However, because this compressed binary representation is optimized for each bitstring, one cannot assume that two "identical" bitstrings (which were created differently) have identical internal representations. Caché selects from four separate bitstring internal representations to optimize for both sparse bitstrings and non-sparse bitstrings. Therefore, while matching operations on individual bits yield predictable results, comparisons of entire bitstrings may not.

$BIT bitstring functions support a maximum bitstring length of 262,104 bits (32763 x 8) for Caché. (Unlike with certain InterSystems legacy products, it is not an error in Caché to perform an operation on a bit that is beyond the bitstring length.) However, it is strongly recommended for performance reasons that you divide long bitstrings into chunks of less than 65,280 bits. This is the maximum number of bits that can fit in a single 8KB database block.

Bits in a bitstring are numbered with the first (leftmost) bit as position 1. All bitstring comparisons are performed left-to-right.

In the examples, bitstrings are shown within matching square brackets (\[...\]), with the bits delimited by commas. For example, a bitstring of four 1 bits is shown as \[1,1,1,1\], with the least significant bits to the right.

In all the bitstring functions, variables named bitstring are bitstrings that can be specified as values, variables, or expressions.

$BIT Functions and $ZBIT Functions

The $BIT functions replace the earlier $ZBIT functions. New code should only use the $BIT functions; the $ZBIT functions will continue to be supported for legacy applications. The $BIT functions and the $ZBIT functions are incompatible; $BIT functions compress bitstrings, $ZBIT functions do not. Therefore, the two types of bitstring functions should not be used on the same bitstring. Attempting to do so results in an <INVALID BIT STRING> error.

You can use the [$SYSTEM.Bit.ZBitToBit()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Bit#ZBitToBit) method to convert a $ZBIT-format bit string to a $BIT-format bit string. This is shown in the following example:

ZBitSet
  SET zb\=$ZBITSTR(4,1)       /\* the $ZBIT string 1111 \*/
  SET zbnew\=$ZBITSET(zb,2,0) /\* the $ZBIT string 1011 \*/
ZBitToBit
  SET bb\=$SYSTEM.Bit.ZBitToBit(zbnew)
  WRITE !,$BIT(bb,1),$BIT(bb,2),$BIT(bb,3),$BIT(bb,4)

 

See Also

*   [$BITCOUNT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fbitcount) function
    
*   [$BITFIND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fbitfind) function
    
*   [$BITLOGIC](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fbitlogic) function
    
*   [$FACTOR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ffactor) function
    
*   [$ZBOOLEAN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzboolean) function
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fascii "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fbitcount "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fbit.xml**