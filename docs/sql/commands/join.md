# JOIN

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_join

---

 

Caché SQL Reference

JOIN

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_intransaction "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_lock "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_COMMANDS "SQL Commands") \]  >  \[ [JOIN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_join "JOIN") \]

Go to: Description One-Way Outer Joins Mixing Outer and Inner Joins Performance with Multiple Joins and Implicit Joins Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A SELECT subclause that creates a table based on the data in two tables.

Synopsis

table1 \[\[AS\] t-alias\] CROSS JOIN table2 \[\[AS\] t-alias\] |

table1 \[\[AS\] t-alias\] , table2 \[\[AS\] t-alias\]

table1 \[\[AS\] t-alias\]
NATURAL \[INNER\] JOIN |
NATURAL LEFT \[OUTER\] JOIN |
NATURAL RIGHT \[OUTER\] JOIN |
table2 \[\[AS\] t-alias\] 

table1 \[\[AS\] t-alias\]
\[INNER\] JOIN |
LEFT \[OUTER\] JOIN |
RIGHT \[OUTER\] JOIN |
FULL \[OUTER\] JOIN
table2 \[\[AS\] t-alias\] 
ON condition-expression

table1 \[\[AS\] t-alias\]
\[INNER\] JOIN |
LEFT \[OUTER\] JOIN |
RIGHT \[OUTER\] JOIN |
table2 \[\[AS\] t-alias\] 
USING (identifier-commalist)

(The above join syntax is used in the SELECT statement FROM clause. Other symbolic join syntax can be used in other SELECT statement clauses.)

Description

A join is an operation that combines two tables to produce a joined table, optionally subject to one or more restrictive conditions. Every row of the new table must satisfy the restrictive condition(s). Joins provide the means of linking data in one table with data in another table and are frequently used in defining reports and queries.

There are several syntactical forms for representing joins. The preferred form is specifying an explicit join expression in a SELECT statement as part of the [FROM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_from) clause. A FROM clause join expression can contain multiple joins.

Caché uses complex optimization algorithms to maximize performance of multiple join operations. In most cases, this default optimization strategy provides optimal results. However, Caché also provides join optimization keywords such as %INORDER and %FULL that you can use immediately after the FROM keyword to specify the optimization strategy used for multiple joins. For a description of these optimization keywords, refer to the [FROM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_from) clause.

Note:

Caché SQL also supports implicit joins using arrow syntax (–>) in the SELECT statement select-item list, WHERE clause, ORDER BY clause, and elsewhere. An implicit join is specified to perform a left outer join of a table with a field from another table; an explicit join is specified to join two tables. This implicit join syntax can be a useful substitute for explicit join syntax, or appear in the same query with explicit join syntax. There are, however some important restrictions on combining arrow syntax with explicit join syntax. These restrictions are described below. For information on using arrow syntax, refer to [Implicit Joins](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_implicitjoins) in Using Caché SQL.

JOIN Definitions

Caché supports many different syntactical forms of JOIN. However, these many formulations refer to the following five types of joins.

ANSI Join Syntax

Syntactical Equivalents

CROSS JOIN

Same as symbolic representation: table1,table2 (a list of tables separated by commas) in the FROM clause.

INNER JOIN

Same as JOIN. Symbolic representation: "=" (in a WHERE clause).

LEFT OUTER JOIN

Same as LEFT JOIN. The symbolic representation: "=\*" (in a WHERE clause) has been deprecated and should not be used in new code. [Arrow syntax](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_implicitjoins) (->) also performs a left outer join.

RIGHT OUTER JOIN

Same as RIGHT JOIN. The symbolic representation: "\*=" (in a WHERE clause) has been deprecated and should not be used in new code.

FULL OUTER JOIN

Same as FULL JOIN.

Unless otherwise indicated, all join syntax is specified in the FROM clause.

*   A CROSS JOIN is a join that crosses every row of the first table with every row of the second table. This results in a Cartesian product, a large, logically comprehensive table with much data duplication. Usually this join is performed by providing a comma-separated list of tables in the FROM clause, then using the WHERE clause to specify restrictive conditions. The %INORDER or %STARTTABLE optimization keyword cannot be used with a cross join. Attempting to do so results in an SQLCODE -34 error. For further details on join optimization keywords, refer to the [FROM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_from) clause.
    
*   An INNER JOIN is a join that links rows of the first table with rows of a second table, excluding any row in the first table that finds no corresponding row in the second table. This results in a small table in which every field contains unique data.
    
*   A LEFT OUTER JOIN and a RIGHT OUTER JOIN are in most respects functionally identical (with reversed syntax), and thus are frequently referred to collectively as one-way outer joins. A one-way outer join is a join that links rows of the first (source) table with rows of a second table, including all rows from the first table even if there is no match in the second table. This results in a table in which some fields of the first (source) table may be paired with NULL data.
    
    When specifying a one-way outer join, the order in which you name the tables in the FROM clause is very important. For a LEFT OUTER JOIN, the first table you specify is the source table for the join. For a RIGHT OUTER JOIN the second table you specify is the source table for the join.
    
*   A FULL OUTER JOIN is a join that combines the results of performing both a LEFT OUTER JOIN and a RIGHT OUTER JOIN on the two tables. It includes all rows found in either the first table or the second table, and fills in NULLs for missing matches on either side.
    

CROSS JOIN Considerations

The explicit use of the JOIN keyword has higher precedence than specifying a cross join using comma syntax. Caché thus interprets t1,t2 JOIN t3 as t1,(t2 JOIN t3). Earlier versions of Caché did not support this syntax precedence; join syntax was parsed in left-to-right order, so that t1,t2 JOIN t3 was interpreted as (t1,t2) JOIN t3. To maintain left-to-right parsing, this join must be re-specified as t1 CROSS JOIN t2 JOIN t3.

You cannot perform a cross join involving a local table and an ODBC table linked through a gateway connection. For example, FROM Sample.Person,Mylink.Person. Attempting to do so results in SQLCODE -161: “References to an SQL connection must constitute a whole subquery”. To perform this cross join you must specify the linked table as a subquery. For example, FROM Sample.Person,(SELECT \* FROM Mylink.Person).

NATURAL Joins

A NATURAL JOIN is an INNER JOIN, LEFT OUTER JOIN, or RIGHT OUTER JOIN prefixed with the NATURAL keyword. Prefixing a join with the word NATURAL specifies that you are joining on all the columns of the two tables that have the same name. Because a NATURAL join automatically performs an equality condition on all columns having the same name, it is not possible to specify an ON clause or a USING clause. Attempting to do so results in an SQLCODE -25 error.

Only simple base table references (not views or subqueries) are supported for either operand of a NATURAL join.

A NATURAL join can only be specified as the first join within a join expression.

A NATURAL join does not merge columns with the same name.

A FULL JOIN cannot be prefixed with the NATURAL keyword. Attempting to do so results in an SQLCODE -94 error.

ON Clause

An INNER JOIN, LEFT OUTER JOIN, RIGHT OUTER JOIN, or FULL OUTER JOIN may have an ON clause. An ON clause contains one or more condition expressions used to limit the values returned by the join operation. A join with an ON clause can be specified anywhere within a join expression. A join with an ON clause can specify tables, views, or subqueries for either operand of the join.

The ON clause consists of one or more condition expression predicates. These include most of the [predicates](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_predicates) supported by Caché SQL. However, you cannot use a collection predicate to limit a join operation; the Caché SQL collection predicates are FOR SOME %ELEMENT, %CONTAINS, and %CONTAINSTERM.

You can associate multiple condition expressions using AND, OR, and NOT logical operators. AND takes precedence over OR. Parentheses can be used to nest and group condition expressions. Unless grouped by parentheses, predicates using the same logical operator are executed in strict left-to-right order.

An ON clause has the following restrictions:

*   A join with an ON clause can only use ANSI join keyword syntax.
    
*   A join with an ON clause cannot take the NATURAL keyword prefix. This results in an SQLCODE -25 error.
    
*   A join with an ON clause cannot take a USING clause. This results in an SQLCODE -25 error.
    
*   An ON clause cannot include =\* or \*= symbolic syntax. This results in an SQLCODE -68 error.
    
*   An ON clause cannot include arrow syntax (–>). This results in an SQLCODE -67 error. For a description of arrow syntax, refer to [Implicit Joins](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_implicitjoins) in Using Caché SQL.
    
*   An ON clause can only reference tables explicitly specified in the ANSI keyword JOIN operation. Other tables specified in the FROM clause may not be referenced in the ON clause. This results in an SQLCODE -23 error.
    
*   An ON clause can only reference columns that are in the operands of the JOIN. Syntax precedence in multiple joins can cause the ON clause to fail. For example, the query SELECT \* FROM t1,t2 JOIN t3 ON t1.p1=t3.p3 fails because t1 and t3 are not operands of a join; t1 joins with the result set of t2 JOIN t3. Either of the following changes in syntax result in the successful execution of this query: SELECT \* FROM t1 CROSS JOIN t2 JOIN t3 ON t1.p1=t3.p3 or SELECT \* FROM t2,t1 JOIN t3 ON t1.p1=t3.p3.
    
*   OUTER JOIN with an ON clause restriction. If all the conditions affecting a table use comparisons that may pass null values, and that table is itself a target of an outer join, this can result in an SQLCODE -94 error: Unsupported usage of OUTER JOIN. The following is a LEFT OUTER JOIN example of this type of invalid join:
    
    SELECT \*
    FROM Table1
       LEFT JOIN Table2 ON Table1.k \= Table2.k
       LEFT JOIN Table3 ON COALESCE(Table1.k,Table2.k) \= Table3.k
    
    Similar examples using FULL OUTER JOIN or RIGHT OUTER JOIN also have this restriction.
    

For optimal performance, fields referenced in an ON clause should (in most cases) have an associated index. The collation type of a field referenced in an ON clause should match the collation type that it has in the corresponding index. A collation type mismatch can cause an index to not be used. For further details on collation type matching, refer to [Index Collation](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_indices#GSQLOPT_indices_collation) in the “Defining and Building Indices” chapter of Caché SQL Optimization Guide. In very specific situations you may wish to prevent the use of an index for an ON clause condition by prefacing it with the %NOINDEX keyword. For further details on indices and performance, refer to the [Index Analyzer](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_optquery#GSQLOPT_optquery_ind) and [Index Optimization Options](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_optquery#GSQLOPT_optquery_from) in the Caché SQL Optimization Guide.

USING Clause

An INNER JOIN, LEFT OUTER JOIN, or RIGHT OUTER JOIN may have a USING clause. Only simple base table references (not views or subqueries) are supported for either operand of a join with a USING clause. A join with a USING clause can only be specified as the first join within a join expression. A join with a USING clause cannot take the NATURAL keyword prefix, or an ON clause.

A USING clause lists one or more column names, separated by commas and enclosed within parentheses. The parentheses are required. Duplicate column names are ignored. A USING clause does not merge columns with the same name.

A USING clause is a brief way to represent the equality conditions expressed in an ON clause. Thus: t1 INNER JOIN t2 USING (a,b) is equivalent to t1 INNER JOIN t2 ON t1.a=t2.a AND t1.b=t2.b

One-Way Outer Joins

Caché supports one-way outer joins: LEFT OUTER JOIN and RIGHT OUTER JOIN.

With standard "inner" joins, when rows of one table are linked with rows of a second table, a row in the first table that finds no corresponding row in the second table is excluded from the output table.

With one-way outer joins, all rows from the first table are included in the output table even if there is no match in the second table. With one-way outer joins, the first table pulls relevant information out of the second table but never sacrifices its own rows for lack of a match in the second table.

For example, if a query lists Table1 first and creates a left outer join, then it should be able to see all the rows in Table1 even if they don't have corresponding records in Table2.

When specifying a one-way outer join, the order in which you name the tables in the FROM clause is very important. For a left outer join, the first table you specify is the source table for the join. For a right outer join the second table you specify is the source table for the join. For this reason, the %INORDER or %STARTTABLE optimization keyword cannot be used with a right outer join. The following syntax is contradictory and results in an SQLCODE -34 error: FROM %INORDER table1 RIGHT OUTER JOIN table2 ON.... For further details on join optimization keywords, refer to the [FROM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_from) clause.

Outer Join Syntax

Caché supports three formats for representing outer joins:

1.  The ANSI standard syntax: LEFT OUTER JOIN and RIGHT OUTER JOIN. SQL Standard syntax puts the outer join in the FROM clause of the SELECT statement, rather than the WHERE clause, as shown in the following example:
    
    FROM tbl1 LEFT OUTER JOIN tbl2 ON (tbl1.key = tbl2.key) 
    
2.  The ODBC Specification outer join extension syntax, using the escape-syntax {oj join-expression }, where join-expression is any ANSI standard join syntax.
    
3.  Symbolic outer join extension syntax, using a condition such as A=\*B in the WHERE clause. A left outer join is specified using the symbol =\* in place of = in the WHERE clause. A right outer join is specified using the symbol \*= in place of = in the WHERE clause. (Note that this is the reverse of the syntax used by Microsoft SQL Server and Sybase.)
    
    Note:
    
    Use of symbolic outer join syntax (=\* and \*=) is strongly discouraged, and cannot be used with an ON clause. Use ANSI standard syntax: LEFT OUTER JOIN and RIGHT OUTER JOIN. Further restrictions on symbolic outer join syntax are listed below.
    

While the three outer join formats are interchangeable and can be mixed, we strongly recommend the use of ANSI standard syntax whenever possible, as it is the only one compatible with ODBC (and portable to the latest Microsoft products). Additionally, ANSI standard syntax can specify many operations not specifiable in principle with symbolic syntax. Finally, InterSystems has no intention of supporting new features, enhanced validation, and optimizer improvements for the older, symbolic outer join syntax.

Symbolic Syntax (\*=, =\*) Outer Join Restrictions

For a symbolic syntax outer join in the WHERE clause, the following operand values may be used:

*   A column of a base table. No restrictions apply to specifying a base table column on either side of the outer join. However, you cannot specify an expression using a base table column (such as substring(field1,4,3)) on either side of a symbolic syntax outer join. Expressions are permitted with ANSI standard syntax.
    
*   A column of a streamed view or subquery. A streamed view is a view that contains a DISTINCT, GROUP BY, or UNION keyword, or that has columns generated using aggregate functions. No restrictions apply to specifying a streamed view column on either side of the outer join.
    
*   A column of a view or subquery that isn't streamed must expand to a base table column if it is used on the left side of a left outer join (=\*) or the right side of a right outer join (\*=).
    
*   For a left outer join (=\*), the right operand should not contain [arrow syntax](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_implicitjoins). A construction such as WHERE a =\* b->c is not supported. Arrow syntax is supported for the left operand.
    
*   For a right outer join (\*=), the left operand should not contain [arrow syntax](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_implicitjoins). A construction such as WHERE a->b \*= c is not supported. Arrow syntax is supported for the right operand.
    
*   When using symbolic syntax, multiple condition expressions may only be joined with the AND logical operator. The OR and NOT logical operators may not be used.
    

The following is a valid query using symbolic syntax for a left outer join:

SELECT a.Name,a.Age,b.Name
FROM Sample.Person AS a,Sample.Employee AS b 
WHERE a.Name \=\* b.Name AND a.Name %STARTSWITH 'G'

 

If a query contains both a FROM clause containing an ANSI standard outer join and a WHERE clause containing a symbolic syntax outer join, and these two joins are in conflict, Caché performs an inner join. Using both ANSI standard syntax and symbolic syntax in the same query is strongly discouraged.

Null Padding

A one-way outer join performs null padding. This mean that if a row of the source table has a NULL value for the merged column, a null value is returned for the corresponding field from the non-source table.

The left outer join condition is expressed by the following syntax:

A LEFT OUTER JOIN B ON A.x=B.y

This specifies that every row in A is returned. For each A row returned, if there is a B row such that A.x=B.y, all of the corresponding B values are also returned.

If there is no B row such that A.x=B.y, null padding causes all B values for that A row to return as null.

For example, consider the Patient table that contains information about patients, including a field Patient.DocID specifying and ID code for the patient’s primary doctor. Some patients in the database do not have a primary doctor, so for those patient records the Patient.DocID field is NULL. Now, we perform a join between the Patient table and the Doctor table to generate a table of patient names and corresponding doctor names.

The following example is an INNER JOIN.

SELECT Patient.PName,Doctor.DName
   FROM Patient INNER JOIN Doctor
   ON Patient.DocID\=Doctor.DocID

An INNER JOIN does not perform null padding. Therefore, no patient name without a corresponding doctor name is returned.

A one-way outer join does perform null padding. Therefore, a patient name without a corresponding doctor name returns a NULL for Doctor.DName.

SELECT Patient.PName,Doctor.DName
   FROM Patient LEFT OUTER JOIN Doctor
   ON Patient.DocID\=Doctor.DocID

Order of Operations

One-way outer join conditions, including the necessary null padding, are applied before other conditions. Therefore, a condition in the WHERE clause that cannot be satisfied by a null-padded value (for example, a range or equality condition on a field in B) effectively converts the one-way outer join of A and B into a regular join (an inner join).

For example, if you add the clause "WHERE Doctor.Age < 45" to the two "Patient" table queries above, it makes them equivalent. However, if you add the clause "WHERE Doctor.Age < 45 OR Doctor.Age IS NULL", it preserves the difference between the two queries.

Mixing Outer and Inner Joins

Caché supports all syntax of mixed inner joins and outer joins in any order.

Performance with Multiple Joins and Implicit Joins

By default, the query optimizer sequences multiple join operations in its best estimation of the optimal sequence. This is not necessarily the join sequence order that you specified in the query. You can specify the [%INORDER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_from#RSQL_from_inorder), [%FIRSTTABLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_from#RSQL_from_firsttable), or [%STARTTABLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_from#RSQL_from_starttable) query optimization option in the FROM clause to explicitly specify the order in which the tables are joined.

The query optimizer may perform subquery flattening, converting certain subqueries to explicit joins. This substantially improves join performance when the number of subqueries is small. When the number of subqueries is more than one or two, subquery flattening may, in some cases, actually slightly degrade performance. You can specify the [%NOFLATTEN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_from#RSQL_from_firsttable) query optimization option in the FROM clause to explicitly specify that subquery flattening should not be performed.

The query optimizer only performs subquery flattening when the total number of joins in a query, after subquery flattening, would not exceed 15 joins. Specifying more than 15 joins, when some of those joins are [implicit joins](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_implicitjoins) or joined subqueries, can result is a significant degradation in query performance.

Examples

The following examples display the results of the JOIN operations performed on Table1 and Table2.

     Table1                Table2
Column1  Column2      Column1  Column3
  aaa      bbb          ggg      hhh
  ccc      ccc          xxx      zzz
  xxx      yyy
  hhh      zzz

CROSS JOIN Example

The statement:

SELECT \* FROM Table1 CROSS JOIN Table2

yields the table:

Column1  Column2  Column1  Column3
  aaa      bbb      ggg      hhh
  aaa      bbb      xxx      zzz
  ccc      ccc      ggg      hhh
  ccc      ccc      xxx      zzz
  xxx      yyy      ggg      hhh
  xxx      yyy      xxx      zzz
  hhh      zzz      ggg      hhh
  hhh      zzz      xxx      zzz

NATURAL JOIN Example

The statement:

SELECT \* FROM Table1 NATURAL JOIN Table2

yields the table

Column1  Column2  Column1  Column3
  xxx      yyy      xxx      zzz

Note that the Caché implementation of NATURAL JOIN does not merge columns with the same name.

INNER JOIN with an ON Clause Example

The statement:

SELECT \* FROM Table1 INNER JOIN Table2
     ON Table1.Column1\=Table2.Column3

yields the table:

Column1  Column2  Column1  Column3
  hhh      zzz      ggg      hhh

INNER JOIN with a USING Clause Example

The statement:

SELECT \* FROM Table1 INNER JOIN Table2
  USING (Column1)

yields the table:

Column1  Column2  Column1  Column3
  xxx      yyy      xxx      zzz 

Note that the Caché implementation of a USING clause does not merge columns with the same name.

LEFT OUTER JOIN Example

The statement:

SELECT \* FROM Table1 LEFT OUTER JOIN Table2
  ON Table1.Column1\=Table2.Column3

yields the table:

Column1  Column2  Column1  Column3 
  aaa      bbb      null     null
  ccc      ccc      null     null
  xxx      yyy      null     null
  hhh      zzz      ggg      hhh

RIGHT OUTER JOIN Example

The statement:

SELECT \* FROM Table1 RIGHT OUTER JOIN Table2
     ON Table1.Column1\=Table2.Column3

yields the table:

Column1  Column2  Column1  Column3
  hhh      zzz      ggg      hhh
  null     null     xxx      zzz

FULL OUTER JOIN Example

The statement:

SELECT \* FROM Table1 FULL OUTER JOIN Table2
  ON Table1.Column1\=Table2.Column3

yields the table:

Column1  Column2  Column1  Column3 
  aaa      bbb      null     null
  ccc      ccc      null     null
  xxx      yyy      null     null
  hhh      zzz      ggg      hhh
  null     null     xxx      zzz

See Also

*   [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select) statement, [FROM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_from) clause, [ORDER BY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_order) clause, [WHERE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_where) clause
    
*   [ALTER TABLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_altertable), [CREATE TABLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable), [DROP TABLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_droptable)
    
*   [INSERT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_insert), [UPDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_update)
    
*   “[Defining Tables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_tables)” chapter in Using Caché SQL
    
*   “[Querying the Database](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_queries)” chapter in Using Caché SQL
    
*   [SQLCODE error messages](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RERR_sql) listed in the Caché Error Reference
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_intransaction "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_lock "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_join.xml**