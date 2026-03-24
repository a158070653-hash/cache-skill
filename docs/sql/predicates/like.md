# LIKE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_like

---

 

Caché SQL Reference

LIKE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_inset "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_matches "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Predicate Conditions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_PREDICATE_CONDITONS "SQL Predicate Conditions") \]  >  \[ [LIKE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_like "LIKE") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Matches a value with a pattern string containing literals and wildcards.

Synopsis

scalar-expression LIKE pattern \[ESCAPE char\]

Arguments

scalar-expression

A scalar expression (most commonly a data column) whose values are being compared with pattern.

pattern

A quoted string representing the pattern of characters to match with each value in scalar-expression. The pattern string can contain literal characters, and the underscore (\_) and percent (%) wildcard characters.

ESCAPE char

Optional — A string containing a single character. This char character can be used in pattern to specify that the character immediately following it is to be treated as a literal.

Description

The LIKE predicate allows you to select those data values that match the character or characters specified in pattern. The pattern may contain wildcard characters. If pattern does not match any of the scalar expression values, LIKE returns the null string.

LIKE can be used wherever a [predicate condition](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_predicates) can be specified, as described in the [Overview of Predicates](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_predicates) page of this manual.

The LIKE predicate supports the following wildcards:

LIKE Wildcard Characters

Character

Matches

\_

Any single character.

%

Any sequence of 0 or more characters. (In accordance with the SQL standard, NULL is not considered a sequence of 0 characters, and is thus not selected by this wildcard.)

In Dynamic SQL or Embedded SQL, a pattern can represent wildcard characters and input parameters or input host variables as concatenated strings, as shown in the [Examples](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_like#RSQL_like_examples) section.

Collation Types

The pattern string uses the same [collation type](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_basics#GSQL_basics_collationfundamentals) as the column it is matching against. By default, string data types collate as not case-sensitive. If LIKE is applied against a field with the default collation type (or the legacy ALPHAUP collation type), the LIKE clause returns matches that ignore letter case. You can use the %SQLSTRING collation type to perform a LIKE string comparison that is case-sensitive.

The following example returns all names that contain the substring “Ro”. Because LIKE is not case-sensitive, LIKE '%Ro%' returns Robert, Rogers, deRocca, LaRonga, Brown, Mastroni, and so forth:

SELECT Name FROM Sample.Person
WHERE Name LIKE '%Ro%'

 

Compare this to the [Contains operator](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_langelements#GSQL_langelements_ops_relational) (\[), which uses EXACT (case-sensitive) collation:

SELECT Name FROM Sample.Person
WHERE Name \[ 'Ro'

 

By using the %SQLSTRING collation type, you can use LIKE to return only those names that contain the case-sensitive substring “Ro”. It would not return Mastroni or Brown:

SELECT Name FROM Sample.Person
WHERE %SQLSTRING(Name) LIKE '%Ro%'

 

In the above example, the leading space that %SQLSTRING appended to Name values was handled by the % wildcard. A more robust example would specify the collation type on both sides of the predicate:

SELECT Name FROM Sample.Person
WHERE %SQLSTRING(Name) LIKE %SQLSTRING('%Ro%')

 

Do not use the [%ALPHAUP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_alphaup) or [%STRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_string_pct) functions with pattern, because these functions remove the punctuation marks that pattern uses as wildcard characters.

Refer to [%SQLUPPER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_sqlupper) for further information on case transformation functions.

All Values, Empty String Values, and NULL

If the pattern value is percent (%), LIKE selects all values for the specified field, including empty string values:

SELECT Name,FavoriteColors FROM Sample.Person
WHERE FavoriteColors LIKE '%'

 

It does not select fields that are NULL.

Specifying a pattern value of empty string returns empty string values.

SELECT Name,FavoriteColors FROM Sample.Person
WHERE FavoriteColors LIKE ''

 

Specifying a pattern value of NULL is not a meaningful operation. It completes successfully, but returns no values.

SELECT Name,FavoriteColors FROM Sample.Person
WHERE FavoriteColors LIKE NULL

 

Like most predicates, LIKE can be inverted using the NOT logical operator. Neither LIKE nor NOT LIKE can be used to return NULL fields. To return NULL fields use [IS NULL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_isnull).

ESCAPE Clause

ESCAPE permits the use of a wildcard character as a literal character within pattern. ESCAPE char, if provided and if it is a single character, indicates that any character directly following it in pattern is to be understood as a literal character, rather than a wildcard or formatting character. The following example shows the use of ESCAPE to return values that contain the string '\_SYS':

SELECT \* FROM MyTable
WHERE symbol\_field LIKE '%\\\_SYS%' ESCAPE '\\'

%SelectMode

The LIKE predicate does not use the current [%SelectMode](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_dynsql#GSQL_dynsql_selectmode) setting. A pattern should be specified in Logical format, regardless of the %SelectMode setting. Attempting to specify a pattern in ODBC format or Display format commonly results in no data matches or unintended data matches.

You can use the [%EXTERNAL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_external) or [%ODBCOUT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_odbcout) format-transform functions to transform the scalar-expression field that the predicate operates upon. This allows you to specify the pattern in Display format or ODBC format. However, using a format-transform function prevents the use of the index for the field, and can thus have a significant performance impact.

In the following Dynamic SQL example, the LIKE predicate specifies the date pattern in Logical format, not in %SelectMode=1 (ODBC) format. Rows with DOB Logical values beginning with 41 (dates from April 4 1953 ($HOROLOG 41000) through December 28 1955 ($HOROLOG 41999)) are selected:

  ZNSPACE "SAMPLES"
  SET q1 \= "SELECT Name,DOB FROM Sample.Person "
  SET q2 \= "WHERE DOB LIKE '41%'"
  SET myquery \= q1\_q2
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET tStatement.%SelectMode\=1
  SET qStatus \= tStatement.%Prepare(myquery)
   IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
  SET rset \= tStatement.%Execute()
  DO rset.%Display()
  WRITE !,"End of data"

 

The following Dynamic SQL example uses the [%ODBCOUT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_odbcout) format-transform function to transform the DOB field matched by the predicate. This allows you to specify the LIKE pattern in ODBC format. It selects rows with DOB field ODBC values beginning with 195 (dates within the range of years 1950 through 1959). However, specifying the format-transform function prevents the use of an index for DOB field values:

  ZNSPACE "SAMPLES"
  SET q1 \= "SELECT Name,DOB FROM Sample.Person "
  SET q2 \= "WHERE %ODBCOUT(DOB) LIKE '195%'"
  SET myquery \= q1\_q2
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET tStatement.%SelectMode\=1
  SET qStatus \= tStatement.%Prepare(myquery)
   IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
  SET rset \= tStatement.%Execute()
  DO rset.%Display()
  WRITE !,"End of data"

 

Literal Substitution Override

You can override literal substitution during compile pre-parsing by enclosing the LIKE predicate argument with double parentheses. For example, WHERE Name LIKE (('Mc%')) or WHERE Name LIKE (('%son%')). This may improve query performance by improving overall selectivity and/or subscript bounding selectivity. However, it should be avoided when the same query is called multiple times with different values, as it will result in the creation of a separate cached query for each query call.

Examples

The following example uses the WHERE clause to select Name values that contain “son”, including those that begin or end with “son”. By default, LIKE string comparisons are not case-sensitive:

SELECT %ID,Name FROM Sample.Person
WHERE Name LIKE '%son%'

 

The following Embedded SQL example returns the same result set as the previous example. Note how the input host variable (:subname) is specified in the LIKE pattern using the concatenation operator:

  ZNSPACE "SAMPLES"
  SET subname\="son"
  &sql(DECLARE C1 CURSOR FOR SELECT %ID,Name INTO :id,:nameout FROM Sample.Person
       WHERE Name LIKE '%'\_:subname\_'%')
  &sql(OPEN C1)
 &sql(FETCH C1)
 WHILE (SQLCODE \= 0) {
     WRITE id," ",nameout,!
    &sql(FETCH C1) }
  &sql(CLOSE C1)

 

The following Dynamic SQL example returns the same result set as the previous example. Note how the input parameter (?) is specified in the LIKE pattern using the concatenation operator:

  ZNSPACE "SAMPLES"
  SET myquery \= "SELECT %ID,Name FROM Sample.Person WHERE Name LIKE '%'\_?\_'%'"
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET qStatus \= tStatement.%Prepare(myquery)
   IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
  SET rset \= tStatement.%Execute("son")
  DO rset.%Display()

 

The following example uses the WHERE clause to select FavoriteColors values that contain “blue”. The FavoriteColors field is a %List field; the % wildcards handle the %List formatting characters:

SELECT Name,FavoriteColors FROM Sample.Person
WHERE FavoriteColors LIKE '%blue%'

 

The following example uses a HAVING clause to select records for people whose age starts with a 1 followed by a single character. It displays the average for all ages and the average for the ages selected by the HAVING clause. It orders the results by age. All returned values have ages from 10 through 19.

SELECT Name,
       Age,
       AVG(Age) AS AvgAge,
       AVG(Age %AFTERHAVING) AS AvgTeen
FROM Sample.Person
HAVING Age LIKE '1\_'
ORDER BY Age

 

See Also

*   [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select) statement [HAVING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_having) clause [WHERE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_where) clause
    
*   [%MATCHES](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_matches) predicate
    
*   [%PATTERN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_pattern) predicate
    
*   [Overview of Predicates](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_predicates)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_inset "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_matches "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_like.xml**