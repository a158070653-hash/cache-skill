# %PATTERN

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_pattern

---

 

Caché SQL Reference

%PATTERN

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_null "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_some "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Predicate Conditions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_PREDICATE_CONDITONS "SQL Predicate Conditions") \]  >  \[ [%PATTERN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_pattern "%PATTERN") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Matches a value with a pattern string containing literals, wildcards, and character type codes.

Synopsis

scalar-expression %PATTERN pattern

Arguments

scalar-expression

A scalar expression (most commonly a data column) whose values are being compared with pattern.

pattern

A quoted string representing the pattern of characters to match with each value in scalar-expression. The pattern string can contain literal characters enclosed in double quotes, letter codes that specify types of characters, and numbers and the period (.) character as wildcard characters.

Description

The %PATTERN predicate allows you to match a pattern of character type codes and literals to the data values supplied by scalar-expression. If pattern matches a complete scalar expression value, this value is returned. If pattern does not fully match any of the scalar expression values, %PATTERN returns the null string.

%PATERN can be used wherever a [predicate condition](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_predicates) can be specified, as described in the [Overview of Predicates](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_predicates) page of this manual.

%PATTERN uses the same pattern codes as the Caché ObjectScript pattern match operator (the ? operator). A pattern consists of one or more pairs of a repetition count followed by a value. A repetition count can be an integer, a period (.) meaning “any number of characters”, or a range specified by using a combination of a period with integers. A value can be either a character type code letter or a literal string (specified in quotes).

Note that a pattern often consists of multiple repetition/value pairs, because the pattern must exactly match the entire data value. For this reason, many patterns end with the “.E” pair, which means that the rest of the data value can consist of any number of characters of any type.

A few simple examples of pattern match pairs:

*   1L means one (and only one) lowercase letter.
    
*   1"L" means one literal character “L”.
    
*   1"617" means one literal string “617”.
    
*   .U means any number of uppercase letters.
    
*   .E means any number of printable characters of any type.
    
*   .3A means any number up to three (three or less) letters (either uppercase or lowercase).
    
*   3.N means three or more numeric digits.
    
*   3.6N means three to six (inclusive) numeric digits.
    

Pattern matches are case sensitive. Pattern matching is based on the EXACT value of scalar-expression, not its collation value. Therefore, a literal letter specified in a %PATTERN operation is always matched case sensitive, even when the collation of scalar-expression is not case sensitive.

In Dynamic SQL the SQL query is specified as an ObjectScript string, delimited by double quotes. For this reason, double quotes within a pattern string must be doubled. Thus the pattern for a US dollar amount: '1"$"1.N1"."2N' would be specified in Dynamic SQL as '1""$""1.N1"".""2N'.

For further details on pattern codes, refer to [Pattern Matching](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_operators#GCOS_operators_pattern) in the [Operators and Expressions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_operators) chapter of Using Caché ObjectScript.

%SelectMode

The %PATTERN predicate does not use the current [%SelectMode](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_dynsql#GSQL_dynsql_selectmode) setting. A pattern should be specified in Logical format, regardless of the %SelectMode setting. Attempting to specify a pattern in ODBC format or Display format commonly results in no data matches or unintended data matches.

You can use the [%EXTERNAL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_external) or [%ODBCOUT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_odbcout) format-transform functions to transform the scalar-expression field that the predicate operates upon. This allows you to specify the pattern in Display format or ODBC format. However, using a format-transform function prevents the use of the index for the field, and can thus have a significant performance impact.

In the following Dynamic SQL example, the %PATTERN predicate specifies the date pattern in Logical format, not in %SelectMode=1 (ODBC) format. Rows with DOB Logical values beginning with 41 (dates from April 4 1953 ($HOROLOG 41000) through December 28 1955 ($HOROLOG 41999)) are selected:

  ZNSPACE "SAMPLES"
  SET q1 \= "SELECT Name,DOB FROM Sample.Person "
  SET q2 \= "WHERE DOB %PATTERN '1""41""3N' "
  SET myquery \= q1\_q2
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET tStatement.%SelectMode\=1
  SET qStatus \= tStatement.%Prepare(myquery)
   IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
  SET rset \= tStatement.%Execute()
  DO rset.%Display()
  WRITE !,"End of data"

 

The following Dynamic SQL example uses the [%ODBCOUT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_odbcout) format-transform function to transform the DOB field matched by the predicate. This allows you to specify the %PATTERN pattern in ODBC format. It selects rows with DOB field ODBC values beginning with 195 (dates within the range of years 1950 through 1959). However, specifying the format-transform function prevents the use of an index for DOB field values:

  ZNSPACE "SAMPLES"
  SET q1 \= "SELECT Name,DOB FROM Sample.Person "
  SET q2 \= "WHERE %ODBCOUT(DOB) %PATTERN '1""195"".E' "
  SET myquery \= q1\_q2
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET tStatement.%SelectMode\=1
  SET qStatus \= tStatement.%Prepare(myquery)
   IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
  SET rset \= tStatement.%Execute()
  DO rset.%Display()
  WRITE !,"End of data"

 

Examples

The following example uses a %PATTERN operator in the WHERE clause to select Home\_State values in which the first character is any uppercase letter and the second character is the letter “C”:

SELECT Name,Home\_State FROM Sample.Person
WHERE Home\_State %PATTERN '1U1"C"'

This example selects records with a Home\_State of North Carolina (NC) or South Carolina (SC).

The following example uses a %PATTERN operator in the WHERE clause to select Name values that start with an uppercase letter followed by a lowercase letter.

SELECT Name FROM Sample.Person
WHERE Name %PATTERN '1U1L.E'

The pattern here translates as: 1U (one uppercase letter), followed 1L (one lowercase letter), followed by .E (any number of characters of any type). Note that this pattern would exclude names such as ”JONES”, O'Reilly” and “deGastyne”.

The following example uses a %PATTERN operator in a HAVING clause to select records for people whose first name starts with the letters “Jo”, and to return the count of records searched and records returned.

SELECT Name,
       COUNT(Name) AS TotRecs,
       COUNT(Name %AFTERHAVING) AS JoRecs
FROM Sample.Person
HAVING Name %PATTERN '1U.L1","1"Jo".E'

In this case, the Name field values are formatted as Lastname,Firstname and may contain an optional middle name or initial. To reflect this name format, the pattern here translates as: 1U (one uppercase letter), followed .L (any number of lowercase letters), followed by 1"," (one literal comma character), followed by 1"Jo" (one literal string with the value “Jo”), followed by .E (any number of characters of any type).

See Also

*   [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select) statement [HAVING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_having) clause [WHERE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_where) clause
    
*   [LIKE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_like) predicate
    
*   [%MATCHES](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_matches) predicate
    
*   [Overview of Predicates](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_predicates)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_null "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_some "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_pattern.xml**