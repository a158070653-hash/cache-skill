# DECODE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_decode

---

 

Caché SQL Reference

DECODE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dayofyear "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_degrees "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [DECODE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_decode "DECODE") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A function that evaluates a given expression and returns a specified value.

Synopsis

DECODE(expr {,search,result}\[,default\])

Arguments

expr

The expression to be decoded.

search

The value to which expr is compared.

result

The value which is returned if expr matches search.

default

Optional — The default value which is returned if expr does not match any search.

Description

You can specify multiple search,result pairs, separated by commas. You can specify one default. The maximum number of parameters in the DECODE expression (including expr, search, result, and default) is about 100. The search, result, and default values can be derived from expressions.

To evaluate a DECODE expression, Caché compares expr to each search value, one by one:

*   If expr is equal to a search value, the corresponding result is returned.
    
*   If expr is not equal to any search value, the default value is returned, or, if default is omitted, null is returned.
    

Caché evaluates each search value only before comparing it to expr, rather than evaluating all search values before comparing any of them to expr. Therefore, Caché never evaluates a search if a previous search is equal to expr.

In a DECODE expression, Caché considers two nulls to be equivalent. If expr is null, Caché returns the result of the first search that is also null.

Note that DECODE is supported for Oracle compatibility.

Data Type of Returned Value

DECODE returns the data type of the first result argument. If the data type of the first result argument cannot be determined, DECODE returns VARCHAR. For numeric values, DECODE returns the largest length, precision, and scale from all of the possible result argument values.

If the data types of result and default are different, the data type returned is the type most compatible with all of the possible return values, the data type with the highest [data type precedence](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datatype#RSQL_datatype_precedence). For example, if result is an integer and default is a fractional number, DECODE returns a value with data type NUMERIC. This is because NUMERIC is the data type with the highest precedence that is compatible with both.

Examples

The following example “decodes” ages from 13 through 19 as 'Teen'; the default is 'Adult':

SELECT Name,Age,DECODE(Age,
       13,'Teen',14,'Teen',15,'Teen',16,'Teen',
       17,'Teen',18,'Teen',19,'Teen',
       'Adult') AS AgeBracket
FROM Sample.Person
WHERE Age \> 12

 

The following example decodes NULLs. If there is no value for FavoriteColors, DECODE replaces it with the string ‘No Preference’; otherwise, it returns the FavoriteColors value:

SELECT Name,DECODE(FavoriteColors,
                   NULL,'No Preference',
                   $LISTTOSTRING(FavoriteColors,'^')) AS ColorPreference
FROM Sample.Person
ORDER BY Name

 

The following example decodes color preferences. If the person has a single favorite color, that color name is replaced by a letter abbreviation. If the person has more than one favorite color, DECODE returns the FavoriteColors value:

SELECT Name,DECODE(FavoriteColors,
                   $LISTBUILD('Red'),'R',
                   $LISTBUILD('Orange'),'O',
                   $LISTBUILD('Yellow'),'Y',
                   $LISTBUILD('Green'),'G',
                   $LISTBUILD('Blue'),'B',
                   $LISTBUILD('Purple'),'V',
                   $LISTBUILD('White'),'W',
                   $LISTBUILD('Black'),'K',
                   $LISTTOSTRING(FavoriteColors,'^')) 
FROM Sample.Person
WHERE FavoriteColors IS NOT NULL
ORDER BY FavoriteColors

 

Note that the [ORDER BY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_order) clause sorts by the original field values. The following example sorts by the DECODE values:

SELECT Name,DECODE(FavoriteColors,
                   $LISTBUILD('Red'),'R',
                   $LISTBUILD('Orange'),'O',
                   $LISTBUILD('Yellow'),'Y',
                   $LISTBUILD('Green'),'G',
                   $LISTBUILD('Blue'),'B',
                   $LISTBUILD('Purple'),'V',
                   $LISTBUILD('White'),'W',
                   $LISTBUILD('Black'),'K',
                   $LISTTOSTRING(FavoriteColors,'^')) AS ColorCode
FROM Sample.Person
WHERE FavoriteColors IS NOT NULL
ORDER BY ColorCode

 

The following example decodes the numeric code in the Company code field in Employee records and returns the corresponding department name. If an employee’s Company code is not 1 through 10, DECODE returns the default of “Admin (non-tech)”:

SELECT Name,
DECODE (Company,
   1, 'TECH MARKETING', 2, 'TECH SALES', 3, 'DOCUMENTATION', 
   4, 'BASIC RESEARCH', 5, 'SOFTWARE DEVELOPMENT', 6, 'HARDWARE DEVELOPMENT',
   7, 'QUALITY TESTING', 8, 'FIELD SUPPORT', 9, 'PHONE SUPPORT',
   10, 'TECH TRAINING',
       'Admin (non-tech)') AS TechJobs
FROM Sample.Employee

 

The expression has Company as the expr parameter and uses ten pairs of search and result parameters. "Admin (non-tech)" is the default parameter.

See Also

*   [CASE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_case)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dayofyear "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_degrees "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_decode.xml**