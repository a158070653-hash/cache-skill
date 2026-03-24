# FROM

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_from

---

 

Caché SQL Reference

FROM

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_fetch "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_grant "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_COMMANDS "SQL Commands") \]  >  \[ [FROM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_from "FROM") \]

Go to: Description Query Optimization Options Table-Valued Functions in the FROM Clause Subqueries in the FROM Clause Optional FROM Clause See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

A SELECT clause that specifies one or more tables to query.

Synopsis

SELECT ... FROM \[optimize-option\] table-ref \[\[AS\] t-alias\]\[,table-ref \[\[AS\] t-alias\]\]\[,...\]

Arguments

optimize-option

Optional — A single keyword, or a series of keywords separated by spaces, that specify query optimization options (optimizer hints). The following keywords are supported: [%ALLINDEX](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_from#RSQL_from_allindex), [%FIRSTTABLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_from#RSQL_from_firsttable) tablename, [%FULL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_from#RSQL_from_full), [%IGNOREINDEX](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_from#RSQL_from_ignoreindex) name, [%INORDER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_from#RSQL_from_inorder), [%NOFLATTEN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_from#RSQL_from_noflatten), [%NOMERGE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_from#RSQL_from_nomerge), [%NOREDUCE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_from#RSQL_from_noreduce), [%NOSVSO](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_from#RSQL_from_nosvso), [%NOTOPOPT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_from#RSQL_from_notopopt), [%NOUNIONOROPT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_from#RSQL_from_nounionoropt), [%PARALLEL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_from#RSQL_from_parallel), and [%STARTTABLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_from#RSQL_from_starttable).

table-ref

One or more [tables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_tables), [views](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_views), [table-valued functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_from#RSQL_from_tvf), or [subqueries](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_from#RSQL_from_subq) from which data is being retrieved, specified as a comma-separated list or with JOIN syntax. Some restrictions apply on using views with JOIN syntax. You can specify a subquery, enclosed in parentheses.

AS t-alias

Optional — An alias for the table name. Must be a valid [identifier](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_identifiers). For further details see the “Identifiers” chapter of Using Caché SQL. Can be specified with or without the optional AS keyword.

Description

The FROM clause specifies one or more tables (or views, or subqueries) from which data is queried within a [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select) statement. If no table data is being queried, the FROM clause is optional, as described below.

Multiple tables are specified as a comma-separated list, or a list separated by other JOIN syntax. Each table name can optionally be supplied an alias.

Table name aliases are used when specifying field names for multiple tables in the SELECT statement. If two (or more) tables are specified in the FROM clause, you indicate which table’s field you want by specifying tablename.fieldname for each field in the SELECT select-item clause. Because table names are often long names, a short table name alias is useful in this context (t-alias.fieldname).

The following example show the use of table name aliases:

SELECT e.Name,c.Name
FROM Sample.Company AS c,Sample.Employee AS e

 

The AS keyword can be omitted. It is provided for compatibility and clarity.

Supplying a Schema Name to a Table Reference

A table-ref name is either qualified (schema.tablename) or unqualified (tablename). An unqualified table name is supplied either the [system-wide default schema name](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_tables#GSQL_tables_schema), or (if provided) a schema name from the schema search path:

*   In [Embedded SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql) you can use the [#SQLCompile Path](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_macros#GCOS_macros_mpp_lbSQLCompile_Path) macro directive to supply the schema for a unqualified table name. The following example provides a search path containing two schema names:
    
    #SQLCompile Path\=Cinema,Sample
    
    For further details, refer to “ObjectScript Macros and the Macro Preprocessor” in Using Caché ObjectScript.
    
*   In [Dynamic SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_dynsql) you can use the [%SQL.Statement](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SQL.Statement) %New method to supply a schema search path that Caché uses to resolve unqualified table names. The following example provides a search path containing two schema names:
    
      SET tStatement \= ##class(%SQL.Statement).%New(0,"Cinema","Sample")
    
    For further details, refer to “Using Dynamic SQL” in Using Caché SQL.
    

Multiple Table Names in the FROM Clause

When you specify multiple table names in a FROM clause, Caché SQL performs join operations on those tables. The type of join performed is specified by a join keyword phrase or symbol between each pair of table names. When two table names are separated by a comma, a cross join is performed. For further details on the different types of joins and their syntax, refer to [JOIN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_join).

The sequence in which joins are performed is automatically determined by the SQL query optimizer and is not based on the sequence that the tables are listed in the query. If desired, you can control the sequence in which joins are performed by specifying a query optimization option.

The following three SELECT statements show the row counts for two individual tables, and the row count for a SELECT specifying both tables. This latter results in a much larger table, a Cartesian product, where every row in the first table is matched with every row of the second table, an operation known as a [Cross Join](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_join).

SELECT COUNT(\*)
FROM Sample.Company

 

SELECT COUNT(\*)
FROM Sample.Vendor

 

SELECT COUNT(\*)
FROM Sample.Company,Sample.Vendor

 

You can perform the same operation using explicit CROSS JOIN syntax:

SELECT COUNT(\*)
FROM Sample.Company CROSS JOIN Sample.Vendor

 

In most cases, the extensive data duplication of a cross join is not desirable, and some other type of join is preferable.

If you specify a [WHERE clause](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_where) in the SELECT statement, the cross join is performed, then the WHERE clause predicate(s) determine the result set. This is equivalent to performing an INNER JOIN with an ON clause. Thus the following two examples return identical results:

SELECT p.Name,p.Home\_State,em.Name,em.Office\_State
FROM Sample.Person AS p, Sample.Employee AS em
WHERE p.Name %STARTSWITH 'E' AND em.Name %STARTSWITH 'E'

 

SELECT p.Name,p.Home\_State,em.Name,em.Office\_State
FROM Sample.Person AS p INNER JOIN Sample.Employee AS em
ON p.Name %STARTSWITH 'E' AND em.Name %STARTSWITH 'E'

 

You can specify explicit join syntax (rather than using commas) in the FROM table-ref list to perform other types of join operations. For further details, refer to [JOIN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_join).

Query Optimization Options

By default, the Caché SQL query optimizer uses sophisticated and flexible algorithms to optimize the performance of complex queries involving join operations and/or multiple indexes. In most cases, these defaults provide optimal performance. However, in infrequent cases, you may wish to give “hints” to the query optimizer, specifying one or more aspects of query optimization. For this reason, Caché SQL provides optimize-option keywords in the FROM clause. You can specify multiple optimization keywords in any order, separated by blank spaces. For further details, refer to [Optimizing SQL Queries](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_optquery) in the Caché SQL Optimization Guide.

You can use optimize-option FROM clause keywords in a simple SELECT statement, in a CREATE VIEW view definition SELECT statement, or in a subquery SELECT statement within the FROM clause.

%ALLINDEX

This optional keyword specifies that all indexes that provide any benefit are used for the first table in the query join order. This keyword should only be used when there are multiple defined indexes. The optimizer default is to use only those indexes that the optimizer judges to be most beneficial. By default, this includes all efficient equality indexes, and selected indexes of other types. %ALLINDEX uses all possibly beneficial indexes of all types. Testing all indexes has a larger overhead, but under some circumstances it may provide better performance than the default optimization. This option is especially helpful when using multiple range condition indexes and inefficient equality condition indexes. In these circumstances, accurate index selectivity may not be available to the query optimizer. %ALLINDEX can be used with %IGNOREINDEX to include/exclude specific indexes. Generally, %ALLINDEX should not be used with a TOP clause query.

You can use %STARTTABLE with %ALLINDEX to specify which table the %ALLINDEX applies to.

You can specify exceptions to %ALLINDEX for specific conditions with the %NOINDEX condition-level hint. The %NOINDEX hint is placed in front of each query selection [condition](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_predicates) for which no index should be used. For example, WHERE %NOINDEX hiredate < ?. This is most commonly used when the overwhelming majority of the data is not excluded by the condition. With a less-than (<) or greater-than (>) condition, use of the %NOINDEX condition-level hint is often beneficial. With an equality condition, use of the %NOINDEX condition-level hint provides no benefit. With a [join condition](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_join), %NOINDEX is not supported for =\* and \*= WHERE clause outer joins; %NOINDEX is supported for ON clause joins. For further details, refer to “[Using Indices](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_optquery#GSQLOPT_optquery_ind)” in the “Optimizing Query Performance” chapter in Caché SQL Optimization Guide.

%FIRSTTABLE

%FIRSTTABLE tablename

This optional keyword specifies that the query optimizer should start to performs joins with the specified tablename. The tablename names a table that is specified later in the join sequence. The join order for the remaining tables is left to the query optimizer. This hint is functionally identical to %STARTTABLE, but provides you with the flexibility to specify the join table sequence in any order.

The tablename must be a simple identifier, either a table alias or an unqualified table name. A qualified table name (schema.table) cannot be used. If the query specifies a table alias, the table alias must be used as tablename. For example:

FROM %FIRSTTABLE P Sample.Employee AS E JOIN Sample.Person AS P ON E.Name = P.Name

%FIRSTTABLE and %STARTTABLE both enable you to specify the initial table to use for join operations. %INORDER enables you to specify the order of all tables used for join operations. These three keywords are mutually exclusive; specify one and one only. If these keywords are not used the query optimizer performs joins on tables in the sequence it considers optimal, regardless of the sequence in which the tables are listed.

You cannot use %FIRSTTABLE or %STARTTABLE to begin the join order with the right-hand side of a LEFT OUTER JOIN (or the left-hand side of a RIGHT OUTER JOIN). Attempting to do so results in an SQLCODE -34 error: “Optimizer failed to find a usable join order”.

For further details, refer to the %STARTTABLE query optimization option.

%FULL

This optional keyword specifies that the compiler optimizer examines all alternative join sequences to maximize access performance. For example, when creating a stored procedure, the increased compile time may be worthwhile to provide for more optimized access. The default optimization is to not examine less likely join sequences when there are many tables in the FROM clause. %FULL overrides this default behavior.

You might specify both the %INORDER and the %FULL keywords when the FROM clause includes tables accessed with [arrow syntax](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_implicitjoins), which lead to tables whose order is unconstrained.

%IGNOREINDEX

This optional keyword specifies that the query optimizer ignore the specified index or list of indices. (The deprecated synonym %IGNOREINDICES is supported for backwards compatibility.)

Following this keyword you specify one or more index names. Multiple index names must be separated by commas. You can specify an index name using either of the following formats:

%IGNOREINDEX \[\[schemaname.\]tablename.\]indexname \[,...\] %IGNOREINDEX \[\[schemaname.\]tablename.\]\* \[,...\]

The schemaname and tablename are optional. If omitted, the current default schema and the table name specified as FROM table-ref are used. The asterisk (\*) wildcard specifies all of the index names for the specified table. You can specify index names in any order. Caché SQL does not validate the index names you specify (or their schemaname and tablename); a nonexistent or duplicate index name is simply ignored.

By using this optimization constraint, you can cause the query optimizer to not use an index that is not optimal for a specific query. By specifying all index names but one, you can, in effect, force the query optimizer to use the remaining index.

You can also ignore a specific index for a specific condition expression by prefacing the condition with the %NOINDEX keyword. For further details, refer to “[Using Indices](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_optquery#GSQLOPT_optquery_ind)” in the “Optimizing Query Performance” chapter in the Caché SQL Optimization Guide.

%INORDER

This optional keyword specifies that the query optimizer performs joins in the order that the tables are listed in the FROM clause. This minimizes compile time. The join order of tables referenced with arrow syntax is unrestricted (for information on using arrow syntax, refer to [Implicit Joins](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_implicitjoins) in Using Caché SQL). Flattening of subqueries and index usage are unaffected.

%INORDER cannot be used with a CROSS JOIN or a RIGHT OUTER JOIN. If the table order specified is inconsistent with the requirements of an outer join, an SQLCODE -34 error is generated: “Optimizer failed to find a usable join order.” To avoid this, it is recommended that %INORDER, when used with outer joins, only be used with ANSI-style left outer joins or full outer joins.

Views and table subqueries are processed in the order that they are specified in the FROM clause.

*   Streamed View: %INORDER has no effect on the order of processing of tables within the view.
    
*   Merged View: %INORDER causes the view tables to be processed in the view’s FROM clause order, at the point of reference to the view.
    

Compare this keyword with %FIRSTTABLE and %STARTTABLE, both of which specify only the initial join table, rather than the full join order. See %STARTTABLE for a table of merge behaviors with different join order optimizations.

The %INORDER and %PARALLEL optimizations cannot be used together; if both are specified, %PARALLEL is ignored.

%NOFLATTEN

This optional keyword is specified in the FROM clause of a quantified subquery — a subquery that returns a boolean value. It specifies that the compiler optimizer should inhibit subquery flattening. This optimization option disables “flattening” (the default), which optimizes a query containing a quantified subquery by effectively integrating the subquery into the query: adding the tables of the subquery to the FROM clause of the query and converting conditions in the subquery to joins or restrictions in the query's WHERE clause.

The following are examples of quantified subqueries using %NOFLATTEN:

SELECT Name,Home\_Zip FROM Sample.Person WHERE Home\_Zip IN 
      (SELECT Office\_Zip FROM %NOFLATTEN Sample.Employee)

 

SELECT Name,(SELECT Name FROM Sample.Company WHERE EXISTS
             (SELECT \* FROM %NOFLATTEN Sample.Company WHERE Revenue \> 500000000))
 FROM Sample.Person

 

The %INORDER and %STARTTABLE optimizations implicitly specify %NOFLATTEN.

%NOMERGE

This optional keyword is specified in the FROM clause of a subquery. It specifies that the compiler optimizer should inhibit the conversion of a subquery to a view. This optimization option disables the optimizing of a query containing a subquery by adding the subquery to the FROM clause of the query as an in-line view; comparisons from the subquery to fields of the query are moved to the query's WHERE clause as joins.

%NOREDUCE

This optional keyword is specified in the FROM clause of a streamed subquery — a subquery that returns a result set of rows, a [subquery in the enclosing query’s FROM clause](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_from#RSQL_from_subq). It specifies that the compiler optimizer should inhibit the merging of the subquery (or view) into the containing query.

In the following example, the query optimizer would normally “reduce” this query by performing a Cartesian product join of Sample.Person with the subquery. The %NOREDUCE optimization option prevents this. Caché instead builds a temporary index on gname and performs the join on this temporary index:

SELECT \* FROM Sample.Person AS p, 
   (SELECT Name||'goo' AS gname FROM %NOREDUCE Sample.Employee) AS e
    WHERE p.name||'goo' \= e.gname

%NOSVSO

This optional keyword is specified in the FROM clause of a quantified subquery — a subquery that returns a boolean value. It specifies that the compiler optimizer should inhibit Set-Valued Subquery Optimization (SVSO).

In most cases, Set-Valued Subquery Optimization improves the performance of [\[NOT\] EXISTS](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_exists) and [\[NOT\] IN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_in) subqueries, especially with subqueries with only one, separable correlating condition. It does this by populating a temporary index with the data values that fulfill the condition. Rather than repeatedly executing the subquery, Caché looks up these values in the temporary index. For example, SVSO optimizes NOT EXISTS (SELECT P.num FROM Products P WHERE S.num=P.num AND P.color='Pink') by creating a temporary index for P.num.

SVSO optimizes subqueries where the [ALL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_all) or [ANY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_any) keyword is used with a relative operator ( >, >=, <, or <=) and a subquery, such as ...WHERE S.num > ALL (SELECT P.num ...). It does this by replacing the subquery expression sqbExpr (P.num in this example) with MIN(sqbExpr) or MAX(sqbExpr), as appropriate. This supports fast computation when there is an index on sqbExpr.

The %INORDER and %STARTTABLE optimizations do not inhibit Set-Valued Subquery Optimization.

%NOTOPOPT

This optional keyword is specified when using a [TOP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_top) clause with an [ORDER BY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_order) clause. By default, TOP with ORDER BY optimizes for fastest time-to-first-row. Specifying %NOTOPOPT (no TOP optimization) instead optimizes the query for fastest retrieval of the complete result set.

%NOUNIONOROPT

This optional keyword is specified in the FROM clause of a query or subquery. It disables the automatic optimizations provided for multiple OR conditions and for subqueries against a UNION query expression. These automatic optimizations transform multiple OR conditions to UNION subqueries, or UNION subqueries to OR conditions, where deemed appropriate. These UNION/OR transformations allow EXISTS and other low-level predicates to migrate to top-level conditions where they are available to Caché query optimizer indexing. These default transformations are desirable in most situations.

However, in some situations these UNION/OR transformations impose a significant overhead burden. %NOUNIONOROPT disables these automatic UNION/OR transformations for all conditions in the WHERE clause associated with this FROM clause. Thus, in a complex query, you can disable these automatic UNION/OR optimizations for one subquery while allowing them in other subqueries.

The %INORDER and %STARTTABLE optimizations inhibit OR-to-UNION optimizations. The %INORDER and %STARTTABLE optimizations do not inhibit UNION-to-OR optimizations.

%PARALLEL

This optional keyword is specified in the FROM clause of a query. It suggests that Caché perform parallel processing of the query, using multiple processors (if applicable). This can significantly improve performance of some queries that uses one or more [COUNT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_count), [SUM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_sum), [AVG](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_avg), [MAX](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_max), or [MIN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_min) aggregate functions, and/or a [GROUP BY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_groupby) clause, as well as many other types of queries. These are commonly queries that process a large quantity of data and return a small result set. For example, SELECT AVG(SaleAmt) FROM %PARALLEL User.AllSales GROUP BY Region would likely use parallel processing.

A query that specifies both individual fields and an aggregate function and does not include a GROUP BY clause cannot perform parallel processing. For example, SELECT Name,AVG(Age) FROM %PARALLEL Sample.Person does not perform parallel processing, but SELECT Name,AVG(Age) FROM %PARALLEL Sample.Person GROUP BY Home\_State does perform parallel processing.

%PARALLEL is intended for SELECT queries and their subqueries. An [INSERT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_insert#RSQL_insertselect) command subquery cannot use %PARALLEL.

Specifying %PARALLEL may degrade performance for some queries. Running a query with %PARALLEL on a system with multiple concurrent users may result in degraded overall performance.

Note:

A query that specifies %PARALLEL must be run in a database that is read/write, not readonly. Otherwise, a <PROTECT> error may occur.

OpenVMS does not support parallelization. The %PARALLEL keyword is parsed, but ignored.

Regardless of the presence of the %PARALLEL keyword in the FROM clause, some queries may use linear processing, not parallel processing: some queries do not support parallel processing; some queries, when optimized, may be found to not benefit from parallel processing. You can determine if and how Caché has partitioned a query for parallel processing using [Show Plan](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_optquery#GSQLOPT_optquery_showplan).

For further details, refer to [Parallel Query Processing](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_optquery#GSQLOPT_optquery_parallel) in the “Optimizing Query Performance” chapter of the Caché SQL Optimization Guide.

%STARTTABLE

This optional keyword specifies that the query optimizer should start to performs joins with the first table listed in the FROM clause. The join order for the remaining tables is left to the query optimizer. Compare this keyword with %INORDER, which specifies the complete join order.

%STARTTABLE cannot be used with a CROSS JOIN or a RIGHT OUTER JOIN. You cannot use %STARTTABLE (or %FIRSTTABLE) to begin the join order with the right-hand side of a LEFT OUTER JOIN (or the left-hand side of a RIGHT OUTER JOIN). If the start table specified is inconsistent with the requirements of an outer join, an SQLCODE -34 error is generated: “Optimizer failed to find a usable join order.” To avoid this, it is recommended that %STARTTABLE, when used with outer joins, only be used with ANSI-style left outer joins or full outer joins.

The following table shows the merge behavior when combining a superquery parent and an in-line view with %INORDER and %STARTTABLE optimizations:

 

Superquery with no join optimizer

Superquery with %STARTTABLE

Superquery with %INORDER

View with no join optimizer

merge view if possible

If the view is the superquery start: don't merge.

Otherwise, merge view if possible.

merge if possible; view's underlying tables are unordered.

View with %STARTTABLE

don't merge

If the view is the superquery start: merge, if possible. View's start table becomes superquery's start table.

Otherwise, don’t merge.

don't merge

View with %INORDER

don't merge

don't merge

If the view is not controlled by the %INORDER: don't merge.

Otherwise, merge view if possible; view's order becomes substituted into superquery join order.

The %FIRSTTABLE hint is functionally identical to %STARTTABLE, but provides you with the flexibility to specify the join table sequence in any order.

Table-Valued Functions in the FROM Clause

A table-valued function is a class query that is projected as a stored procedure and returns a single result set. A table-valued function is any class query which has SqlProc TRUE. A class query used as a table-valued function must be compiled in either LOGICAL or RUNTIME mode. When used as a table-valued function and compiled in RUNTIME mode, the table-valued function query will be called in LOGICAL mode.

A table-valued function follows the same naming conventions as a stored procedure name for a class query. Parameter parentheses are mandatory; the parentheses may be empty, enclose a literal or a host variable, or a comma-separated list of literals and host variables. If you specify no parameters (empty parentheses or the null string), the table-valued function returns all data rows.

To issue a query using a table-valued function, the user must hold the EXECUTE privilege on the stored procedure that defines the table-valued function. The user must also have SELECT privileges on the tables or views accessed by the table-valued function query.

In the following example, the class query Sample.Person.ByName is projected as a stored procedure and can thus be used as a table-valued function:

SELECT Name,DOB FROM Sample.SP\_Sample\_By\_Name('A')

The following Dynamic SQL example specifies the same table-valued function. It uses the %Execute() method to supply parameter values to the ? input parameter:

  ZNSPACE "SAMPLES"
  SET myquery\="SELECT Name,DOB FROM Sample.SP\_Sample\_By\_Name(?)"
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET qStatus \= tStatement.%Prepare(myquery)
    IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
  SET rset \= tStatement.%Execute("A")
  DO rset.%Display()
  WRITE !,"End of A data",!!
  SET rset \= tStatement.%Execute("B")
  DO rset.%Display()
  WRITE !,"End of B data"

 

A table-valued function can only be used in the FROM clause of either a SELECT statement or a [DECLARE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_declare) statement. A table-valued function name can be qualified with a schema name or unqualified (without a schema name); an unqualified name uses the default schema. In a SELECT statement FROM clause, a table-valued function can be used wherever a table name can be used. It can be used in a view or a subquery, and can be joined to other table-ref items using a comma-separated list or explicit JOIN syntax.

A table-valued function cannot be directly used in an INSERT, UPDATE, or DELETE statement. You can, however, specify a subquery for these commands that specifies a table-valued function.

Cache SQL does not define the EXTENTSIZE for a table-valued function, or the SELECTIVITY for table-valued function columns.

Subqueries in the FROM Clause

You can specify a subquery in the FROM clause. This is known as a streamed subquery. The subquery is treated the same as a table, including its use in JOIN syntax and the optional assignment of an alias using the AS keyword. A FROM clause can contain multiple tables, views, and subqueries in any combination, subject to the restrictions of the JOIN syntax, as described in [JOIN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_join).

A subquery is enclosed in parentheses. The following example shows a subquery in a FROM clause:

SELECT name,region
FROM (SELECT t1.name,t1.state,t2.region
      FROM Employees AS t1 LEFT OUTER JOIN Regions AS t2
      ON t1.state\=t2.state)
GROUP BY region

A subquery can specify a [TOP clause](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_top). A subquery can contain an [ORDER BY clause](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_order) when paired with a TOP clause.

A subquery can use SELECT \* syntax, subject to the following restriction: because a FROM clause results in a value expression, a subquery containing SELECT \* must yield only one column.

A join within a subquery cannot be a NATURAL join or take a USING clause.

FROM Subqueries and %VID

When a FROM subquery is invoked, it returns a %VID for each subquery row returned. A %VID is an integer counter field; its values are system-assigned, unique, non-null, non-zero, and non-modifiable. The %VID is only returned when explicitly specified. It is returned as data type INTEGER. Because %VID values are sequential integers, they are far more meaningful if the subquery returns ordered data; a subquery can only use an ORDER BY clause when it is paired with a TOP clause.

Because the %VID is a sequential integer, it can be used to determine the ranking of items in a subquery with an ORDER BY clause. In the following example, the 10 newest records are listed in Name order, but their timestamp ranking is easily seen using the %VID values:

SELECT Name,%VID,TimeStamp FROM
   (SELECT TOP 10 \* FROM MyTable ORDER BY TimeStamp DESC)
ORDER BY Name 

One common use of the %VID is to “window” the result set, dividing execution into sequential subsets that fit the number of lines available in a display window. For example, display 20 records, then wait for the user to press Enter, then display the next 20 records.

The following example uses %VID to “window” the results into subsets of 10 records:

  ZNSPACE "SAMPLES"
  SET myq\=4
  SET myq(1)\="SELECT %VID,\* "
  SET myq(2)\="FROM (SELECT TOP 60 Name,Age FROM Sample.Person "
  SET myq(3)\="WHERE Age > 55 ORDER BY Name) "
  SET myq(4)\="WHERE %VID BETWEEN ? AND ?"
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET qStatus \= tStatement.%Prepare(.myq)
   IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
  FOR i\=1:10:60 {
  SET rset \= tStatement.%Execute(i,i+9)
  WHILE rset.%Next() {
     DO rset.%Print() } 
  WRITE !!
  }
  WRITE "End of data"

 

For details on using %VID, refer to the [Defining and Using Views](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_views) chapter of Using Caché SQL.

Optional FROM Clause

If no table data is referenced (directly or indirectly) by the SELECT item list, the FROM clause is optional. This kind of SELECT may be used to return data from functions, operator expressions, constants, or host variables. For a query that references no table data:

*   If the FROM clause is omitted, a maximum of one row of data is returned, regardless of the TOP keyword value; TOP 0 returns no data. The [DISTINCT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_distinct) clause is ignored. No privileges are required.
    
*   If the FROM clause is specified, it must specify an existing table in the current namespace. You must have SELECT privilege for that table, even though the table is not referenced. The number of identical rows of data returned is equal to the number of rows in the specified table, unless you specify a TOP or DISTINCT clause, or limit it with a WHERE or HAVING clause. Specifying a [DISTINCT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_distinct) clause limits the output to a single row of data. The TOP keyword limits the output to the number of rows specified by the TOP value; TOP 0 returns no data.
    

With or without a FROM clause, subsequent clauses (WHERE, GROUP BY, HAVING or ORDER BY) may be specified. A WHERE or HAVING clause may be used to determine whether or not to return results, or how many identical rows of results to return. These clauses may reference a table, even if no FROM clause is specified. A GROUP BY or ORDER BY clause may be specified, but these clauses are not meaningful.

The following are examples of SELECT statements that reference no table data. Both examples return one row of information.

The following example omits the FROM clause. The DISTINCT keyword is not needed, but may be specified. No SELECT clauses are permitted.

SELECT 3+4 AS Arith,
      {fn NOW} AS NowDateTime,
      {fn DAYNAME({fn NOW})} AS NowDayName,
      UPPER('MixEd cASe EXPreSSioN') AS UpCase,
      {fn PI} AS PiConstant   

 

The following example includes a FROM clause. The DISTINCT keyword is used to return a single row of data. The FROM clause table reference must be a valid table. The ORDER BY clause is permitted here, but is meaningless. Note that the ORDER BY clause must specify a valid select item alias:

SELECT DISTINCT 3+4 AS Arith,
    {fn NOW} AS NowDateTime,
    {fn DAYNAME({fn NOW})} AS NowDayName,
    UPPER('MixEd cASe EXPreSSioN')  AS UpCase,
    {fn PI} AS PiConstant
FROM Sample.Person
ORDER BY NowDateTime  

 

The following examples both use a WHERE clause to determine whether or not to return results. The first includes a FROM clause and uses the DISTINCT keyword is to return a single row of data. The second omits the FROM clause, and therefore returns at most a single row of data. In both cases, the WHERE clause table reference must be a valid table for which you have SELECT privilege:

SELECT DISTINCT
   {fn NOW} AS DataOKDate
FROM Sample.Person
WHERE FOR SOME (Sample.Person)(Name %STARTSWITH 'A')  

 

SELECT {fn NOW} AS DataOKDate
WHERE FOR SOME (Sample.Person)(Name %STARTSWITH 'A')

 

See Also

*   [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select)
    
*   [JOIN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_join)
    
*   “[Querying the Database](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_queries)” chapter in Using Caché SQL
    
*   “[Defining Tables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_tables)” chapter in Using Caché SQL
    
*   “[Optimizing SQL Queries](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_optquery)” in the Caché SQL Optimization Guide.
    
*   [SQLCODE error messages](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RERR_sql) listed in the Caché Error Reference
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_fetch "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_grant "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_from.xml**