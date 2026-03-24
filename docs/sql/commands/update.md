# UPDATE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_update

---

 

Caché SQL Reference

UPDATE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_unlock "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_usedatabase "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_COMMANDS "SQL Commands") \]  >  \[ [UPDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_update "UPDATE") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Sets new values for specified columns in a specified table.

Synopsis

UPDATE \[%NOFPLAN\] \[restriction\] table-ref \[\[AS\] t-alias\]
   value-assignment-statement 
   \[FROM \[optimize-option\] table-ref1 \[\[AS\] t-alias\]
         {,table-ref2 \[\[AS\] t-alias\]} \]
   \[WHERE condition-expression\]

UPDATE \[restriction\] table-ref \[\[AS\] t-alias\]
   value-assignment-statement 
   \[WHERE CURRENT OF cursor\]

value-assignment-statement ::=
   SET column1 = scalar-expression1 {,column2 = scalar-expression2} ...  |
   \[ (column1{,column2} ...) \] VALUES (scalar-expression1 {,scalar-expression2} ...)  |
   VALUES :array()

Arguments

%NOFPLAN

Optional — The %NOFPLAN keyword specifies that Caché will ignore the frozen plan (if any) for this operation and generate a new query plan. The frozen plan is retained, but not used. For further details, refer to [Frozen Plans](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_frozenplans) in Caché SQL Optimization Guide.

restriction

Optional — One or more of the following keywords, separated by spaces: %NOLOCK, %NOCHECK, %NOINDEX, %NOTRIGGER.

table-ref

The name of an existing [table](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_tables) where data is to be updated. You can also specify a [view](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_views) through which to perform the update on a table.

AS t-alias

Optional — An alias for a table-ref (table or view) name. An alias must be a valid [identifier](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_identifiers). The AS keyword is optional.

FROM table-ref1

Optional — A [FROM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_from) clause used to specify the table or tables used to determine which rows are to be updated.

Multiple tables can be specified as a comma-separated list or associated with ANSI join keywords. Any combination of tables or views can be specified. If you specify a comma between two table-refs here, Caché performs a [CROSS JOIN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_join) on the tables and retrieves data from the results table of the JOIN operation. If you specify ANSI join keywords between two table-refs here, Caché performs the specified join operation. For further details, refer to the [JOIN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_join) page of this manual.

You can optionally specify one or more optimize-option keywords to optimize query execution. The available options are: %ALLINDEX, %FIRSTTABLE tablename, %FULL, %INORDER, %IGNOREINDICES, %NOFLATTEN, %NOMERGE, %NOSVSO, %NOTOPOPT, %NOUNIONOROPT, and %STARTTABLE. See [FROM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_from) clause for more details.

WHERE condition-expression

Optional — Specifies one or more boolean [predicates](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_predicates) used to determine which rows are to be updated. If a WHERE clause (or a WHERE CURRENT OF clause) is not supplied, UPDATE updates all the rows in the table. For further details, see [WHERE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_where).

WHERE CURRENT OF cursor

Optional: Embedded SQL only — Specifies that the UPDATE operation updates the record at the current position of [cursor](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_cursor). You can specify a WHERE CURRENT OF clause or a WHERE clause, but not both. For further details, see [WHERE CURRENT OF](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_wherecurrentof).

column

Optional — The name of an existing column. Multiple column names are specified as a comma-separated list. If omitted, all columns are updated.

scalar-expression

A column data value expressed as a scalar expression. Multiple data values are specified as a comma-separated list with each data value corresponding in sequence to a column.

:array()

Embedded SQL only — An array of values specified as a host variable. The lowest subscript level of the array must be unspecified. Thus :myupdates(), :myupdates(5,), and :myupdates(1,1,) are all valid specifications.

Description

An UPDATE command changes existing values for columns in a table. You can update data in a table directly, update through a [view](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_views), or update using a subquery enclosed in parentheses. Updating through a view is subject to requirements and restrictions, as described in [CREATE VIEW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createview).

The UPDATE command provides one or more new column values to one or more existing base table rows that contain those columns. Assignment of data values to columns is done using a value-assignment-statement. By default, a value-assignment-statement updates all rows in the table.

More commonly, an UPDATE specifies the updating of a specific row (or rows) based on a condition-expression. By default, an UPDATE operation goes through all of the rows of a table and updates all rows that satisfy the condition-expression. If no rows satisfy the condition-expression, UPDATE completes successfully and sets SQLCODE=100 (No more data).

You can specify a WHERE clause or a WHERE CURRENT OF clause (but not both). If the WHERE CURRENT OF clause is used, UPDATE updates the record at the current position of the cursor. For details on positioned operations, see [WHERE CURRENT OF](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_wherecurrentof).

The UPDATE operation sets the [%ROWCOUNT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_svars) local variable to the number of updated rows, and the [%ROWID](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_svars) local variable to the Row ID of the last row updated.

By default, the UPDATE operation is an all-or-nothing event. Either all specified rows and columns are updated, or none are.

INSERT OR UPDATE

The [INSERT OR UPDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_insertorupdate) statement is a variant of the INSERT statement, the performs both insert and update operations. First it attempts to perform an insert operation. If the insert request fails due to a UNIQUE KEY violation (for the field(s) of some unique key, there exists a row that already has the same value(s) as the row specified for the insert), then it automatically turns into an update request for that row, and INSERT OR UPDATE uses the specified field values to update the existing row.

Privileges

To perform an update, you must either have table-level UPDATE privilege for the specified table (or view) or column-level UPDATE privilege for the specified column(s). When updating all fields in a row, note that column-level privileges cover all table columns named in the GRANT command; table-level privileges cover all table columns, including those added after the privilege was assigned. Failing to have the necessary privileges results in an SQLCODE -99 error (Privilege Violation). You can determine if the current user has UPDATE privilege by invoking the [%CHECKPRIV](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_checkpriv) command. You can determine if a specified user has table-level UPDATE privilege by invoking the [$SYSTEM.SQL.CheckPriv()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#CheckPriv) method. For privilege assignment, refer to the [GRANT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_grant) command.

When a property is defined as [ReadOnly](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ROBJ_property_readonly), the corresponding table field is also defined as ReadOnly. A ReadOnly field may only be assigned a value using [InitialExpression](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ROBJ_property_initialexpression) or [SqlComputed](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ROBJ_property_sqlcomputed). Attempting to update a value (even a NULL value) for a field for which you have column-level ReadOnly (SELECT or REFERENCES) privilege results in an SQLCODE -138 error: Cannot INSERT/UPDATE a value for a read only field.

You must have SELECT privilege for fields in a WHERE clause, whether or not those fields are to be updated. You must have both SELECT and UPDATE privileges for those fields if they are included in the update field list. In the following example, the Name field must have (at least) column-level SELECT privilege:

UPDATE Sample.Employee (Salary) VALUES (1000000) WHERE Name\='Smith, John'

In the above example, the Salary field requires only column-level UPDATE privilege.

Value Assignment

You can assign new values to specified columns in a variety of ways.

*   Using the SET keyword, specify one or more column = scalar-expression pairs as a comma-separated list. For example:
    
    SET StatusDate='05/12/06',Status='Purged'
    
*   Using the VALUES keyword, specify a list of columns equated to a corresponding scalar-expressions list. For example:
    
    (StatusDate,Status) VALUES ('05/12/06','Purged')
    
    When assigning scalar-expression values to a column list, there must be a scalar-expression for each specified column.
    
*   Using the VALUES keyword without a column list, specify a list of scalar-expressions that implicitly correspond to the columns of the row in column order. The following example specifies all of the columns in the table, specifying a literal value to update the Address column:
    
    VALUES (Name,DOB,'22 Main St. Anytown MA 12345',SSN)
    
    When assigning values to an implicit column list, you must supply a value for every updateable field, in the order that the columns are defined in the DDL. (You do not specify the non-updateable RowId column.) These values can either be a literal to specify a new value, or the field name to specify the existing value. You cannot specify placeholder commas or omit trailing fields.
    
*   Using the VALUES keyword without a column list, specify a subscripted array in which the numeric subscripts correspond to the column numbers, including the non-updateable RowId as column number 1. For example:
    
    VALUES :myarray()
    
    This value assignment can only be performed from [Embedded SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql) using a host variable. Unlike all other value assignments, this usage allows you to delay specifying which columns are to be updated until runtime (by populating the array at runtime). All other types of update require that the columns to be updated must be specified at compile time. For further details, see “[Host Variable as a Subscripted Array](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_hvarray)” in the “Using Embedded SQL” chapter of Using Caché SQL.
    

For program examples demonstrating each of these types of UPDATE, refer to the [Examples section](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_update#RSQL_update_examples), below.

DISPLAY to LOGICAL Data Conversion

Data is stored in LOGICAL mode format. For example, a date is stored as an integer count of days. Data supplied in an UPDATE operation that is not in LOGICAL mode format must be converted to LOGICAL mode format. Compiled SQL supports automatic conversion of UPDATE data values from DISPLAY or ODBC format to LOGICAL format. Automatic conversion of UPDATE data requires two factors: when compiled, the SQL must specify RUNTIME mode; when executed, the SQL must execute in a LOGICAL mode environment.

*   In [Embedded SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql), if you specify #SQLCompile Select=runtime, Caché will compile the SQL statement with code that converts data values from a display format to LOGICAL mode storage format. Caché performs this mode conversion both for single values and for arrays of values. For further details, see [#SQLCompile Select](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_macros#GCOS_macros_mpp_lbSQLCompile_Select) in the “ObjectScript Macros and the Macro Preprocessor” chapter of Using Caché ObjectScript.
    
*   In an SQL [CREATE FUNCTION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createfunction), [CREATE METHOD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createmethod), or [CREATE PROCEDURE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createprocedure) statement, if you specify SELECTMODE RUNTIME, Caché will compile the SQL statement with code that converts data values from a display format to LOGICAL mode storage format.
    

The UPDATE data may be in any format: DISPLAY format (for example, 6/17/2011), ODBC format (for example, 2011-06-17), or LOGICAL format (for example, 62259). The data is stored in LOGICAL format if the SQL execution environment is in LOGICAL mode. This is the default mode for all Caché SQL execution environments.

You can explicitly set the select mode to LOGICAL in SQL execution environments as follows:

*   In a Caché ObjectScript program or from the Caché Terminal interface: invoke the [$SYSTEM.SQL.SetSelectMode(0)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#SetSelectMode) method.
    
*   In [Dynamic SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_dynsql), specify [%SelectMode 0](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SQL.Statement#%SelectMode).
    
*   From the [SQL Shell](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_shell), specify [SET SELECTMODE LOGICAL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_shell#GSQL_shell_selectmode).
    
*   From the Management Portal select the \[Home\] > \[SQL\] interface, then use the Display Mode drop-down list to specify Logical Mode.
    

SQLCODE Errors

By default, a multi-row UPDATE is an atomic operation. If one or more rows cannot be updated, the UPDATE operation fails and no rows are updated. Caché sets the SQLCODE variable, which indicates the success or failure of the UPDATE, and if the operation failed also sets %msg. To update a table, the update must meet all table, column name, and value requirements, as follows.

Tables:

*   The table must exist in the current (or specified) namespace. If the specified table cannot be located, Caché issues an SQLCODE -30 error.
    
*   The table cannot be defined as READONLY. Attempting to compile an UPDATE that references a read-only table results in an SQLCODE -115 error. Note that this error is now issued at compile time, rather than only occurring at execution time. See the description of READONLY objects in the [Other Options for Persistent Classes](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GOBJ_persother) chapter of Using Caché Objects.
    
*   The table cannot be locked IN EXCLUSIVE MODE by another process. Attempting to update a locked table results in an SQLCODE -110 error, with a %msg such as the following: Unable to acquire lock for UPDATE of table 'Sample.Person' on row with RowID = '10'. Note that an SQLCODE -110 error occurs only when the UPDATE statement locates the first record to be updated, then cannot lock it within the timeout period.
    
*   If the UPDATE specifies a non-existent field, an SQLCODE -29 is issued. To list all of the field names defined for a specified table, refer to [Column Names and Numbers](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_tables#GSQL_tables_getcols) in the “Defining Tables” chapter of Using Caché SQL. If the field exists but none of the field values fulfill the UPDATE command’s WHERE clause, no rows are affected and SQLCODE 100 (end of data) is issued.
    
*   If updating a table through a view, the view cannot be defined as WITH READ ONLY. Attempting to do so results in an SQLCODE -35 error. See the [CREATE VIEW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createview) command for further details.
    

Column Names and Values:

*   The update cannot include duplicate field names. Attempting an update that specifies two fields with the same name results in an SQLCODE -377 error.
    
*   You cannot update a field that has been locked by another concurrent process. Attempting to do so results in an SQLCODE -110 error. This SQLCODE error can also occur if you are performing such a large number of updates that a <LOCKTABLEFULL> error occurs.
    
*   You cannot update integer counter fields. These fields are non-modifiable. Attempting to do so generates the following errors: RowId field (SQLCODE -107); [IDENTITY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable#RSQL_createtable_idfield) field (SQLCODE -107); [%Counter](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datatype#RSQL_datatype_rowversion) ([%Library.Counter](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Library.Counter)) field (SQLCODE -105); [ROWVERSION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datatype#RSQL_datatype_rowversion) field (SQLCODE -138). The field values for these fields are system-generated and not user-modifiable. Even when the user can insert an initial value for a counter fields, the user cannot update the value. See the [CREATE TABLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable) command for further details.
    
*   You cannot update a field value if the update would violate the field’s uniqueness constraints. Attempting to update a field with a uniqueness constraint by specifying a duplicate value results in an SQLCODE -120 error. This error is returned if the field has a UNIQUE [data constraint](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable#RSQL_createtable_fconstraints), or if the [unique fields constraint](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable#RSQL_createtable_uniquefields) has been applied to a group of fields. The SQLCODE -120 %msg string includes both the field and the value that violate the uniqueness constraint. For example, “Table 'Sample.WordTest', Constraint 'WORDTEST\_UNIQUE3', Field(s) Firstword="Pronto"; failed unique check”.
    
*   When using a WHERE CURRENT OF clause, you cannot update a field using the current field value to generate an updated value. For example, SET Salary=Salary+100 or SET Name=UPPER(Name). Attempting to do so results in an SQLCODE -69 error: SET <field> = <value expression> not allowed with WHERE CURRENT OF <cursor>.
    
*   You cannot update a SERIAL data type field unless its currently has no data value (NULL), or has a value of 0. Attempting to do so results in an SQLCODE -105 error.
    
*   If updating one of the specified rows would violate foreign key referential integrity (and %NOCHECK is not specified), the UPDATE fails to update any rows and instead issues an SQLCODE -124 error. This does not apply if the foreign key was defined with the NOCHECK keyword.
    
*   You cannot update a non-stream field with stream data. This results in an SQLCODE -303 error, as described below.
    

List Structures

Caché supports the list structure data type %List (data type class %Library.List). This is a compressed binary format, which does not map to a corresponding native data type for Caché SQL. It corresponds to data type VARBINARY with a default MAXLEN of 32749. For this reason, [Dynamic SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_dynsql) cannot use UPDATE or INSERT to set a property value of type %List. For further details, refer to the [Data Types](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datatype) reference page in this manual.

Stream Values

You cannot use a single UPDATE to modify multiple rows that contain a stream value field. Stream data fields must be updated one row at a time.

You can update a Stream field with a literal value, or with an object reference (oref) to an existing stream object. Caché opens this object and copies its contents into the stream field you wish to update.

You cannot update a non-Stream field with the contents of a Stream field. This results in an SQLCODE -303 error: “No implicit conversion of Stream value to non-Stream field in UPDATE assignment is supported”. To update a string field with Stream data, you must first use the SUBSTRING function to convert the first n characters of the Stream data to a string, as shown in the following example:

UPDATE MyTable
     SET MyStringField\=SUBSTRING(MyStreamField,1,2000)

Computed Fields

A field defined with COMPUTECODE may recompute its value as part of the UPDATE operation, as follows:

*   COMPUTECODE: value is computed and stored upon INSERT, value is not changed upon UPDATE.
    
*   COMPUTECODE with COMPUTEONCHANGE: value is computed and stored upon INSERT, is recomputed and stored upon UPDATE.
    
*   COMPUTECODE with DEFAULT and COMPUTEONCHANGE: default value is stored upon INSERT, value is computed and stored upon UPDATE.
    
*   COMPUTECODE with CALCULATED or TRANSIENT: you cannot UPDATE a value for this field because no value is stored. The value is computed when queried. However, if you attempt to update a value in a calculated field, Caché performs validation on the supplied value and issues an error if the value is invalid. If the value is valid, Caché performs no update operation, issues no SQLCODE error, and increments ROWCOUNT.
    

In most cases, you define a computed field as read-only. This prevents an update operation directly changing a value that is intended to be the result of a computation involving other field values. In this case, attempting to use UPDATE to overwrite the value of a computed field results in an SQLCODE -138 error.

However, you may wish to revise a computed field value to reflect an update to one (or more) of its source field values. You can do this by using an update trigger that recomputes the computed field value after you have updated a specified source field. For example, an update to the Salary data field might trip a trigger that recalculates the Bonus computed field. This update trigger recalculates Bonus and completes successfully, even when Bonus is a read-only field. See the [CREATE TRIGGER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtrigger) statement.

FROM Clause

An UPDATE command may have no FROM keyword. It may simply specify the table (or view) to update, and select which rows to update using a WHERE clause.

However, you can also include an optional [FROM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_from) clause after the value-assignment-statement. This FROM clause specifies one or more tables used to determine which records are to be updated. The FROM clause is commonly, but not always, used with a WHERE clause involving multiple tables. A FROM clause can be complex, and can include ANSI [join syntax](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_join). Any syntax supported in a SELECT FROM clause is permitted in an UPDATE FROM clause. This UPDATE FROM clause provides functionality compatibility with Transact-SQL.

The following example shows how this FROM clauses might be used. It updates those records from the Employees table where the same EmpId is also found in the Retirees table:

UPDATE Employees AS Emp
     SET retired\='Yes'
     FROM Retirees AS Rt
     WHERE Emp.EmpId \= Rt.EmpId

If the UPDATE table-ref and the FROM clause make reference to the same table, these references may either be to the same table, or to a join of two instances of the table. This depends on how table aliases are used:

*   If neither table reference has an alias, both reference the same table:
    
      UPDATE table1 value-assignment FROM table1,table2   /\* join of 2 tables \*/
    
*   If both table references have the same alias, both reference the same table:
    
      UPDATE table1 AS x value-assignment FROM table1 AS x,table2   /\* join of 2 tables \*/
    
*   If both table references have aliases, and the aliases are different, Caché performs a join of two instances of the table:
    
      UPDATE table1 AS x value-assignment FROM table1 AS y,table2   /\* join of 3 tables \*/
    
*   If the first table reference has an alias, and the second does not, Caché performs a join of two instances of the table:
    
      UPDATE table1 AS x value-assignment FROM table1,table2   /\* join of 3 tables \*/
    
*   If the first table reference does not have an alias, and the second has a single reference to the table with an alias, both reference the same table, and this table has the specified alias:
    
      UPDATE table1 value-assignment FROM table1 AS x,table2   /\* join of 2 tables \*/
    
*   If the first table reference does not have an alias, and the second has more than one reference to the table, Caché considers each aliased instance a separate table and performs a join on these tables:
    
      UPDATE table1 value-assignment FROM table1,table1 AS x,table2        /\* join of 3 tables \*/
      UPDATE table1 value-assignment FROM table1 AS x,table1 AS y,table2   /\* join of 4 tables \*/
    

Restriction Arguments

To use a restriction argument, you must have the corresponding admin-privilege for the current namespace. Refer to [GRANT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_grant) for further details.

Specifying restriction argument(s) restricts processing as follows:

*   %NOCHECK — foreign key referential integrity checking is not performed. Column data validation for data type, maximum length, data constraints, and other validation criteria is also not performed. The WITH CHECK OPTION validation for a view is not performed when performing an UPDATE through a view.
    
    Note:
    
    Because use of %NOCHECK can result in invalid data, this restriction argument should only be used when performing bulk inserts or updates from a reliable data source.
    
*   %NOLOCK — the row is not locked upon UPDATE. This should only be used when a single user/process is updating the database.
    
*   %NOINDEX — the index maps are not set during UPDATE processing.
    
*   %NOTRIGGER — the base table triggers are not pulled (executed) during UPDATE processing. Neither BEFORE nor AFTER triggers are executed.
    

You can specify multiple restriction arguments in any order. Multiple arguments are separated by spaces.

Referential Integrity

If you do not specify %NOCHECK, Caché uses the system configuration setting to determine whether to perform foreign key referential integrity checking. You can set the system default as follows:

*   The [$SYSTEM.SQL.SetFilerRefIntegrity()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#SetFilerRefIntegrity) method call.
    
*   Go to the Management Portal, select \[Home\] > \[Configuration\] > \[General SQL Settings\]. View and edit the current setting of Perform Referential Integrity Checks on Foreign Keys for INSERT, UPDATE, and DELETE. The default is “Yes”. If you change this setting, any new process started after changing it will have the new setting.
    

This setting does not apply to foreign keys that have been defined with the NOCHECK keyword.

During an UPDATE operation, for every foreign key reference which has a field value being updated, a shared lock is acquired on both the old (pre-update) referenced row and the new (post-update) referenced row in the referenced table(s). These rows are locked while performing referential integrity checking and updating the row. The lock is then released (it is not held until the end of the transaction). This ensures that the referenced row is not changed between the referential integrity check and the completion of the update operation. Locking the old row ensures that the referenced row is not changed before a potential rollback of the UPDATE. Locking the new row ensures that the referenced row is not changed between the referential integrity checking and the completion of the update operation.

If an UPDATE operation with %NOLOCK is performed on a [foreign key field defined with CASCADE, SET NULL, or SET DEFAULT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable#RSQL_createtable_fkey), the corresponding referential action changing the foreign key table is also performed with %NOLOCK.

Atomicity

By default, UPDATE, INSERT, DELETE, and TRUNCATE TABLE are atomic operations. An UPDATE either completes successfully or the whole operation is rolled back. If any of the specified rows cannot be updated, none of the specified rows are updated and the database reverts to its state before issuing the UPDATE.

You can modify this default for the current process within SQL by invoking [SET TRANSACTION %COMMITMODE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_settransaction). You can modify this default for the current process in Caché ObjectScript by invoking the [SetAutoCommit()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#SetAutoCommit) method. The following options are available:

*   IMPLICIT or 1 (autocommit on) — The default behavior, as described above. Each UPDATE constitutes a separate transaction.
    
*   EXPLICIT or 2 (autocommit off) — If no transaction is in progress, an UPDATE automatically initiates a transaction, but you must explicitly COMMIT or ROLLBACK to end the transaction. In EXPLICIT mode the number of database operations per transaction is user-defined.
    
*   NONE or 0 (no auto transaction) — No transaction is initiated when you invoke UPDATE. A failed UPDATE operation can leave the database in an inconsistent state, with some of the specified rows updated and some not updated. To provide transaction support in this mode you must use START TRANSACTION to initiate the transaction and COMMIT or ROLLBACK to end the transaction.
    

You can determine the atomicity setting for the current process using the [GetAutoCommit()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#SetAutoCommit) method, as shown in the following Caché ObjectScript example:

  DO $SYSTEM.SQL.SetAutoCommit($RANDOM(3))
  SET x\=$SYSTEM.SQL.GetAutoCommit()
  IF x\=1 {
    WRITE "Default atomicity behavior",!
    WRITE "automatic commit or rollback" }
  ELSEIF x\=0 {
    WRITE "No transaction initiated, no atomicity:",!
    WRITE "failed DELETE can leave database inconsistent",!
    WRITE "rollback is not supported" }
  ELSE { WRITE "Explicit commit or rollback required" }

 

Transaction Locking

If you do not specify %NOLOCK, Caché automatically performs standard record locking on INSERT, UPDATE, and DELETE operations. Each affected record (row) is locked for the duration of the current transaction.

The default lock threshold is 1000 locks per table. This means that if you update more than 1000 records from a table during a transaction, the lock threshold is reached and Caché automatically escalates the locking level from record locks to a table lock. This permits large-scale updates during a transaction without overflowing the lock table.

Caché applies one of the two following lock escalation strategies:

*   “E”-type lock escalation: Caché uses this type of lock escalation if the following are true: (1) the class uses [%CacheStorage](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GOBJ_defpersobj#GOBJ_defpersobj_storage) (you can determine this from the [Catalog Details](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_smp#GSQL_smp_catdetails) in the Management Portal SQL schema display). (2) the class either does not specify an IDKey index, or specifies a single-property IDKey index. “E”-type lock escalation is described in the [LOCK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_clock) command in the Caché ObjectScript Reference.
    
*   Traditional SQL lock escalation: The most likely reason why a class would not use “E”-type lock escalation is the presence of a multi-property IDKey index. In this case, each %Save increments the lock counter. This means if you do 1001 saves of a single object within a transaction, Caché will attempt to escalate the lock.
    

For both lock escalation strategies, you can determine the current system-wide lock threshold value using the [$SYSTEM.SQL.GetLockThreshold()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#GetLockThreshold) method. The default is 1000. This system-wide lock threshold value is configurable:

*   Using the [$SYSTEM.SQL.SetLockThreshold()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#SetLockThreshold) method.
    
*   Using the Management Portal. Go to \[Home\] > \[Configuration\] > \[General SQL Settings\]. View and edit the current setting of Lock Threshold. The default is 1000 locks. If you change this setting, any new process started after changing it will have the new setting.
    

You must have USE permission on the %Admin Manage Resource to change the lock threshold. Caché immediately applies any change made to the lock threshold value to all current processes.

On potential consequence of automatic lock escalation is a deadlock situation that might occur when an attempt to escalate to a table lock conflicts with another process holding a record lock in that table. There are several possible strategies to avoid this: (1) increase the lock escalation threshold so that lock escalation is unlikely to occur within a transaction. (2) substantially lower the lock escalation threshold so that lock escalation occurs almost immediately, thus decreasing the opportunity for other processes to lock a record in the same table. (3) apply a table lock for the duration of the transaction and do not perform record locks. This can be done at the start of the transaction by specifying LOCK TABLE, then UNLOCK TABLE (without the IMMEDIATE keyword, so that the table lock persists until the end of the transaction), then perform updates with the %NOLOCK option.

Automatic lock escalation is intended to prevent overflow of the lock table. However, if you perform such a large number of updates that a <LOCKTABLEFULL> error occurs, UPDATE issues an SQLCODE -110 error.

For further details on transaction locking refer to [Transaction Processing](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_modify#GSQL_modify_transaction) in the “Modifying the Database” chapter of Using Caché SQL.

Row-Level Security

Caché row-level security permits UPDATE to modify any row that security permits it to access. It allows you to update a row even if the update creates a row that security will not permit you to subsequently access. To ensure that an update does not prevent you from subsequent SELECT access to the row, it is recommended that you perform the UPDATE through a view that has a WITH CHECK OPTION. For further details, refer to [CREATE VIEW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createview).

ROWVERSION Counter Increment

If a table has a field of data type [ROWVERSION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datatype#RSQL_datatype_rowversion), performing an update on a row automatically updates the integer value of this field. The ROWVERSION field takes the next sequential integer from the namespace-wide row version counter. Attempting to specify an update value to a ROWVERSION field results in an SQLCODE -138 error.

Examples

The examples in this section update the Sample.MyStudents table. The following example creates the Sample.MyStudents table and populates it with data. Because repeated execution of this example would accumulate records with duplicate data, it uses TRUNCATE TABLE to remove old data before invoking INSERT. Execute this example before invoking the UPDATE examples:

CreateStudentTable
  ZNSPACE "Samples"
    SET stuDDL\=5
    SET stuDDL(1)\="CREATE TABLE Sample.MyStudents ("
    SET stuDDL(2)\="StudentName VARCHAR(32),StudentDOB DATE,"
    SET stuDDL(3)\="StudentAge INTEGER COMPUTECODE {SET {StudentAge}="
    SET stuDDL(4)\="$PIECE(($PIECE($H,"","",1)-{StudentDOB})/365,""."",1)} CALCULATED,"
    SET stuDDL(5)\="Q1Grade CHAR,Q2Grade CHAR,Q3Grade CHAR,FinalGrade VARCHAR(2))"
  SET tStatement \= ##class(%SQL.Statement).%New(0,"Sample")
  SET status \= tStatement.%Prepare(.stuDDL)
  IF status '=1 {WRITE !,"%Prepare failed" QUIT }
  SET rtn \= tStatement.%Execute()
  IF rtn.%SQLCODE\=0 {WRITE !,"Table Create successful"}
  ELSEIF rtn.%SQLCODE\=\-201 {WRITE "Table already exists, SQLCODE=",rtn.%SQLCODE,!}  
  ELSE {WRITE !,"table create failed, SQLCODE=",rtn.%SQLCODE,!
        WRITE rtn.%Message,! }
RemoveOldData
  SET clearit\="TRUNCATE TABLE Sample.MyStudents"
  SET truncstat \= tStatement.%Prepare(clearit)
  IF truncstat '=1 {WRITE !,"Truncate %Prepare failed" QUIT }
  SET truncrtn \= tStatement.%Execute()
  IF truncrtn.%SQLCODE\=0 {WRITE !,"Table old data removed",!}
  ELSEIF truncrtn.%SQLCODE\=100 {WRITE !,"no data to be removed",!}
  ELSE {WRITE !,"truncate failed, SQLCODE=",truncrtn.%SQLCODE," ",truncrtn.%Message,! }
PopulateStudentTable
   SET studentpop\=2
   SET studentpop(1)\="INSERT INTO Sample.MyStudents (StudentName,StudentDOB) "
   SET studentpop(2)\="SELECT Name,DOB FROM Sample.Person WHERE Age <= '21'"
  SET popstat \= tStatement.%Prepare(.studentpop)
  IF popstat '=1 {WRITE !,"Populate %Prepare failed" QUIT }
  SET poprtn \= tStatement.%Execute()
  IF poprtn.%SQLCODE\=0 {WRITE !,"Table Populate successful",!
        WRITE poprtn.%ROWCOUNT," rows inserted"}
  ELSE {WRITE !,"table populate failed, SQLCODE=",poprtn.%SQLCODE,!
        WRITE poprtn.%Message }

 

You can use the following query to display the results of these examples:

SELECT %ID,\* FROM Sample.MyStudents ORDER BY StudentAge,%ID

 

Some of the following UPDATE examples depend on field values set by other UPDATE examples; they should be run in the order specified.

In the following [Dynamic SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_dynsql) example, a SET field=value UPDATE modifies a specified field in selected records. In the MyStudents table, children under the age of 7 are not given grades:

  ZNSPACE "Samples"
    SET studentupdate\=3
    SET studentupdate(1)\="UPDATE Sample.MyStudents "
    SET studentupdate(2)\="SET FinalGrade='NA' "
    SET studentupdate(3)\="WHERE StudentAge <= 6"
  SET tStatement \= ##class(%SQL.Statement).%New(0,"Sample")
  SET upstat \= tStatement.%Prepare(.studentupdate)
  IF upstat '=1 {WRITE !,"%Prepare failed" QUIT }
  SET uprtn \= tStatement.%Execute()
  IF uprtn.%SQLCODE\=0 {WRITE !,"Table Update successful"
                       WRITE !,"Rows updated=",uprtn.%ROWCOUNT," Final RowID=",uprtn.%ROWID}
  ELSE {WRITE !,"Table update failed, SQLCODE=",uprtn.%SQLCODE," ",uprtn.%Message }

 

In the following [cursor-based Embedded SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_cursor) example, a SET field1=value1,field2=value2 UPDATE modifies several fields in selected records. In the MyStudents table, it updates specified student records with Q1 and Q2 grades:

  #SQLCompile Path\=Sample
  NEW SQLCODE,%ROWCOUNT,%ROWID
  &sql(DECLARE StuCursor CURSOR FOR 
        SELECT \* FROM MyStudents
        WHERE %ID IN(10,12,14,16,18,20,22,24) AND StudentAge \> 6)
   &sql(OPEN StuCursor)
   FOR { &sql(FETCH StuCursor)
        QUIT:SQLCODE 
        &sql(Update MyStudents SET Q1Grade\='A',Q2Grade\='A'
       WHERE CURRENT OF StuCursor)
    IF SQLCODE\=0 {
    WRITE !,"Table Update successful"
    WRITE !,"Row count=",%ROWCOUNT," RowID=",%ROWID }
    ELSE {
    WRITE !,"Table Update failed, SQLCODE=",SQLCODE }
    }
    &sql(CLOSE StuCursor)

 

In the following [Dynamic SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_dynsql) example, a field-list VALUES value-list UPDATE modifies the values of several fields in selected records. In the MyStudents table, children who don’t receive a final grade also don’t receive quarterly grades:

  ZNSPACE "Samples"
    SET studentupdate\=3
    SET studentupdate(1)\="UPDATE Sample.MyStudents "
    SET studentupdate(2)\="(Q1Grade,Q2Grade,Q3Grade) VALUES ('x','x','x') "
    SET studentupdate(3)\="WHERE FinalGrade='NA'"
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET upstat \= tStatement.%Prepare(.studentupdate)
  IF upstat '=1 {WRITE !,"%Prepare failed" QUIT }
  SET uprtn \= tStatement.%Execute()
  IF uprtn.%SQLCODE\=0 {WRITE !,"Table Update successful"
                       WRITE !,"Rows updated=",uprtn.%ROWCOUNT," Final RowID=",uprtn.%ROWID}
  ELSE {WRITE !,"Table Update failed, SQLCODE=",uprtn.%SQLCODE," ",uprtn.%Message,! }

 

In the following [Dynamic SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_dynsql) example, a VALUES value-list UPDATE modifies all the field values in selected records. Note that this syntax requires that you specify a value for every field in the record. In the MyStudents table, several children have been withdrawn from school. Their record IDs and names are retained, with the word WITHDRAWN appended to the name; all other data is removed and the DOB field is used for the withdrawal date:

  ZNSPACE "Samples"
    SET studentupdate\=4
    SET studentupdate(1)\="UPDATE Sample.MyStudents "
    SET studentupdate(2)\="VALUES (StudentName||' WITHDRAWN',"
    SET studentupdate(3)\="$PIECE($HOROLOG,',',1),00,'-','-','-','XX') "
    SET studentupdate(4)\="WHERE %ID IN(7,10,22)"
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET upstat \= tStatement.%Prepare(.studentupdate)
  IF upstat '=1 {WRITE !,"%Prepare failed" QUIT }
  SET uprtn \= tStatement.%Execute()
  IF uprtn.%SQLCODE\=0 {WRITE !,"Table Update successful"
                       WRITE !,"Rows updated=",uprtn.%ROWCOUNT," Final RowID=",uprtn.%ROWID}
  ELSE {WRITE !,"Table Update failed, SQLCODE=",uprtn.%SQLCODE," ",uprtn.%Message,! }

 

In the following [Dynamic SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_dynsql) example, a subquery UPDATE uses a subquery to select records. It then modifies these records using SET field=value syntax. Because of the way that StudentAge is calculated from date of birth in Sample.MyStudents, anyone less than a year old has a calculated age of <Null>, and anyone whose date of birth has been nulled has a very high calculated age. Here the StudentName field is flagged for future confirmation of the date of birth:

  ZNSPACE "Samples"
    SET studentupdate\=3
    SET studentupdate(1)\="UPDATE (SELECT StudentName FROM Sample.MyStudents "
    SET studentupdate(2)\="WHERE StudentAge IS NULL OR StudentAge > 21) "
    SET studentupdate(3)\="SET StudentName = StudentName||' \*\*\* CHECK DOB' "
  SET tStatement \= ##class(%SQL.Statement).%New(0,"Sample")
  SET upstat \= tStatement.%Prepare(.studentupdate)
  IF upstat '=1 {WRITE !,"%Prepare failed" QUIT }
  SET uprtn \= tStatement.%Execute()
  IF uprtn.%SQLCODE\=0 {WRITE !,"Table Update successful"
                       WRITE !,"Rows updated=",uprtn.%ROWCOUNT," Final RowID=",uprtn.%ROWID}
  ELSE {WRITE !,"Table Update failed, SQLCODE=",uprtn.%SQLCODE," ",uprtn.%Message,! }

 

In the following [Embedded SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql) example, a VALUES :array() UPDATE modifies the field values specified by column number in the array in selected records. A VALUES :array() update can only be done in Embedded SQL. Note that this syntax requires that you specify each value by DDL column number (including the RowId column). In the MyStudents table, children between 4 and 6 (inclusive) are given a ‘P’ (for ‘Present’) in their Q1Grade (column 5) and Q2Grade (column 6) fields. All other record data remains unchanged:

  ZNSPACE "Samples"
  SET arry(5)\="P"
  SET arry(6)\="P"
  &sql(UPDATE Sample.MyStudents VALUES :arry() 
       WHERE FinalGrade\='NA' AND StudentAge \> 3)
  IF SQLCODE\=0 {WRITE "Table Update successful",!
                WRITE "Rows updated=",%ROWCOUNT," Final RowID=",%ROWID }
  ELSE {WRITE "Table Update failed, SQLCODE=",SQLCODE,! }

 

See Also

*   [INSERT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_insert)
    
*   [INSERT OR UPDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_insertorupdate)
    
*   [DELETE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_delete)
    
*   [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select)
    
*   [VALUES](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_values)
    
*   [FROM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_from)
    
*   [WHERE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_where)
    
*   [WHERE CURRENT OF](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_wherecurrentof)
    
*   [CREATE TABLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable)
    
*   [CREATE VIEW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createview)
    
*   “[Modifying the Database](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_modify)” chapter in Using Caché SQL
    
*   “[Defining Tables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_tables)” chapter in Using Caché SQL
    
*   “[Defining Views](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_views)” chapter of Using Caché SQL
    
*   [Transaction Processing](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_modify#GSQL_modify_transaction) in the “Modifying the Database” chapter of Using Caché SQL
    
*   [SQL configuration settings](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RACS_Settings#RACS_Category_SQL) described in Caché Advanced Configuration Settings Reference.
    
*   [SQLCODE error messages](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RERR_sql) listed in the Caché Error Reference
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_unlock "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_usedatabase "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:38**

Source: **RSQL\_update.xml**