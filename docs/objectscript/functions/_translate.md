# $TRANSLATE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_ftranslate

---

 

Caché ObjectScript Reference

$TRANSLATE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ftext "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fview "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_FUNCTIONS "Caché ObjectScript Functions") \]  >  \[ [$TRANSLATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ftranslate "$TRANSLATE") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Performs character-for-character replacement within a string.

Synopsis

$TRANSLATE(string,identifier,associator)
$TR(string,identifier,associator)

Parameters

string

The target string. It can be a numeric value, a string literal, the name of a variable, or any valid Caché ObjectScript expression.

identifier

The character(s) to search for in string. It can be a numeric value, a string literal, the name of a variable, or any valid Caché ObjectScript expression.

associator

Optional — The replacement character(s) corresponding to each character in the identifier. It can be a numeric value, a string literal, the name of a variable, or any valid Caché ObjectScript expression.

Description

The $TRANSLATE function performs character-for-character replacement within a string. It processes the string parameter one character at a time. Initially, $TRANSLATE sets the output string to the input string. It compares each character in the input string with each character in the identifier parameter. If $TRANSLATE finds a match, it makes note of the position of that character.

*   The two-parameter form of $TRANSLATE removes those characters in the identifier parameter from the output string.
    
*   The three-parameter form of $TRANSLATE replaces the identifier character(s) found in the string with the positionally corresponding character(s) from the associator parameter. Replacement is performed on a character, not a string, basis. If the identifier parameter contains fewer characters than the associator parameter, the excess character(s) in the associator parameter are ignored. If the identifier parameter contains more characters than the associator parameter, the excess character(s) in the identifier parameter are deleted in the output string.
    

$TRANSLATE is case-sensitive.

$TRANSLATE by itself does not change the string parameter. To change the input string, you must SET it equal to a translation of itself.

$TRANSLATE and $REPLACE

$TRANSLATE performs character-for-character matching and replacement. $REPLACE performs string-for-string matching and replacement. $REPLACE can replace a single specified substring of one or more characters with another substring, or remove multiple instances of a specified substring. $TRANSLATE can replace multiple specified characters with corresponding specified replacement characters.

By default, both functions are case-sensitive, start at the beginning of string, and replace all matching instances. $REPLACE has parameters that can be used to change these defaults.

Examples

In the following example, a two-parameter $TRANSLATE removes Numeric Group Separators based on the setting for the current locale:

AppropriateInput
   SET ds\=##class(%SYS.NLS.Format).GetFormatItem("DecimalSeparator")
   IF ds\="." {SET x\="+1,462,543.33"}
   ELSE {SET x\="+1.462.543,33"}
TranslateNum
   WRITE !,"before translation ",x
   SET ngs\=##class(%SYS.NLS.Format).GetFormatItem("NumericGroupSeparator")
   IF ngs\=","     {SET x\=$TRANSLATE(x,",") }
   ELSEIF ngs\="." {SET x\=$TRANSLATE(x,".") }
   ELSEIF ngs\=" " {SET x\=$TRANSLATE(x," ") }
   ELSE {WRITE "Non-standard NumericGroupSeparator:", ngs
         RETURN }
   WRITE !,"after translation  ",x

 

In the following example, a three-parameter $TRANSLATE replaces various Date Separator Characters with slashes. Note that the associator must specify “/” as many times as the number of characters in identifier:

   SET x(1)\="06-23-2014"
   SET x(2)\="06.24.2014"
   SET x(3)\="06/25/2014"
   SET x(4)\="06|26|2014"
   SET x(5)\="06 27 2014"
   FOR i\=1:1:5{
        SET x(i)\=$TRANSLATE(x(i),"- .|","////")
        WRITE "x(",i,") :",x(i),!
   }

 

In the following example, a three-parameter $TRANSLATE “simplifies” Spanish to basic ASCII by replacing accented letters with non-accented letters and removing the question and exclamation sentence prefix punctuation:

  SET esp\="¿Sabes lo que ocurrirá en el año 2016?"
  WRITE "Spanish:",!,esp,!
  SET iden\=$CHAR(225)\_$CHAR(233)\_$CHAR(237)\_$CHAR(241)\_$CHAR(243)\_$CHAR(250)\_$CHAR(161)\_$CHAR(191)
  SET asso\="aeinou"
  WRITE "Identifier: ",iden,!
  WRITE "Associator: ",asso,!
  SET spanglish\=$TRANSLATE(esp,iden,asso)
  WRITE "Spanglish:",!,spanglish

 

Needless to say, this is not a recommended conversion for use on actual Spanish text.

See Also

*   [$EXTRACT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fextract) function
    
*   [$PIECE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fpiece) function
    
*   [$REPLACE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_freplace) function
    
*   [$REVERSE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_freverse) function
    
*   [$ZCONVERT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzconvert) function
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ftext "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fview "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_ftranslate.xml**