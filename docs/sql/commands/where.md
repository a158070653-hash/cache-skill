# WHERE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_where

---

 

Caché SQL Reference

WHERE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_values "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_wherecurrentof "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_COMMANDS "SQL Commands") \]  >  \[ [WHERE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_where "WHERE") \]

Go to: Description Equality Comparison Predicates BETWEEN Predicate IN and %INLIST Predicates Substring Predicates NULL Predicate EXISTS Predicate FOR SOME Predicate FOR SOME %ELEMENT Predicate LIKE, %MATCHES, and %PATTERN Predicates Predicates and Logical Operators See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A SELECT clause that specifies one or more restrictive conditions.

Synopsis

SELECT fields
FROM table
WHERE condition-expression

Arguments

condition-expression

An expression consisting of one or more boolean predicates governing which data values are to be retrieved.

Description

The optional WHERE clause can be used for the following purposes:

*   To specify predicates that restrict which data values are to be returned.
    
*   To specify an explicit join between two tables.
    
*   To specify an implicit join between the base table and a field in another table.
    

The WHERE clause is most commonly used to specify one or more [predicates](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_predicates) that are used to restrict the data (filter out rows) retrieved by a [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select) statement.

The WHERE clause qualifies or disqualifies specific rows from the query selection. The rows that qualify are those for which the condition-expression is true. The condition-expression can be one or more logical tests (predicates).

Multiple predicates can be linked by the AND and OR logical operators. See “[Predicates and Logical Operators](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_where#RSQL_where_logic)” for further details and restrictions.

If a predicate includes division and there are any values in the database that could produce a divisor with a value of zero or a NULL value, you cannot rely on order of evaluation to avoid division by zero. Instead, use a CASE statement to suppress the risk.

A WHERE clause can specify an explicit join between two tables using the = (inner join), =\* (left outer join), and \*= (right outer join) symbolic join operators. For further details, refer to the [JOIN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_join) page of this manual.

A WHERE clause can specify an implicit join between the base table and a field from another table using the arrow syntax (–>) operator. For further details, refer to [Implicit Joins](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_implicitjoins) in Using Caché SQL.

Specifying a Field

The simplest form of a WHERE clause specifies a predicate comparing a field to a value, such as WHERE Age > 21. Valid field values include the following: A column name (WHERE Age > 21); an %ID, %TABLENAME, or %CLASSNAME; a scalar function specifying a column name (WHERE ROUND(Age,-1)=60), a collation function specifying a column name (WHERE %SQLUPPER(Name) %STARTSWITH ' AB').

You cannot specify a field by column number.

You cannot specify a field by column alias; attempting to do so generates an SQLCODE -29 error. However, you can use a subquery to define a column alias, then use this alias in the WHERE clause. For example:

SELECT Interns FROM 
      (SELECT Name AS Interns FROM Sample.Employee WHERE Age<21) 
WHERE Interns %STARTSWITH 'A'

 

You cannot specify an aggregate field; attempting to do so generates an SQLCODE -19 error. However, you can supply an aggregate function value to a WHERE clause by using a subquery. For example:

SELECT Name,Age,AvgAge
FROM (SELECT Name,Age,AVG(Age) AS AvgAge FROM Sample.Person)
WHERE Age < AvgAge
ORDER BY Age

 

Integers and Strings

If a field defined as integer data type is compared to a numeric value, the numeric value is converted to [canonical form](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_numcanonical) before performing the comparison. For example, WHERE Age=007.00 parses as WHERE Age=7. This conversion occurs in all modes.

If a field defined as integer data type is compared to a string value in Display mode, the string is parsed as a numeric value. For instance, an empty string (''), like any non-numeric string, is parsed as the number 0. This parsing follows Caché ObjectScript rules for handling strings as numbers. For example, WHERE Age='twenty' parses as WHERE Age=0; WHERE Age='20something' parses as WHERE Age=20. For further details, refer to [Strings as Numbers](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_nonnumasnum) in the “Data Types and Values” chapter of Using Caché ObjectScript. SQL only performs this parsing in Display mode; in Logical or ODBC mode comparing an integer to a string value returns null.

To compare a string field with a string containing a single quote, double the single quote. For example, WHERE Name %STARTSWITH 'O''' returns O’Neil and O’Connor, but not Obama.

Date and Time

In Caché SQL dates and times are compared and stored using a Logical Mode internal representation. They can be returned in Logical mode, Display Mode, or ODBC mode. For example, the date September 28, 1944 is represented as: Logical mode 37891, Display mode 09/28/1944, ODBC mode 1944-09-28. When specifying a date or time in a condition-expression an error can occur due to a mismatch of SQL mode and date or time format, or due to an invalid date or time value.

A WHERE clause condition-expression must use the date or time format that corresponds to the current mode. For example, when in Logical mode, to return records with a date of birth in 2005, the WHERE clause would appear as follows: WHERE DOB BETWEEN 59901 AND 60265. When in Display mode, the same WHERE clause would appear as follows: WHERE DOB BETWEEN '01/01/2005' AND '12/31/2005'.

Failing to match the condition-expression date or time format to the display mode results in an error:

*   In Display mode or ODBC mode, specifying date data in the incorrect format generates an SQLCODE -146 error. Specifying time data in the incorrect format generates an SQLCODE -147 error.
    
*   In Logical mode, specifying date or time data in the incorrect format does not generate an error, but either returns no data or returns unintended data. This is because Logical mode does not parse a date or time in Display or ODBC format as a date or time value. The following WHERE clause, when executed in Logical mode, returns unintended data: WHERE DOB BETWEEN 37500 AND 38000 AND DOB <> '1944-09-28' returns a range of DOB values, including DOB=37891 (September 28, 1944), which the <> predicate was attempting to omit.
    

An invalid date or time value also generates an SQLCODE -146 or -147 error. An invalid date is one that you can specify in Display mode/ODBC mode, but Caché cannot convert into a Logical mode equivalent. For example, in ODBC mode the following generates an SQLCODE -146 error: WHERE DOB > '1830-01-01' because Caché cannot process a date value prior to December 31, 1840. The following in ODBC mode also generates an SQLCODE -146 error: WHERE DOB BETWEEN '2005-01-01' AND '2005-02-29', because 2005 is not a leap year.

When in Logical mode, a Display mode or ODBC mode value is not parsed as a date or time value, and therefore its value is not validated. For this reason, in Logical mode a WHERE clause such as WHERE DOB > '1830-01-01' does not return an error.

Stream Fields

In most situations, you cannot use a stream field in a WHERE clause predicate. Doing so results in an SQLCODE -313 error. However, the following uses of stream fields are allowed in a WHERE clause:

*   [Stream null testing](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_null): you can specify the predicate streamfield IS NULL or streamfield IS NOT NULL.
    
*   Stream length testing: you can specify a CHARACTER\_LENGTH(streamfield), CHAR\_LENGTH(streamfield), or DATALENGTH(streamfield) function in a WHERE clause predicate.
    
*   Stream substring testing: you can specify a SUBSTRING(streamfield,start,length) function in a WHERE clause predicate.
    

List Structures

Caché supports the list structure data type %List (data type class %Library.List). This is a compressed binary format, which does not map to a corresponding native data type for Caché SQL. It corresponds to data type VARBINARY with a default MAXLEN of 32749. For this reason, [Dynamic SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_dynsql) cannot use %List data in a WHERE clause comparison. For further details, refer to the [Data Types](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datatype) reference page in this manual.

To reference structured list data, use the %INLIST predicate or the FOR SOME %ELEMENT predicate.

To use the data values of a list field in a condition-expression, you can use %EXTERNAL to compare the list values to a predicate. For example, to return all records in which the FavoriteColors list field value consists of the single element 'Red':

SELECT Name,FavoriteColors FROM Sample.Person
WHERE %EXTERNAL(FavoriteColors)\='Red'

 

When %EXTERNAL converts a list to DISPLAY format, the displayed list items appear to be separated by a blank space. This “space” is actually the two non-display characters CHAR(13) and CHAR(10). To use a condition-expression against more than one element in the list, you must specify these characters. For example, to return all records in which the FavoriteColors list field value consists of the two elements 'Orange' and 'Black' (in that order):

SELECT Name,FavoriteColors FROM Sample.Person
WHERE %EXTERNAL(FavoriteColors)\='Orange'||CHAR(13)||CHAR(10)||'Black'

 

List of Predicates

The SQL predicates fall into the following categories:

*   [Equality Comparison Predicates](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_where#RSQL_where_comparisonpredicates)
    
*   [BETWEEN Predicate](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_where#RSQL_where_betweenpredicate)
    
*   [IN and %INLIST Predicates](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_where#RSQL_where_inpredicate)
    
*   [Substring Predicates: %STARTSWITH, %CONTAINS, and %CONTAINSTERM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_where#RSQL_where_substringpredicates)
    
*   [NULL Predicate](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_where#RSQL_where_nullpredicate)
    
*   [EXISTS Predicate](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_where#RSQL_where_existspredicate)
    
*   [FOR SOME Predicate](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_where#RSQL_where_forsomepredicate)
    
*   [FOR SOME %ELEMENT Predicate](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_where#RSQL_where_elementpredicate)
    
*   [LIKE, %MATCHES, and %PATTERN Predicates](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_where#RSQL_where_likepredicate)
    
*   [%INSET](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_inset) and [%FIND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_find) Predicates
    

Predicate Case-Sensitivity

Most predicates use the field’s default [collation](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_basics#GSQL_basics_collation) to determine the case sensitivity of letters in strings. By default, Caché string fields are not case-sensitive.

The %INLIST, Contains operator (\[), %MATCHES, and %PATTERN predicates do not use the field’s default collation. They always uses EXACT collation, which is case-sensitive.

A predicate comparison of two literal strings is always case-sensitive.

Predicate Conditions and %NOINDEX

You can preface a predicate condition with the %NOINDEX keyword to prevent the query optimizer using an index on that condition. This is most useful when specifying a range condition that is satisfied by the vast majority of the rows. For example, WHERE %NOINDEX Age >= 1. For further details, refer to [Index Optimization Options](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_optquery#GSQLOPT_optquery_from) in the Caché SQL Optimization Guide.

Predicate Condition on Outlier Value

If the WHERE clause in a Dynamic SQL query selects on a non-null outlier value, you can significantly improve performance by enclosing the outlier value literal in double parentheses. These double parentheses cause Dynamic SQL to use the outlier selectivity when optimizing. For example, if your business is located in Massachusetts (MA), a large percentage of your employees will reside in Massachusetts. For the Employees table Home\_State field, 'MA' is the outlier value. To optimally select for this value, you should specify WHERE Home\_State=(('MA')).

This syntax should not be used in Embedded SQL or in a view definition. In Embedded SQL or a view definition, the outlier selectivity is always used and requires no special coding.

A WHERE clause in a Dynamic SQL query automatically optimizes for a null outlier value. For example, a clause such as WHERE FavoriteColors IS NULL. No special coding is required for IS NULL and IS NOT NULL predicates when NULL is the outlier value.

Outlier selectivity is determined by running the [Tune Table](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_opttable#GSQLOPT_opttable_tunetable) utility. For further details, refer to [Outlier Optimization](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_opttable#GSQLOPT_opttable_outlieropt) in the “Optimizing Tables” chapter of the Caché SQL Optimization Guide.

Equality Comparison Predicates

The following are the available equality comparison predicates:

SQL Equality Comparison Predicates

Predicate

Operation

\=

Equals

<>

Does not equal

!=

Does not equal

\>

Is greater than

<

Is less than

\>=

Is greater than or equal to

<=

Is less than or equal to

For example:

SELECT Name, Age FROM Sample.Person
WHERE Age < 21

 

SQL defines comparison operations in terms of collation: the order in which values are sorted. Two values are equal if they collate in exactly the same way. A value is greater than another value if it collates after the second value. String [field collation](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_basics#GSQL_basics_collation) takes the field’s default collation. The Caché default collation is not case-sensitive. Thus, a comparison of two string field values or a comparison of a string field value with a string literal is (by default) not case-sensitive. For example, if Home\_State field values are uppercase two-letter strings:

Expression

Value

'MA' = Home\_State

TRUE for values MA.

'ma' = Home\_State

TRUE for values MA.

'VA' < Home\_State

TRUE for values VT, WA, WI, WV, WY.

'ar' >= Home\_State

TRUE for values AK, AL, AR.

Note, however, that a comparison of two literal strings is case-sensitive: WHERE 'ma'='MA' is always FALSE.

BETWEEN Predicate

The BETWEEN comparison operator allows you to select those data values that are in the range specified by the syntax BETWEEN lowval AND highval. This range is inclusive of the lowval and highval values themselves. This is equivalent to a paired greater than or equal to operator and a less than or equal to operator. This comparison is shown in the following example:

SELECT Name,Age FROM Sample.Person
WHERE Age BETWEEN 18 AND 21

 

This returns all the records in the Sample.Person table with an Age value between 18 and 21, inclusive of those values. Note that you must specify the BETWEEN values in ascending order; a predicate such as BETWEEN 21 AND 18 would return no records.

Like most predicates, BETWEEN can be inverted using the NOT logical operator, as shown in the following example:

SELECT Name,Age FROM Sample.Person
WHERE Age NOT BETWEEN 20 AND 55
ORDER BY Age

 

This returns all the records in the Sample.Person table with an Age value less than 20 or greater than 55, exclusive of those values.

BETWEEN is commonly used for a range of numeric values, which collate in numeric order. However, BETWEEN can be used for a collation sequence range of values of any data type.

BETWEEN uses the same collation type as the column it is matching against. By default, string data types collate as not case-sensitive.

For further details, refer to the [BETWEEN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_between) predicate reference page in this manual.

IN and %INLIST Predicates

The [IN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_in) predicate is used for matching a value to an unstructured series of items. It has the following syntax:

WHERE field IN (item1,item2\[,...\])

[Collation](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_basics#GSQL_basics_collation) applies to the IN comparison as it applies to an equality test. IN uses the field’s default collation. By default, comparisons with field string values are not case-sensitive.

The [%INLIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_inlist) predicate is a Caché extension for matching a value to the elements of a Caché list structure. It has the following syntax:

WHERE item %INLIST listfield

%INLIST uses EXACT collation. Therefore, by default, %INLIST string comparisons are case-sensitive.

With either predicate you can perform equality comparisons and subquery comparisons.

For further details, refer to the [IN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_in) and [%INLIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_inlist) predicate reference pages in this manual.

Substring Predicates

You can use the following to compare a field value to a substring:

SQL Substring Predicates

Predicate

Operation

%STARTSWITH

The value must start with the specified substring.

\[

Contains operator. The value must contain the specified substring.

%CONTAINS

%CONTAINSTERM

The value must contain all of the specified substrings. Comparison is word-aware, using stemming and other algorithms.

%STARTSWITH Predicate

The Caché %STARTSWITH comparison operator permits you to perform partial matching on the initial characters of a string or numeric. The following example uses %STARTSWITH. to select those records in which the Name value begins with “S”:

SELECT Name,Age FROM Sample.Person
WHERE Name %STARTSWITH 'S'

 

Like other string field comparisons, %STARTSWITH comparisons use the field’s default collation. By default, string fields are not case-sensitive. For example:

SELECT Name,Home\_City,Home\_State FROM Sample.Person
WHERE Home\_City %STARTSWITH Home\_State

 

For further details, refer to the [%STARTSWITH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_startswith) predicate reference page in this manual.

Contains Operator (\[)

The Contains operator is the open bracket symbol: \[. It permits you to match a substring (string or numeric) to any part of a field value. The comparison is always case-sensitive. The following example uses the Contains operator to select those records in which the Name value contains a “S”:

SELECT Name, Age FROM Sample.Person
WHERE Name \[ 'S'

 

%CONTAINS and %CONTAINSTERM Predicates

The %CONTAINS and %CONTAINSTERM predicates compare a column value to one or more substrings. If multiple substrings are specified, the column value must contain all of the specified substrings.

These comparison predicates are far more sophisticated than the Contains operator. They permit text-aware searching, such as handling word stemming, multiple-word phrases, and indexing on only significant words.

To use %CONTAINS or %CONTAINSTERM, the column value operand must be of type %Text. To do this, you must use Caché Studio to change the property type from %String to %Text.

The %CONTAINS and %CONTAINSTERM predicates are Collection Predicates.

For further details, refer to the [%CONTAINS](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_contains) and [%CONTAINSTERM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_containsterm) reference pages in this manual.

NULL Predicate

This detects undefined values. You can detect all null values, or all non-null values. The NULL predicate has the following syntax:

WHERE field IS \[NOT\] NULL

NULL predicate conditions are one of the few predicates that can be used on stream fields in a WHERE clause.

For further details, refer to the [NULL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_null) predicate reference page in this manual.

EXISTS Predicate

This operates with subqueries to test whether a subquery evaluates to the empty set.

SELECT t1.disease FROM illness\_tab t1 WHERE EXISTS 
 (SELECT t2.disease FROM disease\_registry t2 
 WHERE t1.disease \= t2.disease 
 HAVING COUNT(t2.disease) \> 100) 

For further details, refer to the [EXISTS](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_exists) predicate reference page in this manual.

FOR SOME Predicate

The FOR SOME predicate of the WHERE clause can be used to determine whether or not to return any records based on a condition test of one or more field values. This predicate has the following syntax:

FOR SOME (table \[AS t-alias\]) (fieldcondition)

FOR SOME specifies that fieldcondition must evaluate to true; at least one of the field values must match the specified condition. table can be a single table or a comma-separated list of tables, and each table can optionally take a table alias. fieldcondition specifies one or more conditions for one or more fields within the specified table. Both the table argument and the fieldcondition argument must be delimited by parentheses.

The following example shows the use of the FOR SOME predicate to determine whether to return a result set:

SELECT Name,Age AS AgeWithWorkers
FROM Sample.Person
WHERE FOR SOME (Sample.Person) (Age<65)
ORDER BY Age

 

In the above example, if at least one field contains an Age value less than the specified age, all of the records are returned. Otherwise, no records are returned.

For further details, refer to the [FOR SOME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_forsome) predicate reference page in this manual.

FOR SOME %ELEMENT Predicate

The FOR SOME %ELEMENT predicate of the WHERE clause has the following syntax:

FOR SOME %ELEMENT(field) \[AS e-alias\] (predicate)

The FOR SOME %ELEMENT predicate matches the elements in field with the specified predicate clause value. The SOME keyword specifies that at least one of the elements in field must satisfy the specified predicate condition. The predicate can contain the %VALUE or %KEY keyword.

The FOR SOME %ELEMENT predicate is a Collection Predicate.

For further details, refer to the [FOR SOME %ELEMENT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_forsomeelement) predicate reference page in this manual.

LIKE, %MATCHES, and %PATTERN Predicates

These three predicates allow you to perform pattern matching.

*   [LIKE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_like) allows you to pattern match using literals and wildcards. Use LIKE when you wish to return data values that contain a known substring of literal characters, or contain several known substrings in a known sequence. LIKE uses the collation of its target for letter case comparisons.
    
*   [%MATCHES](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_matches) allows you to pattern match using literals, wildcards, and lists and ranges. Use %MATCHES when you wish to return data values that contain a known substring of literal characters, or contain one or more literal characters that fall within a list or range of possible characters, or contain several such substrings in a known sequence. %MATCHES uses EXACT collation for letter case comparisons.
    
*   [%PATTERN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_pattern) allows you to specify a pattern of character types. For example, '1U4L1",".A' (1 uppercase letter, 4 lowercase letters, one literal comma, followed by any number of letter characters of either case). Use %PATTERN when you wish to return data values that contain a known sequence of character types. %PATTERN can specify known literal characters, but is especially useful when the data value is unimportant, but the character type format of those values is significant.
    

To perform a comparison with the first characters of a string, use the [%STARTSWITH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_startswith) predicate.

Predicates and Logical Operators

Multiple predicates can be associated using the AND and OR logical operators. Multiple predicates can be grouped using parentheses. Because Caché optimizes execution of the WHERE clause using defined indices and other optimizations, the order of evaluation of predicates linked by AND and OR logical operators cannot be predicted. For this reason, the order in which you specify multiple predicates has little or no effect on performance. If strict left-to-right evaluation of predicates is desired, you can use a [CASE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_case) statement.

Note:

The OR logical operator cannot be used to associate a Collection Predicate that references a table field with a predicate that a references a field in a different table. For example,

WHERE FOR SOME %ELEMENT(t1.FavoriteColors) (%VALUE='purple') 
OR t2.Age < 65

The Collection Predicates are [FOR SOME %ELEMENT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_forsomeelement), [%CONTAINS](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_contains), and [%CONTAINSTERM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_containsterm). Because this restriction depends on how the optimizer uses indices, SQL may only enforce this restriction when indices are added to a table. It is strongly suggested that this type of logic be avoided in all queries.

For further details, refer to “[Logical Operators](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_langelements#GSQL_langelements_ops_logical)” in the “Language Elements” chapter of Using Caché SQL.

See Also

*   [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select) statement
    
*   [HAVING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_having) clause
    
*   [Overview of Predicates](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_predicates)
    
*   “[Querying the Database](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_queries)” chapter in Using Caché SQL
    
*   [SQLCODE error messages](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RERR_sql) listed in the Caché Error Reference
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_values "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_wherecurrentof "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:38**

Source: **RSQL\_where.xml**