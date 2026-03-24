# REPLACE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_replace

---

 

Caché SQL Reference

REPLACE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_repeat "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_replicate "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [REPLACE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_replace "REPLACE") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A string function that replaces a substring within a string.

Synopsis

REPLACE(string,oldsubstring,newsubstring)

Arguments

string

A string expression that is the target for the substring search.

oldsubstring

The substring to match within string.

newsubstring

The substring used to replace oldsubstring.

Description

REPLACE searches a string for a substring and replaces all matches. Matching is case-sensitive. If a match is found, it replaces every instance of oldsubstring with newsubstring. The replacement substring may be longer or shorter than the substring it replaces. If the substring cannot be found, REPLACE returns the original string unchanged.

The value returned by REPLACE is always of data type VARCHAR, regardless of the data type of string. This allows for replacement operations such as REPLACE(12.3,'.','\_').

The empty string is a string value. You can, therefore, use the empty string for any argument value. However, note that the Caché ObjectScript empty string is passed to Caché SQL as NULL.

NULL is not a data value in Caché SQL. For this reason, specifying NULL for any of the REPLACE arguments returns NULL, regardless of whether or not a match occurs.

This function provides compatibility with Transact-SQL implementations.

REPLACE, STUFF, and $TRANSLATE

Both REPLACE and STUFF perform substring replacement. REPLACE searches for a substring by data value. STUFF searches for a substring by string position and length.

REPLACE performs a single string-for-string matching and replacement. $TRANSLATE performs character-for-character matching and replacement; it can replace all instances of one or more specified single characters with corresponding specified replacement single characters. It can also remove all instances of one or more specified single characters from a string.

By default, all three functions are case-sensitive and replace all matching instances.

For a list of functions that search for a substring, refer to [String Manipulation](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_stringmanipulation) in the Concepts section of this manual.

Examples

The following example searches for every instance of the substring 'K' and replaces it with the substring 'P':

SELECT REPLACE('KING KONG','K','P')

 

The following embedded SQL example searches for every instance of the substring 'KANSAS' and replaces it with the substring 'NEBRASKA':

  SET str\="KANSAS, ARKANSAS, NEBRASKA"
  &sql(SELECT REPLACE(:str,'KANSAS','NEBRASKA') INTO :x)
  WRITE !,"SQLCODE=",SQLCODE
  WRITE !,"Output string=",x

 

The following example show that REPLACE handles the empty string ('') just like any other string value:

SELECT REPLACE('','','Nothing'),
       REPLACE('KING KONG','','P'),
       REPLACE('KING KONG','K','')

 

The following example shows that REPLACE handles any NULL argument by returning NULL. All of the following REPLACE functions return NULL, including the last, in which no match occurs:

SELECT REPLACE(NULL,'K','P'),
       REPLACE(NULL,NULL,'P'),
       REPLACE('KING KONG',NULL,'P'),
       REPLACE('KING KONG','K',NULL),
       REPLACE('KING KONG','Z',NULL)

 

The following Embedded SQL example is identical to the previous NULLs example. It shows how the Caché ObjectScript empty string host variable is treated as NULL within SQL:

  SET a\=""
  &sql(SELECT 
  REPLACE(:a,'K','P'),
  REPLACE(:a,:a,'P'),
  REPLACE('KING KONG',:a,'P'),
  REPLACE('KING KONG','K',:a),
  REPLACE('KING KONG','Z',:a)
  INTO :v,:w,:x,:y,:z)
  WRITE !,"SQLCODE=",SQLCODE
  WRITE !,"Output string=",v
  WRITE !,"Output string=",w
  WRITE !,"Output string=",x
  WRITE !,"Output string=",y
  WRITE !,"Output string=",z

 

See Also

*   [CHARINDEX](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_charindex) function
    
*   [$FIND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_find) function
    
*   [STUFF](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_stuff) function
    
*   [$TRANSLATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_translate) function
    
*   [String Manipulation](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_stringmanipulation)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_repeat "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_replicate "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_replace.xml**