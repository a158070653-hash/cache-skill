# %MATCHES

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_matches

---

 

Caché SQL Reference

%MATCHES

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_like "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_null "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Predicate Conditions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_PREDICATE_CONDITONS "SQL Predicate Conditions") \]  >  \[ [%MATCHES](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_matches "%MATCHES") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Matches a value with a pattern string containing literals, wildcards, and ranges.

Synopsis

scalar-expression %MATCHES pattern \[ESCAPE char\]

Arguments

scalar-expression

A scalar expression (most commonly a data column) whose values are being compared with pattern.

pattern

A quoted string representing the pattern of characters to match with each value in scalar-expression. The pattern string can contain literal characters, the question mark (?) and asterisk (%) wildcard characters, square brackets used to specify allowed values, and the backslash (\\) used to specify that the character immediately following it is to be treated as a literal. The pattern can also be the empty string or NULL, though it does not match or return NULL items.

ESCAPE char

Optional — A string containing a single character. This char character can be used in pattern to specify that the character immediately following it is to be treated as a literal. If not specified, the default escape character is backslash (\\).

Description

The %MATCHES predicate is a Caché extension for matching a value to a pattern string. %MATCHES returns True or False for the match operation. The pattern string can consist of literal characters, wild card characters, and list or ranges of matching literals.

Pattern matches are case sensitive. Pattern matching is based on the EXACT value of scalar-expression, not its collation value. Therefore, a %MATCHES operation is always case sensitive, even when the collation of scalar-expression is not case sensitive.

%MATCHES supports the following pattern wildcards:

?

Matches any single character of any type.

\*

Matches zero or more characters of any type.

\[abc\]

Matches any one of the characters specified in brackets.

\[a-z\]

Matches character within the range specified in brackets, inclusive of the specified characters.

\[^A-Z\]

\[^a-z\]

\[^0–9\]

These ranges match any characters except those specified in brackets. You can use this syntax to specify no uppercase letters, or no lowercase letters, or no numbers. Only the specified literal ranges shown are supported.

\\

Treats the character following as a literal character, rather than as a wildcard. Backslash is the default escape character; you can specify another character as the escape character using the optional ESCAPE clause.

Like most predicates, %MATCHES can be inverted using the NOT operator: item NOT %MATCHES pattern. Neither %MATCHES nor NOT %MATCHES can be used to return NULL fields. To return NULL fields use [IS NULL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_isnull).

The backslash (\\) character is the default escape character. It can be used to specify that a wildcard character is to be used as a literal match at the specified pattern location. For example, to match a question mark as the first character of a string specify '\\?\*'. To match a question mark as the fourth character of a string specify '???\\?\*'. To match a question mark anywhere in a string specify '\*\\?\*'. To match a string that consists of only an asterisk character specify '\\\*'. To match a string that contains at least one asterisk character specify '\*\\\*\*'. To match a backslash character anywhere in a string specify '\*\\\\\*'.

%MATCHES can be used wherever a [predicate condition](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_predicates) can be specified, as described in the [Overview of Predicates](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_predicates) page of this manual.

%MATCHES is supported for compatibility with Informix SQL.

%SelectMode

The %MATCHES predicate does not use the current [%SelectMode](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_dynsql#GSQL_dynsql_selectmode) setting. A pattern should be specified in Logical format, regardless of the %SelectMode setting. Attempting to specify a pattern in ODBC format or Display format commonly results in no data matches or unintended data matches.

You can use the [%EXTERNAL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_external) or [%ODBCOUT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_odbcout) format-transform functions to transform the scalar-expression field that the predicate operates upon. This allows you to specify the pattern in Display format or ODBC format. However, using a format-transform function prevents the use of the index for the field, and can thus have a significant performance impact.

In the following Dynamic SQL example, the %MATCHES predicate specifies the date pattern in Logical format, not in %SelectMode=1 (ODBC) format. Rows with DOB Logical values beginning with 41 (dates from April 4 1953 ($HOROLOG 41000) through December 28 1955 ($HOROLOG 41999)) are selected:

  ZNSPACE "SAMPLES"
  SET q1 \= "SELECT Name,DOB FROM Sample.Person "
  SET q2 \= "WHERE DOB %MATCHES '41\*'"
  SET myquery \= q1\_q2
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET tStatement.%SelectMode\=1
  SET qStatus \= tStatement.%Prepare(myquery)
   IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
  SET rset \= tStatement.%Execute()
  DO rset.%Display()
  WRITE !,"End of data"

 

The following Dynamic SQL example uses the [%ODBCOUT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_odbcout) format-transform function to transform the DOB field matched by the predicate. This allows you to specify the %MATCHES pattern in ODBC format. It selects rows with DOB field ODBC values beginning with 195 (dates within the range of years 1950 through 1959). However, specifying the format-transform function prevents the use of an index for DOB field values:

  ZNSPACE "SAMPLES"
  SET q1 \= "SELECT Name,DOB FROM Sample.Person "
  SET q2 \= "WHERE %ODBCOUT(DOB) %MATCHES '195\*'"
  SET myquery \= q1\_q2
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET tStatement.%SelectMode\=1
  SET qStatus \= tStatement.%Prepare(myquery)
   IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
  SET rset \= tStatement.%Execute()
  DO rset.%Display()
  WRITE !,"End of data"

 

Examples

The following example returns all last names that begin with “A”:

SELECT Name FROM Sample.Person 
WHERE Name %MATCHES 'A\*'

 

The following example returns all first names that begin with “A”:

SELECT Name FROM Sample.Person 
WHERE Name %MATCHES '\*,A\*'

 

The following example returns all names that contain the letter “A” (in last name, first name, or middle initial):

SELECT Name FROM Sample.Person 
WHERE Name %MATCHES '\*A\*'

 

The following example returns all names that do not contain the letters “A”, “a”, “E” or “e”:

SELECT Name FROM Sample.Person 
WHERE Name NOT %MATCHES '\*\[AaEe\]\*'

 

The following example returns all five-letter last names with first names that begin with “A” through “D”:

SELECT Name FROM Sample.Person 
WHERE Name %MATCHES '?????,\[A-D\]\*'

 

See Also

*   [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select) statement, [HAVING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_having) clause, [WHERE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_where) clause
    
*   [LIKE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_like) predicate
    
*   [%PATTERN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_pattern) predicate
    
*   [Overview of Predicates](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_predicates)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_like "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_null "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_matches.xml**