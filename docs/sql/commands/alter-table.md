# ALTER TABLE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_altertable

---

 

Caché SQL Reference

ALTER TABLE

 [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_alteruser "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_COMMANDS "SQL Commands") \]  >  \[ [ALTER TABLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_altertable "ALTER TABLE") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Modifies a table.

Synopsis

ALTER TABLE table alter-table-action

where alter-table-action is one of the following:
     ADD add-action
     DROP drop-action
     DELETE drop-action
     ALTER \[COLUMN\] identifier alter-column-action
     MODIFY modification-spec

add-action ::= 
     \[CONSTRAINT table\]
     \[(\] FOREIGN KEY identifier (identifier-commalist) 
          REFERENCES table (identifier-commalist)
          \[referential-action\] \[)\]
     |
     \[(\] UNIQUE (identifier-commalist) 
     |
     \[(\] PRIMARY KEY identifier (identifier-commalist) \[)\] 
     | 
     DEFAULT \[(\] default-spec \[)\] FOR identifier
     |
     \[COLUMN\] \[(\] identifier datatype  \[sqlcollation\] 
           \[ %DESCRIPTION literal \]
           \[ DEFAULT \[(\] default-spec \[)\] \]
           \[ UNIQUE\] \[NOT NULL\]
           \[)\]

drop-action ::= 
     FOREIGN KEY identifier |
     PRIMARY KEY |
     CONSTRAINT identifier |
     \[COLUMN\] identifier \[RESTRICT | CASCADE\] 

alter-column-action ::= 
     SET DEFAULT \[(\]default-spec\[)\] |
     DEFAULT \[(\]default-spec\[)\] |
     DROP DEFAULT | 
     NULL | 
     NOT NULL | 
     COLLATE sqlcollation |
     datatype 

modification-spec ::=
     identifier \[datatype\] 
          \[DEFAULT \[(\]default-spec\[)\]\]
          \[CONSTRAINT identifier\] \[NULL\] \[NOT NULL\]

sqlcollation ::=
     {%ALPHAUP | %EXACT | %MINUS | %PLUS | %SPACE |   
       %SQLSTRING \[(literal)\] | %SQLUPPER \[(literal)\] |
       %STRING \[(literal)\] | %UPPER | %MVR }

Arguments

table

The name of the table to be altered.

identifier

The name of the column to be modified. For further details on valid [identifiers](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_identifiers), see the “Identifiers” chapter of Using Caché SQL.

datatype

A valid Caché SQL data type. For a list of valid [data types](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datatype), see the SQL reference material at the end of this manual.

default-spec

A default data value automatically supplied for this field, if not overridden by a user-supplied data value. Allowed values are: a literal value; one of the following keyword options (NULL, USER, CURRENT\_USER, SESSION\_USER, SYSTEM\_USER, CURRENT\_DATE, CURRENT\_TIME, and CURRENT\_TIMESTAMP); or an OBJECTSCRIPT expression. Do not use the SQL [zero-length string](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_langelements#GSQL_langelements_null) as a default value. For further details, see [CREATE TABLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable).

sqlcollation

One of the following SQL collation types: %ALPHAUP, %EXACT, %MINUS, %PLUS, %SPACE, %SQLSTRING, %SQLUPPER, %UPPER, %STRING, or %MVR. %SQLSTRING, %SQLUPPER, and %STRING may be specified with an optional maximum length truncation argument, an integer enclosed in parentheses. The percent sign (%) prefix to these collation parameter keywords is optional. For further details refer to [CREATE TABLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable) and [Collation](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_basics#GSQL_basics_collation) in the “Caché SQL Basics” chapter of Using Caché SQL.

Description

An ALTER TABLE statement modifies a table definition; it can add elements, remove elements, or modify existing elements. You can only perform one operation in each ALTER TABLE statement. The ALTER TABLE DROP statement and the ALTER TABLE DELETE statement are synonyms.

To determine if a specified table exists in the current namespace, use the [$SYSTEM.SQL.TableExists()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#TableExists) method.

Privileges and Locking

The ALTER TABLE command is a privileged operation. Prior to using ALTER TABLE it is necessary for your process to have either %ALTER\_TABLE administrative privilege or an %ALTER object privilege for the specified table. Failing to do so results in an SQLCODE -99 error (Privilege Violation). You can determine if the current user has %ALTER privilege by invoking the [%CHECKPRIV](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_checkpriv) command. You can determine if a specified user has %ALTER privilege by invoking the [$SYSTEM.SQL.CheckPriv()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#CheckPriv) method. You can use the GRANT command to assign %ALTER\_TABLE or %ALTER privileges, if you hold appropriate granting privileges. In embedded SQL, you can use the [$SYSTEM.Security.Login()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Security#Login) method to log in as a user with appropriate privileges:

   DO $SYSTEM.Security.Login("\_SYSTEM","SYS")
   &sql(      )

You must have the %Service\_Login:Use privilege to invoke the $SYSTEM.Security.Login method. For further information, refer to [%SYSTEM.Security](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Security) in the InterSystems Class Reference.

ALTER TABLE cannot be used on a [table created by defining a persistent class](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_tables#GSQL_tables_via_classes), unless the table class definition includes \[[DdlAllowed](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ROBJ_class_ddlallowed)\]. Otherwise, the operation fails with an SQLCODE -300 error with the %msg DDL not enabled for class 'Schema.tablename'>

The ALTER TABLE statement acquires a table-level lock on table. This prevents other processes from modifying the table’s data. This lock is automatically released at the conclusion of the ALTER TABLE operation. When ALTER TABLE locks the corresponding class definition, it uses the [SQL Lock Timeout setting](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_setoption) for the current process.

ADD COLUMN Restrictions

If you attempt to add a field to a table through an ALTER TABLE tablename ADD COLUMN statement:

*   If a column of that name already exists, the statement fails with an SQLCODE -306 error.
    
*   If the statement specifies a NOT NULL constraint on the column and there is no default value for the column, then the statement fails if data already exists in the table. This is because, after the completion of the DDL statement, the NOT NULL constraint is not satisfied for all the pre-existing rows. This generates the [error code](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RERR_sql) SQLCODE -304 (Attempt to add a NOT NULL field with no default value to a table which contains data).
    
*   If the statement specifies a NOT NULL constraint on the column and there is a default value for the column, the statement updates any existing rows in the table and assigns the default value for the column to the field.
    
*   If the statement DOES NOT specify a NOT NULL constraint on the column and there is a default value for the column, then there are no updates of the column in any existing rows. The column value is NULL for those rows.
    

To change this default NOT NULL constraint behaviors, refer to the COMPILEMODE=NOCHECK option of the [SET OPTION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_setoption) command.

If you specify an ordinary data field named “ID” and the [RowID](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable#RSQL_createtable_rowid) field is already named “ID” (the default), the ADD COLUMN operation succeeds. ALTER TABLE adds the ID data column, and renames the RowId column as “ID1” to avoid duplicate names.

If you attempt to add an integer counter field to a table through an ALTER TABLE tablename ADD COLUMN statement:

*   You can add an [IDENTITY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable#RSQL_createtable_idfield) field to a table if the table does not have an IDENTITY field. When you use ADD COLUMN to define this field, Caché populates this field with sequential integers for existing data rows. If the table already has an IDENTITY field, the ALTER TABLE operation fails with an SQLCODE -400 error.
    
    If CREATE TABLE defined a [bitmap extent index](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable#RSQL_createtable_bitmapextent) and later you add an IDENTITY field to the table, and the IDENTITY field is not of type %BigInt, %Integer, %SmallInt, or %TinyInt with a MINVAL of 1 or higher, and there is no data in the table, Caché automatically drops the bitmap extent index.
    
*   You can add one or more [%Counter](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datatype#RSQL_datatype_rowversion) ([%Library.Counter](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Library.Counter)) fields to a table. When you use ADD COLUMN to define this field, existing data rows are NULL for this field.
    
*   You can add a [ROWVERSION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datatype#RSQL_datatype_rowversion) field to a table if the table does not have a ROWVERSION field. When you use ADD COLUMN to define this field, existing data rows are NULL for this field. If the table already has a ROWVERSION field, the ALTER TABLE operation fails with an SQLCODE -400 error.
    

ALTER COLUMN Restriction

You cannot change the [data type](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datatype) of a column that contains data if this change would result in stream data being typed as non-stream data or non-stream data being typed as stream data. Attempting to do so results in an SQLCODE -374 error. If there is no existing data, this type of datatype change is permitted.

If you change the [collation type](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_basics#GSQL_basics_collation) for a column that contains data, you must [rebuild all indices](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_indices#GSQLOPT_indices_build) for that column.

DROP COLUMN Restrictions

You cannot drop a column if that column is used in COMPUTECODE or in a COMPUTEONCHANGE clause. Attempting to do so results in an SQLCODE -400 error.

Deleting a column definition does not delete the corresponding column-level privileges. For example, the privilege granted to a user to insert, update, or delete data on that column. This has the following consequences:

*   If a column is deleted, and then another column with the same name is added, users and roles will have the same privileges on the new column that they had on the old column.
    
*   Once a column is deleted, it is not possible to revoke object privileges for that column.
    

For these reasons, it is generally recommended that you use the [REVOKE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_revoke) command to revoke column-level privileges from a column before deleting the column definition.

If the column to be deleted appears in an index, or is defined in a foreign key constraint or other unique constraint, attempting a DROP COLUMN for that column fails with an SQLCODE -322 error.

ADD PRIMARY KEY Restrictions

You cannot add a primary key constraint to an existing field if that field contains non-unique data or permits NULL values.

If you add a primary key constraint to an existing field, the field may also be automatically defined as an IDKey index. This depends on whether data is present and upon a configuration setting established in one of the following ways:

*   The SQL [SET OPTION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_setoption) PKEY\_IS\_IDKEY statement.
    
*   The [$SYSTEM.SQL.SetDDLPKeyNotIDKey()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#SetDDLPKeyNotIDKey) method call. To determine the current setting, call [$SYSTEM.SQL.CurrentSettings()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#CurrentSettings).
    
*   Go to the Management Portal, select \[Home\] > \[Configuration\] > \[General SQL Settings\]. View the current setting of Are Primary Keys Created through DDL not ID Keys. If set to “Yes” (1), when a Primary Key constraint is specified through DDL it does not automatically become the IDKey index in the class definition. If “No” (0), it does become the IDKey index. Setting this value to “No” can give better performance, but means that the Primary Key fields cannot be updated.
    

The default is “Yes” (1). If this option is set to “No” (0), and the field does not contain data, the primary key index is also defined as the IDKey index. If this option is set to “No”, and the field does contain data, the IDKey index is not defined.

If CREATE TABLE defined a [bitmap extent index](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable#RSQL_createtable_bitmapextent) and later you use ALTER TABLE to add a primary key that is also the IDKey, Caché automatically drops the bitmap extent index.

ADD PRIMARY KEY When Already Exists

What happens when you try to add a primary key to a table that already has a defined primary key is configuration-dependent. By default, Caché rejects an attempt to define a primary key when one already exists and issues an SQLCODE -307 error. You can set this behavior as follows:

*   The [$SYSTEM.SQL.SetDDLNo307()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#SetDDLNo307) method call. To determine the current setting, call [$SYSTEM.SQL.CurrentSettings()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#CurrentSettings), which displays a Suppress SQLCODE=-307 Errors setting.
    
*   Go to the Management Portal, select \[Home\] > \[Configuration\] > \[General SQL Settings\]. View the current setting of Allow Create Primary Key Through DDL When Key Exists.
    

The default is “No” (0). This is the recommended setting for this option.

If this option is set to “Yes” (1), an ALTER TABLE ADD PRIMARY KEY causes Caché to remove the primary key index from the class definition, and then recreates this index using the specified primary key field(s).

However, even if this option is set to allow the creation of a primary key when one already exists, you cannot recreate a primary key index if it is also the IDKEY index and the table contains data. Attempting to do so generates an SQLCODE -307 error.

ADD FOREIGN KEY Restrictions

By default, you cannot have two foreign keys with the same name. Attempting to do so generates an SQLCODE -311 error. This option is configurable using:

*   The [$SYSTEM.SQL.SetDDLNo311()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#SetDDLNo311) method call. To determine the current setting, call [$SYSTEM.SQL.CurrentSettings()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#CurrentSettings), which displays a Suppress SQLCODE=-311 Errors setting.
    
*   Go to the Management Portal, select \[Home\] > \[Configuration\] > \[General SQL Settings\]. View the current setting of Allow DDL ADD Foreign Key Constraint When Foreign Key Exists.
    

The default is “No” (0). This is the recommended setting for this option. When “Yes” (1), you can add a foreign key through DDL even if one with the same name already exists. When “No” (0), this action generates an SQLCODE -311 error.

Your table definition should not have two foreign keys with different names that reference the same identifier-commalist field(s) and perform contradictory referential actions. In accordance with the ANSI standard, Caché SQL does not issue an error if you define two foreign keys that perform contradictory referential actions on the same field (for example, ON DELETE CASCADE and ON DELETE SET NULL). Instead, Caché SQL issues an error when a DELETE or UPDATE operation encounters these contradictory foreign key definitions.

An ADD FOREIGN KEY is constrained when data already exists in the table. To change this default constraint behavior, refer to the COMPILEMODE=NOCHECK option of the [SET OPTION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_setoption) command.

An ADD FOREIGN KEY that references a field (or combination of fields) that can take non-unique values fails with an SQLCODE -314 error, with additional details available through %msg.

When you define an ADD FOREIGN KEY constraint for a single field and the foreign key references the idkey of the referenced table, Caché converts the property in the foreign key into a reference property. This conversion is subject to the following restrictions:

*   The table must contain no data.
    
*   The property on the foreign key cannot be of a persistent class (that is, it cannot already be a reference property).
    
*   The data types and data type parameters of the foreign key field and the referenced idkey field must be the same.
    
*   The foreign key field cannot be an [IDENTITY field](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable#RSQL_createtable_idfield).
    

For further information on foreign keys, refer to the [CREATE TABLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable) command, and to the “[Using Foreign Keys](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_foreignkeys)” chapter in Using Caché SQL.

DROP CONSTRAINT Restrictions

By default, you cannot drop a unique or primary key constraint if it is referenced by a foreign key constraint. Attempting to do so results in an SQLCODE -317 error. To change this default foreign key constraint behavior, refer to the COMPILEMODE=NOCHECK option of the [SET OPTION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_setoption) command.

The effects of dropping a primary key constraint depend on the setting of the Are Primary Keys also ID Keys setting (as described above):

*   If the PrimaryKey index is not also the IDKey index, dropping the primary key constraint drops the index definition.
    
*   If the PrimaryKey index is also the IDKey index, and there is no data in the table, dropping the primary key constraint drops the entire index definition.
    
*   If the PrimaryKey index is also the IDKey index, and there is data in the table, dropping the primary key constraint just drops the PRIMARYKEY qualifier from the IDKey index definition.
    

DROP CONSTRAINT When Non-Existent

What happens when you try to drop a field constraint on a field that does not have that constraint depends on a configuration setting.

*   The [$SYSTEM.SQL.SetDDLNo315()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#SetDDLNo315) method call. To determine the current setting, call [$SYSTEM.SQL.CurrentSettings()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#CurrentSettings), which displays a Suppress SQLCODE=-315 Errors setting.
    
*   Go to the Management Portal, select \[Home\] > \[Configuration\] > \[General SQL Settings\]. View the current setting of Allow DDL DROP of Non-constraint.
    

The default is “No” (0). By default, Caché rejects an attempt to drop a constraint that does not exist and issues an SQLCODE -315 error. However, if this option is set to “Yes” (1), an ALTER TABLE DROP CONSTRAINT causes Caché to perform no operation and not issue an error message.

Examples

The following example uses two embedded SQL programs to create a table, populate two rows, and then alter the table definition. The ALTER TABLE command creates the ColorPreference column and populates it with the value 'Blue' for the two pre-existing rows of the table.

To demonstrate this, please run the two embedded SQL programs in the order shown. (It is necessary to use two embedded SQL programs here because embedded SQL cannot compile an INSERT statement unless the referenced table already exists.)

  DO $SYSTEM.Security.Login("\_SYSTEM","SYS")
  &sql(DROP TABLE Sample.PTest)
      IF SQLCODE\=0 { WRITE !,"Deleted table" }
      ELSE { WRITE "DROP TABLE error SQLCODE=",SQLCODE }
  &sql(CREATE TABLE Sample.PTest (
     Id      INT NOT NULL,
     Name    VARCHAR(35),
     DOB     DATE,
     CONSTRAINT PTestPK PRIMARY KEY (Id) )
     )
     IF SQLCODE\=0 { WRITE !,"Created table" }
     ELSE { WRITE "CREATE TABLE error SQLCODE=",SQLCODE }

 

  DO $SYSTEM.Security.Login("\_SYSTEM","SYS")
  NEW SQLCODE,%msg
  &sql(INSERT INTO Sample.PTest (Id, Name, DOB) 
    VALUES (1, 'David Vanderbilt', 46639))
  IF SQLCODE\=0 { WRITE !,"Inserted data in table"}
  ELSE { WRITE !,"SQLCODE=",SQLCODE,": ",%msg }
  &sql(INSERT INTO Sample.PTest (Id, Name, DOB) 
    VALUES (2, 'Mary Smith', 49759))
  IF SQLCODE\=0 { WRITE !,"Inserted data in table"}
  ELSE { WRITE !,"SQLCODE=",SQLCODE,": ",%msg }
  &sql(ALTER TABLE Sample.PTest 
    ADD COLUMN ColorPreference %String NOT NULL DEFAULT 'Blue')
  IF SQLCODE\=0 {
    WRITE !,"Altered table, SQLCODE=",SQLCODE }
  ELSEIF SQLCODE\=\-306 {
    WRITE !,"SQLCODE=",SQLCODE,": ",%msg }
  ELSE { WRITE "SQLCODE error=",SQLCODE }

 

To view the data, go to the Management Portal, select the Globals option for the SAMPLES namespace. Scroll to “Sample.PTestD” and click the Data option.

See Also

*   [CREATE TABLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable) [DROP TABLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_droptable)
    
*   [JOIN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_join)
    
*   [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select)
    
*   [INSERT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_insert) [UPDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_update) [DELETE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_delete)
    
*   “[Defining Tables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_tables)” chapter in Using Caché SQL
    
*   [SQL configuration settings](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RACS_Settings#RACS_Category_SQL) described in Caché Advanced Configuration Settings Reference.
    
*   [SQLCODE error messages](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RERR_sql) listed in the Caché Error Reference
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

  [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_alteruser "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_altertable.xml**