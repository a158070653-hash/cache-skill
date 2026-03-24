# CREATE TABLE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_createtable

---

 

Caché SQL Reference

CREATE TABLE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createrole "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtrigger "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_COMMANDS "SQL Commands") \]  >  \[ [CREATE TABLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable "CREATE TABLE") \]

Go to: Description SQL Security and Privileges CREATE TABLE and INSERT Table Name GLOBAL TEMPORARY Table %DESCRIPTION, %FILE, %EXTENTSIZE / %NUMROWS, %ROUTINE %CLASSPARAMETER Keyword Options Supported for Compatibility Only Field Definition Field Data Constraints Unique Fields Constraint RowID Record Identifier IDENTITY Field ROWVERSION and %Counter Fields Defining a Primary Key Defining Foreign Keys Examples: Dynamic SQL and Embedded SQL See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Creates a table definition.

Synopsis

CREATE \[GLOBAL TEMPORARY\] TABLE 
table (table-element-commalist)

table-element ::= 
     \[%DESCRIPTION string\] 
     \[%FILE string\] 
     \[{%EXTENTSIZE | %NUMROWS} integer\] 
     \[%PUBLICROWID\] 
     \[%ROUTINE string\] 
     \[{ %CLASSPARAMETER param\_name \[=\] value }\]

     { fieldname datatype
           \[IDENTITY\] |
           {
             \[UNIQUE\]
             \[ NULL | NOT NULL \]
             \[PRIMARY KEY\]
             \[ DEFAULT \[(\]default-spec\[)\] \]
             \[ COMPUTECODE { ObjectScript-code } 
                   \[ COMPUTEONCHANGE (field-commalist) |
                     CALCULATED | TRANSIENT \] \]
             \[ \[COLLATE\] sqlcollation \]
             \[ %DESCRIPTION literal \]
           } , }

     \[{ \[CONSTRAINT uname\] 
          UNIQUE (field-commalist) }\]

    \[ \[CONSTRAINT pkname\] 
          PRIMARY KEY (field-commalist) \] 

     \[{ \[CONSTRAINT fkname\] 
          FOREIGN KEY (field-commalist) REFERENCES table 
              \[(reffield-commalist)\] \[NOCHECK\] \[referential-action\] }\]

sqlcollation::=
          %EXACT | 
          %MVR | 
          %MINUS | 
          %PLUS | 
          %SPACE | 
          %ALPHAUP | 
          %SQLSTRING \[(maxlen)\] |
          %SQLUPPER \[(maxlen)\] | 
          %STRING \[(maxlen)\] |
          %UPPER

This synopsis does not include keywords that are parsed for compatibility only, but perform no operation. These supported no-op keywords are listed in a separate section below.

Arguments

GLOBAL TEMPORARY

Optional — This keyword clause creates the table as a temporary table.

table

The name of the table to be created, specified as a valid [identifier](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_identifiers). A [table name](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_tables) can be qualified (schema.table), or unqualified (table). An unqualified table name takes the system default schema name.

table-element

A comma-separated list of one or more field definitions or keyword phrases.

Each field definition consists of (at minimum) a field name (specified as a valid [identifier](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_identifiers)) followed by a data type.

A keyword phrase can consist of just a keyword (%PUBLICROWID), a keyword followed by literal, or a keyword (%CLASSPARAMETER) followed by a name and associated literal.

sqlcollation

A keyword that specifies the [collation type](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_basics#GSQL_basics_collation) to apply. The %SQLSTRING, %SQLUPPER, and %STRING keywords can be followed by a maxlen integer value, enclosed in parentheses.

uname

pkname

fkname

Optional — The name of a constraint, specified as a valid [identifier](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_identifiers). This optional constraint name is used in [ALTER TABLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_altertable) to identify a defined constraint.

field-commalist

A field name or a comma-separated list of field names in any order. Used to define a unique, primary key, or foreign key constraint. All field names specified for a constraint must also be defined in the [field definition](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable#RSQL_createtable_fields). Must be enclosed in parentheses.

reffield-commalist

Optional — A field name or a comma-separated list of existing field names defined in the referenced table specified in the foreign key constraint. If specified, must be enclosed in parentheses. If omitted, a default value is taken, as described in [Defining Foreign Keys](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable#RSQL_createtable_fkey).

Description

The CREATE TABLE command creates a table definition of the structure specified. Caché automatically creates a [persistent class](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GOBJ_defpersobj) corresponding to this table definition, with properties corresponding to the field definitions. CREATE TABLE does not specify an explicit [StorageStrategy](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ROBJ_class_storagestrategy) in the corresponding class definition. By default, CREATE TABLE specifies the [Final](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ROBJ_class_final) class keyword in the corresponding class definition, indicating that it cannot have subclasses. (You can change this default using either the [SetDDLFinal()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#SetDDLFinal) method, or the corresponding Management Portal [General SQL Settings: DDL tab](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RACS_Settings#RACS_SQLGeneral_DDL) option.)

This reference page describes the following CREATE TABLE considerations:

*   [Security and Privileges](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable#RSQL_createtable_security)
    
*   [Creating a Table and then Inserting Data](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable#RSQL_createtable_insert)
    
*   [Specifying a Table Name](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable#RSQL_createtable_tname)
    
*   [Defining a Temporary Table](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable#RSQL_createtable_temp)
    
*   [%DESCRIPTION, %FILE, %EXTENTSIZE, %ROUTINE keywords](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable#RSQL_createtable_keywords)
    
*   [%CLASSPARAMETER keyword](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable#RSQL_createtable_classparam)
    
*   [Table Options Supported for Compatibility](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable#RSQL_createtable_noops)
    
*   [Defining Fields](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable#RSQL_createtable_fields)
    
*   [Defining Field Data Constraints](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable#RSQL_createtable_fconstraints)
    
*   [Defining the Unique Fields Constraint](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable#RSQL_createtable_uniquefields)
    
*   [The RowID Field, %PUBLICROWID, and Bitmap Extent Index](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable#RSQL_createtable_rowid)
    
*   [Defining an IDENTITY Field](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable#RSQL_createtable_idfield)
    
*   [Defining ROWVERSION and %Counter Fields](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable#RSQL_createtable_counters)
    
*   [Defining the Primary Key](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable#RSQL_createtable_pkey)
    
*   [Defining Foreign Keys](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable#RSQL_createtable_fkey)
    

SQL Security and Privileges

The CREATE TABLE command is a privileged operation. Prior to using CREATE TABLE it is necessary for your process to have %CREATE\_TABLE privileges. Failing to do so results in an SQLCODE -99 error (Privilege Violation). You can use the [GRANT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_grant) command to assign %CREATE\_TABLE privileges to a user or role, if you hold appropriate granting privileges.

This privileges requirement is configurable, using either of the following:

*   Use the [$SYSTEM.SQL.SetSQLSecurity()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#SetSQLSecurity) method call. To determine the current setting, call [$SYSTEM.SQL.CurrentSettings()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#CurrentSettings), which displays an SQL Security ON: setting.
    
*   Go to the Management Portal, select \[Home\] > \[Configuration\] > \[General SQL Settings\]. View the current setting of SQL Security Enabled.
    

The default is “Yes” (1). When “Yes”, a user can only perform actions on a table or view for which that user has been granted privilege. This is the recommended setting for this option.

If this option is set to “No” (0), SQL Security is disabled for any new process started after changing this setting. This means privilege-based table/view security is suppressed. You can create a table without specifying a user. In this case, Dynamic SQL assigns “\_SYSTEM” as user, and Embedded SQL assigns "" (the empty string) as user. Any user can perform actions on a table or view even if that user has no privileges to do so.

Embedded SQL does not use SQL privileges. In Embedded SQL, you can use the [$SYSTEM.Security.Login()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Security#Login) method to log in as a user with appropriate privileges. You must have the %Service\_Login:Use privilege to invoke the $SYSTEM.Security.Login() method. For further information, refer to [%SYSTEM.Security](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Security) in the InterSystems Class Reference.

The following embedded SQL example creates the Employee table:

  DO $SYSTEM.Security.Login("\_SYSTEM","SYS")
  NEW SQLCODE,%msg
  &sql(CREATE TABLE Employee (
     EMPNUM     INT NOT NULL,
     NAMELAST   CHAR(30) NOT NULL,
     NAMEFIRST  CHAR(30) NOT NULL,
     STARTDATE  TIMESTAMP,
     SALARY     MONEY,
     ACCRUEDVACATION   INT,
     ACCRUEDSICKLEAVE  INT,
     CONSTRAINT EMPLOYEEPK PRIMARY KEY (EMPNUM))
  )
  IF SQLCODE\=0 {WRITE !,"Table created"}
  ELSE {WRITE !,"SQLCODE=",SQLCODE,": ",%msg }

 

This table, named Employee, has a number of defined fields. The EMPNUM field (containing the employee's company ID number) is an integer value that cannot be NULL; additionally, it is declared as a primary key for the table. The employee's last and first names each have a field, both of which are character strings with a maximum length of 30, that cannot be NULL. Additionally, there are fields for the employee's start date, accrued vacation time, and accrued sick time (which use the TIMESTAMP and INT [data types](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datatype)).

Use the following program to delete the table created in the previous example:

  DO $SYSTEM.Security.Login("\_SYSTEM","SYS")
  NEW SQLCODE,%msg
  &sql(DROP TABLE Employee)
  IF SQLCODE\=0 {WRITE !,"Table deleted"}
  ELSE {WRITE !,"SQLCODE=",SQLCODE,": ",%msg }

 

CREATE TABLE and INSERT

Embedded SQL is compiled SQL. In [Embedded SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql) you cannot both create a table and insert data into that table in the same program. The reason is as follows: Table creation is performed at runtime. However, the INSERT statement needs to verify the existence of the table at compile time. A SELECT statement needs to verify the existence of its table(s) at compile time, and thus has the same restriction.

A compiled program can freely combine CREATE TABLE statements with DML statements (such as INSERT and SELECT) that refer to other already-existing tables.

You can circumvent this restriction by directing the preprocessor to handle an Embedded SQL program as Deferred SQL. This is done using the [#SQLCompile Mode=Deferred](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_macros#GCOS_macros_mpp_lbSQLCompile_Mode) macro preprocessor directive, as described in the [Preprocessor Directives Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_macros#GCOS_macros_mppdirref) section of Using Caché ObjectScript.

This restriction does not apply to [Dynamic SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_dynsql), which is parsed at runtime.

You can create a table from an existing table definition and insert data from the existing table in a single operation using the [$SYSTEM.SQL.QueryToTable()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#QueryToTable) method.

Table Name

A table name can be qualified or unqualified. A qualified table name has the following syntax:

schema.tablename

An unqualified table name omits schema (and the period (.) character); it takes the [default schema name](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable#RSQL_createtable_tnamenoschema). (You cannot use the Embedded SQL #SQLCompile Path macro directive to resolve unqualified tables in DDL statements; this directive is limited to SELECT and CALL statements.)

A qualified table name can specify either an existing schema name or a new schema name. Specifying an existing schema name places the table within that schema. Specifying a new schema name creates that schema (and associated class package) and places the table within that schema.

Table names and schema names follow [SQL identifier](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_identifiers) naming conventions, subject to additional constraints on the use of non-alphanumeric characters, uniqueness, and maximum length. Names beginning with a % character are reserved for system use. By default, schema names and table names are simple identifiers, and are not case sensitive.

Caché uses the table name to generate a corresponding class name. Caché uses the schema name is used to generate a corresponding class package name. A class name contains only alphanumeric characters (letters and numbers) and must be unique within the first 96 characters. To generate a class name, Caché first strips out symbol (non-alphanumeric) characters from the table name, and then generates a unique class name, imposing uniqueness and maximum length restrictions. To generate a package name, Caché either strips out or performs special processing of symbol (non-alphanumeric) characters in the schema name. Caché then generates a unique package name, imposing uniqueness and maximum length restrictions. For further details on how package and class names are generated from schema and table names, refer to [Table Names and Schema Names](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_tables#GSQL_tables_names) in the “Defining Tables” chapter of Using Caché SQL.

You can use the same name for a schema and a table. You cannot use the same name for a table and a view in the same schema.

A schema name is not case sensitive; the corresponding class package name is case sensitive. If you specify a schema name that differs only in case from an existing class package name, and the package definition is empty (contains no class definitions) Caché reconciles the two names by changing the case of the class package name. For further details on schema names, refer to [Table Names and Schema Names](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_tables#GSQL_tables_names) in the “Defining Tables” chapter of Using Caché SQL.

Caché supports 16-bit (wide) characters for table and field names on Unicode systems. For most locales, accented letters can be used for table names and the accent marks are included in the generated class name, as shown in the following embedded SQL example:

  DO $SYSTEM.Security.Login("\_SYSTEM","SYS")
TableNameValidation
  SET x\=$SYSTEM.SQL.IsValidRegularIdentifier("ñ\_áàéèíìóòúù")
  IF x\=0 {IF $LENGTH("ñ\_áàéèíìóòúù")\>200  
             {WRITE "Tablename is too long" QUIT}
          ELSEIF $SYSTEM.SQL.IsReservedWord("ñ\_áàéèíìóòúù") 
             {WRITE "Tablename is reserved word" QUIT}
          ELSE {
            WRITE "Tablename contains invalid characters",!
            SET nls\=##class(%SYS.NLS.Locale).%New()
            IF nls.Language\="Japanese" {
            WRITE "Japanese locale cannot use accented letters"
            QUIT }
         QUIT }
  }
TableNameWithAccentedLetters
  NEW SQLCODE,%msg
  &sql(CREATE TABLE ñ\_áàéèíìóòúù (
    TESTNUM     INT NOT NULL,
    FIRSTWORD   CHAR (30) NOT NULL,
    LASTWORD    CHAR (30) NOT NULL,
    CONSTRAINT ñ\_áàéèíìóòúùPK PRIMARY KEY (TESTNUM))
  )
  IF SQLCODE\=0 {WRITE !,"Table created"}
  ELSE {WRITE !,"SQLCODE=",SQLCODE,": ",%msg }

 

Use the following program to delete the table created in the previous example:

  DO $SYSTEM.Security.Login("\_SYSTEM","SYS")
  NEW SQLCODE,%msg
  &sql(DROP TABLE ñ\_áàéèíìóòúù)
  IF SQLCODE\=0 {WRITE !,"Table deleted"}
  ELSE {WRITE !,"SQLCODE=",SQLCODE,": ",%msg }

 

Note:

The Japanese locale does not support accented letter characters in identifiers. Japanese identifiers may contain (in addition to Japanese characters) the Latin letter characters A-Z and a-z (65–90 and 97–122), the underscore character (95), and the Greek capital letter characters (913–929 and 931–937).

Default Schema Name

An unqualified table name takes as default the schema name SQLUser. The schema name becomes the class package name. The default schema name SQLUser becomes the default class package name User. For example, either the unqualified table name Employee or the qualified table name SQLUser.Employee would generate the class User.Employee. For this reason, attempting to specify a schema name of User results in an SQLCODE -312 error.

This schema name default can be configured. To return the default schema name for the current process, use the [$SYSTEM.SQL.DefaultSchema()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#DefaultSchema) method call:

  WRITE $SYSTEM.SQL.DefaultSchema()

 

To configure the default schema name use the following:

*   The [$SYSTEM.SQL.SetDefaultSchema()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#SetDefaultSchema) method call.
    
*   Go to the Management Portal, select \[Home\] > \[Configuration\] > \[General SQL Settings\]. View the current setting of Default SQL Schema Name.
    

The default schema name is used when an unqualified table name is encountered in an SQL statement and there is no #import statement specified. This configuration setting has nothing to do with the mappings between SQL schema names and the class package name; it only specifies the default schema name. The default is SQLUser. If you specify \_CURRENT\_USER as the default schema name, the default schema name becomes the user name of the currently logged-in process or, if the process has not logged in, SQLUser becomes the default schema name. If you specify \_CURRENT\_USER/myname as the default schema name, where myname is any string of your choice, then the default schema name becomes the user name of the currently logged-in process or, if the process has not logged in, myname is used as the default schema name. For example, \_CURRENT\_USER/HMO uses HMO as the default schema name if the process has not logged in.

Existing Table

To determine if a table already exists in the current namespace, use [$SYSTEM.SQL.TableExists()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#TableExists).

What happens when you try to create a table that has the same name as an existing table depends on a configuration setting. By default, Caché rejects an attempt to create a table with the name of an existing table and issues an SQLCODE -201 error. This is configurable as follows:

*   The [$SYSTEM.SQL.SetDDLNo201()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#SetDDLNo201) method call. To determine the current setting, call [$SYSTEM.SQL.CurrentSettings()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#CurrentSettings), which displays a Suppress SQLCODE=-201 Errors setting.
    
*   Go to the Management Portal, select \[Home\] > \[Configuration\] > \[General SQL Settings\]. View the current setting of Allow DDL CREATE TABLE or CREATE VIEW for Existing Table.
    

The default is “No” (0). This is the recommended setting for this option. If this option is set to “Yes” (1), Caché deletes the class definition associated with the table and then recreates it. This is much the same as performing a DROP TABLE, deleting the existing table and then performing the CREATE TABLE. In this case, it is strongly recommended that the Does DDL DROP TABLE Delete the Table's Data SQL configuration option be set to “Yes” (the default).

GLOBAL TEMPORARY Table

Specifying the GLOBAL TEMPORARY keyword defines the table as a global temporary table. The table definition is global (available to all processes); the table data is temporary (persists for the duration of the process). The corresponding class definition contains an additional Class parameter SQLTABLETYPE="GLOBAL TEMPORARY". Like standard Caché tables, the ClassType=persistent, and the class includes the [Final](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ROBJ_class_final) keyword, indicating that it cannot have subclasses.

Regardless of which process creates a temporary table, the owner of the temporary table is automatically set to \_PUBLIC. This means that all users can access a cached temporary table definition. For example, if a stored procedure creates a temporary table, the table definition can be accessed by any user that is permitted to invoke the stored procedure. This applies only to the temporary table definition; the temporary table data is specific to the invocation, and therefore can only be accessed by the current user process.

The table definition of a global temporary table is the same as a base table. A global temporary table must have a unique name; attempting to give it the same name as an existing base table results in an SQLCODE -201 error. The table persists until it is explicitly deleted (using DROP TABLE). You can alter the table definition using ALTER TABLE.

The table data (including Stream data) and indices in a global temporary table are temporary. They are stored in process-private globals. This means that this data is only available to the process that created the global temporary table, and this data is deleted when the process terminates.

The following embedded SQL example creates a global temporary table:

  DO $SYSTEM.Security.Login("\_SYSTEM","SYS")
  NEW SQLCODE,%msg
  &sql(CREATE GLOBAL TEMPORARY TABLE TempEmp (
    EMPNUM     INT NOT NULL,
    NAMELAST   CHAR(30) NOT NULL,
    NAMEFIRST  CHAR(30) NOT NULL,
    CONSTRAINT EMPLOYEEPK PRIMARY KEY (EMPNUM))
  )
  IF SQLCODE\=0 {WRITE !,"Table created"}
  ELSE {WRITE !,"SQLCODE=",SQLCODE,": ",%msg }

 

%DESCRIPTION, %FILE, %EXTENTSIZE / %NUMROWS, %ROUTINE

These optional keyword phrases can be specified anywhere in the comma-separated list of table elements.

Caché SQL provides a %DESCRIPTION keyword, which you can use to provide a description for documenting a table or a field. %DESCRIPTION is followed by text string enclosed in single quotes. This text can be of any length, and can contain any characters, including blank spaces. (A single quote character within a description is represented by two single quotes. For example: 'Joe''s Table'.) A table can have a %DESCRIPTION. Each field of a table can have its own %DESCRIPTION, specified after the data type. If you specify more than one table-wide %DESCRIPTION for a table, Caché issues an SQLCODE -82 error. If you specify more than one %DESCRIPTION for a field, Caché retains only the last %DESCRIPTION specified. In Caché Studio, a description appears prefaced by three slashes on the line immediately before the corresponding table (Class) or field (Property). For example:

/// Joe's Table

Caché SQL provides a %FILE keyword, which is used to provide a file name for documenting a table. %FILE is followed by text string enclosed in single quotes. A table definition can have only one %FILE keyword; specifying multiples generates an SQLCODE -83 error.

Caché SQL provides the optional %EXTENTSIZE and %NUMROWS keywords, which are used to store an integer recording the anticipated number of rows in this table. These two keywords are synonymous; %EXTENTSIZE is the preferred Caché term. When a table is being created to hold a known number of rows of data, especially if the initial number of rows is not likely to change subsequently (such as a table of states and provinces), setting %EXTENTSIZE can save space and improve performance. If not specified, the default initial allocation is 100,000 for an standard table, 50 for a temporary table. A table definition can have only one %EXTENTSIZE or %NUMROWS keyword; specifying multiples results in an SQLCODE -84 error.

Caché SQL provides a %ROUTINE keyword, which allows you to specify the routine name prefix for routines generated for this base table. %ROUTINE is followed by text string enclosed in single quotes. For example, %ROUTINE 'myname', generates code in routines named myname1, myname2, and so forth. Extrinsic functions cannot be called from a %ROUTINE. A table definition can have only one %ROUTINE keyword; specifying multiples results in an SQLCODE -85 error. In Caché Studio, the routine name prefix appears as the SqlRoutinePrefix value.

%CLASSPARAMETER Keyword

The optional %CLASSPARAMETER keyword enables you to define a class parameter as part of the CREATE TABLE command. A class parameter is always defined as a constant value. You can specify multiple %CLASSPARAMETER keyword clauses, defining one class parameter per clause. Like all table keyword clauses, %CLASSPARAMETER can be specified anywhere in the comma-separated list of table elements; multiple %CLASSPARAMETER clauses are separated by commas.

The %CLASSPARAMETER keyword is followed by the class parameter name, an optional equal sign, and the literal value (a string or number) to assign to that class parameter. The following example defines two class parameters; the first %CLASSPARAMETER clause uses an equal sign, the second omits the equal sign:

CREATE TABLE OurEmployees (
    %CLASSPARAMETER DEFAULTGLOBAL \= '^EMPLOYEE',
    %CLASSPARAMETER MANAGEDEXTENT 0,
    EMPNUM     INT NOT NULL,
    NAMELAST   CHAR(30) NOT NULL,
    NAMEFIRST  CHAR(30) NOT NULL,
    CONSTRAINT EMPLOYEEPK PRIMARY KEY (EMPNUM))

Some of the class parameters currently in use are: [DEFAULTGLOBAL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25Library.Persistent#DEFAULTGLOBAL), [DSINTERVAL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25Library.Persistent#DSINTERVAL), [DSTIME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25Library.Persistent#DSTIME), [EXTENTQUERYSPEC](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25Library.Persistent#EXTENTQUERYSPEC), [EXTENTSIZE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25Library.Persistent#EXTENTSIZE), [GUIDENABLED](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25Library.Persistent#GUIDENABLED), [IDENTIFIEDBY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25Library.Persistent#IDENTIFIEDBY), [MANAGEDEXTENT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25Library.Persistent#MANAGEDEXTENT), [READONLY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25Library.Persistent#READONLY), [ROWLEVELSECURITY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25Library.Persistent#ROWLEVELSECURITY), [SQLPREVENTFULLSCAN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25Library.Persistent#SQLPREVENTFULLSCAN), [VERSIONCLIENTNAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25Library.Persistent#VERSIONCLIENTNAME), [VERSIONPROPERTY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25Library.Persistent#VERSIONPROPERTY). Refer to the [%Library.Persistent](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Library.Persistent) class for descriptions of these class parameters.

The user can specify additional class parameters as needed. For further details refer to [Class Parameters](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GOBJ_parameters#GOBJ_classes_parameters) in Using Caché Objects.

Options Supported for Compatibility Only

Caché SQL accepts the following CREATE TABLE options for parsing purposes only, to aid in the conversion of existing SQL code to Caché SQL. These options do not provide any actual functionality.

{ON | IN} dbspace-name

LOCK MODE \[ROW | PAGE\]

\[CLUSTERED | NONCLUSTERED\]

WITH FILLFACTOR = literal

MATCH \[FULL | PARTIAL\]

CHARACTER SET identifier

COLLATE identifier  /\* But note use of COLLATE keyword, described below \*/

NOT FOR REPLICATION

Field Definition

Following the table name, a set of parentheses contains the definitions of all of the fields (columns) of the table. Definitions of fields are separated by commas. By convention, each field definition is usually presented on a separate line and indentation is used; this is recommended, but not required. After the last field is defined, remember to provide a closing parenthesis for the field definition.

The parts of a field definition are separated by blank spaces. The field name is listed first, followed by its data characteristics. The data characteristics of a field are presented in the following sequence: the data type, the (optional) data size, then the (optional) data constraints. You can then append an optional field %DESCRIPTION to document the field.

Note:

Caché recommends that you avoid creating tables with over 400 columns. Redesign your database so that either: these columns become rows; the columns are divided among several related tables; or the data is stored in fewer columns as character streams or bit streams.

Field Name

Field names follow [identifier](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_identifiers) conventions, with the same naming restrictions as table names. Field names beginning with a % character should be avoided (field names beginning with %z or %Z are permitted). A field name should not exceed 128 characters. By default, field names are simple identifiers. They are not case sensitive. Attempting to create a field name that differs only in letter case from another field in the same table generates an SQLCODE -306 error. For further details see the “Identifiers” chapter of Using Caché SQL.

Caché uses the field name to generate a corresponding class property name. A property name contains only alphanumeric characters (letters and numbers) and is a maximum of 96 characters in length. To generate this property name, Caché first strips punctuation characters from the field name, and then generates a unique identifier of 96 (or less) characters. Caché substitutes an integer (beginning with 0) for the final character of a field name when this is needed to create a unique property name.

The following example shows how Caché handles field names that differ only in punctuation. The corresponding class properties for these fields are named PatNum, PatNu0, and PatNu1:

CREATE TABLE MyPatients (
     \_PatNum VARCHAR(16),
     %Pat@Num INTEGER,
     Pat\_Num VARCHAR(30),
     CONSTRAINT Patient\_PK PRIMARY KEY (\_PatNum))

The field name, as specified in CREATE TABLE, is shown in the class property as the [SqlFieldName](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ROBJ_property_sqlfieldname) keyword value.

During a dynamic SELECT operation, Caché may generate property name aliases to facilitate common letter case variants. For example, given the field name Home\_Street, Caché might assign the property name aliases home\_street, HOME\_STREET, and HomeStreet. Caché doe not assign an alias if that name would conflict with the name of another field name, or with an alias assigned to another field name.

Data Types

Caché SQL supports most standard SQL [data types](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datatype). A complete list of supported data types is provided in the [Data Types](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datatype) section of this reference.

Caché maps these standard SQL data types to Caché data types by providing an SQL.SystemDataTypes mapping table and an SQL.UserDataTypes mapping table. SQL.UserDataTypes can be added to by the user to include additional user-defined data types.

To view and modify the current data type mappings, go to the Management Portal, select \[Home\] > \[Configuration\] > \[System-defined DDL Mappings\]. To create additional data type mappings, go to the Management Portal, select \[Home\] > \[Configuration\] > \[User-defined DDL Mappings\].

If you specify a data type in SQL for which no corresponding Caché data type exists, the SQL data type name is used as the data type for the corresponding class property. You must create this user-defined Caché data type before DDL runtime (SQLExecute).

You may also override data type mappings for a single parameter value. For instance, suppose you didn't want VARCHAR(100) to map to the supplied standard mapping %String(MAXLEN=100). You could override this by added a DDL data type of 'VARCHAR(100)' to the table and then specify its corresponding Caché type. For example:

VARCHAR(100) maps to MyString100(MAXLEN=100)

Data Size

Following a data type, you can present the permissible data size in parentheses. Whitespace between the data type name and data size parentheses is permitted, but not required.

For a string, data size represents the maximum number of characters. For example:

ProductName VARCHAR (64)

For a numeric that permits fractional numbers, this is represented as a pair of integers (p,s). The first integer (p) is the data type precision, but it is not identical to numerical precision (the number of digits in the number). This is because the underlying Caché data type classes do not have a precision, but instead use this number to calculate the MAXVAL and MINVAL; The second integer (s) is the scale, which specifies the maximum number of decimal digits. For example:

UnitPrice NUMERIC(6,2)  /\* maximum value 9999.99 \*/

To determine the maximum and minimum permissible values for a field, use the following Cache ObjectScript functions:

  WRITE $$maxval^%apiSQL(6,2),!
  WRITE $$minval^%apiSQL(6,2)

 

Note that because p is not a digit count, it can be smaller than the scale s value:

  FOR i\=0:1:6 {
      WRITE "Max for (",i,",2)=",$$maxval^%apiSQL(i,2),!}

 

For further details, refer to the [Data Types](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datatype) reference page in this manual.

Field Data Constraints

Data constraints govern what values are permitted for a field, what the default value is for a field, and what type of collation is used for data values. All of these data constraints are optional. Multiple data constraints can be specified in any order, separated by a blank space. For further details, see [field-constraint](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_fieldconstraint).

NULL and NOT NULL

The NOT NULL data constraint keyword specifies that this field does not accept a null value; in other words, every record must have a specified value for this field. NULL and empty string ('') are different values in Caché. You can input an empty string into a field that accepts character strings, even if that field is defined with a NOT NULL restriction. You cannot input an empty string into a numeric field. For further details, refer to the [NULL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_langelements#GSQL_langelements_null) section of the “Language Elements” chapter of Using Caché SQL.

The NULL data constraint keyword explicitly specifies that this field can accept a null value; this is the default definition for a field.

UNIQUE

The UNIQUE data constraint specifies that this field accepts only unique values. Thus, no two records can contain the same value for this field. The SQL [empty string](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_langelements#GSQL_langelements_null) ('') is considered to be a data value, so with the UNIQUE data constraint applied, no two records can contain an empty string value for this field. A [NULL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_langelements#GSQL_langelements_null) is not considered to be a data value, so the UNIQUE data constraint does not apply to multiple NULLs. To restrict use of NULL for a field, use the NOT NULL keyword constraint.

*   The UNIQUE data constraint requires that all of the values for the specified field be unique values.
    
*   The [UNIQUE fields constraint](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable#RSQL_createtable_uniquefields) (which uses the CONSTRAINT keyword) requires that all of the values for a specified group of fields when concatenated together result in a unique value. None of the individual fields are required to be limited to unique values.
    

DEFAULT

The DEFAULT data constraint specifies the default data value automatically supplied for this field, if not overridden by a user-supplied data value. A string supplied as a default value is a constant, and thus must be enclosed in single quotes. A numeric default value does not require single quotes. For example:

CREATE TABLE membertest
(MemberId INT NOT NULL,
Membership\_status CHAR(13) DEFAULT 'M',
Membership\_term INT DEFAULT 2)

The DEFAULT value is not validated when creating a table. When defined, a DEFAULT value can ignore data type and data constraint restrictions. However, when supplying data to the table, the DEFAULT value is constrained; it is not limited by data type restrictions, but is limited by data constraint restrictions. For example, a field defined Ordernum INT UNIQUE DEFAULT 'No Number' can take the default once, ignoring the INT data type restriction, but cannot take the default a second time, as this would violate the UNIQUE field data constraint.

If no DEFAULT is specified, the implied default is NULL. If a field has a NOT NULL data constraint, you must specify a value for that field, either explicitly or by DEFAULT. Do not use the SQL [zero-length string](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_langelements#GSQL_langelements_null) (empty string) as a NOT NULL default value. Refer to [NULL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_langelements#GSQL_langelements_null) section of the “Language Elements” chapter of Using Caché SQL for further details on NULL and the empty string.

If the [data type](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datatype) is DATE, the DEFAULT data constraint can use the [TO\_DATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_todate) function, as shown in the following example:

CREATE TABLE membertest2
(MemberId INT NOT NULL,
End\_Year DATE DEFAULT TO\_DATE('31-12-2009','DD-MM-YYYY') )

DEFAULT Keywords

The DEFAULT data constraint can accept a keyword option to define its value. The following options are supported: NULL, USER, CURRENT\_USER, SESSION\_USER, SYSTEM\_USER, CURRENT\_DATE, CURRENT\_TIME, CURRENT\_TIMESTAMP, SYSDATE, and OBJECTSCRIPT.

The USER, CURRENT\_USER, and SESSION\_USER default keywords set the field value to the Caché ObjectScript [$USERNAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vusername) special variable, as described in the Caché ObjectScript Reference.

The [CURRENT\_DATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_currentdate), [CURRENT\_TIME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_currenttime), [CURRENT\_TIMESTAMP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_currenttimestamp), [GETDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_getdate), [GETUTCDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_getutcdate), and [SYSDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_sysdate) SQL functions can also be used as DEFAULT values. They are described in their respective reference pages. You can specify CURRENT\_TIMESTAMP with or without a precision value when using it as a DEFAULT keyword.

CREATE TABLE mytest
(TestId INT NOT NULL,
CREATE\_DATE DATE DEFAULT CURRENT\_TIMESTAMP(2),
WORK\_START DATE DEFAULT SYSDATE)

The OBJECTSCRIPT literal keyword phrase enables you to generate a default value by providing a quoted string containing Caché ObjectScript code, as shown in the following example:

CREATE TABLE mytest
(TestId INT NOT NULL,
CREATE\_DATE DATE DEFAULT OBJECTSCRIPT '+$HOROLOG' NOT NULL,
LOGNUM NUMBER(12,0) DEFAULT OBJECTSCRIPT '$INCREMENT(^LogNumber)')

See the [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS) for further information.

COMPUTECODE

The COMPUTECODE data constraint specifies Caché ObjectScript code to compute a default data value for this field. The ObjectScript code is specified within curly braces. Within the ObjectScript code, SQL field names can be specified with curly brace delimiters. The ObjectScript code can consist of multiple lines of code. It can contain [Embedded SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql). Whitespace and line returns are permitted before or after the ObjectScript code curly brace delimiters.

COMPUTECODE specifies the [SqlComputeCode](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ROBJ_property_sqlcomputecode) field name and computation for its value. When you specify a computed field name, either in COMPUTECODE or in the SqlComputeCode class property, you must specify the SQL field name, not the corresponding generated table property name. The [SqlComputeCode](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ROBJ_property_sqlcomputecode) property keyword is described in the Caché Class Definition Reference.

A default data value supplied by compute code must be in Logical (internal storage) mode. Embedded SQL in compute code is automatically compiled and run in Logical mode.

The following example defines the Birthday COMPUTECODE field. It use ObjectScript code to compute its default value from the DOB field value:

CREATE TABLE MyStudents (
   Name VARCHAR(16) NOT NULL,
   DOB DATE,
   Birthday VARCHAR(10) COMPUTECODE {SET {Birthday}\=$PIECE($ZDATE({DOB},9),",")},
   Grade INT
   )

The COMPUTECODE can contain the pseudo-field reference variables {%%CLASSNAME}, {%%CLASSNAMEQ}, {%%OPERATION}, {%%TABLENAME}, and {%%ID}. These pseudo-fields are translated into a specific value at class compilation time. All of these pseudo-field keywords are not case-sensitive.

The COMPUTECODE value is a default; it is only returned if you did not supply a value to the field. The COMPUTECODE value is not limited by data type restrictions. The COMPUTECODE value is limited by the UNIQUE data constraint and other data constraint restrictions. If you specify both a DEFAULT and a COMPUTECODE, the DEFAULT is always taken.

COMPUTECODE can optionally take a COMPUTEONCHANGE, CALCULATED, or TRANSIENT keyword. The following keyword combination behaviors are supported:

*   COMPUTECODE: value is computed and stored upon INSERT, value is not changed upon UPDATE.
    
*   COMPUTECODE with COMPUTEONCHANGE: value is computed and stored upon INSERT, is recomputed and stored upon UPDATE.
    
*   COMPUTECODE with DEFAULT, and COMPUTEONCHANGE: default value is stored upon INSERT, value is computed and stored upon UPDATE.
    
*   COMPUTECODE with CALCULATED or TRANSIENT: no value is stored; value is computed when queried.
    

If there is an error in the ObjectScript COMPUTECODE code, SQL does not detect this error until the code is executed for the first time. Therefore, if the value is first computed upon insert, the INSERT operation fails with an SQLCODE -415 error; if the value is first computed upon update, the UPDATE operation fails with an SQLCODE -415 error; if the value is first computed when queried, the SELECT operation fails with an SQLCODE -400 error.

A COMPUTECODE stored value can be [indexed](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_indices). The application developer is responsible for making sure that computed field stored values are validated and normalized (numbers in [canonical form](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_numcanonical)), based on their data type, especially if you define (or intend to define) an index for the computed field.

COMPUTEONCHANGE

By itself, COMPUTECODE causes a field value to be computed and stored in the database during INSERT; this value remains unchanged by subsequent operations. By default, subsequent UPDATE or trigger code operations do not change the computed value. Specifying the COMPUTEONCHANGE keyword causes subsequent UPDATE or trigger code operations to recompute and replace this stored value.

If you use the COMPUTEONCHANGE clause to specify a field or comma-separated list of fields, any change to the value of one of these fields causes Caché to recompute the COMPUTECODE field value.

If a field specified in COMPUTEONCHANGE is not part of the table specification, an SQLCODE -31 is generated.

In the following example, Birthday is computed upon insert based on the DOB value. Birthday is recomputed when DOB is updated:

CREATE TABLE MyStudents (
   Name VARCHAR(16) NOT NULL,
   DOB DATE,
   Birthday VARCHAR(10) COMPUTECODE {SET {Birthday}\=$PIECE($ZDATE({DOB},9),",")}
      COMPUTEONCHANGE (DOB),
   Grade INT
   )

COMPUTEONCHANGE defines the [SqlComputeOnChange](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ROBJ_property_sqlcomputeonchange) keyword with the %%UPDATE value for the class property corresponding to the field definition. This property value is initially computed as part of the INSERT operation, and recomputed during an UPDATE operation. This property keyword is described in the Caché Class Definition Reference.

CALCULATED and TRANSIENT

Specifying the CALCULATED or TRANSIENT keyword specifies that the COMPUTECODE field value is not saved in the database; it is calculated as part of each query operation that accesses it. This reduces the size of the data storage, but may slow query performance. Because these keywords cause Caché to not store the COMPUTECODE field value, these keywords and the COMPUTEONCHANGE keyword are mutually exclusive. The following is an example of a CALCULATED field:

CREATE TABLE MyStudents (
   Name VARCHAR(16) NOT NULL,
   DOB DATE,
   Days2Birthday INT COMPUTECODE{SET {Days2Birthday}\=$ZD({DOB},14)\-$ZD($H,14)} CALCULATED
   )

CALCULATED defines the [Calculated](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ROBJ_property_calculated) boolean keyword for the class property corresponding to the field definition. TRANSIENT defines the [Transient](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ROBJ_property_transient) boolean keyword for the class property corresponding to the field definition. These property keywords are described in the Caché Class Definition Reference.

CALCULATED and TRANSIENT provide nearly identical behavior, with the following differences. TRANSIENT means that Caché does not store the property. CALCULATED means that Caché does not allocate any instance memory for the property. Thus when CALCULATED is specified, TRANSIENT is implicitly set.

TRANSIENT properties cannot be indexed. CALCULATED properties cannot be indexed unless the property is also [SQLComputed](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ROBJ_property_sqlcomputed).

Collation Parameters

The optional collation parameters specify what type of [string collation](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_basics#GSQL_basics_collation) to use when sorting values for a field. Caché SQL supports ten types of collation. If no collation is specified, the default is %SQLUPPER collation, which is not case-sensitive.

It is recommended that you specify the optional keyword COLLATE before the collation parameter for programming clarity, but this keyword is not required. The percent sign (%) prefix to the various collation parameter keywords is optional.

%EXACT collation follows the ANSI (or Unicode) character collation sequence. This provides case-sensitive string collation and recognizes leading and trailing blanks and tab characters.

The %MVR collation treats a string as a group of substrings. Numeric substrings are sorted in signed numeric collation sequence; non-numeric substrings are sorted in case-sensitive string collation sequence. %MVR is provided for compatibility with MultiValue database systems.

The %ALPHAUP, %SQLUPPER, %STRING, and %UPPER collations convert all letters to uppercase for the purpose of collation. Note that the %ALPHAUP, %STRING, and %UPPER collations are deprecated; %SQLUPPER is the preferred collation for this type of string collation. For further details on not case-sensitive collation, refer to the [%SQLUPPER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_sqlupper) function.

The %SPACE, %SQLUPPER, and %STRING collations append a blank space to the data. This forces string collation of NULL and numeric values.

The %SQLSTRING, %SQLUPPER, and %STRING collations provide an optional maxlen parameter, which must be enclosed in parentheses. maxlen is a truncation integer that specifies the maximum number of characters to consider when performing collation. This parameter is useful when creating indices with fields containing large data values.

The %PLUS and %MINUS collations handle NULL as a zero (0) value.

Caché SQL provides functions for most of these collation types. Refer to the [%EXACT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_exact) [%MVR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_mvr) [%ALPHAUP](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_alphaup), [%SQLSTRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_sqlstring), [%SQLUPPER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_sqlupper), [%STRING](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_string_pct), and [%UPPER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_upper_pct) functions for further details.

Caché ObjectScript provides the [Collation()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Util#Collation) method of the [%SYSTEM.Util](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Util) class for data collation conversion.

Note:

To change the system-wide default collation from %SQLUPPER (which is not case-sensitive) to %SQLSTRING (which is case-sensitive), use the following command:

  WRITE $$SetEnvironment^%apiOBJ("collation","%Library.String","SQLSTRING")

After issuing this command, you must purge indexes, recompile all classes, then rebuild indexes. Do not rebuild indices while the table’s data is being accessed by other users. Doing so may result in inaccurate query results.

%DESCRIPTION

You can provide a description text for a field. This option follows the same conventions as providing a description text for a table. It is described with the other table elements, above.

Unique Fields Constraint

The unique fields constraint imposes a unique value constraint on the combined values of multiple fields. It has the following syntax:

CONSTRAINT uname UNIQUE (f1,f2)

This constraint specifies that the combination of values of fields f1 and f2 must always be unique, even though either of these fields by itself may take non-unique values. You can specify one, two, or more than two fields for this constraint.

All of the fields specified in this constraint must be defined in the field definition. If you specify a field in this constraint that does not also appear in the field definitions, an SQLCODE -86 error is generated. The specified fields should be defined as NOT NULL. None of the specified fields should be defined as [UNIQUE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_fieldconstraint), as this would make specifying this constraint meaningless.

Fields may be specified in any order. The field order dictates the field order for the corresponding index definition. Duplicate field names are permitted. Although you may specify a single field name in the UNIQUE fields constraint, this would be functionally identical to specify the UNIQUE data constraint to that field. A single-field constraint does provide a constraint name for future use.

You may specify multiple unique fields constraint statements in a table definition. Constraint statements can be specified anywhere in the field definition; by convention they are commonly placed at the end of the list of defined fields.

The Constraint Name

The CONSTRAINT keyword and the unique fields constraint name are optional. The following are functionally equivalent:

CONSTRAINT myuniquefields UNIQUE (name,dateofbirth)
UNIQUE (name,dateofbirth)

The constraint name can be any valid [identifier](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_identifiers). The constraint name uniquely identifies the constraint, and is also used to derive the corresponding index name. Specifying CONSTRAINT name is recommended; this constraint name is required when using the [ALTER TABLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_altertable) command to drop a constraint from the table definition.

ALTER TABLE cannot drop a column that is listed in CONSTRAINT UNIQUE. Attempting to do so generates an SQLCODE -322 error.

RowID Record Identifier

In SQL, every record must be identified by a unique integer value, known as the RowID. In Caché SQL you do not need to specify a RowID field. When you create a table and specify the desired data fields, a RowID field is automatically created. This RowID is used internally, but is not mapped to a class property. By default, its existence is only visible when a class is projected to SQL. In this projected SQL, an additional RowID field appears.

For example, a CREATE TABLE statement specifies three fields: Name, Age, Address; the corresponding class contains three class properties: Name, Age, Address; however, the SQL projected from that class also contains an additional field: ID.

By default, this field is named "ID". However, this field name is not reserved. If you define a field named “ID”, Cache names the RowId as “ID1”. If, for example, you then use ALTER TABLE to define a field named “ID1”, Caché renames the RowID as “ID2”, and so forth. For this reason, Caché provides the %ID pseudo-column name (alias) which always returns the RowID (object ID) value, regardless of the name assigned to the RowID.

RowID values are sequential positive integers, beginning with 1. The values generated for the RowID have the following constraints: Each value is unique. The NULL value is not permitted. Collation is EXACT. Values are not modifiable. This default can be changed using the [AllowRowIDUpdate](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=Config.SQL#AllowRowIDUpdate) property of the Config.SQL class. This method should only be used with extreme care.

When records are inserted into the table, Caché assigns each record an integer ID value. By default, Caché performs ID assignment using [$SEQUENCE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fsequence), allowing for the rapid simultaneous populating of the table by multiple processes. (You can configure Caché to perform ID assignment using [$INCREMENT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fincrement) by setting either the [SetDDLUseSequence()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#SetDDLUseSequence) method, or the corresponding Management Portal [General SQL Settings: DDL tab](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RACS_Settings#RACS_SQLGeneral_DDL) option.)

%PUBLICROWID

By default, the RowID is hidden (not displayed by SELECT \*) and PRIVATE. Specifying the %PUBLICROWID keyword makes the RowId not hidden and public. Because this keyword specifies that the table’s RowID is PUBLIC, the RowID can therefore be used as a foreign key reference. If you specify the %PUBLICROWID keyword, the class corresponding to the table is defined with “Not SqlRowIdPrivate”. This optional keyword can be specified anywhere in the comma-separated list of table elements.

ALTER TABLE can not be used to specify %PUBLICROWID.

Bitmap Extent Index

When you create a table using CREATE TABLE, by default Caché automatically defines a [bitmap extent index](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createindex#RSQL_createindex_bitmapextent) for the corresponding class. The SQL MapName of the bitmap extent index is %%DDLBEIndex:

Index DDLBEIndex \[ Extent, SqlName = "%%DDLBEIndex", Type = bitmap \];

This bitmap extent index is not created in any of the following circumstances:

*   The table is defined as a [temporary table](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable#RSQL_createtable_temp).
    
*   The table defines an explicit [IDKey index](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ROBJ_index_idkey).
    
*   The table contains a defined [IDENTITY field](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable#RSQL_createtable_idfield) that does not have MINVAL=1.
    
*   A configuration setting overrides this default operation, either the [SetDDLDefineBitmapExtent()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#SetDDLDefineBitmapExtent) method, or the corresponding Management Portal [General SQL Settings: DDL tab](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RACS_Settings#RACS_SQLGeneral_DDL) option.
    

If, after creating a bitmap index, you invoke [CREATE BITMAPEXTENT INDEX](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createindex) is run against a table where a bitmap extent index was automatically defined, the bitmap extent index previously defined is renamed to the name specified by the CREATE BITMAPEXTENT INDEX statement.

For DDL operations that automatically delete an existing bitmap extent index, refer to [ALTER TABLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_altertable).

For further details refer to “[Bitmap Extent Index](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_indices#GSQLOPT_indices_bitmapextent)” in the “Defining and Building Indices” chapter of Caché SQL Optimization Guide.

IDENTITY Field

Caché SQL automatically creates a RowID field for each table, which contains a system-generated integer that serves as a unique record id (see section below). The optional IDENTITY keyword allows you to define a named field with the same properties as a RowID record id field. An IDENTITY field behaves as a single-field IDKEY value, whose value is a system-generated integer.

Just as with any system-generated ID field, an IDENTITY field has the following characteristics:

*   You can only define one field per table as an IDENTITY field. Attempting to define more than one IDENTITY field for a table generates an SQLCODE -308 error.
    
*   The data type of an IDENTITY field must be an integer data type. If you do not specify a data type, its data type is automatically defined as INTEGER. You can specify any integer data type, such as SMALLINT or BIGINT. Any specified field constraints, such as NOT NULL or UNIQUE are accepted, but ignored.
    
*   Data values are system-generated. They consist of unique, nonzero, positive integers.
    
*   By default, IDENTITY field data values cannot be user-specified. By default, an INSERT statement does not, and can not, specify an IDENTITY field value. Attempting to do so generates an SQLCODE -111 error. To determine whether an IDENTITY field value can be specified, call the [GetIdentityInsert()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#GetIdentityInsert) method of the [%SYSTEM.SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL) class. To change this setting, call the [SetIdentityInsert()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#SetIdentityInsert) method of the [%SYSTEM.SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL) class. For further details, refer to the [INSERT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_insert) statement.
    
*   IDENTITY field data values cannot be modified in an UPDATE statement. Attempting to do so generates an SQLCODE -107 error.
    
*   Caché automatically projects a primary key on the IDENTITY field to ODBC and JDBC. If a CREATE TABLE or ALTER TABLE statement defines a primary key constraint or a unique constraint on an IDENTITY field, or on a set of columns including an IDENTITY field, the constraint definition is ignored and no corresponding primary key or unique index definition is created.
    
*   A SELECT \* statement does return a table's IDENTITY field.
    

Following an INSERT, UPDATE, or DELETE operation, you can use the [LAST\_IDENTITY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_last_identity) function to return the value of the IDENTITY field for the most-recently modified record. If no IDENTITY field is defined, LAST\_IDENTITY returns the RowID value of the most-recently modified record.

The following two Embedded SQL programs create a table with an IDENTITY field and then insert a record into the table, generating an IDENTITY field value. Note that in Embedded SQL the CREATE TABLE and INSERT statements must be in separate programs:

   DO $SYSTEM.Security.Login("\_SYSTEM","SYS")
   &sql(CREATE TABLE Employee (
  EmpNum INT NOT NULL,
  MyID   IDENTITY NOT NULL,
  Name   CHAR(30) NOT NULL,
  CONSTRAINT EMPLOYEEPK PRIMARY KEY (EmpNum))
  )
  IF SQLCODE'=0 {
    WRITE !,"CREATE TABLE error is: ",SQLCODE }
  ELSE {
   WRITE !,"Table created" }

  &sql(INSERT INTO Employee (EmpNum,Name) 
    SELECT ID,Name FROM Sample.Person WHERE Age \>= '25')
  IF SQLCODE'=0 {
    WRITE !,"INSERT error is: ",SQLCODE }
  ELSE {
   WRITE !,"Record inserted into table" }

In this case, the primary key (EmpNum) is taken from the ID field of another table. Thus EmpNum values are unique integers, but (because of the WHERE clause) may contain gaps in their sequence. The IDENTITY field, MyID, assigns a user-visible unique sequential integer to each record.

ROWVERSION and %Counter Fields

You can optionally specify one field of data type [ROWVERSION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datatype#RSQL_datatype_rowversion). This field receives an integer from an automatically incremented namespace-wide counter. This counter increments whenever row data in any ROWVERSION-enabled table is modified by an insert, update, or %Save operation. ROWVERSION values are unique and non-modifiable. This counter is never reset.

You can optionally specify one or more fields of data type [%Counter](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datatype#RSQL_datatype_rowversion) ([%Library.Counter](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Library.Counter)). By default, this field receives an integer from an automatically incremented table counter whenever a row is inserted into the table. However, a user can specify an integer value for this field during an insert, overriding the table counter default. This counter is reset by a TRUNCATE TABLE operation.

Defining a Primary Key

You can explicitly define a field as the primary record identifier by using the PRIMARY KEY clause. There are three syntactic forms for defining a primary key:

CREATE TABLE MyTable (Field1 INT PRIMARY KEY, Field2 INT)

CREATE TABLE MyTable (Field1 INT, Field2 INT, PRIMARY KEY (Field1))

CREATE TABLE MyTable (Field1 INT, Field2 INT, CONSTRAINT MyTablePK PRIMARY KEY (Field1))

The first syntax defines a field as the primary key; by designating it as the primary key, this field is by definition unique and not null. The second and third syntax can be used for a single field primary key but allow for a primary key consisting of more than one field. For example, PRIMARY KEY (Field1,Field2). If you specify a single field, this field is by definition unique and not null. If you specify a comma-separated list of fields, each field is defined as not null but may contain duplicate values, so long as the combination of the field values is a unique value. The third syntax allows you to explicitly name your primary key; the first two syntax forms generate a primary key name as follows: table name + “PKey” + position count integer. Thus the first syntax example has a primary key name of MyTablePKey1. The second syntax example has a primary key name of MyTablePKey3.

A primary key accepts only unique values and does not accept NULL. (The [primary key index](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ROBJ_index_primarykey) property is not automatically defined as Required; however, it effectively is required, since a NULL value cannot be filed or saved for a primary key field.) The [collation type](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_basics#GSQL_basics_collation) of a primary key is specified in the definition of the field itself.

When a class with a defined primary key is projected to SQL, the additional RowID field (named "ID") appears; however, its values are identical to the values of the IDKEY field.

Primary Key As IDKEY

By default, the primary key is not the IDKEY. In many cases this is preferable, because it enables you to update primary key values, set the [collation type](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_basics#GSQL_basics_collation) for the primary key, etc. There are cases where it is preferable to define the primary key as the IDKEY. Be aware that this imposes the IDKEY restrictions on the future use of the primary key.

If you add a primary key constraint to an existing field, the field may also be automatically defined as an [IDKey index](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ROBJ_index_idkey). This depends on whether data is present and upon a configuration setting established in one of the following ways:

*   The SQL [SET OPTION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_setoption) PKEY\_IS\_IDKEY statement.
    
*   The [$SYSTEM.SQL.SetDDLPKeyNotIDKey()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#SetDDLPKeyNotIDKey) method call. To determine the current setting, call [$SYSTEM.SQL.CurrentSettings()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#CurrentSettings).
    
*   Go to the Management Portal, select \[Home\] > \[Configuration\] > \[General SQL Settings\]. View the current setting of Are Primary Keys Created through DDL not ID Keys. If set to “Yes” (1), when a Primary Key constraint is specified through DDL it does not automatically become the [IDKey index](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ROBJ_index_idkey) in the class definition. If “No” (0), it does become the IDKey index. Setting this value to “No” can give better performance, but means that the Primary Key fields cannot be updated.
    

The default is “Yes” (1). If this option is set to “Yes” (1), the primary key does not correspond to IDKEY. Access to records using a primary key that is not the IDKEY is significantly less efficient; however, this type of primary key value can be modified. If this option is set to “No” (0), data access is more efficient, but a key value, once set, can never be modified.

Caché supports properties (fields) that are part of the IDKEY to be [SqlComputed](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ROBJ_property_sqlcomputed). For example, a parent reference field. The property must be a triggered computed field. An IDKEY property defined as SqlComputed is only computed upon the initial save of a new Object or an INSERT operation. UPDATE computation is not supported, because IDKEY fields cannot be updated.

No Primary Key

In most cases, you should explicitly define a primary key. However, if a primary key is not designated, Caché attempts to use another field as the primary key for ODBC/JDBC projection, according to the following rules:

1.  If there is an [IDKey index](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ROBJ_index_idkey) on a single field, report the IDKEY field as the SQLPrimaryKey field.
    
2.  Else if the class is defined with [SqlRowIdPrivate](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ROBJ_class_sqlrowidprivate)\=0 (the default), report the RowID field as the SQLPrimaryKey field.
    
3.  Else if there is an IDKey index, report the IDKEY fields as the SQLPrimaryKey fields.
    
4.  Else do not report an SQLPrimaryKey.
    

Multiple Primary Keys

You can only define one primary key. What happens when you try to specify more than one primary key for a table is configuration-dependent. By default, Caché rejects an attempt to define a primary key when one already exists, or to define the same primary key twice, and issues an SQLCODE -307 error. You can set this behavior as follows:

*   The [$SYSTEM.SQL.SetDDLNo307()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#SetDDLNo307) method call. To determine the current setting, call [$SYSTEM.SQL.CurrentSettings()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#CurrentSettings), which displays a Suppress SQLCODE=-307 Errors setting.
    
*   Go to the Management Portal, select \[Home\] > \[Configuration\] > \[General SQL Settings\]. View the current setting of Allow Create Primary Key Through DDL When Key Exists.
    

The default is “No” (0). If this option is set to “No”, Caché issues an SQLCODE -307 error when an attempt is made to add a primary key constraint to a table through DDL when a primary key constraint already exists for the table. The error is issued even if the second definition of the primary key is identical to the first definition.

For example, the following CREATE TABLE statement:

CREATE TABLE MyTable (f1 VARCHAR(16), 
CONSTRAINT MyTablePK PRIMARY KEY (f1))

creates the primary key (if none exists). A subsequent ALTER TABLE statement:

ALTER TABLE MyTable ADD CONSTRAINT MyTablePK PRIMARY KEY (f1)

generates an SQLCODE -307 error.

If the Allow Create Primary Key through DDL When Key Exists option is set to “Yes” (1), Caché drops the existing primary key constraint and establishes the last-specified primary key as the table's primary key.

Defining Foreign Keys

A foreign key is a field that references another table; the value stored in the foreign key field is a value that uniquely identifies a record in the other table. The simplest form of this reference is shown in the following example, in which the foreign key explicitly references the primary key field CustID in the Customers table:

CREATE TABLE Orders (
   OrderID INT UNIQUE NOT NULL,
   OrderItem VARCHAR,
   OrderQuantity INT,
   CustomerNum INT,
   CONSTRAINT OrdersPK PRIMARY KEY (OrderID),
   CONSTRAINT CustomersFK FOREIGN KEY (CustomerNum) REFERENCES Customers (CustID)
   )

Most commonly, a foreign key references the primary key field of the other table. However, a foreign key can reference an IDKEY or an IDENTITY column. In every case, the foreign key reference must exist in the referenced table and must be defined as unique; the referenced field cannot contain duplicate values or NULL.

In a foreign key definition, you can specify:

*   One field name: FOREIGN KEY (CustomerNum) REFERENCES Customers (CustID). The foreign key field (CustomerNum) and referenced field (CustID) may have different names (or the same name), but must have the same data type and field constraints.
    
*   A comma-separated list of field names: FOREIGN KEY (CustomerNum,SalespersonNum) REFERENCES Customers (CustID,SalespID). The foreign key fields and referenced fields must correspond in number of fields and in order listed.
    
*   An omitted field name: FOREIGN KEY (CustomerNum) REFERENCES Customers.
    

If you define a foreign key and omit the referenced field name, the foreign key defaults as follows:

1.  The primary key field defined for the specified table.
    
2.  If the specified table does not have a defined primary key, the foreign key defaults to the IDENTITY column defined for the specified table.
    
3.  If the specified table has neither a defined primary key nor a defined IDENTITY column, the foreign key defaults to the RowID, if the specified table defines the RowID as public. The specified table definition can do this explicitly, either by specifying the %PUBLICROWID keyword, or through the corresponding class definition with [SqlRowIdPrivate](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ROBJ_class_sqlrowidprivate)\=0 (the default).
    

If none of these defaults apply, Caché issues an SQLCODE -315 error.

In a class definition you can specify a Foreign Key that contains a field based on a parent table IDKEY property, as shown in the following example:

  ForeignKey Claim(CheckWriterPost.Hmo,Id,Claim) References Sample.Claim.Claim(DBMSKeyIndex);

Because the parent field defined in a foreign key of a child has to be part of the parent class's [IDKey index](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ROBJ_index_idkey), the only referential action supported for foreign keys of this type is NO ACTION.

If a foreign key references a nonexistent table, Caché issues an SQLCODE -310 error, with additional information provided in %msg. If a foreign key references a nonunique field, Caché issues an SQLCODE -314 error, with additional information provided in %msg. If the foreign key field references a single field, the two fields must have the same data type and field data constraints.

NOCHECK

If you specify the NOCHECK keyword, Caché does not check foreign key referential integrity. This means that an INSERT or UPDATE operation may specify a value for a foreign key field that does not correspond to a row in the referenced table. The NOCHECK keyword also prevents the execution of the referential action clause for the foreign key.

The SQL query processor can use foreign keys to optimize joins among tables. However, if a foreign key is defined as NOCHECK, the SQL query processor will not consider it as defined. A NOCHECK foreign key is still reported to xDBC catalog queries as a foreign key.

For further information refer to the “[Using Foreign Keys](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_foreignkeys)” chapter in Using Caché SQL.

Referential Action Clause

If a table contains a foreign key, a change to one table has an effect on another table. To keep the data consistent, when you define a foreign key, you also define what effect a change to the record from which the foreign key data comes has on the foreign key value.

A Foreign Key definition may contain two referential action clauses:

ON DELETE ref-action

and

ON UPDATE ref-action

The ON DELETE clause defines the DELETE rule for the referenced table. When an attempt to delete a row from the referenced table is made, the ON DELETE clause defines what action should be taken for the row(s) in the referencing table.

The ON UPDATE clause defines the UPDATE rule for the referenced table. When an attempt to change (update) the primary key value of a row from the referenced table is made, the ON UPDATE clause defines what action should be taken for the row(s) in the referencing table.

Caché SQL supports the following Foreign Key referential actions:

*   NO ACTION
    
*   SET DEFAULT
    
*   SET NULL
    
*   CASCADE
    

NO ACTION — When a row is deleted or its key value updated in the referenced table, all referencing tables are checked to see if any row references the row being deleted or updated. If so, the delete or update fails. (This constraint does not apply if the foreign key references itself.) NO ACTION is the default.

SET NULL — When a row is deleted or its key value updated in the referenced table, all referencing tables are checked to see if any row references the row being deleted or updated. If so, the action causes the foreign key fields which reference the row being deleted or updated to be set to NULL. The foreign key field must allow NULL values.

SET DEFAULT — When a row is deleted or its key value updated in the referenced table, all referencing tables are checked to see if any row references the row being deleted or updated. If so, the action causes the foreign key fields which reference the row being deleted or updated to be set to the field's default value. If the foreign key field does not have a default value, it will be set to NULL. It is important to note that a row must exist in the referenced table which contains an entry for the default value.

CASCADE — When a row is deleted in the referenced table, all referencing tables are checked to see if any row references the row being deleted. If so, the delete causes rows whose foreign key fields which reference the row being deleted to be deleted as well.

When the key value of a row is updated in the referenced table, all referencing tables are checked to see if any row references the row being updated. If so, the update causes the foreign key fields which reference the row being updated to cascade the update to all referencing rows.

Your table definition should not have two foreign keys with different names that reference the same identifier-commalist field(s) and perform contradictory referential actions. In accordance with the ANSI standard, Caché SQL does not issue an error if you define two foreign keys that perform contradictory referential actions on the same field (for example, ON DELETE CASCADE and ON DELETE SET NULL). Instead, Caché SQL issues an error when a DELETE or UPDATE operation encounters these contradictory foreign key definitions.

Here is an embedded SQL example that issues a CREATE TABLE statement that uses both referential action clauses. Note that this example assumes a related table named Physician (with a primary key field of PhysNum) already exists.

  DO $SYSTEM.Security.Login("\_SYSTEM","SYS")
  &sql(CREATE TABLE Patient (
     PatNum VARCHAR(16),
     Name VARCHAR(30),
     DOB DATE,
     Primary\_Physician VARCHAR(16) DEFAULT 'A10001982321',
     CONSTRAINT Patient\_PK PRIMARY KEY (PatNum),
     CONSTRAINT Patient\_Physician\_FK FOREIGN KEY
          Primary\_Physician REFERENCES Physician (PhysNum)
          ON UPDATE CASCADE
          ON DELETE SET NULL)
  )
  WRITE !,"SQL code: ",SQLCODE

For further information refer to the “[Using Foreign Keys](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_foreignkeys)” chapter in Using Caché SQL.

Implicit Foreign Key

It is preferable to explicitly define all foreign keys. However, it is possible to project implicit foreign keys to ODBC/JDBC and the Management Portal.

If a foreign key is not explicitly defined, the rules for an implicit foreign key are as follows:

1.  If there is an explicit foreign key defined, Caché reports this constraint.
    
2.  Else, each reference column in the table is checked to see if the reference is to a table with an index that is a primary key and IDKEY. If so, Caché reports this reference as a foreign key constraint.
    
3.  Else, if the reference field is the parent reference field and the referenced table reports the RowID field as the implicit primary key field, Caché reports this parent reference as a foreign key constraint.
    

If any of these implicit foreign key constraints are covered by an explicit foreign key definition, the implicit foreign key constraint is not defined.

Examples: Dynamic SQL and Embedded SQL

The following examples demonstrate a CREATE TABLE using Dynamic SQL and Embedded SQL. Note that in Dynamic SQL you can create a table and insert data into the table in the same program; in Embedded SQL you must use separate programs to create a table and insert data into that table.

The last program example deletes the table, so that you may run these examples repeatedly.

The following [Dynamic SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_dynsql) example creates the table Sample.MyStudents. Note that because COMPUTECODE is Caché ObjectScript code, not SQL code, the ObjectScript $PIECE function uses double quote delimiters; because the line of code is itself a quoted string, the $PIECE delimiters must be escaped as literals by doubling them, as shown:

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

 

The following [Embedded SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql) example creates the table Sample.MyStudents:

   ZNSPACE "Samples"
  &sql(CREATE TABLE Sample.MyStudents (
       StudentName VARCHAR(32),StudentDOB DATE,
       StudentAge INTEGER COMPUTECODE {SET {StudentAge}\=
       $PIECE(($PIECE($H,",",1)\-{StudentDOB})/365,".",1)} CALCULATED,
       Q1Grade CHAR,Q2Grade CHAR,Q3Grade CHAR,FinalGrade VARCHAR(2))
       )
  IF SQLCODE\=0 {WRITE !,"Created table" }
  ELSEIF SQLCODE\=\-201 {WRITE !,"SQLCODE=",SQLCODE," ",%msg }
  ELSE {WRITE !,"CREATE TABLE failed, SQLCODE=",SQLCODE } 

 

The following example deletes the table created by the prior examples:

  &sql(DROP TABLE Sample.MyStudents)
  IF SQLCODE\=0 {WRITE !,"Table deleted" }
  ELSE {WRITE !,"SQLCODE=",SQLCODE," ",%msg }

 

See Also

*   [ALTER TABLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_altertable), [DROP TABLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_droptable)
    
*   [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select), [JOIN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_join)
    
*   [INSERT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_insert), [UPDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_update), [INSERT OR UPDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_insertorupdate)
    
*   “[Defining Tables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_tables)” chapter in Using Caché SQL
    
*   [SQL configuration settings](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RACS_Settings#RACS_Category_SQL) described in Caché Advanced Configuration Settings Reference.
    
*   [SQLCODE error messages](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RERR_sql) listed in the Caché Error Reference
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createrole "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtrigger "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_createtable.xml**