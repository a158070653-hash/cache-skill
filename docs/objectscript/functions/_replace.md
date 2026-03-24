# $REPLACE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_freplace

---

 

Caché ObjectScript Reference

$REPLACE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_frandom "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_freverse "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$REPLACE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_freplace "$REPLACE") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Performs string-for-string replacement within a string.

Synopsis

$REPLACE(string,oldsub,newsub\[,start\[,count\[,case\]\]\])

Parameters

string

The target string. It can be a numeric value, a string literal, the name of a variable, or any valid Caché ObjectScript expression. If string is an empty string (""), $REPLACE returns an empty string.

oldsub

The substring to search for in string. It can be a numeric value, a string literal, the name of a variable, or any valid Caché ObjectScript expression. If oldsub is an empty string (""), $REPLACE returns string.

newsub

The replacement substring substituted for instances of oldsub in string. It can be a numeric value, a string literal, the name of a variable, or any valid Caché ObjectScript expression. If newsub is an empty string (""), $REPLACE returns string with the occurrences of oldsub removed.

start

Optional — Character count position within string where substring search is to begin. String characters are counted from 1. A value of 0, a negative number, a nonnumeric string or an empty string are equivalent to 1. If omitted, 1 is assumed. If start > 1, the substring of string beginning with that character is returned, with substring substitutions (if any) performed. If start > $LENGTH(string), $REPLACE returns the empty string ("").

count

Optional — Number of substring substitutions to perform. If omitted, the default value is -1, which means perform all possible substitutions. A value of 0, a negative number other than -1, a nonnumeric string or an empty string are equivalent to 0 which means perform no substitutions. count must be used in conjunction with start.

case

Optional — Boolean flag indicating whether matching is to be case-sensitive. 0 = case-sensitive (the default). 1 = not case-sensitive. Any nonzero number is equivalent to 1. Any nonnumeric value is equivalent to 0.

Description

The $REPLACE function performs string-for-string replacements within a string. It searches string for the oldsub substring. If $REPLACE finds a match, it replaces the oldsub substring with newsub. The newsub parameter value may be long or shorter than oldsub; newsub may be an empty string.

By default, $REPLACE begins at the start of string and replaces every instance of oldsub. You can use the optional start parameter to begin comparisons at a specified character count location within the string. The returned string is a substring of string that begins at the start location and replaces every instance of oldsub from that point. You can use the optional count parameter to replace only a specified number of matching substrings.

By default, $REPLACE substring matching is case-sensitive. You can use the optional case parameter to specify not case-sensitive matching.

$REPLACE supports [long strings](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_strings_long) (strings of > 32,767 characters) on Caché instances for which long strings are enabled.

$REPLACE by itself does not change the string parameter. To change the input string, you must SET it equal to the returned value.

$REPLACE and $TRANSLATE

$REPLACE performs string-for-string matching and replacement. $TRANSLATE performs character-for-character matching and replacement. $REPLACE can replace a single specified substring of one or more characters with another substring. $TRANSLATE can replace multiple specified characters with corresponding specified new characters. By default, both functions replace all matching instances in the string.

Examples

In the following example, invocations of $REPLACE match and substitute for the all instances of a substring, and the first two instances of a substring:

   SET x\="1110/1110/1100/1110"
   WRITE !,"before conversion  ",x
   SET all\=$REPLACE(x,"111","AAA")
   WRITE !,"after replacement  ",all
   SET some\=$REPLACE(x,"111","AAA",1,2)
   WRITE !,"after replacement  ",some

 

In the following example, invocations of $REPLACE perform case-sensitive and not case-sensitive matching and replacement of all occurrences in the string:

   SET x\="Yes/yes/Y/YES/Yes"
   WRITE !,"before conversion  ",x
   SET case\=$REPLACE(x,"Yes","NO")
   WRITE !,"after replacement  ",case
   SET nocase\=$REPLACE(x,"Yes","NO",1,\-1,1)
   WRITE !,"after replacement  ",nocase

 

The following example compares the $REPLACE and $TRANSLATE functions:

   SET x\="A mom, o plom, o comal, Pomama"
   WRITE !,"before conversion  ",x
   SET s4s\=$REPLACE(x,"om","an")
   WRITE !,"after replacement  ",s4s
   SET c4c\=$TRANSLATE(x,"om","an")
   WRITE !,"after translation  ",c4c

 

$REPLACE returns "A man, o plan, o canal, Panama"

$TRANSLATE returns "A nan, a plan, a canal, Panana"

In the following example, the four-parameter form of $REPLACE returns only the part of the string beginning with the start point, with the string-for-string replacements performed:

   SET x\="A mon, a plon, a conal, Ponama"
   WRITE !,"before start replacement ",x
   SET ret\=$REPLACE(x,"on","an",8)
   WRITE !,"after start replacement  ",ret  

 

$REPLACE returns "a plan, a canal, Panama"

See Also

*   [$TRANSLATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ftranslate) function
    
*   [$EXTRACT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fextract) function
    
*   [$PIECE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fpiece) function
    
*   [$REVERSE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_freverse) function
    
*   [$ZCONVERT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzconvert) function
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_frandom "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_freverse "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_freplace.xml**