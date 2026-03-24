# $TRANSLATE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_d_translate

---

 

Caché SQL Reference

$TRANSLATE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_totimestamp "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_trim "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [$TRANSLATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_translate "$TRANSLATE") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A string function that performs character-for-character replacement.

Synopsis

$TRANSLATE(string,identifier\[,associator\])

Arguments

string

The target string. It can be a field name, a literal, a host variable, or an SQL expression.

identifier

The character(s) to search for in string. It can be a string or numeric literal, a host variable, or an SQL expression.

associator

Optional — The replacement character(s) corresponding to each character in the identifier. It can be a string or numeric literal, a host variable, or an SQL expression.

Description

The $TRANSLATE function performs character-for-character replacement within a return value string. It processes the string argument one character at a time. It compares each character in string with each character in the identifier argument. If $TRANSLATE finds a match, it makes note of the position of that character.

*   The two-argument form of $TRANSLATE removes all instances of the characters in the identifier argument from the output string.
    
*   The three-argument form of $TRANSLATE replaces all instances of each identifier character found in the string with the positionally corresponding associator character. Replacement is performed on a character, not a string, basis. If the identifier argument contains more characters than the associator argument, the excess characters in the identifier argument are deleted from the output string. If the identifier argument contains fewer characters than the associator argument, the excess character(s) in the associator argument are ignored.
    

$TRANSLATE is case-sensitive.

$TRANSLATE cannot be used to replace NULL with a character.

$TRANSLATE and REPLACE

$TRANSLATE performs character-for-character matching and replacement. [REPLACE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_replace) performs string-for-string matching and replacement. REPLACE can replace a single specified substring of one or more characters with another substring, or remove multiple instances of a specified substring. $TRANSLATE can replace multiple specified characters with corresponding specified replacement characters.

By default, both functions are case-sensitive, start at the beginning of string, and replace all matching instances. REPLACE has arguments that can be used to change these defaults.

Examples

In the following example, a two-argument $TRANSLATE modifies Name values by removing punctuation (commas, spaces, periods, apostrophes, hyphens), returning names that consist of only alphabetic characters. Note that the identifier doubles the apostrophe to escape it as a literal character, rather than a string delimiter:

SELECT TOP 20 Name,$TRANSLATE(Name,', .''-') AS AlphaName 
FROM Sample.Person
WHERE Name %STARTSWITH 'O'

 

In the following example, a three-argument $TRANSLATE modifies Name values by replacing commas and spaces with caret (^) characters, returning names delimited in three pieces (surname, first name, middle initial). Note that the associator must specify “^” as many times as the number of characters in identifier:

SELECT TOP 20 Name,$TRANSLATE(Name,', ','^^') AS PiecesNamePunc
FROM Sample.Person
WHERE Name %STARTSWITH 'O'

 

In the following example, a three-argument $TRANSLATE modifies Name values by both replacing commas and spaces with caret (^) characters (specified in the identifier and associator) and removing periods, apostrophes, and hyphens (specified in the identifier, omitted from the associator):

SELECT TOP 20 Name,$TRANSLATE(Name,', .''-','^^') AS PiecesNameNoPunc 
FROM Sample.Person
WHERE Name %STARTSWITH 'O'

 

See Also

*   [REPLACE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_replace) function
    
*   [STUFF](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_stuff) function
    
*   [String Manipulation](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_stringmanipulation)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_totimestamp "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_trim "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_d\_translate.xml**