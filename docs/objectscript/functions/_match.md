# $MATCH

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fmatch

---

 

Caché ObjectScript Reference

$MATCH

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flocate "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fmethod "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$MATCH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fmatch "$MATCH") \]

Go to: Description Parameters Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Matches a regular expression to a string.

Synopsis

$MATCH(string,regexp)

Parameters

string

The string to be matched.

regexp

A regular expression to match against string. A regular expression consists of one or more meta-characters, and may also contain literal characters.

Description

$MATCH is a boolean function that returns 1 if string and regexp match, and 0 if string and regexp do not match. By default, matching is case sensitive.

Note:

$MATCH is not supported on OpenVMS systems.

Caché ObjectScript support for [regular expressions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_regexp) consists of the $LOCATE and $MATCH functions:

*   $MATCH matches a regular expression to the full string and returns a boolean indicating whether a match occurred.
    
*   $LOCATE matches a regular expression to successive substrings of string and returns the location (and, optionally, the value) of the first match.
    

The [Match()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25Regex.Matcher#Match) method of the [%Regex.Matcher](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Regex.Matcher) class provides the same functionality. The [%Regex.Matcher](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Regex.Matcher) class provides additional functionality for using regular expressions.

Other Caché ObjectScript matching operations use [Caché pattern match](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_operators#GCOS_operators_pattern) operators.

Parameters

string

An expression that evaluates to a [string](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_strings). The expression can be specified as the name of a variable, a numeric value, a string literal, or any valid Caché ObjectScript expression. A string can contain control characters.

regexp

A regular expression used to match against string. A regular expression is an expression that evaluates to a [string](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_strings) consisting of some combination of meta-characters and literals. Meta-characters specify character types and match patterns. Literals specify one or more matching single characters, ranges of characters, or substrings. An extensive regular expression syntax is supported. For details, refer to the “[Regular Expressions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_regexp)” chapter of Using Caché ObjectScript .

Examples

The following example matches a string with a regular expression that specifies that the first character must be an uppercase letter (\\p{LU}), followed by at least one additional character (+ quantifier), and that this second character, and all subsequent characters, must be word characters (letters, numbers, or the underscore character) (\\w):

   SET strng(1)\="Assembly\_17"
   SET strng(2)\="Part5"
   SET strng(3)\="SheetMetalScrew"
   SET n\=1
  WHILE $DATA(strng(n)) {
    IF $MATCH(strng(n),"\\p{LU}\\w+")
      { WRITE strng(n)," : successful match",! }
    ELSE { WRITE strng(n)," : invalid string",! }
    SET n\=n+1 }

 

The following example returns 1, because the hexadecimal regular expression (hex 41) matches the letter “A”:

   WRITE $MATCH("A","\\x41")

 

The following example returns 1, because the specified string matches the format of spaces (\\s) and non-space characters (\\S) specified in the regular expression:

   WRITE $MATCH("A# $ 4","\\S\\S\\s\\S\\s\\S")

 

The following example returns 1, because the specified date matches the format of digits and literals specified in the regular expression:

   SET today\=$ZDATE($HOROLOG)
   WRITE $MATCH(today,"^\\d\\d/\\d\\d/\\d\\d\\d\\d$")

 

Note that this format requires that the day and month each be specified as two digits, so a leading zero is required for values smaller than 10.

The following example returns 1, because each letter in the string is within the corresponding letter range in the regular expression:

   WRITE $MATCH("HAL","\[G-I\]\[A-C\]\[K-Z\]")

 

The following example specifies an invalid regexp parameter. This results in an error, as shown:

   TRY {
   SET str\="abcdef"
   WRITE "match=",$MATCH(str,"\\p{}"),!
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
    
*   [$LOCATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flocate) function
    
*   [$ZSTRIP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzstrip) function
    
*   [Regular Expressions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_regexp) in Using Caché ObjectScript
    
*   [Pattern Matching](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_operators) in Using Caché ObjectScript
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_flocate "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fmethod "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_fmatch.xml**