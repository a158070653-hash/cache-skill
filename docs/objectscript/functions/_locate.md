# $LOCATE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_flocate

---

 

Caché ObjectScript Reference

$LOCATE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistvalid "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fmatch "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$LOCATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flocate "$LOCATE") \]

Go to: Description Parameters Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Locates the first match of a regular expression in a string.

Synopsis

$LOCATE(string,regexp,start,end,val)

Parameters

string

The string to be matched.

regexp

A regular expression to match against string. A regular expression consists of one or more meta-characters, and may also contain literal characters.

start

Optional — An integer specifying the starting position within string from which to match the regexp.

If you omit start, matching begins at the beginning of string. If you omit start and specify end and/or val, you must specify the place-holder comma.

end

Optional — $LOCATE assigns an integer value to this variable if the match is successful. This integer is the next character position after the matched string. Caché passes end [by reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_usercode#GCOS_usercode_args_byref). This parameter must be a local variable. It cannot be an array, a global variable, or a reference to an object property.

val

Optional — $LOCATE assigns a string value to this variable if the match is successful. This string consists of the matched substring. Caché passes val [by reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_usercode#GCOS_usercode_args_byref). This parameter must be a local variable. It cannot be an array, a global variable, or a reference to an object property.

Description

$LOCATE matches a regular expression against successive substrings of a specified string. It returns an integer specifying the starting location of the first regexp match within string. It counts locations from 1. It returns 0 if regexp does not match any subset of string.

Optionally, it can also assign the matching substring to a variable.

If you omit an optional parameter and specify a later parameter, you must specify the appropriate place-holder comma(s).

Note:

$LOCATE is not supported on OpenVMS systems.

Caché ObjectScript support for regular expressions consists of the $LOCATE and $MATCH functions:

*   $LOCATE matches a regular expression to successive substrings of string and returns the location (and, optionally, the value) of the first match.
    
*   $MATCH matches a regular expression to the full string and returns a boolean indicating whether a match occurred.
    

The [Locate()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25Regex.Matcher#Locate) method of the [%Regex.Matcher](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Regex.Matcher) class provides similar functionality as $LOCATE. The [%Regex.Matcher](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Regex.Matcher) class methods provide substantial additional functionality for using regular expressions.

Other Caché ObjectScript matching operations use [Caché pattern match](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_operators#GCOS_operators_pattern) operators.

Parameters

string

An expression that evaluates to a [string](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_strings). The expression can be specified as the name of a variable, a numeric value, a string literal, or any valid Caché ObjectScript expression. A string can contain control characters.

If string is the empty string and regexp cannot match the empty string, $LOCATE returns 0; end and val are not set.

If string is the empty string and regexp can match the empty string, $LOCATE returns 1; end is set to 1, and val is set to the empty string.

regexp

A regular expression used to match against string to locate the desired substring. A regular expression is an expression that evaluates to a [string](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_strings) consisting of some combination of meta-characters and literals. Meta-characters specify character types and match patterns. Literals specify one or more matching single characters, ranges of characters, or substrings. An extensive regular expression syntax is supported. For details, refer to the “[Regular Expressions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_regexp)” chapter of Using Caché ObjectScript .

start

An integer specifying the starting position within string from which to match the regexp. 1 or 0 specify starting at the beginning of string. A start value equal to the length of string + 1 always returns 0. A start value greater than the length of string + 1 generates a <REGULAR EXPRESSION> error with ERROR #8351.

Regardless of the start position, $LOCATE returns the position of the first match as a count from the beginning of the string.

end

An output variable that $LOCATE assigns an integer value if the locate operation found a match. The assigned value is the location of the first character position after the matched substring, counting from the beginning of the string. If the match occurs at the end of the string, this character position can be one more that the total string length. If a match was not found, the end value is left unchanged. If end has not been previously set, the variable remains undefined.

The end variable cannot be a reference to an object property.

By using the same variable for start and end, you can invoke $LOCATE repeatedly to find all of the matches in the string. This is shown in the following example, which locates the positions of the vowels in the alphabet:

   SET alphabet\="abcdefghijklmnopqrstuvwxyz"
   SET pos\=1
   SET val\=""
   FOR i\=1:1:5 {
     WRITE $LOCATE(alphabet,"\[aeiou\]",pos,pos,val)
     WRITE " is the position of the ",i,"th vowel: ",val,! } 

 

val

An output variable that $LOCATE assigns a string value if the locate operation found a match. This string value is the matching substring. If a match was not found, the val value is left unchanged. If val has not been previously set, the variable remains undefined.

The val variable cannot be a reference to an object property.

Examples

The following example returns 4, because the regexp literal “de” matches at the 4th character of the string:

   WRITE $LOCATE("abcdef","de")

 

The following example returns 8, because regexp specifies a lowercase letter string of three characters, which is first found here as the substring “fga” starting a position 8:

  WRITE $LOCATE("ABC-de-fgabc123ABC","\[\[:lower:\]\]{3}")

 

The following example returns 5, because the specified regexp format of spaces (\\s) and non-space characters (\\S) is found beginning at the 5th character of the string. This example omits the start parameter; it sets the end variable to 11, which is the character after the matched substring.

   WRITE $LOCATE("AAAAA# $ 456789","\\S\\S\\s\\S\\s\\S",,end)

 

The following example returns 9, because regexp specifies a letter string of three characters, and the start parameter states it must begin at or after position 6:

  SET end\="",val\=""
  WRITE $LOCATE("abc-def-ghi-jkl","\[\[:alpha:\]\]{3}",6,end,val),!
  WRITE "the position after the matched string is: ",end,!
  WRITE "the matched value is: ",val

 

The end parameter is set to 12, and the val parameter is set to “ghi”.

The following example shows that a numeric is resolved to canonical form before regexp is matched to the resulting string. The end parameter is set to 5, one character beyond the end of the 4–character string “1.23”:

   WRITE $LOCATE(123E-2,"\\.\\d\*",1,end,val),!
   WRITE "end is: ",end,!
   WRITE "value is: ",val,!

 

The following example sets the start parameter to a value greater than the length of string+1. This results in an error, as shown:

   TRY {
   SET str\="abcdef"
   SET strlen\=$LENGTH(str)
   WRITE "start=string length, match=",$LOCATE(str,"\\p{L}",strlen),!
   WRITE "start=string length+1, match=",$LOCATE(str,"\\p{L}",strlen+1),!
   WRITE "start=string length+2, match=",$LOCATE(str,"\\p{L}",strlen+2),!
   }
  CATCH exp {
    WRITE !!,"CATCH block exception handler:",!
    IF 1\=exp.%IsA("%Exception.SystemException") {
      WRITE "System exception",!
      WRITE "Name: ",$ZCVT(exp.Name,"O","HTML"),!
      WRITE "Location: ",exp.Location,!
      WRITE "Code: ",exp.Code,!! 
      WRITE "%Regex.Matcher status:"
      SET err\=##class(%Regex.Matcher).LastStatus()
      DO $SYSTEM.Status.DisplayError(err) }
    ELSE {WRITE "Unexpected exception type",! }
    RETURN
  }

 

See Also

*   [$CHAR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fchar) function
    
*   [$MATCH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fmatch) function
    
*   [Regular Expressions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_regexp) in Using Caché ObjectScript
    
*   [Pattern Matching](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_operators) in Using Caché ObjectScript
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flistvalid "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fmatch "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_flocate.xml**