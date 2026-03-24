# $ZBOOLEAN

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fzboolean

---

 

Caché ObjectScript Reference

$ZBOOLEAN

 [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzconvert "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [System and Other Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_SYSTEMFUNCTIONS "System and Other Functions") \]  >  \[ [$ZBOOLEAN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzboolean "$ZBOOLEAN") \]

Go to: Description Parameters Examples Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Bitwise logical operation function.

Synopsis

$ZBOOLEAN(arg1,arg2,bit\_op)
$ZB(arg1,arg2,bit\_op)

Parameters

arg1

The first argument. An integer or a string, or a variable or expression that resolve to an integer or string. All characters must have an ASCII value between 0 and 255. Cannot be a floating point number.

arg2

The second argument. An integer or a string, or a variable or expression that resolve to an integer or string. All characters must have an ASCII value between 0 and 255. Cannot be a floating point number.

bit\_op

An integer indicating the operation to be performed (see table below.) Permitted values are 0 through 15, inclusive.

Description

$ZBOOLEAN performs the bitwise logical operation specified by bit\_op on two arguments, arg1 and arg2. $ZBOOLEAN returns the results of the bitwise combination of arg1 and arg2, as specified by the bit\_op value. You can view the results using the ZZDUMP command.

$ZBOOLEAN performs its operations on either character strings or numbers. For character strings, it performs logical AND and OR operations on each character in the string. For numbers, it performs a logical AND and OR operation on the entire number as a unit. To force the evaluation of a numeric string as a number, preface the string with a plus sign (+).

Note:

$ZBOOLEAN does not support Unicode characters with a value larger than ASCII 255. To apply $ZBOOLEAN to a string of 16-bit Unicode characters, you must first use [$ZWUNPACK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzwunpack), followed by $ZBOOLEAN, followed by [$ZWPACK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzwpack).

$ZBOOLEAN and $BITLOGIC use different data formats. The results of one cannot be used as input to the other.

The bitwise operations includes 16 possible Boolean combinations of arg1 and arg2. The following table lists these combinations.

Bit Mask in bit\_op

Operation Performed

0

0

1

arg1 & arg2 (logical AND)

2

arg1 & ~arg2

3

arg1

4

~arg1 & arg2

5

arg2

6

arg1 ^ arg2 (logical XOR (exclusive or))

7

arg1 ! arg2 (logical OR (inclusive or))

8

~(arg1 ! arg2)

9

~(arg1 ^ arg2)

10

~arg2 (logical NOT)

11

arg1 ! ~arg2

12

~arg1 (logical NOT)

13

~arg1 ! arg2

14

~(arg1 & arg2)

15

\-1 (one’s complement of 0)

Where:

& is logical AND

! is logical OR

~ is logical NOT

^ is exclusive OR

For further details, see [Operators](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_operators) in Using Caché ObjectScript.

All $ZBOOLEAN operations parse both arg1 and arg2, including bit\_op values 0, 3, 5, 10, 12, and 15.

The $ZBOOLEAN arg1 and arg2 parameters can resolve to one of the following types:

*   An integer. A positive or negative whole decimal number of up to 18 digits. No characters other than the numbers 0–9 and, optionally, one or more leading plus and minus signs are permitted. Leading zeros are ignored.
    
*   A string. Enclosed in quotation marks, a string of any length with any contents is permitted. Note that the string “123” and the integer 123 are not the same. A null string is permitted, but if arg2 is the null string, $ZBOOLEAN always returns the value of arg1, regardless of the bit\_op value.
    
*   A signed string. A string preceded by a plus or minus sign is parsed as an integer, regardless of the string’s contents. Signed strings are subject to the same length restriction as integers. A signed null string is equivalent to zero.
    

It is strongly recommended that arg1 and arg2 either both resolve to an integer or both resolve to a string. Generally, arg1 and arg2 should be the same data type; combining an integer and a string in a $ZBOOLEAN operation does not give a useful result in most cases.

Parameters

arg1

The first argument in the bitwise logical expression. For strings, the length of the returned value is always the same as the length of this argument.

arg2

The second argument in the bitwise logical expression.

bit\_op

The bitwise logical operation to be performed, specified as a numeric code from 0 to 15, inclusive. Because this code is handled as a bit mask, a value of 16=0, 17=1, 18=2, etc.

The bit\_op values 0 and 15 return a constant value, but they also evaluate the arguments. If arg1 is an integer (or signed string), bit\_op 0 returns 0, and bit\_op 15 returns –1 (the one’s complement of 0.) If arg1 is a string, bit\_op 0 returns a low value (hex 00) for each character in arg1, and bit\_op 15 returns a high value (hex FF) for each character in arg1. If arg2 is the null string (""), both operations return the literal value of arg1.

The bit\_op values 3, 5, 10 and 12 perform a logical operation on only one of the arguments, but they evaluate both arguments.

*   bit\_op\=3 always returns the value of arg1, regardless of the value of arg2.
    
*   bit\_op\=5 returns the value of arg2 when the two arguments have the same data type. However if one argument is a string and the other argument is an integer (or signed string) results are unpredictable. If arg2 is the null string $ZBOOLEAN always returns the literal value of arg1.
    
*   bit\_op\=10 returns the one’s complement value of arg2 if both arguments are integers. If arg1 is a string, the operation returns a high order character for each character in arg1.. If arg2 is a string, and arg1 is an integer, the bitwise operation is performed on the arg2 string. If arg2 is the null string $ZBOOLEAN always returns the literal value of arg1.
    
*   bit\_op\=12 returns the one’s complement value of arg1 if it is an integer (or signed string), for any value of arg2 except the null string. If arg1 is a string, the operation returns the one’s complement (as a hex value) of each character in arg1. If arg2 is the null string $ZBOOLEAN always returns the literal value of arg1.
    

Examples

The following three examples all illustrate the same AND operation. These examples AND the ASCII values of lowercase letters with the ASCII value of the underscore character, resulting in the ASCII values of the corresponding uppercase letters.

   WRITE $ZBOOLEAN("abcd","\_",1)

 

displays ABCD.

The lowercase "a" = \[01100001\] (ASCII decimal 97)

The underscore character "\_" = \[01011111\] (ASCII decimal 95)

The uppercase "A" = \[01000001\] (ASCII decimal 65)

The following example performs the same AND operation as the previous example, but uses the ASCII decimal values of the arguments. The function $ASCII("a") returns the decimal value 97 for the first argument:

   WRITE $ZBOOLEAN($ASCII("a"),95,1)

 

displays 65.

The following example performs the same AND operation, using a $CHAR value as the second argument:

   WRITE $ZBOOLEAN("a",$CHAR(95),1)

 

displays A.

The following examples illustrate logical OR:

   WRITE $ZBOOLEAN(1,0,7)

 

displays 1.

   WRITE $ZBOOLEAN(1,1,7)

 

displays 1.

   WRITE $ZBOOLEAN(2,1,7)

 

displays 3.

   WRITE $ZBOOLEAN(2,2,7)

 

displays 2.

   WRITE $ZBOOLEAN(3,2,7)

 

displays 3.

The following logical OR examples demonstrate the difference between string comparisons and number comparisons:

   WRITE $ZBOOLEAN(64,255,7)

 

compares the two values as numbers and displays 255.

   WRITE $ZBOOLEAN("64","255",7)

 

compares the two values as strings and displays 65.

   WRITE $ZBOOLEAN(+"64",+"255",7)

 

the plus signs force the comparison of the two values as numbers, and displays 255.

The following examples illustrate exclusive OR:

   WRITE $ZBOOLEAN(1,0,6)

 

displays 1.

   WRITE $ZBOOLEAN(1,1,6)

 

displays 0.

   WRITE $ZBOOLEAN(2,1,6)

 

displays 3.

   WRITE $ZBOOLEAN(2,2,6)

 

displays 0.

   WRITE $ZBOOLEAN(3,2,6)

 

displays 1.

   WRITE $ZBOOLEAN(64,255,6)

 

displays 191.

The following example shows a 4-byte entity with all bytes set to 1:

   WRITE $ZBOOLEAN(5,1,15)

 

displays -1.

The following example will set x to 3 bytes with all bits set to 1:

   SET x\=$ZBOOLEAN("abc",0,15)
   WRITE !,$LENGTH(x)
   WRITE !,$ASCII(x,1)," ",$ASCII(x,2)," ",$ASCII(x,3)

 

The first WRITE displays 3; the second WRITE displays 255 255 255.

Notes

Integer Processing

Before $ZBOOLEAN performs the bitwise operation, it interprets each numeric value as either an 8-byte or a 4-byte signed binary value, depending on size. $ZBOOLEAN always interprets a numeric value as a series of bytes. The boolean operation uses these bytes as a string argument. The result type is the same as the type of arg1.

If either arg1 or arg2 is numeric and cannot be represented as an 8-byte signed integer (larger than 18 decimal digits), a <FUNCTION> error results. If both arg1 and arg2 are numeric and one of them requires 8 bytes to be represented, then both values are interpreted as 8-byte binary values.

After the previous transformations are complete, the given Boolean combination is applied bit by bit to arg1 and arg2 to yield the result. The result returned is always the same length as arg1 (after the above transformations of numeric data). If the length of arg2 is less than the length of arg1, then arg2 is repeatedly combined with successive substrings of arg1 in left to right fashion.

$ZBOOLEAN always interprets the numeric value as a series of bytes in little-endian order, with the low-order byte first, no matter what the native byte order of your machine is.

Internal Structure of $ZBOOLEAN Values

The following table lists the internal rules for $ZBOOLEAN. You do not need to understand these rules to use $ZBOOLEAN; they are presented here for reference purposes only.

There are four possible states of any two bits being compared from within arg1 and arg2. The Boolean operation generates a true result (=1) if and only if bit\_op has the bit mask shown in the table.

Bit in arg1

Bit in arg2

Bit Mask in bit\_op Decimal

Bit Mask in bit\_op Binary

0

0

8

1000

0

1

4

0100

1

0

2

0010

1

1

1

0001

EQV and IMP Logical Operators

$ZBOOLEAN indirectly supports EQV and IMP logical operators. These logical operators are defined as follows:

*   EQV is a logical equivalence between two expressions. It is represented by $ZBOOLEAN(arg1,arg2,9). This is logically ~(arg1 ^ arg2) which is logically identical to ((~arg1) & (~arg2)) ! (arg1 & arg2).
    
*   IMP is a logical implication between two expressions. It is represented by $ZBOOLEAN(arg1,arg2,13). This is logically (~arg1) ! arg2.
    

See Also

*   [ZZDUMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czzdump) command
    
*   [Operators](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_operators) in Using Caché ObjectScript
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

  [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzconvert "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fzboolean.xml**