# HAVING

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_having

---

 

Caché SQL Reference

HAVING

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_groupby "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_insert "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_COMMANDS "SQL Commands") \]  >  \[ [HAVING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_having "HAVING") \]

Go to: Description Logical Predicates Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A SELECT clause that specifies one or more restrictive conditions.

Synopsis

SELECT field
FROM table
GROUP BY field
HAVING condition-expression

SELECT aggregatefunc(field %AFTERHAVING)
FROM table
\[GROUP BY field\]
HAVING condition-expression

Arguments

condition-expression

An expression consisting of one or more boolean predicates governing which data values are to be retrieved.

Description

The optional HAVING clause appears after the FROM clause and the optional WHERE and GROUP BY clauses, and before the optional ORDER BY clause.

The HAVING clause of a [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select) statement qualifies or disqualifies specific rows from the query selection. The rows that qualify are those for which the condition-expression is true. The condition-expression is a series of logical tests (predicates) which can be linked by the AND and OR logical operators. For further details, see the [WHERE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_where) clause.

The HAVING clause is like a WHERE clause that can operate on groups, rather than on the full data set. Thus, in most cases, the HAVING clause is used either with an [aggregate function](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_aggregatefunctions) using the %AFTERHAVING keyword, or in combination with a [GROUP BY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_groupby) clause, or both.

A HAVING clause condition-expression can also specify an [aggregate function](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_aggregatefunctions). A WHERE clause condition-expression cannot specify an aggregate function. This is shown in the following example:

SELECT Name,Age,AVG(Age) AS AvgAge
FROM Sample.Person
HAVING Age \> AVG(Age)
ORDER BY Age

 

A HAVING clause often serves to compare aggregates of sub-populations against aggregates for an entire population.

Aggregate Functions in the select-item List

The HAVING clause selects which rows to return. By default, this row selection does not determine the value of aggregate functions in the select-item list. This is because the HAVING clause is parsed after aggregate functions in the select-item list.

In the following example, only those rows with Age > 65 are returned. But the AVG(Age) is calculated based on all rows, not just those selected by the HAVING clause:

SELECT Name,Age,AVG(Age) AS AvgAge FROM Sample.Person
HAVING Age \> 65
 ORDER BY Age

 

Compare this to a WHERE clause, which selects both which rows to return and which row values to include in aggregate functions in the select-item list:

SELECT Name,Age,AVG(Age) AS AvgAge FROM Sample.Person
WHERE Age \> 65
ORDER BY Age

 

A HAVING clause can be used in a query that only returns aggregate values. The HAVING clause value determines whether to return 1 row (containing the aggregate values) or 0 rows. Thus you can use a HAVING clause to only return an aggregate calculation when an aggregate threshold in the HAVING clause is achieved. For example, the following program only returns an average of the col values for all records when there are at least 100 records. If there are less than 100 records, the average of the col values for all records might not be deemed meaningful, and therefore should not be returned:

SELECT AVG(col) FROM mytable HAVING COUNT(\*)\>99

%AFTERHAVING

The %AFTERHAVING keyword can be used with an aggregate function in the select-item list to specify that the aggregate operation is to be performed after the HAVING clause condition is applied.

SELECT Name,Age,AVG(Age) AS AvgAge,
 AVG(Age %AFTERHAVING) AS AvgMiddleAge
 FROM Sample.Person
 HAVING Age \> 40 AND Age < 65
 ORDER BY Age

 

The %AFTERHAVING keyword only gives meaningful results if both of the following considerations are met:

*   The select-item list must contain at least one item that is a non-aggregate field reference. This field reference may be to any field in any table specified in the FROM clause, a field referenced using an implicit join (arrow syntax), the %ID alias, or an asterisk (\*).
    
*   The HAVING clause condition must apply at least one non-aggregate condition. Therefore, HAVING Age>50, HAVING Age>AVG(Age), or HAVING Age>50 AND MAX(Age)>75 are valid conditions, but HAVING Age>50 OR MAX(Age)>75 is not a valid condition.
    

The following example uses a HAVING clause with a GROUP BY clause to return the state average age, and the state average age for people that are older than the average age for all records in the table. It also uses a subquery to return the average age for all records in the table:

SELECT Home\_State,(SELECT AVG(Age) FROM Sample.Person) AS AvgAgeAllRecs,
       AVG(Age) AS AvgAgeByState,AVG(Age %AFTERHAVING) AS AvgOlderByState 
FROM Sample.Person
GROUP BY Home\_State
HAVING Age \> AVG(Age)
ORDER BY Home\_State

 

Logical Predicates

The SQL predicates fall into the following categories:

*   [Equality Comparison Predicates](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_having#RSQL_having_comparisonpredicates)
    
*   [BETWEEN Predicate](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_having#RSQL_having_betweenpredicate)
    
*   [IN and %INLIST Predicates](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_having#RSQL_having_inpredicate)
    
*   [%STARTSWITH Predicate](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_having#RSQL_having_startswithpredicate)
    
*   [Contains Operator (\[)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_having#RSQL_having_containsop)
    
*   [FOR SOME Predicate](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_having#RSQL_having_forsomepredicate)
    
*   [NULL Predicate](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_having#RSQL_having_nullpredicate)
    
*   [EXISTS Predicate](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_having#RSQL_having_existspredicate)
    
*   [LIKE, %MATCHES, and %PATTERN Predicates](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_having#RSQL_having_likepredicate)
    
*   [%INSET](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_inset) and [%FIND](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_find) Predicates
    

Note:

You cannot use the %CONTAINS or %CONTAINSTERM comparison predicate, or the FOR SOME %ELEMENT predicate in a HAVING clause. These comparison predicates can only be used in a WHERE clause.

Predicate Case-Sensitivity

Most predicates use the field’s default [collation](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_basics#GSQL_basics_collation) to determine the case sensitivity of letters in strings. By default, Caché string fields are not case-sensitive.

The %INLIST, Contains operator (\[), %MATCHES, and %PATTERN predicates do not use the field’s default collation. They always uses EXACT collation, which is case-sensitive.

A predicate comparison of two literal strings is always case-sensitive.

Predicate Conditions and %NOINDEX

You can preface a predicate condition with the %NOINDEX keyword to prevent the query optimizer using an index on that condition. This is most useful when specifying a range condition that is satisfied by the vast majority of the rows. For example, HAVING %NOINDEX Age >= 1. For further details, refer to [Index Optimization Options](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_optquery#GSQLOPT_optquery_from) in the Caché SQL Optimization Guide.

Equality Comparison Predicates

The following are the available comparison predicates:

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

The following example uses a comparison predicate. It returns one record for each Age less than 21:

SELECT Name, Age FROM Sample.Person
GROUP BY Age
HAVING Age < 21
ORDER BY Age

 

Note that SQL defines comparison operations in terms of collation: the order in which values are sorted. Two values are equal if they collate in exactly the same way. A value is greater than another value if it collates after the second value. String data type [field collation](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_basics#GSQL_basics_collation) is based on the field’s default collation. By default, it is not case-sensitive. Thus, a comparison of two string field values or a comparison of a string field value with a string literal is (by default) not case-sensitive. For example, if Home\_State field values are uppercase two-letter strings:

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

This is equivalent to a paired greater than or equal to and less than or equal to. The following example uses a BETWEEN predicate. It returns one record for each Age between 18 and 35, inclusive of 18 and 35:

SELECT Name, Age FROM Sample.Person
GROUP BY Age
HAVING Age BETWEEN 18 AND 35
ORDER BY Age

 

For further details, refer to the [BETWEEN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_between) reference page in this manual.

IN and %INLIST Predicates

The [IN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_in) predicate is used for matching a value to an unstructured series of items.

The [%INLIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_inlist) predicate is a Caché extension for matching a value to the elements of a list structure.

With either predicate you can perform equality comparisons and subquery comparisons.

IN has two formats. The first serves as shorthand for the use of multiple equality comparisons linked together with the OR operator. For instance:

SELECT Name, Home\_State FROM Sample.Person
GROUP BY Home\_State
HAVING Home\_State IN ('ME','NH','VT','MA','RI','CT')

 

evaluates true if Home\_State equals any of the values inside the parenthetical list. The list elements can be constants or expressions. [Collation](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_basics#GSQL_basics_collation) applies to the IN comparison as it applies to an equality test. By default, IN comparisons with field string values are not case-sensitive.

When dates or times are used for IN predicate equality comparisons, the appropriate data type conversions are automatically performed. If the HAVING clause field is type TimeStamp, values of type Date or Time are converted to Timestamp. If the HAVING clause field is type Date, values of type TimeStamp or String are converted to Date. If the HAVING clause field is type Time, values of type TimeStamp or String are converted to Time.

The following examples both perform the same equality comparisons and return the same data. The GROUP BY field specifies to return only one record for each successful equality comparison. The DOB field is of data type Date:

SELECT Name,DOB FROM Sample.Person 
GROUP BY DOB
HAVING DOB IN ({d '1951-02-02'},{d '1987-02-28'})

 

SELECT Name,DOB FROM Sample.Person
GROUP BY DOB
HAVING DOB IN ({ts '1951-02-02 02:37:00'},{ts '1987-02-28 16:58:10'})

 

For further details refer to [Date and Time Constructs](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dateconstruct).

The %INLIST predicate can be used to perform an equality comparison on the elements of a list structure. %INLIST uses EXACT collation. Therefore, by default, %INLIST string comparisons are case-sensitive. For further details on list structures, see the SQL [$LIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_d_list) function.

The following example uses %INLIST to match a string value to the elements of the FavoriteColors list field:

SELECT Name,FavoriteColors FROM Sample.Person 
HAVING 'Red' %INLIST FavoriteColors

 

It returns all records where FavoriteColors includes the element “Red”.

The following embedded SQL example matches Home\_State column values to the elements of the northne (northern New England states) list:

   SET northne\=$LISTBUILD("VT","NH","ME")
   &sql(DECLARE StateCursor CURSOR FOR 
        SELECT Name,Home\_State
        INTO :name,:state FROM Sample.Person
        HAVING Home\_State %INLIST :northne)
   &sql(OPEN StateCursor)
   NEW SQLCODE,%ROWCOUNT,%ROWID
   FOR { &sql(FETCH StateCursor)
        QUIT:SQLCODE  
        WRITE !,"#",%ROWCOUNT," Name=",name," State=",state,!
 }
   WRITE !,"Final Fetch SQLCODE: ",SQLCODE
   &sql(CLOSE StateCursor)

 

You can also use IN or %INLIST with a subquery to test whether a column value (or any other expression) equals any of the subquery row values. For example:

SELECT Name,Home\_State FROM Sample.Person
HAVING Name IN 
 (SELECT Name FROM Sample.Employee
 HAVING Salary < 50000)

 

Note that the subquery must have exactly one item in the SELECT list.

For further details, refer to the [IN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_in) and [%INLIST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_inlist) reference pages in this manual.

%STARTSWITH Predicate

The Caché %STARTSWITH comparison operator permits you to perform partial matching on the initial characters of a string or numeric. The following example uses %STARTSWITH. It selects by age, then returns a record for each Name that begins with “S”:

SELECT Name,Age FROM Sample.Person
WHERE Age \> 30
HAVING Name %STARTSWITH 'S'
ORDER BY Name

 

Like other string field comparisons, %STARTSWITH comparisons are not case-sensitive. For further details, refer to the [%STARTSWITH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_startswith) reference page in this manual.

Contains Operator (\[)

The Contains operator is the open bracket symbol: \[. It permits you to match a substring (string or numeric) to any part of a field value. The comparison is always case-sensitive. The following example uses the Contains operator in a HAVING clause to select those records in which the Home\_State value contains a “K”, and then do an %AFTERHAVING count on those states:

SELECT Home\_State,COUNT(Home\_State) AS States,
   COUNT(Home\_State %AFTERHAVING) AS KStates
 FROM Sample.Person
 HAVING Home\_State \[ 'K'

 

FOR SOME Predicate

The FOR SOME predicate of the HAVING clause determines whether or not to return a result set based on a condition test of one or more field values. This predicate has the following syntax:

FOR SOME (table\[AS t-alias\]) (fieldcondition)

FOR SOME specifies that fieldcondition must evaluate to true; at least one of the field values must match the specified condition. table can be a single table or a comma-separated list of tables, and can optionally take a table alias. fieldcondition specifies one or more conditions for one or more fields within the specified table. Both the table argument and the fieldcondition argument must be delimited by parentheses.

The following example shows the use of the FOR SOME predicate:

SELECT Name,Age
FROM Sample.Person
HAVING FOR SOME (Sample.Person)(Age\>20)
ORDER BY Age

 

In the above example, if at least one field contains an Age value greater than 20, all of the records are returned. Otherwise, no records are returned.

For further details, refer to the [FOR SOME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_forsome) reference page in this manual.

NULL Predicate

This detects undefined values. You can detect all null values, or all non-null values:

SELECT Name, FavoriteColors FROM Sample.Person
HAVING FavoriteColors IS NULL 

 

SELECT Name, FavoriteColors FROM Sample.Person
HAVING FavoriteColors IS NOT NULL 
ORDER BY FavoriteColors

 

Using the GROUP BY clause, you can return one record for each non-null value for a specified field:

SELECT Name, FavoriteColors FROM Sample.Person
GROUP BY FavoriteColors
HAVING FavoriteColors IS NOT NULL 
ORDER BY FavoriteColors

 

For further details, refer to the [NULL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_null) reference page in this manual.

EXISTS Predicate

This operates with subqueries to test whether a subquery evaluates to the empty set.

SELECT t1.disease FROM illness\_tab t1 WHERE EXISTS 
 (SELECT t2.disease FROM disease\_registry t2 
 WHERE t1.disease \= t2.disease 
 HAVING COUNT(t2.disease) \> 100) 

For further details, refer to the [EXISTS](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_exists) reference page in this manual.

LIKE, %MATCHES, and %PATTERN Predicates

These three predicates allow you to perform pattern matching.

*   [LIKE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_like) allows you to pattern match using literals and wildcards. Use LIKE when you wish to return data values that contain a known substring of literal characters, or contain several known substrings in a known sequence. LIKE uses the collation of its target for letter case comparisons.
    
*   [%MATCHES](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_matches) allows you to pattern match using literals, wildcards, and lists and ranges. Use %MATCHES when you wish to return data values that contain a known substring of literal characters, or contain one or more literal characters that fall within a list or range of possible characters, or contain several such substrings in a known sequence. %MATCHES uses EXACT collation for letter case comparisons.
    
*   [%PATTERN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_pattern) allows you to specify a pattern of character types. For example, '1U4L1",".A' (1 uppercase letter, 4 lowercase letters, one literal comma, followed by any number of letter characters of either case). Use %PATTERN when you wish to return data values that contain a known sequence of character types. %PATTERN is especially useful when the data value is unimportant, but the character type format of those values is significant. %PATTERN can also specify known literal characters. It uses EXACT collation for literal comparisons, which are always case-sensitive.
    

To perform a comparison with the first characters of a string, use the [%STARTSWITH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_startswith) predicate.

Examples

The following example returns a row for each state that has at least one person under the age of 21. For each row it returns the average, minimum, and maximum ages of all people in the state.

SELECT Home\_State, MIN(Age) AS Youngest,
  AVG(Age) AS AvgAge, MAX(Age) AS Oldest
 FROM Sample.Person
 GROUP BY Home\_State
 HAVING Age < 21
 ORDER BY Youngest

 

The following example returns a row for each state that has at least one person under the age of 21. For each row it returns the average, minimum, and maximum ages of all people in the state. Using the %AFTERHAVING keyword, it also returns the average age of those people in the state under the age of 21 (AvgYouth), and the age of the oldest person in the state under the age of 21 (OldestYouth).

SELECT Home\_State,AVG(Age) AS AvgAge,
   AVG(Age %AFTERHAVING) AS AvgYouth,
   MIN(Age) AS Youngest, MAX(Age) AS Oldest,
   MAX(Age %AFTERHAVING) AS OldestYouth
 FROM Sample.Person
 GROUP BY Home\_State
 HAVING Age < 21
 ORDER BY AvgAge

 

For further examples of %AFTERHAVING, refer to the individual [aggregate functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_aggregatefunctions).

See Also

*   [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select) statement
    
*   [WHERE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_where) clause
    
*   [GROUP BY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_groupby) clause
    
*   [Overview of Predicates](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_predicates)
    
*   “[Querying the Database](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_queries)” chapter in Using Caché SQL
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_groupby "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_insert "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_having.xml**