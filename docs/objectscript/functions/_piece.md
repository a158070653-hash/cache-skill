# $PIECE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fpiece

---

 

Caché ObjectScript Reference

$PIECE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fparameter "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fprefetchoff "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$PIECE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fpiece "$PIECE") \]

Go to: Description Parameters Replacing a Substring Using SET $PIECE Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Returns or replaces a substring, using a delimiter.

Synopsis

$PIECE(string,delimiter,from,to)
$P(string,delimiter,from,to)

SET $PIECE(string,delimiter,from,to)=value
SET $P(string,delimiter,from,to)=value

Parameters

string

The target string in which delimited substrings are identified. Specify string as an expression that evaluates to a quoted string or a numeric value. In SET $PIECE syntax, string must be a variable or a multi-dimensional property.

delimiter

A delimiter used to identify substrings within string. Specify delimiter as an expression that evaluates to a quoted string containing one or more characters.

from

Optional — An expression that evaluates to a code specifying the location of a substring, or the beginning of a range of substrings, within string. Substrings are separated by a delimiter, and counted from 1. Permitted values are n (a positive integer specifying the substring count from the beginning of string), \* (specifying the last substring in string), and \*-n (offset integer count of substrings counting backwards from end of string). SET $PIECE syntax also supports \*+n (offset integer count of substrings to append beyond the end of string). Thus, the first delimited substring is 1, the second delimited substring is 2, the last delimited substring is \*, and the next-to-last delimited substring is \*-1. If from is omitted, it defaults to the first delimited substring.

to

Optional — An expression that evaluates to a code specifying the ending substring for a range of substrings within string. Must be used with from. Permitted values are n (a positive integer specifying the substring count from the beginning of string), \* (specifying the last substring in string), and \*-n (offset integer count of substrings from end of string). SET $PIECE syntax also supports \*+n (offset integer for a range of substrings to append beyond the end of string). If to is prior to from in string, no operation is performed and no error is generated.

Description

$PIECE identifies substrings within string by the presence of a delimiter. If the delimiter does not occur in string, the entire string is treated as a single substring.

$PIECE can be used in two ways:

*   To [return a substring](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fpiece#RCOS_fpiece_return) from string. This uses the $PIECE(string,delimiter,from,to) syntax.
    
*   To [replace a substring](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fpiece#RCOS_fpiece_replace) within string. It identifiers a substring and replaces it with another substring. The replacement substring may be the same length, longer, or shorter than the original substring. This uses the SET $PIECE(string,delimiter,from,to)=value syntax.
    

Note:

$PIECE is a general-purpose function for handling a string containing delimited substrings. For handling a MultiValue dynamic array string containing the specific MultiValue delimiters, use the [$MV](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fmv) function.

Returning a Substring

When returning a specified substring (piece) from string, the substring returned depends on the parameters used:

*   $PIECE(string,delimiter) returns the first substring in string. If delimiter occurs in string, this is the substring that precedes the first occurrence of delimiter. If delimiter does not occur in string, the returned substring is string.
    
*   $PIECE(string,delimiter,from) returns a substring whose location is specified by the from parameter. Substrings are delimited by delimiters and the beginning and end of string. The delimiter itself is not returned.
    
*   $PIECE(string,delimiter,from,to) returns a range of substrings including the substring specified in from through the substring specified in to (inclusive). This four-argument form of $PIECE returns a substring that includes any intermediate occurrences of delimiter that occur between the from and to substrings. If to is greater than the number of substrings, the returned substring includes all substrings to the end of string.
    

Parameters

string

When $PIECE is used to [return a substring](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fpiece#RCOS_fpiece_return), string can be a string literal enclosed in quotation marks, a canonical numeric, a variable, an object property, or any valid Caché ObjectScript expression that evaluates to a string or a numeric. If you specify a null string ("") as the target string, $PIECE always returns the null string, regardless of the other parameter values.

A target string usually contains instances of a character (or character string) which are used as delimiters. This character or character string cannot also be used as a data value within string.

When $PIECE is used with SET on the left hand side of the equals sign to [replace a substring](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fpiece#RCOS_fpiece_replace), string can be a variable or a [multidimensional property](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ROBJ_property_multidimensional) reference; it cannot be a non-multidimensional object property.

delimiter

The search string to be used to delimit substrings within string. It can be a string literal enclosed in quotation marks, a canonical numeric, a variable or any valid Caché ObjectScript expression that evaluates to a string or a numeric.

Commonly, a delimiter is a designated character which is never used as data within string, but is set aside solely for use as a delimiter separating substrings. For example, if delimiter is “^”, the string “Red^Orange^Yellow” contains three delimited substrings.

A delimiter can be a multi-character string, the individual characters of which can be used within string data. For example, if delimiter is “^#”, the string “Red^Orange^#^Yellow#Green#^Blue” contains two delimited substrings: “Red^Orange” and “^Yellow#Green#^Blue”.

Commonly, string does not begin or end with a delimiter. If string begins or ends with a delimiter, $PIECE treats this delimiter as demarcating a substring with a null string ("") value. For example, if delimiter is “^”, the string “^Red^Orange^Yellow^” contains five delimited substrings; substrings 1 and 5 have null string values.

If the specified delimiter is not in string, $PIECE returns the entire string. If the specified delimiter is the null string (""), $PIECE returns the null string.

from

The location of a substring within string. Use n (a positive integer) to count delimited substrings from the beginning of string. Use \* to specify the last delimited substring in string. Use \*-n to count delimited substrings by offset from the last delimited substring in string.

*   1 specifies the first substring of string (the substring that precedes the first occurrence of delimiter). If string does not contain the specified delimiter, a from value of 1 returns string. If from is omitted, it defaults to 1.
    
*   2 specifies the second substring of string (the substring that appears between the first and second occurrences of delimiter, or between the first occurrence of delimiter and the end of string).
    
*   \* specifies the last substring of string (the substring that follows the last occurrence of delimiter). If string does not contain the specified delimiter, a from value of \* returns string.
    
*   \*-1 specifies the next-to-last substring of string. \*-n counts by offset from the last substring of string. \*-0 is the last substring of string; \* and \*-0 are functionally identical.
    
*   For SET $PIECE syntax only — \*+n (an asterisk followed by a positive number) appends delimited substrings by offset beyond the end of string. Thus, \*+1 appends a delimited substring beyond the end of string, \*+2 appends a delimited substring two positions beyond the end of string, padding with delimiters.
    
*   If from is the null string (""), zero, a negative number, or specifies a count or offset beyond the number of substrings in string, $PIECE returns a null string.
    

$PIECE converts a from numeric to canonical form (resolving leading plus and minus signs and removing leading zeros), then truncates it to an integer.

If the from parameter is used with the to parameter, it identifies the start of a range of substrings to be returned as a string, and should be less than the value of to.

to

The number of the substring within string that ends the range initiated by the from parameter. The returned string includes both the from and to substrings, as well as any intermediate substrings and the delimiters separating them. The to parameter must be used with from and should be greater than the value of from.

Use n (a positive integer) to count delimited substrings from the beginning of string. Use \* to specify the last delimited substring in string. Use \*-n to count delimited substrings by offset backwards from the last delimited substring in string.

For SET $PIECE syntax only — \*+n (an asterisk followed by a positive number) specifies the end of a range of substrings to append beyond the end of string.

*   If from is less than to, $PIECE returns a string consisting of all of the delimited substrings within this range, including the from and to substrings. This returned string contains the substrings and the delimiters within this range. If to is greater than the number of delimited substrings, the returned string contains all the string data (substrings and delimiters) beginning with the from substring and continuing to the end of string.
    
*   If from is equal to to, $PIECE returns the from substring. This can occur if from and to are the same value, or are different values that reference the same substring.
    
*   If from is greater than to, is zero (0), or is the null string (""), $PIECE returns a null string.
    

$PIECE converts a to numeric to canonical form (resolving leading plus and minus signs and removing leading zeros), then truncates it to an integer.

Specifying \*-n and \*+n Parameter Values

When using a variable to specify \*-n or \*+n, you must always specify the asterisk and a sign character in the parameter itself.

The following are valid specifications of \*-n:

  SET count\=2
  SET alph\="a^b^c^d"
  WRITE $PIECE(alph,"^",\*\-count)

 

  SET count\=\-2
  SET alph\="a^b^c^d"
  WRITE $PIECE(alph,"^",\*+count)

 

The following is a valid specification of \*+n:

  SET count\=2
  SET alph\="a^b^c^d"
  SET $PIECE(alph,"^",\*+count)\="F"
  WRITE alph

 

Whitespace is permitted within these parameter values.

Examples: Returning a Delimited Substring

In the following example, each $PIECE returns the specified substring as identified by the "," delimiter:

   SET colorlist\="Red,Green,Blue,Yellow,Orange,Black"
   WRITE $PIECE(colorlist,","),!     ; returns "Red" (substring 1) by default
   WRITE $PIECE(colorlist,",",3),!   ; returns "Blue" the third substring
   WRITE $PIECE(colorlist,",",\*),!   ; returns "Black" the last substring
   WRITE $PIECE(colorlist,",",\*\-1),! ; returns "Orange" the next-to-last substring

 

In the following example, $PIECE returns the integer and fractional parts of a number:

   SET int\=$PIECE(123.999,".")
   SET frac\=$PIECE(123.999,".",\*)
   WRITE "integer=",int," fraction =.",frac

 

The following example returns "Blue,Yellow,Orange", the third through fifth substrings in colorlist, as delimited by ",":

   SET colorlist\="Red,Green,Blue,Yellow,Orange,Black"
   SET extract\=$PIECE(colorlist,",",3,5)
   WRITE extract

 

The following WRITE statements all return the first substring “123”, showing that these formats are equivalent when from and to have a value of 1:

   SET numlist\="123#456#789"
   WRITE !,"2-arg=",$PIECE(numlist,"#")
   WRITE !,"3-arg=",$PIECE(numlist,"#",1)
   WRITE !,"4-arg=",$PIECE(numlist,"#",1,1)

 

In the following example, both $PIECE functions returns the entire string string, because there are no occurrences of delimiter in string:

   SET colorlist\="Red,Green,Blue,Yellow,Orange,Black"
   SET extract1\=$PIECE(colorlist,"#")
   SET extract2\=$PIECE(colorlist,"#",1,4)
   WRITE "#   =",extract1,!,"#,1,4=",extract2

 

The following example $PIECE returns the second substring from an object property:

  SET tStatement \= ##class(%SQL.Statement).%New()
  SET tStatement.%SchemaPath\="MyTests,Sample,Cinema"
  WRITE "whole schema path: ",tStatement.%SchemaPath,!
  WRITE "2nd piece of schema path: ",$PIECE(tStatement.%SchemaPath,",",2),!

 

The following two examples use more complex delimiters.

This example uses a delimiter string “#-#” to return three substrings of the string numlist. Here, the component characters of the delimiter string, “#” and “-”, can be used as data values; only the specified sequence of characters (#-#) is set aside:

   SET numlist\="1#2-3#-#45##6#-#789"
   WRITE !,$PIECE(numlist,"#-#",1)
   WRITE !,$PIECE(numlist,"#-#",2)
   WRITE !,$PIECE(numlist,"#-#",3)

 

The following example uses a non-ASCII delimiter character (in this case, the Unicode character for pi), specified using the $CHAR function, and inserted into string by using the concatenate operator (\_):

   IF $SYSTEM.Version.IsUnicode()  {
     SET a \= $CHAR(960)
     SET colorlist\="Red"\_a\_"Green"\_a\_"Blue"
     SET extract1\=$PIECE(colorlist,a)
     SET extract2\=$PIECE(colorlist,a,2)
     SET extract3\=$PIECE(colorlist,a,2,3)
     WRITE extract1,!,extract2,!,extract3
     }
   ELSE {WRITE "This example requires a Unicode installation of Caché"}

 

Replacing a Substring Using SET $PIECE

When making assignments with the SET command, you can use $PIECE to the left, as well as to the right, of the equals sign. When used to the left of the equals sign, $PIECE designates a substring to be replaced by the assigned value.

When $PIECE is used with SET on the left hand side of the equals sign, string can be a valid variable name. If the variable does not exist, SET $PIECE defines it. The string parameter can also be a [multidimensional property](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ROBJ_property_multidimensional) reference; it cannot be a non-multidimensional object property. Attempting to use SET $PIECE on a non-multidimensional object property results in an <OBJECT DISPATCH> error.

The use of $PIECE (and $LIST and $EXTRACT) in this context differs from other standard functions because it modifies an existing value, instead of just returning a value. You cannot use SET (a,b,c,...)=value syntax with $PIECE (or $LIST or $EXTRACT) on the left of the equals sign, if the function uses relative offset syntax: \* representing the end of a string and \*-n or \*+n representing relative offset from the end of the string. You must instead use SET a=value,b=value,c=value,... syntax.

Examples: Replacing a Delimited Substring

The following example changes the value of colorlist to "Magenta,Green,Cyan,Yellow,Orange,Black":

   SET colorlist\="Red,Green,Blue,Yellow,Orange,Black"
   WRITE colorlist,!
   SET $PIECE(colorlist,",",1)\="Magenta"
   WRITE colorlist,!
   SET $PIECE(colorlist,",",\*\-3)\="Cyan"
   WRITE colorlist,!

 

The replacement substring may, of course, be longer or shorter than the original, and may include delimiters:

   SET colorlist\="Red,Green,Blue,Yellow,Orange,Black"
   WRITE colorlist,!
   SET $PIECE(colorlist,",",3)\="Turquoise,Aqua,Teal"
   WRITE colorlist,!

 

If you specify a from and to argument, the included substrings are replaced by the specified value, in this case the 4th through 6th delimited substrings:

   SET colorlist\="Red,Blue,Yellow,Green,Orange,Black"
   WRITE !,colorlist
   SET $PIECE(colorlist,",",4,6)\="Yellow+Blue,Yellow+Red"
   WRITE !,colorlist

 

You can append one or more delimited substrings either by delimited substring count (using n), or by offset from the end of string (using \*+n). SET $PIECE appends additional delimiters as needed to append the delimited substring(s) at the specified location. The following examples both change the value of colorlist to "Green^Blue^^Red", padding with an extra empty string delimited substring:

   SET colorlist\="Green^Blue"
   SET $PIECE(colorlist,"^",4)\="Red"
   WRITE colorlist

 

   SET colorlist\="Green^Blue"
   SET $PIECE(colorlist,"^",\*+2)\="Red"
   WRITE colorlist

 

If delimiter doesn't appear in string, $PIECE treats string as a single piece and performs the same substitutions described above. If there is no from argument specified, the new value replaces the original string:

   SET colorlist\="Red,Green,Blue"
   WRITE colorlist,!
   SET $PIECE(colorlist,"^")\="Purple^Orange"
   WRITE colorlist

 

If delimiter doesn't appear in string, and from is specified as an integer greater than 1, $PIECE appends from\-1 delimiters and the supplied value to the end of string:

   SET colorlist\="Red,Green,Blue"
   WRITE colorlist,!
   SET $PIECE(colorlist,"^",3)\="Purple"
   WRITE colorlist

 

If from represents a position prior to the beginning of the string, Caché performs no operation:

   SET colorlist\="Red,Green,Blue"
   WRITE colorlist,!
   SET $PIECE(colorlist,",",\*\-7)\="Purple"
   WRITE colorlist

 

If from represents a position prior to the beginning of the string and to is provided, Caché treats from as position 1:

   SET colorlist\="Red,Green,Blue"
   WRITE colorlist,!
   SET $PIECE(colorlist,",",\*\-7,1)\="Purple"
   WRITE colorlist

 

Initializing a String Variable

The string variable does not need to be defined before being assigned a value. The following example initializes newvar to the character pattern ">>>>>>TOTAL":

   SET $PIECE(newvar,">",7)\="TOTAL"
   WRITE newvar

 

See the ["SET with $PIECE and $EXTRACT"](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cset#RCOS_cset62) section of the SET command documentation for more information.

Delimiter is Null String

If the delimiter is the null string, the new value replaces the original string, regardless of the values of the from and to arguments.

The following two examples both set colorlist to “Purple”:

   SET colorlist\="Red,Green,Blue"
   WRITE !,colorlist
   SET $PIECE(colorlist,"")\="Purple"
   WRITE !,colorlist

 

   SET colorlist\="Red,Blue,Yellow,Green,Orange,Black"
   WRITE !,colorlist
   SET $PIECE(colorlist,"",3,5)\="Purple"
   WRITE !,colorlist

 

$PIECE with Parameters over 32,768 Characters

The following example creates a string of 5 periods and a null:

   SET x\=""
   SET $PIECE(x,".",6)\=""
   WRITE x

 

Now consider the following example that creates a string of 32767 periods and a null:

   SET x\=""
   SET $PIECE(x,".",32768)\=""

Although technically within the maximum length of a string, this example generates a <MAXSTRING> error if [long strings](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_strings_long) are not enabled. Long strings are enabled system-wide by default. If you wish to use $PIECE with a parameter greater than 32,767 characters, you can check or set the system-wide long strings setting using the [EnableLongStrings](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=Config.Miscellaneous#EnableLongStrings) property of the Config.Miscellaneous class.

Notes

Using $PIECE to Unpack Data Values

$PIECE is typically used to "unpack" data values that contain multiple fields delimited by a separator character. Typical delimiter characters include the slash (/), the comma (,), the space ( ), and the semicolon (;). The following sample values are good candidates for use with $PIECE:

"John Jones/29 River St./Boston MA, 02095"
"Mumps;Measles;Chicken Pox;Diptheria"
"45.23,52.76,89.05,48.27"

$PIECE and $LENGTH

The two-argument form of $LENGTH returns the number of substrings in a string, based on a delimiter. Use $LENGTH to determine the number of substrings in a string, and then use $PIECE to extract individual substrings, as shown in the following example:

   SET sentence\="The quick brown fox jumped over the lazy dog's back."
   SET delim\=" "
   SET countdown\=$LENGTH(sentence,delim)
   SET countup\=1
   FOR reps\=countdown:\-1:1 {
      SET extract\=$PIECE(sentence,delim,countup)
      WRITE !,countup," ",extract
      SET countup\=countup+1
   }
   WRITE !,"All done!"

 

Null Values

$PIECE does not distinguish between a delimited substring with a null string value, and a nonexistent substring. Both return a null string value. For example, the following examples both return the null string for a from value of 7:

   SET colorlist\="Red,Green,Blue,Yellow,Orange,Black"
   SET extract1\=$PIECE(colorlist,",",6)
   SET extract2\=$PIECE(colorlist,",",7)
   WRITE "6=",extract1,!,"7=",extract2

 

   SET colorlist\="Red,Green,Blue,Yellow,Orange,Black,"
   SET extract1\=$PIECE(colorlist,",",6)
   SET extract2\=$PIECE(colorlist,",",7)
   WRITE "6=",extract1,!,"7=",extract2

 

In the first case, there is no seventh substring; a null string is returned. In the second case there is a seventh substring, as indicted by the delimiter at the end of the string; the value of this seventh substring is the null string.

The following example shows null values within a string. It extracts substrings 1 and 3. These substrings exists, but both contain a null string. (Substring 1 is defined as the string preceding the first delimiter character):

   SET colorlist\=",Red,,Blue,"
   SET extract1\=$PIECE(colorlist,",")
   SET extract3\=$PIECE(colorlist,",",3)
   WRITE !,"sub1=",extract1,!,"sub3=",extract3

 

The following examples also returns a null string, because the specified substrings do not exist:

   SET colorlist\="Red,Green,Blue,Yellow,Orange,Black"
   SET extract\=$PIECE(colorlist,",",0)
   WRITE !,"Length=",$LENGTH(extract),!,"Value=",extract

 

   SET colorlist\="Red,Green,Blue,Yellow,Orange,Black"
   SET extract\=$PIECE(colorlist,",",8,20)
   WRITE !,"Length=",$LENGTH(extract),!,"Value=",extract

 

Nested $PIECE Operations

To perform complex extractions, you can nest $PIECE references within each other. The inner $PIECE returns a substring that is operated on by the outer $PIECE. Each $PIECE uses its own delimiter. For example, the following returns the state abbreviation “MA”:

   SET patient\="John Jones/29 River St./Boston MA 02095"
   SET patientstateaddress\=$PIECE($PIECE(patient,"/",3)," ",2)
   WRITE patientstateaddress

 

The following is another example of nested $PIECE operations, using a hierarchy of delimiters. First, the inner $PIECE uses the caret (^) delimiter to find the second piece of nestlist: "A,B,C". Then the outer $PIECE uses the comma (,) delimiter to return the first and second pieces ("A,B") of the substring "A,B,C":

   SET nestlist\="1,2,3^A,B,C^@#!"
   WRITE $PIECE($PIECE(nestlist,"^",2),",",1,2)

 

$PIECE Compared with $EXTRACT and $LIST

$PIECE determines a substring by counting user-defined delimiter characters within the string. $PIECE takes as input an ordinary character string containing multiple instances of a character (or string) intended for use as a delimiter.

[$EXTRACT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fextract) determines a substring by counting characters from the beginning of a string. $EXTRACT takes as input an ordinary character string.

[$LIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flist) determines an element from an encoded list by counting elements (not characters) from the beginning of the list. The $LIST functions specify substrings without using a designated delimiter. If setting aside a delimiter character or character sequence is not appropriate to the type of data (for example, bitstring data), you should use the [$LISTBUILD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistbuild) and [$LIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flist) functions to store and retrieve substrings. You can convert a delimited string into a list using the [$LISTFROMSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistfromstring) function. You can convert a list to a delimited string using the [$LISTTOSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flisttostring) function.

The data storage strategies used by $PIECE and the $LIST functions are incompatible, and their use should not be combined. For example, attempted to use $PIECE on a list created using $LISTBUILD yields unpredictable results and should be avoided.

See Also

*   [SET](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cset) command
    
*   [$EXTRACT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fextract) function
    
*   [$LENGTH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flength) function
    
*   [$LIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flist) function
    
*   [$LISTBUILD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistbuild) function
    
*   [$LISTFROMSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistfromstring) function
    
*   [$LISTTOSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flisttostring) function
    
*   [$REVERSE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_freverse) function
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fparameter "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fprefetchoff "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fpiece.xml**