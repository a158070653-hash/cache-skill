# SELECT

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_select

---

 

Caché SQL Reference

SELECT

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_savepoint "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_setoption "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_COMMANDS "SQL Commands") \]  >  \[ [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select "SELECT") \]

Go to: Description The DISTINCT Clause The TOP Clause The select-item FROM Clause WHERE Clause GROUP BY Clause HAVING Clause ORDER BY Clause SELECT and Transaction Processing Query Metadata Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Retrieves rows from one or more tables within a database.

Synopsis

\[(\] SELECT \[%NOFPLAN\] \[%NOLOCK\]
    \[DISTINCT \[BY (item {,item})\] | ALL\] 
    \[TOP {int | ALL}\]
    select-item {,select-item}
    \[INTO host-variable-list\]
    \[FROM \[optimize-option\] table-ref \[\[AS\] t-alias\]
      {,table-ref \[\[AS\] t-alias\]} \]
    \[WHERE condition-expression\]
    \[GROUP BY scalar-expression\]
    \[HAVING condition-expression\]
    \[ORDER BY item-order-list \[ASC | DESC\] \]
\[)\]

select-item ::= 
  \[t-alias.\]\*   |
  \[t-alias.\]scalar-expression \[\[AS\] c-alias\]
    {,\[t-alias.\]scalar-expression \[\[AS\] c-alias\]}

Arguments

%NOFPLAN

Optional — The %NOFPLAN keyword specifies that Caché will ignore the frozen plan (if any) for this query and generate a new query plan. The frozen plan is retained, but not used. For further details, refer to [Frozen Plans](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_frozenplans) in Caché SQL Optimization Guide.

%NOLOCK

Optional — The %NOLOCK keyword specifies that Caché will perform no locking on any of the specified tables. If you specify this keyword, the query retrieves data in [READ UNCOMMITTED mode](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_settransaction#RSQL_settransaction_isolation), regardless of current transaction’s isolation mode. For further details, refer to [Transaction Processing](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_modify#GSQL_modify_transaction) in the “Modifying the Database” chapter of Using Caché SQL.

DISTINCT

DISTINCT BY (item)

ALL

Optional — The [DISTINCT clause](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select#RSQL_select_distinct) specifies that each row returned must contain a unique value for the specified field or combination of fields. A DISTINCT keyword specifies that the select-item value(s) must be unique. A DISTINCT BY keyword clause specifies that item value(s) must be unique. An item (or a comma-separated list of items) is enclosed in parentheses. Commonly, an item is the name of a column. It may or may not also be listed as a select-item.

Optional — The ALL keyword specifies that all rows that meet the SELECT criteria be returned. This is the default for Caché SQL. The ALL keyword performs no operation; it is provided for SQL compatibility.

See [DISTINCT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_distinct) clause for more details.

TOP int

TOP ALL

Optional — The TOP clause limits the number of rows returned to the number specified in int. If no ORDER BY clause is specified in the query, which records are returned as the “top” rows is unpredictable. If an ORDER BY clause is specified, the top rows accord to the specified order. The DISTINCT keyword (if specified) is applied before TOP, specifying that int number of unique values are to be returned. The int argument can be either a positive integer or a [Dynamic SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_dynsql) ? input parameter that resolves to a positive integer. If no TOP keyword is specified, the default is to display all the rows that meet the SELECT criteria.

TOP ALL is only meaningful in a subquery or in a CREATE VIEW statement. It is used to support the use of an ORDER BY clause in these situations, fulfilling the requirement that an ORDER BY clause must be paired with a TOP clause in a subquery or a query used in a CREATE VIEW. TOP ALL does not restrict the number of rows returned.

See [TOP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_top) clause for more details.

select-item

One or more [columns (or other values) to be retrieved](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select#RSQL_select_selectitem). Multiple select-items are specified as a comma-separated list. You can also retrieve all columns by using the \* symbol.

INTO host-variable-list

Optional — (Embedded SQL only): One or more [host variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_hvars) into which select-item values are placed. Multiple host variables are specified as a comma-separated list or as a single host variable array. See [INTO](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_into) clause for more details.

FROM table-ref

Optional — A reference to one or more [tables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_tables) from which data is being retrieved. A valid table-ref is required for every FROM clause, even if the SELECT makes no reference to that table. A SELECT that makes no references to table data can omit the FROM clause.

A table-ref can be specified as one or more [tables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_tables), [views](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_views), [table-valued functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_from#RSQL_from_tvf), or [subqueries](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_from#RSQL_from_subq), specified as a comma-separated list or with JOIN syntax. Some restrictions apply on using views with JOIN syntax. A subquery must be enclosed in parentheses.

A table-ref is either qualified (schema.tablename) or unqualified (tablename). An unqualified table-ref is supplied either the [system-wide default schema name](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_tables#GSQL_tables_schema), or (if provided) a schema name from the schema search path. In [Embedded SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql) you can use the [#SQLCompile Path](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_macros#GCOS_macros_mpp_lbSQLCompile_Path) macro directive to supply a schema search path. In [Dynamic SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_dynsql) you can use the [%SQL.Statement](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SQL.Statement) %New method to supply a schema search path.

Multiple tables can be specified as a comma-separated list or associated with ANSI join keywords. Any combination of tables or views can be specified. If you specify a comma between two table-refs here, Caché performs a [CROSS JOIN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_join) on the tables and retrieves data from the results table of the JOIN operation. If you specify ANSI join keywords between two table-refs here, Caché performs the specified join operation. For further details, refer to the [JOIN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_join) page of this manual.

You can optionally assign an alias (t-alias) to each table-ref. The AS keyword is optional.

You can optionally specify one or more optimize-option keywords to optimize query execution. The available options are: %ALLINDEX, %FIRSTTABLE, %FULL, %INORDER, %IGNOREINDEX, %NOFLATTEN, %NOMERGE, %NOREDUCE, %NOSVSO, %NOTOPOPT, %NOUNIONOROPT, %PARALLEL, and %STARTTABLE. See [FROM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_from) clause for more details.

WHERE condition-expression

Optional — A qualifier specifying one or more predicate conditions for what data is to be retrieved. See [WHERE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_where) clause for more details.

GROUP BY scalar-expression

Optional — A comma-separated list of one or more scalar expressions specifying how the retrieved data is to be organized; these may include [column names](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable#RSQL_createtable_fields). See [GROUP BY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_groupby) clause for more details.

HAVING condition-expression

Optional — A qualifier specifying one or more predicate conditions for what data is to be retrieved. See [HAVING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_having) clause for more details.

ORDER BY item-order-list

Optional — A select-item or a comma-separated list of items that specify the order in which rows are displayed. Each item can have an optional ASC (ascending order) or DESC (descending order). The default is ascending order. An ORDER BY clause is used on the results of a query. An ORDER BY clause in a [subquery](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_from#RSQL_from_subq) (for example, in a UNION statement) must be paired with a TOP clause. If no ORDER BY clause is specified, the order of the records returned is unpredictable. See [ORDER BY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_order) clause for more details.

scalar-expression

A field identifier, an expression containing a field identifier, or a general expression, such as a function call or an arithmetic operation.

AS t-alias

Optional — An [alias for a table or view name](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select#RSQL_select_fromclause_talias) (table-ref). An alias must be a valid [identifier](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_identifiers); it can be a delimited identifier. For further details see the “Identifiers” chapter of Using Caché SQL. The AS keyword is optional.

AS c-alias

Optional — An [alias for a column name](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select#RSQL_select_selectitem_calias) (select-item). An alias must be a valid [identifier](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_identifiers). For further details see the “Identifiers” chapter of Using Caché SQL. The AS keyword is optional.

Description

The SELECT statement performs a query that retrieves data from a Caché database. In its simplest form, it retrieves one or more items from a single table. The items are specified by the select-item list and the table is specified by the FROM table-ref clause. In more complex queries, a SELECT can retrieve data from multiple tables and can retrieve data using views.

A SELECT can also be used to return a value from an SQL function, a host variable, or a literal. A SELECT query can combine returning these non-database values with retrieving values from tables or views. When a SELECT is only used to return such non-database values, the FROM clause is optional. See [FROM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_from) clause for more details.

The values returned from a SELECT query are known as a result set. In Dynamic SQL, SELECT retrieves values into the [%SQL.Statement](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SQL.Statement) class. Refer to the [Dynamic SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_dynsql) chapter of Using Caché SQL, and the [%SQL.Statement](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SQL.Statement) class in the InterSystems Class Reference.

Caché sets a status variable [SQLCODE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_svars), which indicates the success or failure of the SELECT. In addition, the SELECT operation sets the [%ROWCOUNT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_svars) local variable to the number of selected rows. Successful completion of a SELECT generally sets SQLCODE=0 and %ROWCOUNT to the number of rows selected. In the case of an embedded SQL containing a simple SELECT, data from (at most) one row is selected, so SQLCODE=0 and %ROWCOUNT is set to either 0 or 1. However, in the case of an embedded SQL SELECT that declares a cursor and fetches data from multiple rows, the operation completes when the cursor has been advanced to the end of the data (SQLCODE=100); at that point, %ROWCOUNT is set to the total number of rows selected. Refer to the [FETCH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_fetch) command for further details.

Uses of SELECT

You can use a SELECT statement in the following contexts:

*   As an independent query.
    
*   As a subquery, a SELECT statement that supplies values to a clause of an enclosing SELECT statement. A subquery in a SELECT statement can be specified in the [select-item](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select#RSQL_select_selectitem) list, in a [FROM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_from#RSQL_from_subq) clause, or in a WHERE clause with an [EXISTS](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_exists) or [IN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_in) predicate. A subquery can also be specified in an [UPDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_update) or [DELETE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_delete) statement. A subquery must be enclosed in parentheses.
    
*   As a leg of a [UNION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_union). The [UNION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_union) statement allows you to combine two or more SELECT statements into a single query.
    
*   As part of a [CREATE VIEW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createview) defining the data available to the view.
    
*   As part of a [DECLARE CURSOR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_declare) used with Embedded SQL.
    
*   As part of an [INSERT with a SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_insert#RSQL_insertselect). An INSERT statement can use a SELECT to insert data values for multiple rows into a table, selecting the data from another table.
    

You can enclose the entire SELECT statement with one or more sets of parentheses, as follows:

*   Parentheses are optional for an independent SELECT query, a UNION leg SELECT query, a CREATE VIEW SELECT query, or a DECLARE CURSOR SELECT query. Enclosing a SELECT query in parentheses causes it to follow the syntax rules for a subquery; specifically, an ORDER BY clause must be paired with a TOP clause.
    
*   Parentheses are mandatory for a subquery. One set of parentheses is mandatory; you can specify additional optional sets of parentheses.
    
*   Parentheses are not permitted for an INSERT statement SELECT query.
    

Specifying optional parentheses generates a separate [cached query](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_cachedqueries) for each set of parentheses added.

Privileges

To perform a SELECT query on one or more tables, you must either have column-level SELECT privileges for all of the specified select-item column(s), or table-level SELECT privileges for the specified table-ref table(s) or view(s). A select-item column specified using a table alias (such as t.Name or "MyAlias".Name) only requires column-level SELECT privilege, not table-level SELECT privilege.

When using SELECT \*, note that column-level privileges cover all table columns named in the GRANT statement; table-level privileges cover all table columns, including those added after the privilege was assigned.

Failing to have the necessary privileges results in an SQLCODE -99 error (Privilege Violation). You can determine if the current user has SELECT privilege by invoking the [%CHECKPRIV](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_checkpriv) command. You can determine if a specified user has table-level SELECT privilege by invoking the [$SYSTEM.SQL.CheckPriv()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#CheckPriv) method. For privilege assignment, refer to the [GRANT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_grant) command.

Note:

Having table-level SELECT privilege for a table is not a sufficient test that the table actually exists. If the specified user has the %All role, CheckPriv() returns 1 even if the specified table or view does not exist.

A SELECT query that does not have a FROM clause does not require any SELECT privileges. A SELECT query that contains a FROM clause requires SELECT privilege, even if no column data is accessed by the query.

Required Clauses

The following are required clauses for all SELECT statements:

*   A [select-item](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select#RSQL_select_selectitem) list, a comma-separated list of one or more items (the select-item arguments) to be retrieved from the table or otherwise generated. Most commonly, these items are the names of columns in a table. The select-item consists of either a scalar expression specifying one or more individual items, or an asterisk (\*) referring to all the columns of a base table.
    
*   A [FROM clause](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select#RSQL_select_fromclause) specifies one or more tables, views, or subqueries from which rows are to be retrieved. These tables may be associated by [JOIN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_join) expressions. In Caché SQL a FROM clause with a valid table-ref is required for a SELECT that makes any reference to table data. A FROM clause is optional for a SELECT that does not access table data. The optional FROM clause is further described in the [FROM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_from) clause reference page.
    

Optional Clauses

The following optional clauses operate on the virtual table that a FROM clause returns. All are optional, but, if used, must appear in the order specified:

*   A [DISTINCT clause](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_distinct), which specifies that only distinct (non-duplicate) values should be returned.
    
*   A [TOP clause](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_top), which specifies how many rows to return.
    
*   A [WHERE clause](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select#RSQL_select_whereclause), which specifies boolean predicate conditions that rows must match. The WHERE clause predicate condition both determines which rows are returned and limits the values supplied to aggregate functions to the values from those rows. These conditions are specified by one or more [predicates](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_predicates) linked by logical operators; the WHERE clause returns all records that satisfy these predicate conditions. A WHERE clause predicate cannot include aggregate functions.
    
*   A [GROUP BY clause](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select#RSQL_select_groupbyclause), which specifies a comma-delimited list of columns. These organize a query’ result set into subsets with matching values for one or more columns and determine the ordering of the rows returned. GROUP BY allows scalar expressions as well as columns.
    
*   A [HAVING clause](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select#RSQL_select_havingclause), which specifies boolean predicate conditions that rows must match. These conditions are specified by one or more [predicates](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_predicates) linked by logical operators. The HAVING clause predicate condition determines which rows are returned, but (by default) it does not limits the values supplied to aggregate functions to the values from those rows. This default can be overridden using the %AFTERHAVING keyword. A HAVING clause predicate can specify aggregate functions. These predicates typically operate on each group specified by a GROUP BY clause.
    
*   An [ORDER BY clause](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select#RSQL_select_orderbyclause), which specifies the order in which rows should be displayed. An ORDER BY clause in a subquery or a CREATE VIEW query must be paired with a TOP clause.
    

The DISTINCT Clause

The DISTINCT keyword clause causes redundant field values to be eliminated. It has two forms:

*   SELECT DISTINCT: Returns one row for each unique combination of select-item values. You can specify one or more than one select-items. For example, the following query returns a row with Home\_State and Age values for each unique combination of Home\_State and Age values:
    
    SELECT DISTINCT Home\_State,Age FROM Sample.Person
    
     
    
*   SELECT DISTINCT BY (item): Returns one row for each unique combination of item values. You can specify a single item or a comma-separated list of items. The select-item list may, but does not have to, include the specified item(s). For example, the following query returns a row with Name and Age values for each unique combination of Home\_State and Age values:
    
    SELECT DISTINCT BY (Home\_State,Age) Name,Age FROM Sample.Person
    
     
    

Either type of DISTINCT clause can specify more than one item to test for uniqueness. Listing more than one item retrieves all rows that are distinct for the combination of both items. DISTINCT does consider NULL a unique value. For further details, see the [DISTINCT clause](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_distinct) reference page.

The TOP Clause

The TOP keyword clause specifies that the SELECT statement return only a specified number of rows. It returns the specified number of rows that appear at the “top” of the returned virtual table. By default, which rows are the “top” rows of the table is unpredictable. However, Caché applies the [DISTINCT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_distinct) and [ORDER BY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_order) clauses (if specified) before selecting the TOP rows. For further details, see the [TOP clause](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_top) reference page.

The select-item

This is a mandatory element for all SELECT statements. Commonly, a select-item refers to a field in the table(s) specified in the FROM clause. A select-item consists of one or more of the following items, with multiple items separated by commas:

*   A column name (field name), with or without a table name alias:
    
    SELECT Name,Age FROM Sample.Person
    
     
    
    Field names are not case-sensitive. However, the label associated with the field in the result set uses the letter case of the [SqlFieldName](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ROBJ_property_sqlfieldname) as specified in the table definition, not the letter case specified in the select-item. See “[Field Column Alias](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select#RSQL_select_selectitem_caliasfield)” for further details on letter case resolution.
    
    To list all of the column names defined for a specified table, refer to [Column Names and Numbers](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_tables#GSQL_tables_getcols) in the “Defining Tables” chapter of Using Caché SQL.
    
    To display the [RowId](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_tables#GSQL_tables_idfield), you can use the %ID pseudo-property alias, which displays the RowId regardless of what name it is assigned. By default, the name of the RowId is ID, but Caché may rename it if there is a user-defined field named ID.
    
    A SELECT on a stream field returns the oref (object reference) of the opened stream object.
    
    When the FROM clause specifies more than one table or view, you must include the table name (or a table name alias) as part of the select-item, using periods, as shown in the following two examples:
    
    Full table name:
    
    SELECT Sample.Company.Name, Sample.Person.Name
          FROM Sample.Person, Sample.Company
    
     
    
    Table name alias:
    
    SELECT p.Name, c.Name
          FROM Sample.Person AS p, Sample.Company AS c
    
     
    
    You can use a [collation function](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_basics#GSQL_basics_collation) to specify the sorting and display of a select-item field. You can supply the collation function without parentheses (SELECT %SQLUPPER Name) or with parentheses (SELECT %SQLUPPER(Name)). If the collation function specifies truncation, the parentheses are required (SELECT %SQLUPPER(Name,10)).
    
    When the select-item references an embedded object property (embedded serial class data), use underline syntax. Underline syntax consists of the name of the object property (container), an underscore, and the property within the embedded object: for example, Home\_City and Home\_State. (In other contexts, index tables for example, these are represented using dot syntax: Home.City.) For further details, refer to “[Controlling the SQL Projection of Collection Properties](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GOBJ_propcoll#GOBJ_propcoll_sqlproj)” in Using Caché Objects.
    
    SELECT Home\_City,Home\_State FROM Sample.Person
    
     
    
    You can use SELECT to directly query a container field, rather than using underline syntax. Because the data returned is in list format, you may want to use a $LISTTOSTRING or $LISTGET function to display the data. For example:
    
    SELECT $LISTTOSTRING(Home,'^') AS HomeAddress FROM Sample.Person
    
     
    
*   A subquery. A subquery returns a single column from a specified table. This column can be the values of a single table field (SELECT Name), or the values of multiple table fields returned as a single column, either by using concatenation (SELECT Home\_City||Home\_State) or by specifying a container field (SELECT Home). A subquery can use arrow syntax. A subquery cannot use asterisk syntax, even when the table cited in the subquery has only one data field.
    
    One common use of a subquery is to specify an aggregate function that is not subject to the GROUP BY clause. In the following example, the GROUP BY clause groups ages by decades (for example, 25 through 34). The AVG(Age) select-item gives the average age of each group, as defined by the GROUP BY clause. In order to get the average age of all of the records in all groups, it uses a subquery:
    
    SELECT Age AS Decade,
           COUNT(Age) AS PeopleInDecade,
           AVG(Age) AS AvgAgeForDecade,
           (SELECT AVG(Age) FROM Sample.Person) AS AvgAgeAllDecades
    FROM Sample.Person
    GROUP BY ROUND(Age,\-1)
    ORDER BY Age
    
     
    
*   Arrow syntax, used to access a field from a table other than the FROM clause table. This is known as an implicit join. In the following example, the Sample.Employee table contains a Company field containing the row Id for the corresponding company name in the Sample.Company table. The arrow syntax retrieves the company name from that table:
    
    SELECT Name,Company\->Name AS CompanyName
          FROM Sample.Employee
    
     
    
    In this case, you must have SELECT privileges for the referenced table: either table-level SELECT privilege, or column-level SELECT privilege for both the referenced field and the RowID column of the referenced table. For further details on arrow syntax, refer to [Implicit Joins (Arrow Syntax)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_implicitjoins) in Using Caché SQL.
    
*   Asterisk syntax (\*), which selects all the columns in a table in [column number order](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_tables#GSQL_tables_getcols):
    
    SELECT TOP 5 \* FROM Sample.Person
    
     
    
    Hidden fields are not listed by asterisk syntax. By default, the [RowId](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_tables#GSQL_tables_idfield) is hidden (not displayed by SELECT \*). However, if the table was defined with %PUBLICROWID, SELECT \* returns the RowId field and all non-hidden fields. By default, the name of this field is ID, but Caché may rename it if there is a user-defined field named ID.
    
    Note:
    
    Most of the example tables supplied in the Samples namespace are defined with %PUBLICROWID.
    
    Other hidden fields are also not listed by asterisk syntax. For example, the Home and Office fields of Sample.Person (which reference embedded object properties in the Sample.Address embedded serial class) are not displayed by SELECT \*.
    
    If the select-item is an asterisk and more than one table is specified, it selects all the columns in all of the joined tables:
    
    SELECT TOP 5 \* FROM Sample.Company,Sample.Employee
    
     
    
    Asterisk syntax can be qualified or unqualified. If the select-item is qualified by prefixing a table name (or table name alias) and period (.) before the asterisk, the select-item selects all the columns in the specified table. Qualified asterisk syntax can be combined with other select items for other tables.
    
    In the following example, select-item consists of an unqualified asterisk syntax that selects all columns from the table. Note that you can also specify duplicate column names (in this case Name) and non-column select-item elements (in this case {fn NOW}):
    
    SELECT TOP 5 {fn NOW} AS QueryDate,
                 Name AS Client,
                 \*
    FROM Sample.Person
    
     
    
    In the following example, select-item consists of qualified asterisk syntax that selects all columns from one table, and a list of column names from another table.
    
    SELECT TOP 5 E.Name AS EmpName, 
                 C.\*, 
                 E.Home\_State AS EmpState
    FROM Sample.Employee AS E, Sample.Company AS C
    
     
    
    Note:
    
    SELECT \* is a fully supported part of Caché SQL that can be extremely convenient during application development and debugging. However, in production applications the preferred programming practice is to explicitly list the selected fields, rather than using the asterisk syntax form. Explicitly listing fields makes your application clearer and easier to understand, easier to maintain, and easier to search for fields by name.
    
*   An XMLELEMENT, XMLFOREST, or XMLCONCAT function, which place XLM (or HTML) tags around the data values retrieved from specified column names. Refer to [XMLELEMENT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_xmlelement) for further details.
    
*   A select-item list containing one or more arithmetic expressions. These arithmetic operations generally use one or more field values:
    
    SELECT Name, Age, Age \- AVG(Age) FROM Sample.Person
    
     
    
    If a select-item arithmetic operation includes division, and there are any values for that field in the database that could produce a divisor with a value of zero or a NULL value, you cannot rely on order of testing to avoid division by zero. Instead, use a case statement to suppress the risk.
    
    It is also possible to include arithmetic operations that do not involve any field values in a select-item list:
    
    SELECT Name, Age, 9 \- 6 FROM Sample.Person
    
     
    
*   A select-item list containing one or more SQL conversion functions:
    
    SELECT Name,UCASE(Name) FROM Sample.Person
    
     
    
*   A select-item list containing one or more SQL [aggregate functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_aggregatefunctions). An aggregate function always returns a single value. The argument of an aggregate function may be any of the following:
    
    *   A single column name—computes the aggregate for all non-null values of the rows selected by the query:
        
        SELECT AVG(Age) FROM Sample.Person
        
         
        
    *   A scalar expression is also permitted to compute an aggregate:
        
        SELECT SUM(Age) / COUNT(\*) FROM Sample.Person
        
         
        
    *   Asterisk syntax (\*) — used with the COUNT function to compute the number of rows in the table:
        
        SELECT COUNT(\*) FROM Sample.Person
        
         
        
    *   A select distinct function — computes the aggregate by eliminating redundant values:
        
        SELECT COUNT(DISTINCT Home\_State) FROM Sample.Person
        
         
        
    *   While ANSI SQL does not allow the combination of column names and aggregate functions in a single SELECT statement, Caché SQL extends the standard by allowing this:
        
        SELECT Name, COUNT(DISTINCT Home\_State) FROM Sample.Person
        
         
        
    *   An aggregate function using %FOREACH. This causes the aggregate to be computed for each distinct value of a column or columns:
        
        SELECT DISTINCT Home\_State, AVG(Age %FOREACH(Home\_State)) 
             FROM Sample.Person
        
         
        
    *   An aggregate function using %AFTERHAVING. This causes the aggregate to be computed on a sub-population specified with the HAVING clause:
        
        SELECT Name,AVG(Age %AFTERHAVING) 
              FROM Sample.Person
              HAVING (Age \> AVG(Age))
        
         
        
        would return those records where Age is greater than average age, giving the average age for those persons whose age is above the average for all persons in the database.
        
*   A user-defined class method stored as a procedure. May be an unqualified method name or a qualified method name. In the following example, MyMethods.RandomLetter() is a class method:
    
    SELECT MyMethods.RandomLetter()
    
    The return value from the method is automatically converted from Logical format to Display/ODBC format. An input value to the method is, by default, not converted from Display/ODBC format to Logical format. However, input display-to-logical conversion can be configured systemwide using the [$SYSTEM.SQL.SetSQLFunctionArgConversion()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#SetSQLFunctionArgConversion) method. You can use [$SYSTEM.SQL.GetSQLFunctionArgConversion()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#GetSQLFunctionArgConversion) to determine the current configuration of this option.
    
    If the specified method does not exist in the current namespace, Caché generates a SQLCODE -359 error. If the specified method is ambiguous (could refer to more than one method), Caché generates a SQLCODE -358 error. For further details, refer to [CREATE PROCEDURE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createprocedure) or [CREATE METHOD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createmethod).
    
*   A [%ID, %TABLENAME or %CLASSNAME pseudo-field variable](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_queries#GSQL_queries_pseudovars) keyword. %ID returns the [RowId](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_tables#GSQL_tables_idfield) (record ID), which is, by default, a hidden field. %TABLENAME returns the current table name. %CLASSNAME returns the name of the class corresponding to the current table. If the query references multiple tables, you can prefix the keyword with a table alias. For example, t1.%TABLENAME.
    
*   An extrinsic Caché ObjectScript function call operating on a database column:
    
    SELECT $$REFORMAT^ABC(name)FROM MyTable
    
    Note that you can only invoke extrinsic functions within an SQL statement if you configured this option system-wide. Go to the Management Portal, select \[Home\] > \[Configuration\] > \[General SQL Settings\]. View and edit the current setting of Allow Extrinsic Functions in SQL Statements. The default is “No”; by default, attempting to invoke extrinsic functions issues an SQLCODE -372 error.
    
    You cannot use an extrinsic function to call a % routine (a routine with a name that begins with the % character). Attempting to do so issues an SQLCODE -373 error.
    
*   A select-item literal, which does not reference any fields in the specified table:
    
    SELECT UCASE('fred') FROM Sample.Person
    
     
    
    SELECT 7 \* 7, 7 \* 8 FROM Sample.Person
    
     
    
    The way a numeric literal is specified determines its data type. Therefore, the string '123' is reported as data type VARCHAR, and the numeric 123 is reported as data type INTEGER or NUMERIC.
    
    When all of the select-items references no table data, the FROM clause is optional. If you include the FROM clause, the specified table must exist. For further details on optional FROM clause, refer to the [FROM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_from) clause reference page.
    
    Select item text literals can be used to produce a more readable output, as shown in the following example:
    
    SELECT TOP 10 Name,'was born on',%EXTERNAL(DOB)
    FROM Sample.Person
    
     
    

The Column Alias

When specifying a select-item, you can use the AS keyword to specify an alias for the name of a column:

SELECT Name AS PersonName, DOB AS BirthDate, ...

The column alias is displayed as the column header in the result set. Specifying a column alias is optional; a default is always provided. A column alias is displayed with the specified letter case; it is not, however, case sensitive when referenced in an ORDER BY clause. The c-alias name must be a valid [identifier](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_identifiers). A c-alias name can be a [delimited identifier](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_identifiers#GSQL_identifiers_delimitedids). For further details see the “Identifiers” chapter of Using Caché SQL.

The AS keyword is not required, but makes the query text easier to read. Thus the following is also valid syntax:

SELECT Name PersonName, DOB BirthDate, ...

SQL does not perform uniqueness checking for column aliases. It is possible (though not desirable) for a field column and a column alias to have the same name, or for two column aliases to be identical. Such non-unique column aliases may cause an SQLCODE -24 “Ambiguous sort column” error when referenced by an ORDER BY clause. Column aliases, like all SQL identifiers, are not case sensitive.

Use of column aliases in other SELECT clauses is governed by [query semantic processing order](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_queries#GSQL_queries_select). You can reference a column by its column alias in an [ORDER BY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_order) clause. You cannot reference a column alias in another select-item in the select list, in a DISTINCT BY clause, a WHERE clause, a GROUP BY clause, or a HAVING clause. You cannot reference a column alias in a JOIN operation’s ON clause or USING clause. You can, however, use a subquery to make a column alias available for use by other these other SELECT clauses, as shown in the “[Querying the Database](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_queries#GSQL_queries_select)” chapter of Using Caché SQL.

Field Column Aliases

A select-item field name is not case-sensitive. However, unless you supply a column alias, the name of a field column in the result set follows the letter case of the [SqlFieldName](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ROBJ_property_sqlfieldname) associated with the column property. The letter case of the SqlFieldName corresponds to the field name as specified in the table definition, not as specified in the select-item list. Therefore, SELECT name FROM Sample.Person returns the field column label as Name. Using a field column alias allows you to specify the letter case to display, as shown in the following example:

SELECT name,name AS NAME
FROM Sample.Person

 

Letter case resolution takes time. To maximize SELECT performance, you can specify the exact letter case of the field name, as specified in the table definition. However, determining the exact letter case of a field in the table definition is often inconvenient and prone to error. Instead, you can use a field column alias to avoid letter case issues. Note that all references to the field column alias must match in letter case.

The following Dynamic SQL example requires letter case resolution (the SqlFieldNames are “Latitude” and “Longitude”):

  ZNSPACE "SAMPLES"
  SET myquery \= "SELECT latitude,longitude FROM Sample.USZipCode"
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET qStatus \= tStatement.%Prepare(myquery)
   IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
  SET rset \= tStatement.%Execute()
   WHILE rset.%Next() {WRITE rset.latitude," ",rset.longitude,! }

The following Dynamic SQL example does not requires letter case resolution, and therefore executes faster:

  ZNSPACE "SAMPLES"
  SET myquery \= "SELECT latitude AS northsouth,longitude AS eastwest FROM Sample.USZipCode"
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET qStatus \= tStatement.%Prepare(myquery)
   IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
  SET rset \= tStatement.%Execute()
   WHILE rset.%Next() {WRITE rset.northsouth," ",rset.eastwest,! }

The t-alias table alias prefix is not included in the column name. Therefore, in the following example, both columns are labeled as Name:

SELECT p.Name,e.Name
FROM Sample.Person AS p LEFT JOIN Sample.Employee AS e ON p.Name\=e.Name

 

To distinguish the columns in a query that specifies multiple tables, you should specify column aliases:

SELECT p.Name AS PersonName,e.Name AS EmployeeName
FROM Sample.Person AS p LEFT JOIN Sample.Employee AS e ON p.Name\=e.Name

 

You may also wish to provide a column alias to make the data easier to understand. In the following example, the table column “Home\_State” is renamed “US\_State\_Abbrev”:

SELECT Name,Home\_State AS US\_State\_Abbrev
FROM Sample.Person

 

Note that %ID references a specific column, and therefore returns that field name (ID, by default), or a specified column alias, as shown in the following example:

SELECT %ID,%ID AS Ident,Name
FROM Sample.Person

 

Non-Field Column Aliases

Non-field columns are automatically assigned a column name. If you provide no alias for such fields, Caché SQL supplies a unique column name, such as “Expression\_1”, or “Aggregate\_3”. The integer suffix refers to the select-item position as specified in the SELECT statement. They are not a count of fields of that type.

The following are automatically assigned column names (n is an integer):

*   Aggregate\_n: an [aggregate function](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_aggregatefunctions), such as AVG(Age) or COUNT(\*). A column is named Aggregate\_n if the outermost operation is an aggregate function, even when this aggregate contains an expression. For example, COUNT(Name)+COUNT(Spouse) is Expression\_n, but MAX(COUNT(Name)+COUNT(Spouse)) is Aggregate\_n.
    
*   HostVar\_n: a host variable. This may be a literal, such as ‘text’, 123, or the empty string (''), an input variable (:myvar), or a [? input parameter](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_dynsql#GSQL_dynsql_executemethod_inparams) replaced by a literal. Note that any expression evaluation on a literal, such concatenation or an arithmetic operation, makes it an Expression\_n. A literal value supplied to a ? parameter is returned unchanged without expression evaluation. For example, supplying 5+7 returns the string '5+7' as HostVar\_n.
    
*   Literal\_n: a [pseudo-field variable](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_queries#GSQL_queries_pseudovars) such as %TABLENAME, or the NULL specifier. Note that %ID is not Literal\_n; it is given the column name of the actual RowID field.
    
*   Subquery\_n: the result of a subquery that specifies a single select-item. The select-item may be a field, aggregate function, expression, or literal. You specify the column alias after the subquery, not within the subquery.
    
*   Expression\_n: any operation in the select-item list on a field or on an Aggregate\_n, HostVar\_n, Literal\_n, or Subquery\_n select-item changes its column name to Expression\_n. This includes arithmetic operations (Age+5), concatenation ('USA:'||Home\_State), data type CAST operations, SQL collation functions (%UPPER(Name)), SQL scalar functions ($LENGTH(Name)), CASE expressions, and special variables (such as CURRENT\_DATE or $ZPI).
    

In the following example, the aggregate field column created by the AVG function is given the column alias “AvgAge”; its default name is “Aggregate\_3” (an aggregate field in position 3 in the SELECT list).

SELECT Name, Age, AVG(Age) AS AvgAge FROM Sample.Person

 

The following example is identical to the previous, except that the AS keyword is here omitted. The use of this keyword is recommended, but not required.

SELECT Name, Age, AVG(Age) AvgAge FROM Sample.Person

 

The following example show how to specify a column alias for a select-item subquery:

SELECT Name AS PersonName,
       (SELECT Name FROM Sample.Employee) AS EmpName,
       Age AS YearsOld
FROM Sample.Person

 

FROM Clause

The FROM table-ref clause specifies one or more [tables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_tables), [views](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_views), table-valued functions, or [subqueries](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_from#RSQL_from_subq). You can specify any combination of these table-ref types as a comma-separated list or with JOIN syntax. If you specify a single table-ref, the specified data is retrieved from that table or view. If you specify multiple table-refs, SQL performs a join operation on the tables, merging their data into a results table from which the specified data is retrieved.

If you specify more than one table-ref, you can separate these table names with commas or with explicit join syntax keywords. For further details on specifying a comma-separated list of table names, see the [FROM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_from) clause reference page. For further details on specifying multiple table names with explicit JOIN syntax (such as RIGHT JOIN or \*=) see the [JOIN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_join) reference page.

You can use the [$SYSTEM.SQL.TableExists()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#TableExists) or [$SYSTEM.SQL.ViewExists()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#ViewExists) method to determine whether a table or view exists in the current namespace. You can use the [$SYSTEM.SQL.CheckPriv()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#CheckPriv) method to determine if you have [SELECT privileges](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select#RSQL_select_privileges) for that table or view.

The Table Alias

When specifying a table-ref, you can use the AS keyword to specify an alias for that table name or view name:

FROM Sample.Person AS P

The AS keyword is not required, but makes the query text easier to read. The following is valid equivalent syntax:

FROM Sample.Person P

The t-alias name must be a valid [identifier](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_identifiers). A t-alias name can be a [delimited identifier](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_identifiers#GSQL_identifiers_delimitedids). A t-alias must be unique among table aliases within the query. A t-alias, like all identifiers, is not case sensitive. Therefore, you cannot specify two t-alias names that differ only in letter case. This results in an SQLCODE -20 “Name conflict” error. For further details see the “Identifiers” chapter of Using Caché SQL.

The table alias is used as a prefix (with a period) to a field name to indicate the table to which the field belongs. For example:

SELECT P.Name, E.Name
FROM Sample.Person AS P, Sample.Employee AS E

 

You must use a table reference prefix when a query specifies multiple tables that have the same field name. A table reference prefix can be a t-alias (as shown above) or it can be the fully qualified table name, as shown in the following equivalent example:

SELECT Sample.Person.Name, Sample.Employee.Name
FROM Sample.Person, Sample.Employee

 

Specifying a table alias is optional when a query references only one table (or view). Specifying a table alias is optional (but recommended) when a query references multiple tables (and/or views) and the field names referenced are unique to each table. Specifying a table alias is required when a query references multiple tables (and/or views) and the field names referenced are the same in different tables. Failing to specify a t-alias (or fully qualified table name) prefix results in an SQLCODE -27 “Field %1 is ambiguous among the applicable tables” error.

A t-alias only uniquely identifies a field for query execution; to uniquely identify a field for query result set display you must also use a column alias ([c-alias](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select#RSQL_select_selectitem_calias)). The following example uses both table aliases (Per and Emp) and column aliases (PName and Ename):

SELECT Per.Name AS PName, Emp.Name AS EName
FROM Sample.Person AS Per, Sample.Employee AS Emp
WHERE Per.Name %STARTSWITH 'G'

 

You can use the same name for a field, a column alias, and/or a table alias without a naming conflict.

A t-alias prefix is used wherever it is necessary to distinguish which table is being referred to. Some examples of this are shown in the following:

SELECT P.%ID As PersonID,
       AVG(P.Age) AS AvgAge,
       Z.%TABLENAME||'=' AS Tablename,
       Z.\*
FROM Sample.Person AS P, Sample.USZipCode AS Z
WHERE P.Home\_City \= Z.City
GROUP BY P.Home\_City
ORDER BY Z.City 

 

WHERE Clause

The WHERE clause qualifies or disqualifies specific rows from the query selection. The rows that qualify are those for which the condition-expression is true. The condition-expression is a list of logical tests (predicates) which can be linked by the AND and OR logical operators. These predicates may be inverted using the NOT unary logical operator.

The SQL predicates fall into the following categories:

*   [Comparison Predicates](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_where#RSQL_where_comparisonpredicates)
    
*   [BETWEEN Predicate](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_where#RSQL_where_betweenpredicate)
    
*   [LIKE Predicate](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_where#RSQL_where_likepredicate)
    
*   [NULL Predicate](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_where#RSQL_where_nullpredicate)
    
*   [IN and %INLIST Predicates](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_where#RSQL_where_inpredicate)
    
*   [EXISTS Predicate](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_where#RSQL_where_existspredicate)
    
*   [FOR SOME Predicate](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_where#RSQL_where_forsomepredicate)
    
*   [FOR SOME %ELEMENT Predicate](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_where#RSQL_where_elementpredicate)
    

For further details on these logical predicates, see the [WHERE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_where) clause reference page. The condition-expression cannot contain aggregate functions. If you wish to specify a selection condition using a value returned by an aggregate function, use a [HAVING clause](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_having).

A WHERE clause can specify an explicit join between two tables using the = (inner join), =\* (left outer join), and \*= (right outer join) symbolic join operators. For further details, refer to the [JOIN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_join) page of this manual.

A WHERE clause can specify an implicit join between the base table and a field from another table using the arrow syntax (–>) operator. For further details, refer to [Implicit Joins](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_implicitjoins) in Using Caché SQL.

GROUP BY Clause

The GROUP BY clause takes the resulting rows of a query and breaks them up into individual groups according to one or more database columns. When you use SELECT in conjunction with GROUP BY, one row is retrieved for each distinct value of the GROUP BY fields. The GROUP BY clause is conceptually similar to the Caché extension %FOREACH, but GROUP BY operates on an entire query, while %FOREACH allows selection of aggregates on sub-populations without restricting the entire query population. For instance:

SELECT Home\_State, COUNT(Home\_State) AS Population
 FROM Sample.Person
  GROUP BY Home\_State

 

This query returns one row for each distinct Home\_State.

For further details, see the [GROUP BY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_groupby) clause reference page.

HAVING Clause

The HAVING clause is like a WHERE clause that operates on groups. It is typically used in combination with the GROUP BY clause, or with the %AFTERHAVING keyword. The HAVING clause qualifies or disqualifies specific rows from the query selection. The rows that qualify are those for which the condition-expression is true. The condition-expression is a list of logical tests (predicates) which can be linked by the AND and OR logical operators. The condition-expression can contain aggregate functions. For further details, see the [HAVING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_having) clause reference page.

ORDER BY Clause

An ORDER BY clause consists of the ORDER BY keywords followed by a select-item or a comma-separated list of items that specify the order in which rows are displayed. Each item can have an optional ASC (ascending order) or DESC (descending order). The default is ascending order. An ORDER BY clause is applied to the results of a query, and is frequently paired with a TOP clause. For further details, see the [ORDER BY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_order) clause reference page.

The following example returns the selected fields for all rows in the database, and orders these rows in ascending order by age:

SELECT Home\_State, Name, Age 
FROM Sample.Person
ORDER BY Age

 

SELECT and Transaction Processing

A transaction performing a query is defined as either READ COMMITTED or READ UNCOMMITTED. A query that is not in a transaction is defined as READ UNCOMMITTED.

*   If READ UNCOMMITTED, a SELECT returns the current state of the data, including changes made to the data by transactions in progress which have not been committed. These changes may be subsequently rolled back.
    
*   If READ COMMITTED, the behavior depends on the contents of the SELECT statement. Normally, a SELECT statement in read committed mode would only return insert and update changes to data that has been committed. Data rows that have been deleted by a transaction in progress are not returned, even though these deletes have not been committed and may be rolled back.
    
    However, if the SELECT statement contains a %NOLOCK keyword, a [DISTINCT clause](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_distinct), or a [GROUP BY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_groupby) clause, the SELECT returns the current state of the data, including changes made to data during the current transaction which have not been committed. An aggregate function in a SELECT also returns the current state of the data for the specified column(s), including uncommitted changes.
    

For further details, refer to [SET TRANSACTION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_settransaction#RSQL_settransaction_isolation) and [START TRANSACTION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_starttransaction).

Query Metadata

You can use Dynamic SQL to return metadata about the query, such as the number of columns specified in the query, the name (or alias) of a column specified in the query, and the data type of a column specified in the query. For further details, refer to the [Dynamic SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_dynsql) chapter of Using Caché SQL, and the [%SQL.Statement](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SQL.Statement) class in the InterSystems Class Reference.

Examples

The following four examples perform similar queries, using different combinations of SELECT clauses. Note that these clauses must be specified in the correct order. In all four examples, three fields are selected from the Sample.Person table: Name, Home\_State, and Age, and two fields (AvgAge and AvgMiddleAge) are computed.

HAVING/ORDER BY

In the following example, the AvgAge computed field is computed on all records in Sample.Person. The HAVING clause governs the AvgMiddleAge computed field, calculating the average age of those over 40 from all records in Sample.Person. Thus, every row has the same value for AvgAge and AvgMiddleAge. The ORDER BY clause sequences the display of the rows alphabetically by the Home\_State field value.

SELECT Name,Home\_State,Age,AVG(Age) AS AvgAge,
 AVG(Age %AFTERHAVING) AS AvgMiddleAge
 FROM Sample.Person
 HAVING Age \> 40
 ORDER BY Home\_State

 

WHERE/HAVING/ORDER BY

In the following example, the WHERE clause limits the selection to the seven specified northeastern states. The AvgAge computed field is computed on the records from those Home\_States. The HAVING clause governs the AvgMiddleAge computed field, calculating the average age of those over 40 from the records from the specified Home\_States. Thus, every row has the same value for AvgAge and AvgMiddleAge. The ORDER BY clause sequences the display of the rows alphabetically by the Home\_State field value.

SELECT Name,Home\_State,Age,AVG(Age) AS AvgAge,
 AVG(Age %AFTERHAVING) AS AvgMiddleAge
 FROM Sample.Person
 WHERE Home\_State IN ('ME','NH','VT','MA','RI','CT','NY')
 HAVING Age \> 40
 ORDER BY Home\_State

 

GROUP BY/HAVING/ORDER BY

The GROUP BY clause causes the AvgAge computed field to be separately computed for each Home\_State group. The GROUP BY clause also limits the output display to the first record encountered from each Home\_State. The HAVING clause governs the AvgMiddleAge computed field, calculating the average age of those over 40 in each Home\_State group. The ORDER BY clause sequences the display of the rows alphabetically by the Home\_State field value.

SELECT Name,Home\_State,Age,AVG(Age) AS AvgAge,
 AVG(Age %AFTERHAVING) AS AvgMiddleAge
 FROM Sample.Person
 GROUP BY Home\_State
 HAVING Age \> 40
 ORDER BY Home\_State

 

WHERE/GROUP BY/HAVING/ORDER BY

The WHERE clause limits the selection to the seven specified northeastern states. The GROUP BY clause causes the AvgAge computed field to be separately computed for each of these seven Home\_State groups. The GROUP BY clause also limits the output display to the first record encountered from each specified Home\_State. The HAVING clause governs the AvgMiddleAge computed field, calculating the average age of those over 40 in each of the seven Home\_State groups. The ORDER BY clause sequences the display of the rows alphabetically by the Home\_State field value.

SELECT Name,Home\_State,Age,AVG(Age) AS AvgAge,
 AVG(Age %AFTERHAVING) AS AvgMiddleAge
 FROM Sample.Person
 WHERE Home\_State IN ('ME','NH','VT','MA','RI','CT','NY')
 GROUP BY Home\_State
 HAVING Age \> 40
 ORDER BY Home\_State

 

Embedded SQL and Dynamic SQL Examples

Embedded SQL and Dynamic SQL can be used to issue a SELECT query from within a program in another language. Embedded SQL can be included in Caché ObjectScript code. Dynamic SQL can be included in either Caché ObjectScript code or Caché Basic code.

The following embedded SQL program retrieves data values from one record and places them in the output [host variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_hvars) specified in the [INTO clause](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_into).

   NEW SQLCODE,%ROWCOUNT
   &sql(SELECT Home\_State,Name,Age
        INTO :a, :b, :c
        FROM Sample.Person)
   IF SQLCODE\=0 {
     WRITE !,"  Name=",b
     WRITE !,"  Age=",c
     WRITE !,"  Home State=",a
     WRITE !,"Row count is: ",%ROWCOUNT }
   ELSE {
     WRITE !,"SELECT failed, SQLCODE=",SQLCODE  }

 

This program retrieves (at most) one row, so the %ROWCOUNT variable is set to either 0 or 1. To retrieve multiple rows, you must declare a cursor and use the [FETCH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_fetch) command. For further details, refer to the [Embedded SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql) chapter in Using Caché SQL.

The following Dynamic SQL example first tests whether the desired table exists and checks the current user’s SELECT privilege for that table. It then executes the query and returns a result set. It uses the WHILE loop to repeatedly invoke the %Next method for the first 10 records of the result set. It displays three field values using %GetData methods that specify the field position as specified in the SELECT statement:

  ZNSPACE "Samples"
  SET tname\="Sample.Person"
  IF $SYSTEM.SQL.TableExists(tname)
     & $SYSTEM.SQL.CheckPriv($USERNAME,"1,"\_tname,"s")
     {GOTO SpecifyQuery}
  ELSE {WRITE "Table unavailable"  QUIT}
SpecifyQuery
  SET myquery \= 3
  SET myquery(1) \= "SELECT Home\_State,Name,SSN,Age"
  SET myquery(2) \= "FROM "\_tname
  SET myquery(3) \= "ORDER BY Name"
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET tStatus \= tStatement.%Prepare(.myquery)
  IF tStatus '= 1 { WRITE "%Prepare error",!
                  QUIT }
  SET rset \= tStatement.%Execute()
  IF rset.%SQLCODE\=0 {
    SET x\=0
    WHILE x < 10 {
     SET x\=x+1
     SET status\=rset.%Next()
     WRITE rset.%GetData(2)," "   /\* Name field \*/
     WRITE rset.%GetData(1)," "   /\* Home\_State field \*/
     WRITE rset.%GetData(4),!     /\* Age field \*/
    }
    WRITE !,"End of Data"
    WRITE !,"SQLCODE=",rset.%SQLCODE," Row Count=",rset.%ROWCOUNT
  }
  ELSE {
    WRITE !,"SELECT failed, SQLCODE=",rset.%SQLCODE }

 

For further details, refer to the [Dynamic SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_dynsql) chapter in Using Caché SQL.

See Also

*   SELECT clauses: [DISTINCT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_distinct), [FROM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_from), [GROUP BY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_groupby), [HAVING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_having), [INTO](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_into), [ORDER BY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_order), [TOP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_top), [WHERE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_where)
    
*   [JOIN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_join), [UNION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_union)
    
*   [CREATE VIEW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createview)
    
*   [CREATE TABLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable), [ALTER TABLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_altertable), [DROP TABLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_droptable)
    
*   [CREATE QUERY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createquery), [DROP QUERY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dropquery)
    
*   [INSERT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_insert), [INSERT OR UPDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_insertorupdate), [UPDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_update), [DELETE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_delete)
    
*   “[Querying the Database](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_queries)” chapter in Using Caché SQL
    
*   [SQL configuration settings](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RACS_Settings#RACS_Category_SQL) described in Caché Advanced Configuration Settings Reference.
    
*   [SQLCODE error messages](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RERR_sql) listed in the Caché Error Reference
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_savepoint "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_setoption "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_select.xml**