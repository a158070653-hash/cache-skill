# ISNUMERIC

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_isnumeric

---

 

Caché SQL Reference

ISNUMERIC

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_isnull "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_jsonarray "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_FUNCTIONS "SQL Functions") \]  >  \[ [ISNUMERIC](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_isnumeric "ISNUMERIC") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A numeric function that tests for a valid number.

Synopsis

ISNUMERIC(check-expression)

Arguments

check-expression

The expression to be evaluated.

Description

ISNUMERIC evaluates check-expression and returns one of the following values:

*   Returns 1 if check-expression is a valid number. A valid number can either be a numeric expression or a string that represents a valid number.
    
    *   A numeric expression is first converted to [canonical form](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_numcanonical), resolving multiple leading signs; therefore, a numeric expression such as +-+++34 is a valid number.
        
    *   A numeric string is not converted before evaluation. A numeric string must have at most one leading sign to evaluate as a valid number. A numeric string with a trailing decimal point evaluates as a valid number.
        
*   Returns 0 if check-expression is not a valid number. Any string that contains a non-numeric character is not a valid number. A numeric string with more than one leading sign, such as '+-+++34', is not evaluated as a valid number. A Caché encoded list always returns 0, even if its element(s) are valid numbers. An empty string ISNUMERIC('') returns 0.
    
*   Returns NULL if check-expression is NULL. ISNUMERIC(NULL) returns null.
    

ISNUMERIC generates an SQLCODE -7, exponent out of range error if a scientific notation exponent is greater than 308 (308 – (number of integers - 1)). For example, ISNUMERIC(1E309) and ISNUMERIC(111E307) both generate this error code. If an exponent numeric string less than or equal to '1E145' returns 1; an exponent numeric string greater than '1E145' returns 0.

The ISNUMERIC function is very similar to the Caché ObjectScript $ISVALIDNUM function. However, these two functions return different values when the input value is NULL.

Examples

In the following example, all of the ISNUMERIC functions return 1:

SELECT ISNUMERIC(99) AS MyInt,
       ISNUMERIC('-99') AS MyNegInt,
       ISNUMERIC('-0.99') AS MyNegFrac,
       ISNUMERIC('-0.00') AS MyNegZero,
       ISNUMERIC('-0.09'+7) AS MyAdd,
       ISNUMERIC('5E2') AS MyExponent

 

The following example returns NULL if FavoriteColors is NULL; otherwise, it returns 0, because FavoriteColors is not a numeric field:

SELECT Name,
ISNUMERIC(FavoriteColors) AS ColorPref
FROM Sample.Person

 

See Also

*   [IFNULL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_ifnull) function
    
*   [ISNULL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_isnull) function
    
*   [NULLIF](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_nullif) function
    
*   Caché ObjectScript function: [$ISVALIDNUM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fisvalidnum)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_isnull "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_jsonarray "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_isnumeric.xml**