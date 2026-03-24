# %STARTSWITH

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_startswith

---

 

Caché SQL Reference

%STARTSWITH

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_some "Go back to the previous section.") 

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Predicate Conditions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_PREDICATE_CONDITONS "SQL Predicate Conditions") \]  >  \[ [%STARTSWITH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_startswith "%STARTSWITH") \]

Go to: Description Other Equivalence Comparisons Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Matches a value with a substring specifying initial characters.

Synopsis

scalar-expression %STARTSWITH substring

Arguments

scalar-expression

A scalar expression (most commonly a data column) whose values are being compared with substring.

substring

An expression that resolves to a string or a numeric containing the first character or characters to match with values in scalar-expression.

Description

The %STARTSWITH predicate allows you to select those data values that begin with the character or characters specified in substring. If substring does not match any of the scalar expression values, %STARTSWITH returns the null string. This match is always performed on the logical (internal storage) data value, regardless of the display mode.

%STARTSWITH can be used wherever a [predicate condition](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_predicates) can be specified, as described in the [Overview of Predicates](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_predicates) page of this manual.

Collation Types

%STARTSWITH uses the same collation type as the column it is matched against. By default, string data types collate as not case-sensitive.

If you assign a different collation type to the column, this collation type is matched to the literal value of the %STARTSWITH substring. For example, the following string comparison is case-sensitive:

SELECT Name FROM Sample.Person WHERE %EXACT(Name) %STARTSWITH 'Ra'

 

You can also apply the same collation type to the %STARTSWITH substring. The following two examples are equivalent:

SELECT Name FROM Sample.Person WHERE %SQLSTRING(Name) %STARTSWITH ' Ra'

 

SELECT Name FROM Sample.Person WHERE %SQLSTRING(Name) %STARTSWITH %SQLSTRING('Ra')

 

A %STARTSWITH string comparison that is not case-sensitive and ignores blank spaces and punctuation marks (except commas):

SELECT Name FROM Sample.Person
WHERE %STRING(Name) %STARTSWITH %STRING(' od ')

 

Using %STRING, this example can select both O'Donnell and Odem.

Refer to [%SQLUPPER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_sqlupper) for further information on case transformation functions.

%SelectMode

The %STARTSWITH predicate cannot use the current [%SelectMode](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_dynsql#GSQL_dynsql_selectmode) setting. A substring must be specified in Logical format, regardless of the %SelectMode setting. Specifying predicate value(s) in ODBC or Display format commonly results in no data matches or unintended data matches. This applies mainly to dates, times, and Caché format lists (%List).

In the following Dynamic SQL example, the %STARTSWITH predicate must specify the date substring in Logical format, not in %SelectMode=1 (ODBC) format. Rows with DOB Logical values beginning with 41 (dates from April 4 1953 ($HOROLOG 41000) through December 28 1955 ($HOROLOG 41999)) are selected:

  ZNSPACE "SAMPLES"
  SET q1 \= "SELECT Name,DOB FROM Sample.Person "
  SET q2 \= "WHERE DOB %STARTSWITH '41%'"
  SET myquery \= q1\_q2
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET tStatement.%SelectMode\=1
  SET qStatus \= tStatement.%Prepare(myquery)
   IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
  SET rset \= tStatement.%Execute()
  DO rset.%Display()
  WRITE !,"End of data"

 

List Fields

If scalar-expression is a list field, %STARTSWITH can use %EXTERNAL to compare the list values to substring. For example, to determine all records in which the FavoriteColors list field begins with 'Bl':

SELECT Name,FavoriteColors FROM Sample.Person
WHERE %EXTERNAL(FavoriteColors) %STARTSWITH 'Bl'

 

When %EXTERNAL converts a list to DISPLAY format, the displayed list items appear to be separated by a blank space. This “space” is actually the two non-display characters CHAR(13) and CHAR(10). To use %STARTSWITH with more than one element in the list, you must specify these characters:

SELECT Name,FavoriteColors FROM Sample.Person
WHERE %EXTERNAL(FavoriteColors) %STARTSWITH 'Orange'||CHAR(13)||CHAR(10)||'B'

 

Filtering Out NULLs

*   If the scalar-expression is any non-null data value and the substring is an “empty” value, %STARTSWITH always returns the scalar-expression.
    
*   If the scalar-expression is null and the substring is an “empty” value, %STARTSWITH does not return the scalar-expression.
    

An “empty” substring value can be any of the following: NULL, CHAR(0), the empty string (''), a string consisting of only blank spaces (' '), CHAR(32) the space character, and CHAR(9) the tab character. Be default, %STARTSWITH uses all of these values for filtering out nulls.

To return scalar-expression values that consist of only whitespace characters, you must use %EXACT collation.

In all of the following examples, %STARTSWITH returns the same results. It restricts the result set to non-null FavoriteColors values:

SELECT Name,FavoriteColors FROM Sample.Person
WHERE FavoriteColors %STARTSWITH NULL

 

SELECT Name,FavoriteColors FROM Sample.Person
WHERE FavoriteColors %STARTSWITH ''

 

SELECT Name,FavoriteColors FROM Sample.Person
WHERE FavoriteColors %STARTSWITH '   '

 

SELECT Name,FavoriteColors FROM Sample.Person
WHERE FavoriteColors %STARTSWITH CHAR(9)

 

Note that the %EXTERNAL collation type is not used for scalar-expression when filtering nulls from a list field.

%STARTSWITH NULL and empty string behavior differs with a compound substring, because of the definitions of NULL and empty string. When you concatenate a value with NULL, the result is NULL. When you concatenate a value with the empty string, the result is the value. This is shown in the following examples:

SELECT Name,FavoriteColors
FROM Sample.Person
WHERE %EXTERNAL(FavoriteColors) %STARTSWITH 'B'||NULL
/\* Selects all non-null rows \*/

 

SELECT Name,FavoriteColors
FROM Sample.Person
WHERE %EXTERNAL(FavoriteColors) %STARTSWITH 'B'||''
/\* Selects all values that begin with B \*/

 

Leading and Trailing Blanks

In most cases, %STARTSWITH treats leading blanks the same as any other character. For example, %STARTSWITH ' B' can be used to select field values with exactly one leading blank followed by the letter B. However, a substring containing only blanks does not select for leading blanks; it selects for non-null values.

%STARTSWITH behavior with trailing blanks depends on the data type and collation type. %STARTSWITH ignores trailing blanks in a string substring. %STARTSWITH does not ignore trailing blanks in a numeric, date, or list substring.

In the following example, %STARTSWITH restricts the result set to names that begin with 'M'. Because Name is a string data type, the trailing blanks in the substring are ignored:

SELECT Name FROM Sample.Person
WHERE Name %STARTSWITH 'M      '

 

In the following example, %STARTSWITH eliminates all rows from the result set because the trailing blanks in the substring are not ignored for a numeric value:

SELECT Name,Age FROM Sample.Person
WHERE Age %STARTSWITH '6      '

 

In the following example, %STARTSWITH eliminates all rows from the result set because the trailing blank in the substring is not ignored for a list value:

SELECT Name,FavoriteColors FROM Sample.Person
WHERE %EXTERNAL(FavoriteColors) %STARTSWITH 'Blue '

 

However, in the following example, the result set consists of those list values that start with Blue followed by a list delimiter (which is displayed as a blank space); in other words, lists beginning with ‘Blue’ that contain more than one item:

SELECT Name,FavoriteColors FROM Sample.Person
WHERE %EXTERNAL(FavoriteColors) %STARTSWITH 'Blue'||CHAR(13)||CHAR(10)

 

Range of Subscripts

When scalar-expression is retrieved from a subscript, %STARTSWITH can be used as an index-limiting range condition, narrowing the range of scalar-expression subscript values that needs to be traversed. The logic is to start the subscript range with the given substring prefix value, and stop as soon as the subscript value no longer starts with substring.

National Collation Ambiguous Characters

In some national languages two characters or character combinations are considered first-pass collation equivalent. Commonly this is a character with or without an accent mark, such as in the Czech2 locale, in which CHAR(65) and CHAR(193) both collate as “A”. %STARTSWITH recognizes these characters as equivalent.

The following example shows the first-pass collation for Czech2 CHAR(65) (A) and CHAR(193) (Á):

M
MA
MÁ
MAC
MÁC
MACX
MÁCX
MAD
MÁD
MB 

It is important to note that you cannot know at query compile time which national collation would be used at run time. Therefore, %STARTSWITH subscript traversal code has to be written so that it will correctly satisfy any likely runtime situation.

Other Equivalence Comparisons

%STARTSWITH performs an equivalence comparison on the initial character(s) of a string. You can perform other types of equivalence comparisons by using string comparison operators. These include the following:

*   An equivalence comparison on the entire string, using the equal sign operator:
    
    SELECT Name,Home\_State FROM Sample.Person
    WHERE Home\_State \= 'VT'
    
     
    
    This example selects any record that contains the Home\_State field value “VT”. By default, this string comparison is not case-sensitive.
    
*   An non-equivalence comparison on the entire string, using the does not equal operator:
    
    SELECT Name,Home\_State FROM Sample.Person
    WHERE Home\_State <> 'MA'
    ORDER BY Home\_State
    
     
    
    This example selects all records that where the Home\_State field value is not equal to “MA”. By default, this string comparison is not case-sensitive.
    
*   An equivalence comparison on the entire string to multiple values, using the IN keyword operator:
    
    SELECT Name,Home\_State FROM Sample.Person
    WHERE Home\_State IN ('VT','MA','NH','ME')
    ORDER BY Home\_State
    
     
    
    This example selects any record that contains any of the specified Home\_State field values. By default, this string comparison is not case-sensitive.
    
*   An equivalence comparison on the entire string to a value pattern, using the %PATTERN keyword operator:
    
    SELECT Name,Home\_State FROM Sample.Person
    WHERE Home\_State %PATTERN '1U1"C"'
    ORDER BY Home\_State
    
     
    
    This example selects any record that contains a Home\_State field value that matches the pattern of 1U (one uppercase letter) followed by 1"C" (one literal letter “C”). This pattern would be fulfilled by the Home\_State abbreviations “NC” or “SC”.
    
*   An equivalence comparison of a substring to a value, using the contains operator:
    
    SELECT Name FROM Sample.Person
    WHERE Name \[ 'y'
    
     
    
    This example selects all Name records that contain the lowercase letter “y”. By default, this string comparison is case-sensitive.
    
*   A word-aware equivalence comparison of one or more substrings to a value, using the %CONTAINS or %CONTAINSTERM comparison operators. These operators can only be used on strings redefined with the %Text property.
    
*   An equivalence comparison of a substring with one or more wildcards to a value, using the LIKE keyword operator:
    
    SELECT Name FROM Sample.Person
    WHERE Name LIKE '\_a%'
    
     
    
    This example selects all Name records that contain the letter “a” as the second letter. This string comparison uses the Name collation type to determine whether the comparison is case-sensitive or not case-sensitive.
    

For further details on these and other comparison conditional predicates, refer to the [WHERE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_where) clause.

Examples

The following example uses the WHERE clause to select Name values that start with the letter “R” or “r”. By default, %STARTSWITH string comparisons are not case-sensitive:

SELECT Name FROM Sample.Person
WHERE Name %STARTSWITH 'r'

 

The following example returns one record for each distinct Home\_State name that begins with “M”:

SELECT DISTINCT Home\_State FROM Sample.Person
WHERE Home\_State %STARTSWITH 'M'
ORDER BY Home\_State

 

The following example uses a HAVING clause to select records for people whose age starts with a 2, displays the average for all ages and the average for the ages selected by the HAVING clause. It orders the results by age:

SELECT Name,
       Age,
       AVG(Age) AS AvgAge,
       AVG(Age %AFTERHAVING) AS Avg20
FROM Sample.Person
HAVING Age %STARTSWITH 2
ORDER BY Age

 

The following example performs a %STARTSWITH comparison with the internal date format value for the DOB (date of birth) field. In this case, it select all dates from 11/5/1988 ($H\=54000) through 08/1/1991 ($H\=54999):

SELECT Name,DOB
FROM Sample.Person
WHERE DOB %STARTSWITH 54
ORDER BY DOB

 

See Also

*   [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select) statement [HAVING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_having) clause [WHERE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_where) clause
    
*   [Overview of Predicates](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_predicates)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_some "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_startswith.xml**