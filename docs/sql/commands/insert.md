# INSERT

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_insert

---

 

Caché SQL Reference

INSERT

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_having "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_insertorupdate "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_COMMANDS "SQL Commands") \]  >  \[ [INSERT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_insert "INSERT") \]

Go to: Description Embedded SQL and Dynamic SQL Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Adds a new row (or rows) to a table.

Synopsis

INSERT \[%NOFPLAN\] \[restriction\] \[INTO\] table
          SET column1 = scalar-expression1 {,column2 = scalar-expression2} ...  |
          \[ (column1{,column2} ...) \] VALUES (scalar-expression1 {,scalar-expression2} ...)  |
          VALUES :array()  |
          \[ (column1{,column2} ...) \] query  |
          DEFAULT VALUES

Arguments

%NOFPLAN

Optional — The %NOFPLAN keyword specifies that Caché will ignore the frozen plan (if any) for this operation and generate a new query plan. The frozen plan is retained, but not used. For further details, refer to [Frozen Plans](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_frozenplans) in Caché SQL Optimization Guide.

restriction

Optional — One or more of the following keywords, separated by spaces: %NOLOCK, %NOCHECK, %NOINDEX, %NOTRIGGER.

table

The name of the [table](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_tables) or [view](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_views) on which to perform the insert operation. This argument may be a subquery. The INTO keyword is optional.

column

Optional — A column name or comma-separated list of [column names](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable#RSQL_createtable_fields) that correspond in sequence to the supplied list of values. If omitted, the list of values is applied to all columns in column-number order.

scalar-expression

A scalar expression or comma-separated list of scalar expressions that supplies the data values for the corresponding column fields.

:array()

Embedded SQL only — A dynamic local array of values specified as a [host variable](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_hvars). The lowest subscript level of the array must be unspecified. Thus :myupdates(), :myupdates(5,), and :myupdates(1,1,) are all valid specifications.

query

A query’s result set that supplies the data values for the corresponding column fields for one or more rows.

Description

The INSERT statement can be used in two ways:

*   An INSERT can insert data values for one new row into a table.
    
*   An [INSERT with a SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_insert#RSQL_insertselect) can insert data values for multiple new rows into a table.
    

An INSERT statement adds one new row to a table. This operation sets the %ROWCOUNT variable to the number of affected rows (always either 0 or 1).

An INSERT statement combined with a SELECT statement can insert multiple new rows to a table. This technique is commonly used to populate a table with existing data extracted from other tables. This use of INSERT is described in the “[INSERT Query Results](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_insert#RSQL_insertselect)” section below.

INSERT OR UPDATE

The [INSERT OR UPDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_insertorupdate) statement is a variant of the INSERT statement that performs both insert and update operations. First it attempts to perform an insert operation. If the insert request fails due to a UNIQUE KEY violation (for the field(s) of some unique key, there exists a row that already has the same value(s) as the row specified for the insert), then it automatically turns into an update request for that row, and INSERT OR UPDATE uses the specified field values to update the existing row.

Table and Column Arguments

You can specify the table argument to insert into a table directly, insert through a view, or insert via a subquery. Inserting through a view is subject to requirements and restrictions, as described in [CREATE VIEW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createview). The following is an example of an INSERT using a subquery in place of the table argument:

INSERT INTO (SELECT field1 AS ff1 FROM MyTable) (ff1) VALUES ('test')

The subquery target must be updateable, following the same criteria used to determine if a view's query is updateable. Attempting to INSERT using a view or a subquery that is not updateable generates an SQLCODE -35 error.

If you omit the column list argument, the INSERT assumes all columns are to be inserted, in column number order. If you specify a column list, the individual values must correspond positionally with the column names in the column list.

Privileges

To insert one or more rows of data into a table, you must have either table-level privileges or column-level privileges for that table.

Table-level Privileges

You must have both INSERT and SELECT privileges for the table. Failing to have these privileges results in an SQLCODE -99 error (Privilege Violation). You can determine if the current user has these privileges by invoking the [%CHECKPRIV](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_checkpriv) command. You can determine if a specified user has these privileges by invoking the [$SYSTEM.SQL.CheckPriv()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#CheckPriv) method. For privilege assignment, refer to the [GRANT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_grant) command.

Table-level privileges are equivalent to (but not identical to) having column-level privileges on all columns of the table.

Column-level Privileges

If you do not have table-level INSERT privilege, you must have column-level INSERT privilege for at least one column of the table. To insert a specified value into a column, you must have column-level INSERT privilege for that column. Only those columns for which you have INSERT privilege receive the value specified in the INSERT command.

If you do not have column-level INSERT privilege for a specified column, Caché SQL inserts the column's default value (if defined), or NULL (if no default is defined). If you do not have INSERT privilege for a column that has no default and is defined as NOT NULL, Caché issues an SQLCODE -99 (Privilege Violation) error at Prepare time.

If the INSERT command specifies fields in a WHERE clause of a result set SELECT, you must have SELECT privilege for those fields if they are not data insert fields, and both SELECT and INSERT privileges for those fields if they are included in the result set.

When a property is defined as [ReadOnly](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ROBJ_property_readonly), the corresponding table field is also defined as ReadOnly. A ReadOnly field may only be assigned a value using [InitialExpression](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ROBJ_property_initialexpression) or [SqlComputed](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ROBJ_property_sqlcomputed). Attempting to insert a value for a field for which you have column-level ReadOnly (SELECT or REFERENCES) privilege results in an SQLCODE -138 error: Cannot INSERT/UPDATE a value for a read only field.

You can use [%CHECKPRIV](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_checkpriv) to determine if you have the appropriate column-level privileges. See the [GRANT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_grant) command for privilege assignment.

Value Assignment

When inserting a record, you can assign values to specified columns in a variety of ways. All non-specified columns must have a defined default value.

*   Using the SET keyword, specify one or more column = scalar-expression pairs as a comma-separated list. For example:
    
    SET StatusDate='05/12/06',Status='Purged'
    
*   Using the VALUES keyword, specify a list of columns equated to a corresponding scalar-expressions list. For example:
    
    (StatusDate,Status) VALUES ('05/12/06','Purged')
    
    When assigning scalar-expression values to a column list, there must be a scalar-expression for each specified column.
    
*   Using the VALUES keyword without a column list, specify a list of scalar-expressions that implicitly correspond to the columns of the row in column order. For example:
    
    VALUES ('Fred Wang',65342,'22 Main St. Anytown MA','123-45-6789')
    
    Values must be specified in column number order. You must specify a value for every base table column that takes a user-supplied value; an INSERT using column order cannot take defined field default values. The RowID column cannot be user-specified, and is therefore not included in this syntax.
    
*   Using the VALUES keyword without a column list, specify a dynamic local array of scalar-expressions that implicitly correspond to the columns of the row in column order. For example:
    
    VALUES :myarray()
    
    This value assignment can only be performed from [Embedded SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql) using a host variable. Unlike all other value assignments, this usage allows you to delay specifying which columns are to be inserted until runtime (by populating the array at runtime). All other types of insert require that you specify which columns are to be inserted at compile time.
    
    Values must be specified in column number order. You must specify a value for every base table column that takes a user-supplied value; an INSERT using column order cannot take defined field default values. Supplied array values must begin with array(2). Column 1 is the RowID field; you cannot specify a value for the RowID field. For further details, see “[Host Variable as a Subscripted Array](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_hvarray)” in the “Using Embedded SQL” chapter of Using Caché SQL.
    

If you specify column names and corresponding data values, you can omit columns for which there is a defined default value. An INSERT can insert a default value for most field data types, including stream fields.

If you do not specify column names, data values must correspond positionally to the defined column list. You must specify a value for every user-specifiable base table column; defined default values cannot be used. (You can, of course, specify an empty string as a column value.)

To list all of the column names and column numbers defined for a specified table, refer to [Column Names and Numbers](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_tables#GSQL_tables_getcols) in the “Defining Tables” chapter of Using Caché SQL.

SQLCODE Errors

By default, an INSERT is an all-or-nothing event: either the row (or rows) is inserted completely or not at all. Caché returns a status variable SQLCODE, indicating the success or failure of the INSERT. To insert a row into a table, the insert must meet all table, field name, and field value requirements, as follows.

Tables:

*   The table must already exist. Attempting an insert to a nonexistent table results in an SQLCODE -30 error. Because INSERT checks for the table's existence at compile time, a single compiled SQL program (such as an Embedded SQL program) cannot create a table (using [CREATE TABLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable)) and then insert values into it.
    
*   The table cannot be defined as READONLY. Attempting to compile an INSERT that references a ReadOnly table results in an SQLCODE -115 error. Note that this error is now issued at compile time, rather than only occurring at execution time. See the description of READONLY objects in the [Other Options for Persistent Classes](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GOBJ_persother) chapter of Using Caché Objects.
    
*   If updating a table through a view, the view cannot be defined as WITH READ ONLY. Attempting to do so results in an SQLCODE -35 error. See the [CREATE VIEW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createview) command for further details.
    

Field Names:

*   The field must exist. Attempting an insert to a nonexistent field results in an SQLCODE -29 error. To list all of the field names defined for a specified table, refer to [Column Names and Numbers](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_tables#GSQL_tables_getcols) in the “Defining Tables” chapter of Using Caché SQL.
    
*   The insert must specify all required fields. Attempting to insert a row without specifying a value for a required field results in an SQLCODE -108 error.
    
*   The insert cannot include duplicate field names. Attempting to insert a row containing two fields with the same name results in an SQLCODE -377 error.
    
*   The insert cannot include fields that are defined as READONLY. Attempting to compile an INSERT that references a ReadOnly field results in an SQLCODE -138 error. Note that this error is now issued at compile time, rather than only occurring at execution time.
    

Field Values:

*   Every field value must pass data type validation. Attempting to insert a field value inappropriate to the field data type results in an SQLCODE -104 error. For example, attempting to insert a data value of the wrong data type, or a value larger than the defined maximum data size. Another example is when an invalid DOUBLE number is supplied via ODBC or JDBC. Note that this applies only to a specified data value; a field’s DEFAULT value, if taken, does not have to pass data type validation.
    
*   A field defined as NOT NULL cannot be specified as NULL. Attempting to do so results in an SQLCODE -108 error, indicating that you have not specified a required field.
    
*   A field value must obey uniqueness constraints. Attempting to insert a duplicate field value in a field with a uniqueness constraint results in an SQLCODE -119 error. This error is returned if the field has a UNIQUE [data constraint](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable#RSQL_createtable_fconstraints), or if the [unique fields constraint](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable#RSQL_createtable_uniquefields) has been applied to a group of fields. This error can occur when you specify a duplicate value, or when you do not specify a value and a second use of the field’s DEFAULT would supply a duplicate value. The SQLCODE -119 %msg string includes both the field and the value that violate the uniqueness constraint. For example “Table 'Sample.MyKids', Constraint 'MYKIDS\_UNIQUE1', Field(s) KidName="Molly Bloom"; failed unique check”.
    
*   By default, an insert cannot specify values for fields for which the value is system-generated, such as the RowID, IDKey, or IDENTITY field. By default, attempting to insert a non-NULL field value for any of these fields results in an SQLCODE -111 error. Attempting to insert a NULL for one of these fields causes Caché to override the NULL with a system-generated value; the insert completes successfully and no error code is issued.
    
    If a field of data type [ROWVERSION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datatype#RSQL_datatype_rowversion) is defined, it is automatically assigned a system-generated counter value when a row is inserted. Attempting to insert a value into a ROWVERSION field results in an SQLCODE -138 error.
    
    An IDENTITY field can be made to accept user-specified values. By setting the [SetIdentityInsert()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#SetIdentityInsert) method of the [%SYSTEM.SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL) class you can override the IDENTITY field default constraint and allow inserts of unique integer values to IDENTITY fields. (You can return the current setting for this constraint by calling the [GetIdentityInsert()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#GetIdentityInsert) method.) Inserting an IDENTITY field value changes the IDENTITY counter so that subsequent system-generated values increment from this user-specified value. Attempting to insert a NULL for an IDENTITY field generates an SQLCODE -108 error.
    
    IDKey data has the following restriction. Because multiple IDKey fields in an index are delimited using the “||” (double vertical bar) characters, you cannot include this character string in IDKey field data.
    
*   An insert cannot include a field whose value violates foreign key referential integrity, unless the %NOCHECK restriction argument is specified, or the foreign key was defined with the NOCHECK keyword. Otherwise, attempting an insert that violates foreign key referential integrity results in an SQLCODE -121 error.
    
*   A field value cannot be a subquery. Attempting to specify a subquery as a field value results in an SQLCODE -144 error.
    

INSERT DEFAULT VALUES

You can insert a row into a table that has all of its field values set to default values. Fields that have a defined default value are set to that value. Fields without a defined default value are set to NULL. This is done using the following command:

INSERT INTO Mytable DEFAULT VALUES

Fields defined with the NOT NULL constraint and no defined DEFAULT fail this operation with an SQLCODE -108.

Fields defined with the UNIQUE constraint can be inserted using this statement. If a field is defined with a UNIQUE constraint and no DEFAULT value, repeated invocations insert multiple rows with this UNIQUE field set to NULL. If a field is defined with a UNIQUE constraint and a DEFAULT value, this statement can only be used once. A second invocation fails with an SQLCODE -119.

DEFAULT VALUES inserts a row with a system-generated integer values for counter fields. These include the RowID, and the optional IDENTITY field, %Counter field, and ROWVERSION field.

Insert Counter Values

A table can optionally have one field defined as [IDENTITY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable#RSQL_createtable_idfield). By default, this field receives an integer from an automatically incremented table counter whenever a row is inserted into the table. By default, an insert cannot specify a value for this field. However, this default is configurable. An IDENTITY field value cannot be modified by an update operation. This counter is reset by a TRUNCATE TABLE operation.

A table can optionally have one or more fields defined as data type [%Counter](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datatype#RSQL_datatype_rowversion) ([%Library.Counter](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Library.Counter)). By default, this field receives an integer from an automatically incremented table counter whenever a row is inserted into the table. However, a user can specify an integer value for this field during an insert, overriding the table counter default. A %Counter field value cannot be modified by an update operation. This counter is reset by a TRUNCATE TABLE operation.

A table can optionally have one field defined as data type [ROWVERSION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datatype#RSQL_datatype_rowversion). If this field is defined, an insert operation automatically inserts an integer from a namespace-wide counter into this field. An update operation also automatically inserts an integer from a namespace-wide counter into this field, replacing the prior value. No user-specified, calculated, or default value can be inserted for a ROWVERSION field. This counter cannot be reset.

Insert Computed Values

A field defined with COMPUTECODE may insert a value as part of the INSERT operation, unless the field is CALCULATED. If you supply a value for a COMPUTED field or if this field has a default value, INSERT stores this explicit value. Otherwise, the field value is computed, as follows:

*   COMPUTECODE: value is computed and stored upon INSERT, value is not changed upon UPDATE.
    
*   COMPUTECODE with COMPUTEONCHANGE: value is computed and stored upon INSERT, is recomputed and stored upon UPDATE.
    
*   COMPUTECODE with DEFAULT and COMPUTEONCHANGE: default value is stored upon INSERT, value is computed and stored upon UPDATE.
    
*   COMPUTECODE with CALCULATED or TRANSIENT: you cannot INSERT a value for this field because no value is stored. The value is computed when queried. However, if you attempt to insert a value into a calculated field, Caché performs validation on the supplied value and issues an error if the value is invalid. If the value is valid, Caché performs no insert operation, issues no SQLCODE error, and increments ROWCOUNT.
    

If the compute code contains a programming error (for example, divide by zero), the INSERT operation fails with an SQLCODE -415 error.

DISPLAY to LOGICAL Data Conversion

Data is stored in LOGICAL mode format. For example, a date is stored as an integer count of days. Input data that is not in LOGICAL mode format must be converted to LOGICAL mode format. Compiled SQL supports automatic conversion of input values from DISPLAY or ODBC format to LOGICAL format. Automatic conversion of input data requires two factors: when compiled, the SQL must specify RUNTIME mode; when executed, the SQL must execute in a LOGICAL mode environment.

*   In [Embedded SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql), if you specify #SQLCompile Select=runtime, Caché will compile the SQL statement with code that converts input values from a display format to LOGICAL mode storage format. Caché perfroms this mode conversion both for single values and for arrays of values. For further details, see [#SQLCompile Select](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_macros#GCOS_macros_mpp_lbSQLCompile_Select) in the “ObjectScript Macros and the Macro Preprocessor” chapter of Using Caché ObjectScript.
    
*   In an SQL [CREATE FUNCTION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createfunction), [CREATE METHOD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createmethod), or [CREATE PROCEDURE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createprocedure) statement, if you specify SELECTMODE RUNTIME, Caché will compile the SQL statement with code that converts input values from a display format to LOGICAL mode storage format.
    

The input data may be in any format: DISPLAY format (for example, 6/17/2011), ODBC format (for example, 2011-06-17), or LOGICAL format (for example, 62259). The data is stored in LOGICAL format if the SQL execution environment is in LOGICAL mode. This is the default mode for all Caché SQL execution environments.

You can explicitly set the select mode to LOGICAL in SQL execution environments as follows:

*   In a Caché ObjectScript program or from the Caché Terminal interface: invoke the [$SYSTEM.SQL.SetSelectMode(0)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#SetSelectMode) method.
    
*   In [Dynamic SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_dynsql), specify [%SelectMode 0](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SQL.Statement#%SelectMode).
    
*   From the [SQL Shell](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_shell), specify [SET SELECTMODE LOGICAL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_shell#GSQL_shell_selectmode).
    
*   From the Management Portal select the \[Home\] > \[SQL\] interface, then use the Display Mode drop-down list to specify Logical Mode.
    

INSERT Field of SERIAL Data Type

An INSERT operation can specify one of the following values for a field with the SERIAL data type, with the following results:

*   No value, 0 (zero), or a nonnumeric value: Caché ignores the specified value, and instead increments this field's current serial counter value by 1, and inserts the resulting integer into the field.
    
*   A positive integer value: Caché inserts the user-specified value into the field, and changes the serial counter value for this field to this integer value.
    

Thus a SERIAL field contains a series incremental integer values. These values are not necessarily continuous or unique. For example, the following is a valid series of values for a SERIAL field: 1, 2, 3, 17, 18, 25, 25, 26, 27. Sequential integers are either Caché-generated or user-supplied; nonsequential integers are user-supplied. If you wish SERIAL field values to be unique, you must apply a UNIQUE constraint on the field.

Restriction Arguments

To use a restriction argument, you must have the corresponding admin-privilege for the current namespace. Refer to [GRANT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_grant) for further details.

Specifying restriction argument(s) restricts processing as follows:

*   %NOCHECK — foreign key referential integrity checking is not performed. Column data validation for data type, maximum length, data constraints, and other validation criteria is also not performed. The WITH CHECK OPTION validation for a view is not performed when performing an INSERT through a view.
    
    Note:
    
    Because use of %NOCHECK can result in invalid data, this restriction argument should only be used when performing bulk inserts or updates from a reliable data source.
    
*   %NOLOCK — the row is not locked upon INSERT. This should only be used when a single user/process is updating the database.
    
*   %NOINDEX — the index maps are not set during INSERT processing.
    
*   %NOTRIGGER — the base table triggers are not pulled during INSERT processing.
    

You can specify multiple restriction arguments in any order. Multiple arguments are separated by spaces.

Referential Integrity

If you do not specify %NOCHECK, Caché uses the system configuration setting to determine whether to perform foreign key referential integrity checking. You can set the system default as follows:

*   The [$SYSTEM.SQL.SetFilerRefIntegrity()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#SetFilerRefIntegrity) method call.
    
*   Go to the Management Portal, select \[Home\] > \[Configuration\] > \[General SQL Settings\]. View and edit the current setting of Perform Referential Integrity Checks on Foreign Keys for INSERT, UPDATE, and DELETE. The default is “Yes”. If you change this setting, any new process started after changing it will have the new setting.
    

This setting does not apply to foreign keys that have been defined with the NOCHECK keyword.

During an INSERT operation, for every foreign key reference a shared lock is acquired on the corresponding row in the referenced table. This row is locked while performing referential integrity checking and inserting the row. The lock is then released (it is not held until the end of the transaction). This ensures that the referenced row is not changed between the referential integrity check and the completion of the insert operation.

However, if you specify the %NOLOCK restriction argument, no locking is performed either on the specified table or on the corresponding foreign key row in the referenced table.

Child Table Insert

During an INSERT operation on a child table, a shared lock is acquired on the corresponding row in the parent table. This row is locked while inserting the child table row. The lock is then released (it is not held until the end of the transaction). This ensures that the referenced parent row is not changed during the insert operation.

List Structures

Caché supports the list structure data type %List (data type class %Library.List). This is a compressed binary format, which does not map to a corresponding native data type for Caché SQL. It corresponds to data type VARBINARY with a default MAXLEN of 32749. For this reason, [Dynamic SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_dynsql) cannot use INSERT or UPDATE to set a property value of type %List. For further details, refer to the [Data Types](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datatype) reference page in this manual.

Stream Data

You cannot use a single INSERT to insert multiple rows containing a stream value. Rows containing stream data must be inserted one row at a time.

You can insert the following types of data values into a stream field:

*   An object reference (oref) to a stream object. Caché opens this object and copies its contents into the new stream field. For example:
    
        set oref\=##class(%Stream.GlobalCharacter).%New()
        do oref.Write("Technique 1")
    
        //do the insert; use an actual oref
        &sql(INSERT INTO MyStreamTable (MyStreamField) VALUES (:oref))
    
*   A string version of an oref of a stream, for example:
    
        set oref\=##class(%Stream.GlobalCharacter).%New()
        do oref.Write("Technique 2")
    
        //next line converts oref to a string oref
        set string\=oref\_""
    
        //do the insert
        &sql(INSERT INTO MyStreamTable (MyStreamField) VALUES (:string))
    
*   A numeric value, such as 1 or -1.
    
*   A string literal whose first character is not numeric, for example:
    
        set literal\="Technique 3"
    
        //do the insert; use a string
        &sql(INSERT INTO MyStreamTable (MyStreamField) VALUES (:literal))
    
    If the first character is numeric, SQL interprets the literal as the string form of an oref instead. For example, the value 2@User.MyClass would be considered the string version of an oref, and not a string literal.
    

Attempting to insert an improperly defined stream value results in an SQLCODE -412 error: General Stream Error.

Microsoft Access

To use INSERT to add data to a Caché table using Microsoft Access, either mark the table RowID as private or define a unique index on one or more additional fields.

INSERT Query Results

A single INSERT can be used to insert multiple rows into a table by combining it with a SELECT statement. The SELECT extracts column data from multiple rows of one table, and the INSERT creates corresponding new rows containing this column data in another table. However, you cannot use this technique to insert multiple rows if the data contains a stream value.

You can limit the number of rows inserted by specifying a [TOP clause](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_top) in the [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select) statement. You can also use an [ORDER BY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_order) clause in the SELECT statement to determine which rows will be selected by the TOP clause.

An INSERT with SELECT operation sets the [%ROWCOUNT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_svars) variable to the number of rows inserted (either 0 or a positive integer).

The following example uses two embedded SQL programs to show this use of INSERT. The first example uses CREATE TABLE to create a new table Sample.MyStudents, and the second example populates this table with data extracted from Sample.Person. (Alternatively, you can create a new table from an existing table definition and insert data from the existing table in a single operation using the [$SYSTEM.SQL.QueryToTable()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#QueryToTable) method.)

To demonstrate this, please run the first embedded SQL program, then run the second. (It is necessary to use two embedded SQL programs here because embedded SQL cannot compile an INSERT statement unless the referenced table already exists.)

The following program creates the MyStudents table with two stored data fields, and one calculated field:

   ZNSPACE "Samples"
   WRITE !,"Creating table"
  &sql(CREATE TABLE Sample.MyStudents (
    StudentName VARCHAR(32),
    StudentDOB DATE,
    StudentAge INTEGER COMPUTECODE {SET {StudentAge}\=
                       $PIECE(($PIECE($H,",",1)\-{StudentDOB})/365,".",1)}
                       CALCULATED )
    )
  IF SQLCODE\=0 {
    WRITE !,"Created table, SQLCODE=",SQLCODE }
  ELSEIF SQLCODE\=\-201 {
    WRITE !,"Table already exists, SQLCODE=",SQLCODE }

 

The following program uses INSERT to populate the MyStudents table with query results. Because the StudentAge field is calculated you cannot supply a value to this field; its value is calculated each time the MyStudents table is queried:

  ZNSPACE "Samples"
  WRITE !,"Populating table with data"
  NEW SQLCODE,%ROWCOUNT,%ROWID
  &sql(INSERT INTO Sample.MyStudents (StudentName,StudentDOB)
     SELECT Name,DOB
     FROM Sample.Person WHERE Age <= '21')
  IF SQLCODE\=0 {
    WRITE !,"Number of records inserted=",%ROWCOUNT
    WRITE !,"Row ID of last record inserted=",%ROWID }
  ELSE {
    WRITE !,"Insert failed, SQLCODE=",SQLCODE }

 

Note that executing this INSERT program multiple times will succeed, but produces generally undesirable results. Each execution populates Sample.MyStudents with another set of records (%ROWCOUNT) with identical Name and DOB field values, automatically assigning each record a unique row ID (%ROWID).

To view the data, go to the Management Portal, select the Globals option for the SAMPLES namespace. Scroll to “Sample.MyStudentsD” and click the Data option.

The following programs display the MyStudents table data, then delete this table:

SELECT \* FROM Sample.MyStudents ORDER BY StudentAge

 

  &sql(DROP TABLE Sample.MyStudents)
  IF SQLCODE\=0 {WRITE !,"Table deleted" }
  ELSE {WRITE !,"SQLCODE=",SQLCODE," ",%msg }

 

By default, an Insert Query Results operation is an atomic operation. Either all of the specified rows are inserted in a table, or none of the rows are inserted. For example, if inserting one of the specified rows would violate foreign key referential integrity, the INSERT fails and no rows are inserted. This default is modifiable, as described below.

Atomicity

By default, INSERT, UPDATE, DELETE, and TRUNCATE TABLE are atomic operations. An INSERT either completes successfully or the whole operation is rolled back. If any of the specified rows cannot be inserted, none of the specified rows are inserted and the database reverts to its state before issuing the INSERT.

You can modify this default for the current process within SQL by invoking [SET TRANSACTION %COMMITMODE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_settransaction). You can modify this default for the current process in Caché ObjectScript by invoking the [SetAutoCommit()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#SetAutoCommit) method. The following options are available:

*   IMPLICIT or 1 (autocommit on) — The default behavior, as described above. Each INSERT constitutes a separate transaction.
    
*   EXPLICIT or 2 (autocommit off) — If no transaction is in progress, an INSERT automatically initiates a transaction, but you must explicitly COMMIT or ROLLBACK to end the transaction. In EXPLICIT mode the number of database operations per transaction is user-defined.
    
*   NONE or 0 (no auto transaction) — No transaction is initiated when you invoke INSERT. A failed INSERT operation can leave the database in an inconsistent state, with some of the specified rows inserted and some not inserted. To provide transaction support in this mode you must use START TRANSACTION to initiate the transaction and COMMIT or ROLLBACK to end the transaction.
    

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

The default lock threshold is 1000 locks per table. This means that if you insert more than 1000 records from a table during a transaction, the lock threshold is reached and Caché automatically escalates the locking level from record locks to a table lock. This permits large-scale inserts during a transaction without overflowing the lock table.

Caché applies one of the two following lock escalation strategies:

*   “E”-type lock escalation: Caché uses this type of lock escalation if the following are true: (1) the class uses [%CacheStorage](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GOBJ_defpersobj#GOBJ_defpersobj_storage) (you can determine this from the [Catalog Details](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_smp#GSQL_smp_catdetails) in the Management Portal SQL schema display). (2) the class either does not specify an IDKey index, or specifies a single-property IDKey index. “E”-type lock escalation is described in the [LOCK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_clock) command in the Caché ObjectScript Reference.
    
*   Traditional SQL lock escalation: The most likely reason why a class would not use “E”-type lock escalation is the presence of a multi-property IDKey index. In this case, each %Save increments the lock counter. This means if you do 1001 saves of a single object within a transaction, Caché will attempt to escalate the lock.
    

For both lock escalation strategies, you can determine the current systemwide lock threshold value using the [$SYSTEM.SQL.GetLockThreshold()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#GetLockThreshold) method. The default is 1000. This systemwide lock threshold value is configurable:

*   Using the [$SYSTEM.SQL.SetLockThreshold()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#SetLockThreshold) method.
    
*   Using the Management Portal. Go to \[Home\] > \[Configuration\] > \[General SQL Settings\]. View and edit the current setting of Lock Threshold. The default is 1000 locks. If you change this setting, any new process started after changing it will have the new setting.
    

You must have USE permission on the %Admin Manage Resource to change the lock threshold. Caché immediately applies any change made to the lock threshold value to all current processes.

On potential consequence of automatic lock escalation is a deadlock situation that might occur when an attempt to escalate to a table lock conflicts with another process holding a record lock in that table. There are several possible strategies to avoid this: (1) increase the lock escalation threshold so that lock escalation is unlikely to occur within a transaction. (2) substantially lower the lock escalation threshold so that lock escalation occurs almost immediately, thus decreasing the opportunity for other processes to lock a record in the same table. (3) apply a table lock for the duration of the transaction and do not perform record locks. This can be done at the start of the transaction by specifying LOCK TABLE, then UNLOCK TABLE (without the IMMEDIATE keyword, so that the table lock persists until the end of the transaction), then perform inserts with the %NOLOCK option.

Automatic lock escalation is intended to prevent overflow of the lock table. However, if you perform such a large number of inserts that a <LOCKTABLEFULL> error occurs, INSERT issues an SQLCODE -110 error.

For further details on transaction locking refer to [Transaction Processing](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_modify#GSQL_modify_transaction) in the “Modifying the Database” chapter of Using Caché SQL.

Row-Level Security

Caché row-level security permits INSERT to add a row even if the row security is defined so that you will not be permitted to subsequently access the row. To ensure that an INSERT does not prevent you from subsequent SELECT access to the row, it is recommended that you perform the INSERT through a view that has a WITH CHECK OPTION. For further details, refer to [CREATE VIEW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createview).

Embedded SQL and Dynamic SQL Examples

The following Embedded SQL example creates a new table Sample.MyKids. The examples that follow use INSERT to populate this table with data. After the INSERT examples, an example is provided to delete Sample.MyKids.

CreateTable
   ZNSPACE "Samples"
   &sql(CREATE TABLE Sample.MyKids (
    KidName VARCHAR(16) UNIQUE NOT NULL,
    KidDOB INTEGER NOT NULL,
    KidPetName VARCHAR(16) DEFAULT 'no pet') )
  IF SQLCODE\=0 {
    WRITE !,"Table created" }
  ELSEIF SQLCODE\=\-201 {WRITE !,"Table already exists"  QUIT}
  ELSE {
    WRITE !,"CREATE TABLE failed. SQLCODE=",SQLCODE }

 

The following [Embedded SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql) example inserts a row with two field values (the third field, KidPetName, takes a default value). Note that the table schema name is supplied by the [#SQLCompile Path](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_macros#GCOS_macros_mpp_lbSQLCompile_Path) macro directive:

EmbeddedSQLInsertByColName
  #SQLCompile Path\=Sample
  NEW SQLCODE,%ROWCOUNT,%ROWID
  &sql(INSERT INTO MyKids (KidName,KidDOB) VALUES 
   ('Molly',60000))
  IF SQLCODE\=0 {
    WRITE !,"Insert succeeded"
    WRITE !,"Row count=",%ROWCOUNT
    WRITE !,"Row ID=",%ROWID
    QUIT }
  ELSEIF SQLCODE\=\-119 {
    WRITE !,"Duplicate record not written",!
    WRITE %msg,!
    QUIT }
  ELSE {
    WRITE !,"Insert failed, SQLCODE=",SQLCODE }

 

The following [Embedded SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql) example inserts a row with three field values using the table's column order:

EmbeddedSQLInsertByColOrder
  NEW SQLCODE,%ROWCOUNT,%ROWID
  &sql(INSERT INTO Sample.MyKids VALUES ('Josie','40100','Fido') )
  IF SQLCODE\=0 {
    WRITE !,"Insert succeeded"
    WRITE !,"Row count=",%ROWCOUNT
    WRITE !,"Row ID=",%ROWID
    QUIT }
  ELSEIF SQLCODE\=\-119 {
    WRITE !,"Duplicate record not written",!
    WRITE %msg,!
    QUIT }
  ELSE {
    WRITE !,"Insert failed, SQLCODE=",SQLCODE }

 

The following [Embedded SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql) example uses host variables to insert a row with two field values. The INSERT syntax used here specifies column\=value pairs:

EmbeddedSQLInsertHostVars
  #SQLCompile Path\=Sample
  NEW SQLCODE,%ROWCOUNT,%ROWID
  SET x \= "Sam"
  SET y \= "57555"
  &sql(INSERT INTO MyKids SET KidName\=:x,KidDOB\=:y )
  IF SQLCODE\=0 {
    WRITE !,"Insert succeeded"
    WRITE !,"Row count=",%ROWCOUNT
    WRITE !,"Row ID=",%ROWID
    QUIT }
  ELSEIF SQLCODE\=\-119 {
    WRITE !,"Duplicate record not written",!
    WRITE %msg,!
    QUIT }
  ELSE {
    WRITE !,"Insert failed, SQLCODE=",SQLCODE }

 

The following [Embedded SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql) example uses a host variable array to insert a row with three field values. Array elements are numbered in column order. Note that user-supplied array values start with myarray(2); the first array element corresponds to the RowID, which is automatically supplied and cannot be user-defined:

EmbeddedSQLInsertHostVarArray
  #SQLCompile Path\=Sample
  NEW SQLCODE,%ROWCOUNT,%ROWID
  SET myarray(2)\="Deborah"
  SET myarray(3)\=60200
  SET myarray(4)\="Bowie"
  &sql(INSERT INTO MyKids VALUES :myarray())
  IF SQLCODE\=0 {
    WRITE !,"Insert succeeded"
    WRITE !,"Row count=",%ROWCOUNT
    WRITE !,"Row ID=",%ROWID
    QUIT }
  ELSEIF SQLCODE\=\-119 {
    WRITE !,"Duplicate record not written",!
    WRITE %msg,!
    QUIT }
  ELSE {
    WRITE !,"Insert failed, SQLCODE=",SQLCODE }

 

The following Caché ObjectScript [Dynamic SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_dynsql) example uses the [%SQL.Statement](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SQL.Statement) class to insert a row with three field values. Note that the table schema name is supplied in the [%New()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SQL.Statement#%New) method:

COSDynamicSQLInsert
  SET x \= "Noah"
  SET y \= "61000"
  SET z \= "Luna"
  SET sqltext \= "INSERT INTO MyKids (KidName,KidDOB,KidPetName) VALUES (?,?,?)"
  SET tStatement \= ##class(%SQL.Statement).%New(0,"Sample")
  SET tStatus \= tStatement.%Prepare(sqltext)
    IF tStatus '=1 {WRITE !,"%Prepare failed"  QUIT }
  SET rtn \= tStatement.%Execute(x,y,z)
  IF rtn.%SQLCODE\=0 {
    WRITE !,"Insert succeeded"
    WRITE !,"Row count=",rtn.%ROWCOUNT
    WRITE !,"Row ID=",rtn.%ROWID }
  ELSEIF rtn.%SQLCODE\=\-119 {
    WRITE !,"Duplicate record not written",!,rtn.%Message
    QUIT }
  ELSE {
    WRITE !,"Insert failed, SQLCODE=",rtn.%SQLCODE }

 

The following Caché Basic [Dynamic SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_dynsql) example uses the [%SQL.Statement](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SQL.Statement) class to insert a row with three field values:

BasicDynamicSQLInsert:
  x \= "Martha"
  y \= "59000"
  z \= "Avery"
  sqltext \= "INSERT INTO MyKids (KidName,KidDOB,KidPetName) VALUES (?,?,?)"
  tStatement \= New %SQL.Statement(0,"Sample")
  status \= tStatement.%Prepare(sqltext)
  IF status<\>1 THEN
      PrintLn "%Prepare failed"
      Return
  END IF
  rtn \= tStatement.%Execute(x,y,z)
  IF rtn.%SQLCODE\=0 THEN
    Println "Insert succeeded"
    Println "Row count=",rtn.%ROWCOUNT
    Println "Row ID=",rtn.%ROWID
  ELSE
    Println "Insert failed SQLCODE=",rtn.%SQLCODE
    Println rtn.%Message
  END IF

 

For further details, refer to the [Embedded SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql) and [Dynamic SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_dynsql) chapters in Using Caché SQL.

The following Embedded SQL example displays the inserted records, then deletes the Sample.MyKids table:

DisplayAndDeleteTable
  ZNSPACE "Samples"
  SET myquery \= "SELECT \* FROM Sample.MyKids"
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET tStatus \= tStatement.%Prepare(myquery)
    IF tStatus '=1 {WRITE !,"%Prepare failed"  QUIT }
  SET rset \= tStatement.%Execute()
  DO rset.%Display()
  WRITE !,"End of data"
  &sql(DROP TABLE Sample.MyKids)
   IF SQLCODE\=0 {
    WRITE !,"Deleted table"
    QUIT }
  ELSE {
    WRITE !,"Table delete failed, SQLCODE=",SQLCODE }

 

The following [Embedded SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql) example demonstrates the use of a host variable arrays. Note that with a host variable array, you can use a dynamic local array with an unspecified last subscript to pass an array of values to INSERT at runtime. For example:

  NEW SQLCODE,%ROWCOUNT,%ROWID
  &sql(INSERT INTO Sample.Employee VALUES :emp('profile',))
  WRITE !,"SQL Error code: ",SQLCODE," Row Count: ",%ROWCOUNT

causes each field in the inserted "Employee" row to be set to:

emp("profile",col)

where "col" is the field’s column number in the Sample.Employee table.

The following example shows how the results of a SELECT query can be used as the data input into an INSERT statement, supplying the data for multiple rows:

INSERT INTO StudentRoster (NAME,GPA,ID\_NUM)
     SELECT FullName,GradeAvg,ID
     FROM temp WHERE SchoolYear \= '2004'

See Also

*   [INSERT OR UPDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_insertorupdate)
    
*   [UPDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_update)
    
*   [DELETE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_delete)
    
*   [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select)
    
*   [CREATE TABLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable)
    
*   [JOIN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_join)
    
*   [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select)
    
*   [VALUES](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_values)
    
*   “[Modifying the Database](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_modify)” chapter in Using Caché SQL
    
*   “[Defining Tables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_tables)” chapter in Using Caché SQL
    
*   “[Defining Views](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_views)” chapter in Using Caché SQL
    
*   [Transaction Processing](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_modify#GSQL_modify_transaction) in the “Modifying the Database” chapter of Using Caché SQL
    
*   [SQL configuration settings](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RACS_Settings#RACS_Category_SQL) described in Caché Advanced Configuration Settings Reference.
    
*   [SQLCODE error messages](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RERR_sql) listed in the Caché Error Reference
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_having "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_insertorupdate "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_insert.xml**