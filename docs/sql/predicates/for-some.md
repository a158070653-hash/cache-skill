# FOR SOME

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_forsome

---

 

Caché SQL Reference

FOR SOME

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_find "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_forsomeelement "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Predicate Conditions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_PREDICATE_CONDITONS "SQL Predicate Conditions") \]  >  \[ [FOR SOME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_forsome "FOR SOME") \]

Go to: Description See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Determines whether to return a record based on a condition test of field values.

Synopsis

FOR SOME (table \[AS t-alias\]) (fieldcondition)

Arguments

table

table can be a single table or a comma-separated list of tables. The enclosing parentheses are mandatory.

AS t-alias

Optional — An alias for the preceding table name. An alias must be a valid [identifier](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_identifiers); it can be a delimited identifier. For further details see the “Identifiers” chapter of Using Caché SQL. The AS keyword is optional.

fieldcondition

fieldcondition specifies one or more condition expressions referencing one or more fields. Thefieldcondition is enclosed with parentheses. You can specify multiple condition expressions within fieldcondition using AND (&) and OR (!) logical operators.

Description

The FOR SOME predicate allows you to determine whether or not to return a record based on a boolean condition test of the values of one or more fields in a table. If fieldcondition evaluates as true, the record is returned. If fieldcondition evaluates as false, the record is not returned.

FOR SOME can be used wherever a [predicate condition](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_predicates) can be specified, as described in the [Overview of Predicates](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_predicates) page of this manual.

At Caché release 2015.1 and subsequent, delimiting parentheses are mandatory for the table (and its optional t-alias) argument. Delimiting parentheses are also mandatory for the fieldcondition argument. Whitespace is permitted, but not required, between these two sets of parentheses.

Commonly, FOR SOME is used to determine whether to return a record from a table based on the contents of a record in another table. FOR SOME can also be used to determine whether to return a record from a table based on the contents of a record in the same table. In this latter case, either all records are returned or no records are returned.

In the following example, FOR SOME returns all records in the Sample.Person table in which its Name field value matches the Name field value in the Sample.Employee table:

SELECT Name,COUNT(Name) AS NameCount
FROM Sample.Person AS p
WHERE FOR SOME (Sample.Employee AS e)(e.Name\=p.Name)
ORDER BY Name

 

In the following example, FOR SOME returns records in the Sample.Person table based on a boolean test of the same table. This program returns all Sample.Person records if at least one record has an Age value greater than 65. Otherwise, it returns no records. Because at least one record in Sample.Person has an Age field value greater than 65, all Sample.Person records are returned:

SELECT Name,Age,COUNT(Name) AS NameCount
FROM Sample.Person
WHERE FOR SOME (Sample.Person)(Age\>65)
ORDER BY Age

 

Like most predicates, FOR SOME can be inverted using the NOT logical operator, as shown in the following example:

SELECT Name,Age,COUNT(Name) AS NameCount
FROM Sample.Person
WHERE NOT FOR SOME (Sample.Person)(Age\>65)
ORDER BY Age

 

Compound Conditions

A fieldcondition can contain more than one condition expression. The set of conditions is enclosed in parentheses. Multiple conditions are specified with the logical operators AND and OR, which can also be specified using the & and ! symbols. A logical operator may be followed by the NOT unary operator. By default, conditions are evaluated in left-to-right order. You can specify a different order of evaluation by grouping multiple conditions using parentheses.

SELECT Name,COUNT(Name) AS NameCount
FROM Sample.Person AS p
WHERE FOR SOME (Sample.Employee AS e)(e.Name\=p.Name AND p.Name %STARTSWITH 'A')
ORDER BY Name

 

SELECT Name,COUNT(Name) AS NameCount
FROM Sample.Person AS p
WHERE FOR SOME (Sample.Employee AS e)(e.Name\=p.Name OR  p.Name %STARTSWITH 'A')
ORDER BY Name

 

In the following example, FOR SOME returns all records in the Sample.Person table in which its Name field value matches the Name field value in the Sample.Employee table, and their residence (Home\_State) is in the same state as their office (Office\_State):

SELECT Name,Home\_State,COUNT(Name) AS NameCount
FROM Sample.Person AS p
WHERE FOR SOME (Sample.Employee AS e)(p.Name\=e.Name AND p.Home\_State\=e.Office\_State)
ORDER BY Name

 

Multiple Tables

You can specify multiple tables as a comma-separated list before the fieldcondition. The condition that determines whether to return records may reference the table from which data is being selected, or may reference field values in another table. Table aliases are usually required to associate each specified field with its table.

In the following example, all records are returned if there is at least one Name in the Sample.Person table that is also found in the Sample.Employee table. Because this condition is true for at least one record, all Sample.Person records are returned:

SELECT Name AS PersonName,Age,COUNT(Name) AS NameCount
FROM Sample.Person
WHERE FOR SOME (Sample.Employee AS e,Sample.Person AS p) (e.Name\=p.Name)
ORDER BY Name

 

In the following example, all records are returned if there is at least one Name in the Sample.Person table that is also found in the Sample.Company table. Because names of persons and names of companies (in this data set) are never the same, this condition is not true for any record. Therefore, no Sample.Person records are returned:

SELECT Name AS PersonName,Age,COUNT(Name) AS NameCount
FROM Sample.Person
WHERE FOR SOME (Sample.Company AS c,Sample.Person AS p) (c.Name\=p.Name)
ORDER BY Name

 

See Also

*   [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select) statement, [HAVING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_having) clause, [WHERE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_where) clause
    
*   [Overview of Predicates](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_predicates)
    
*   [FOR SOME %ELEMENT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_forsomeelement) predicate
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_find "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_forsomeelement "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_forsome.xml**