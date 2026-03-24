# CASE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_case

---

 

Caché SQL Reference

CASE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_call "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_checkpriv "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_COMMANDS "SQL Commands") \]  >  \[ [CASE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_case "CASE") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Chooses one of a specified set of values depending on some condition.

Synopsis

CASE 
  WHEN search\_condition THEN value\_expression
  \[ WHEN search\_condition THEN value\_expression ... \]
  \[ ELSE value\_expression \]
END

CASE value\_expression
  WHEN value\_expression THEN value\_expression
  \[ WHEN value\_expression THEN value\_expression ... \]
  \[ ELSE value\_expression \]
END

Arguments

search\_condition

An SQL boolean expression.

value\_expression

An SQL expression (such as a literal value or field name.)

Description

The CASE expression allows you to make comparison tests on series of values, returning when it encounters the first match.

The CASE expression comes in two forms: Simple and Searched.

The Simple CASE expression tests a series of value expressions (specified by a WHEN clause) to see if they are equal to a given value expression:

SELECT
CASE Field1
  WHEN 1 THEN 'ONE'
  WHEN 2 THEN 'TWO'
  ELSE NULL
END
FROM MyTable

The value associated with the first matching expression is returned as the value of the CASE expression.

Numeric value\_expression values may have different data types. The data type returned is the type most compatible with all of the possible result values, the data type with the highest [data type precedence](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datatype#RSQL_datatype_precedence). For numeric value\_expression values CASE returns the largest length, precision, and scale from all of the possible result values. A result value of NULL has the lowest data type precedence; however, if all result values are NULL, the data type returned is VARCHAR.

The Searched CASE expression tests a series of search conditions (specified by a WHEN clause), finds the first WHEN condition that evaluates to true, and returns the value associated with it:

SELECT
CASE
  WHEN Field1 \= 1 THEN 'ONE'
  WHEN Field1 \= 2 THEN 'TWO'
  ELSE NULL
END
FROM MyTable

With either form of CASE expression, you can use an ELSE clause to specify what value to return if none of the WHEN clause conditions are true. If you omit the ELSE clause and none of the WHEN clause conditions are true, CASE returns NULL.

A CASE comparison tests for NULL must use the IS NULL or IS NOT NULL keyword phrase. NULL is not a data value (it represents the absence of a value). For this reason, any equality or arithmetic test for NULL always return false. A CASE expression that compares NULL and any data value always returns false. For example, NULL < 1 and NULL > 1 both return false. A CASE expression that equates NULL with NULL also returns false.

The end of a CASE expression is marked by an END token.

You can use the [GREATEST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_greatest) and [LEAST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_least) functions to return the value from a series of values that is the largest or smallest (collates highest or lowest) of the provided values.

Examples

The following query is an example of a Simple CASE expression, where specified field values are replaced by supplied values. Note the use of the RetireAge column alias after the END keyword; the optional AS keyword is omitted in this example:

SELECT Name,
CASE Age
  WHEN 65 THEN 'Retire this year'
  WHEN 64 THEN 'Retire next year' 
  ELSE 'Past retirement age '|| Age
END RetireAge
FROM Sample.Person
WHERE Age \> 63
ORDER BY Age

 

The following query is another example of a Simple CASE expression. This query labels rows with certain Home\_State values as either “Northern NE” or “Southern NE”, and sets all other Home\_State values in this column to NULL. It uses the As clause to label this column as “NewEnglanders”, and also displays Names and the original Home\_State values. The resulting rows are ordered first by the NewEnglanders column (in descending order), and within this alphabetically by Home\_State, and then by Name.

SELECT Name,
CASE Home\_State
  WHEN 'VT' THEN 'Northern NE'
  WHEN 'NH' THEN 'Northern NE'
  WHEN 'ME' THEN 'Northern NE'
  WHEN 'MA' THEN 'Southern NE'
  WHEN 'CT' THEN 'Southern NE'
  WHEN 'RI' THEN 'Southern NE'
  ELSE NULL
END AS NewEnglanders, Home\_State
FROM Sample.Person
ORDER BY NewEnglanders DESC,Home\_State,Name

 

The following query is an example of a Searched CASE expression. It uses logical operators (greater than (>), logical AND (&), logical OR (!)) to specify a boolean statement for each WHEN clause. The first WHEN clause that tests True sets the value expression that follows the THEN keyword. In this example, the Age and Home\_State field values are used to identify three types of Yankees: Old Yankees, Yankees (residents of the six New England states), and likely fans of the New York Yankees baseball team:

SELECT Name,
CASE
WHEN Age \> 55 & Home\_State \= 'VT' 
   ! Home\_State\='ME' ! Home\_State\='NH'
   ! Home\_State\='MA' ! Home\_State\='CT'
   ! Home\_State\='RI'
THEN 'Old Yankee'
WHEN Home\_State \= 'VT' 
   ! Home\_State\='ME' ! Home\_State\='NH'
   ! Home\_State\='MA' ! Home\_State\='CT'
   ! Home\_State\='RI'
THEN 'Yankee'
WHEN Home\_State\='NY' THEN 'Yankees Fan'
   ELSE Home\_State
END AS Yankees
FROM Sample.Person

 

The following example shows that any comparison with NULL always returns false:

SELECT TOP 5 Name,
CASE NULL
  WHEN NULL THEN 'Null = Null'
  WHEN 0 THEN 'Null = 0'
  WHEN '' THEN 'Null = empty string'
  WHEN CHAR(0) THEN 'Null = CHAR(0)'
  ELSE 'Null Arithmetic Invalid'
END
FROM Sample.Person

 

The following example shows how to use CASE with a field that has NULLs:

SELECT TOP 20 Name,
CASE
  WHEN FavoriteColors IS NULL THEN 'No Colors'
  ELSE $LISTTOSTRING(FavoriteColors,':')
END
FROM Sample.Person

 

See Also

*   SQL functions: [DECODE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_decode) [GREATEST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_greatest) [LEAST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_least) [NULLIF](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_nullif) [COALESCE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_coalesce)
    
*   Caché ObjectScript function: [$CASE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fcase)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_call "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_checkpriv "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_case.xml**