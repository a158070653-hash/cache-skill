# $ZSTRIP

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fzstrip

---

 

Caché ObjectScript Reference

$ZSTRIP

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzseek "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzwascii "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [System and Other Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_SYSTEMFUNCTIONS "System and Other Functions") \]  >  \[ [$ZSTRIP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzstrip "$ZSTRIP") \]

Go to: Description Parameters Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Removes types of characters and individual characters from a specified string.

Synopsis

$ZSTRIP(string,action,remchar,keepchar)

Parameters

string

The string to be stripped.

action

What to strip from string. An action consists of an action code followed by a one or more mask codes. The mask code is optional when specifying remchar. An action is specified as a quoted string.

remchar

Optional — A string of specific character values to remove. If action does not contain a mask code, remchar lists the characters to remove. If action contains a mask code, remchar lists additional characters to remove that are not covered by the action parameter’s mask code.

keepchar

Optional — A string of specific character values to not remove that are designated for removal by the action parameter’s mask code. A mask code must be specified to specify keepchar.

Description

The $ZSTRIP function removes types of characters and/or individual character values from the specified string. In the action parameter you specify an action code indicating the kind of remove operation to perform, and (optionally) a mask code specifying the types of characters to remove. You can specify individual character values to remove using the remchar parameter. $ZSTRIP can remove both types of characters (such as all lowercase letters) and listed character values (such as the letters “AEIOU”) in the same operation. You can use the optional remchar and keepchar parameters to modify the effects of the action parameter’s mask code by specifying individual character values to remove or to keep.

For further information, refer to the [Pattern Matching Operators](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_operators) section of Using Caché ObjectScript. You can also select types of characters, character sequences, and ranges of characters using the Regular Expression functions [$LOCATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flocate) and [$MATCH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fmatch).

Parameters

action

A string indicating what characters to strip, specified as an action code, optionally followed by one or more mask codes.

Action Codes

\*

Strip all characters that match the mask code(s).

<

Strip leading characters that match the mask code(s).

\>

Strip trailing characters that match the mask code(s).

<>

Strip leading and trailing characters that match the mask code(s).

\=

Strip repeating characters that match the mask code(s). When encountering a series of repeated characters, this code strips the duplicate characters leaving a single instance. This code only strips duplicate adjacent characters. Thus stripping “a” from “aaaaaabc” yields “abc”, but stripping “a” from “abaca” returns the string “abaca” unchanged. The duplicate character test is case sensitive.

<=>

Strip leading, trailing, and repeating characters that match the mask code(s).

An action code can consist of the \* character, or any combination of a single <, >, or = character.

To strip types of characters, the action string should consist of an action code followed by one or more mask codes. To strip specific character values, omit the mask code, and specify a remchar value. You can specify both a mask code and a remchar value. If you specify neither a mask code nor a remchar value, $ZSTRIP returns string.

Mask Codes

E

Strip everything.

A

Strip all alphabetic characters.

P

Strip punctuation characters, including blank spaces.

C

Strip control characters (0-31, 127-159).

N

Strip numeric characters. Note that a numeric string is not converted to canonical form before applying $ZSTRIP.

L

Strip lowercase alphabetic characters.

U

Strip uppercase alphabetic characters.

W

Strip whitespaces ($C(9), $C(32), $C(160)).

Mask codes can be specified as uppercase or lowercase characters.

A mask code character can be preceded by a Unary Not (') meaning do not remove characters of this type. You must specify at least one mask code without a Unary Not before specifying a Unary Not mask code. All mask codes without a Unary Not must precede the mask codes with a Unary Not.

remchar

Specific characters to remove, specified as a quoted string. These remchar characters can be specified in any order and duplicates are permitted.

If you do not specify a mask code, $ZSTRIP applies the action parameter to the remchar character(s). If you specify a mask code, remchar specifies one or more additional characters to remove. For example, if you specified in the action parameter that you want to remove all numeric characters ("\*N"), but you also want to remove the letter “E” (used to represent scientific notation), you would add the string “E” as the remchar parameter, as shown in the second $ZSTRIP:

   SET str\="first:123 second:12E3"
   WRITE $ZSTRIP(str,"\*N"),!
   WRITE $ZSTRIP(str,"\*N","E")

 

keepchar

Specific characters not to remove. For example, if you specified that you wanted to remove all white spaces and alphabetic characters (\*WA), but preserve uppercase M, you would add the string “M” as the keepchar parameter.

Examples

The following example strips out all numeric characters. Because a numeric string is not converted to canonical form, the characters + and E are not stripped out:

   SET str\="+123E4"
   WRITE $ZSTRIP(str,"\*N")

 

returns: +E

In the following example, the first $ZSTRIP strips all punctuation characters, the second $ZSTRIP strips all punctuation characters except whitespace characters.

   SET str\="ABC#$%^ DEF& \*GHI\*\*\*"
   WRITE $ZSTRIP(str,"\*P"),!
   WRITE $ZSTRIP(str,"\*P'W")

 

The following example strips out all characters, except lowercase letters ('L). However, the example uses the remchar parameter to strip the lowercase x while preserving all other lowercase characters:

   SET str\="xXx-Aa BXXbx Cxc Dd xxEeX^XXx"
   WRITE $ZSTRIP(str,"\*E'L","x")

 

returns: abcde

The following example strips out all characters, except lowercase letters ('L). In this case, the example does not specify a remchar parameter value (but does specify the delimiting commas), but does specify the keepchar parameter to preserve uppercase A, B, and C:

   SET str\="X-Aa BXXb456X CXc Dd XXEeX^XFFFfXX"
   WRITE $ZSTRIP(str,"\*E'L",,"ABC")

 

returns: AaBbCcdef

The following example does not specify a mask code; it specifies to remove the letters “X” and “x” wherever they occur in the string. All other characters in the string are returned.

   SET str\="+x $1x,x23XX4XX.X56XxxxxxX"
   WRITE $ZSTRIP(str,"\*","Xx")

 

returns: \+ $1,234.56

The following example does not specify a mask code; it specifies to remove the character “x” as a leading or trailing character, and to removed repeating “x”s wherever they occurs within the string:

   SET str\="xxxxx00xx0111xxx01x0000xxxxx"
   WRITE $ZSTRIP(str,"<=>","x")

 

returns: 00x0111x01x0000

The following example strips out all numeric, alphabetic, and punctuation characters, except whitespace and lowercase letters. Note that all mask codes without a Unary Not must precede any mask codes with a Unary Not:

   SET str\="Aa66\*&% B&$b Cc987 #Dd Ee"
   WRITE $ZSTRIP(str,"\*NAP'W'L")

 

returns: a b c d e

The following example strips out leading, trailing, and repeating characters that match the mask code A (all alphabetic characters):

   SET str\="ABC123DDDEEFFffffGG5555567HI JK"
   WRITE $ZSTRIP(str,"<=>A")

 

It returns 123DEFfG5555567HI; $ZSTRIP stripped leading characters (ABC) until it encountered a character of a type not included in the mask (1), and stripped trailing characters from the end of the string until it encountered a non-mask character (the blank space). Repeated characters of the mask type were reduced to a single occurrence (DDDEE = DE); note that the repeat test is case-sensitive (FFffff = Ff). Repeated characters that are not of the mask type (55555) are unaffected.

The following example strips out all characters except the hexadecimal digits 0–9 and A-F:

   SET str\="123$ GYJF870B-QD  @#%+"
   WRITE $ZSTRIP(str,"\*E'N",,"ABCDEF")

 

returns: 123F870BD

See Also

*   [$EXTRACT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fextract) function
    
*   [$ZCONVERT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzconvert) function
    
*   [$LOCATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flocate) and [$MATCH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fmatch) functions for Regular Expressions
    
*   [Pattern Matching](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_operators) operators in Using Caché ObjectScript
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzseek "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzwascii "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fzstrip.xml**