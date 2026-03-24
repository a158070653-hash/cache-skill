# CREATE INDEX

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_createindex

---

 

Caché SQL Reference

CREATE INDEX

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createfunction "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createmethod "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_COMMANDS "SQL Commands") \]  >  \[ [CREATE INDEX](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createindex "CREATE INDEX") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Creates an index for a table.

Synopsis

CREATE \[UNIQUE | BITMAP | BITMAPEXTENT | BITSLICE \] INDEX index-name  
      ON \[TABLE\] \[schema-name.\]table-name
      (field-name, ...)
      \[WITH DATA  (datafield-name, ...)\]

Arguments

UNIQUE

Optional — A constraint that ensures there will not be two rows in the table with identical values in all the fields in the index. You cannot specify this keyword for a bitmap or bitslice index.

The UNIQUE keyword can be followed by (or replaced by) the CLUSTERED or NONCLUSTERED keywords. These keywords are no-ops; they are provided for compatibility with other vendors.

BITMAP

Optional — Indicates that a [bitmap index](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createindex#RSQL_createindex_bitmap) should be created. A bitmap index enables rapid queries on fields with a small number of distinct values.

BITMAPEXTENT

Optional — Indicates that a [bitmapextent index](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createindex#RSQL_createindex_bitmapextent) should be created. At most one bitmapextent index can be created for a table. No field-name is specified with BITMAPEXTENT.

BITSLICE

Optional — Indicates that a [bitslice index](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createindex#RSQL_createindex_bitslice) should be created. A bitslice index enables very fast evaluation of certain expressions, such as sums and range conditions. This is a specialized index type, which should only be used to solve very specific problems.

index-name

The index being defined. The name is an [identifier](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_identifiers). For further details, see the “Identifiers” chapter of Using Caché SQL.

table-name

The name of an existing [table](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_tables) for which the index is being defined. You cannot create an index for a view. The table’s schema name is optional.

field-name

One or more [field](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable#RSQL_createtable_fields) names that serve as the basis for the index. Field names must be enclosed in parentheses. Multiple field names are separated by commas.

Each field name can be followed by an ASC or DESC keyword. These keywords are no-ops; they are provided for compatibility with other vendors.

WITH DATA (datafield-name)

Optional — One or more [field](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable#RSQL_createtable_fields) names to be defined as [Data properties](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ROBJ_index_data) for the index. Field names must be enclosed in parentheses. Multiple field names are separated by commas. You cannot specify a WITH DATA clause when specifying a BITMAP or BITSLICE index.

See additional compatibility syntax below.

Description

CREATE INDEX creates a sorted index on the specified field (or fields) of the named table. Caché uses indices to improve performance of query operations. Caché automatically maintains indices during INSERT, UPDATE, and DELETE operations, and this index maintenance may negatively affect performance of these data modification operations.

You can create an index using the CREATE INDEX command or by adding an index definition to a class definition, as described in the [Defining and Building Indices](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_indices) chapter of Caché SQL Optimization Guide. You can delete an index by using the [DROP INDEX](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dropindex) command.

CREATE INDEX can be used to create any of the following three types of index:

*   A regular index ([Type](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ROBJ_index_type)\=index): Specify either CREATE INDEX (for non-unique values) or CREATE UNIQUE INDEX (for unique values).
    
*   A [bitmap index](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_indices#GSQLOPT_indices_bitmap) ([Type](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ROBJ_index_type)\=bitmap): Specify CREATE BITMAP INDEX.
    
*   A bitslice index ([Type](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ROBJ_index_type)\=bitslice): Specify CREATE BITSLICE INDEX.
    

You can also define an index using the [%Dictionary.IndexDefinition](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Dictionary.IndexDefinition) class.

Privileges and Locking

The CREATE INDEX command is a privileged operation. Prior to using CREATE INDEX it is necessary for your process to have either %ALTER\_TABLE administrative privileges or the %ALTER privilege for the specified table. Failing to do so results in an SQLCODE -99 error (Privilege Violation). You can determine if the current user has %ALTER privilege by invoking the [%CHECKPRIV](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_checkpriv) command. You can determine if a specified user has %ALTER privilege by invoking the [$SYSTEM.SQL.CheckPriv()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#CheckPriv) method. You can use the GRANT command to assign these privileges, if you hold appropriate granting privileges.

CREATE INDEX cannot be used on a [table created by defining a persistent class](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_tables#GSQL_tables_via_classes), unless the table class definition includes \[[DdlAllowed](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ROBJ_class_ddlallowed)\]. Otherwise, the operation fails with an SQLCODE -300 error with the %msg DDL not enabled for class 'Schema.tablename'>

The CREATE INDEX statement acquires a table-level lock on table-name. This prevents other processes from modifying the table’s data. This lock is automatically released at the conclusion of the CREATE INDEX operation. CREATE INDEX maintains a lock on the corresponding class definition until the completion of the create index operation, including the population of the index data.

Options Supported for Compatibility Only

Caché SQL accepts the following CREATE INDEX options for parsing purposes only, to aid in the conversion of existing SQL code to Caché SQL. These options do not provide any actual functionality.

CLUSTERED | NONCLUSTERED
owner.catalog.
ASC | DESC

The following is an example showing the placement of these no-op keywords:

CREATE UNIQUE CLUSTERED INDEX index-name  ON TABLE owner.catalog.schema.table   (field1 ASC, field2 DESC)

Index Name

The name of an index must be unique within a given table. Index names follow [identifier](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_identifiers) conventions, subject to the restrictions below. By default, index names are simple identifiers. An index name should not exceed 128 characters. Index names are not case-sensitive.

Caché uses the name you supply (which it refers to as the “SqlName”) to generate a corresponding index name in the class and the global. This index name contains only alphanumeric characters (letters and numbers) and is a maximum of 96 characters in length. To generate an index name, Caché first strips punctuation characters from the SqlName you supply, and then generates a unique identifier of 96 (or less) characters, substituting a capital letter for the final character when this is needed to create a unique index name.

*   An index name can be the same as a field, table, or view name, but such name duplication is not advised.
    
*   An index name (after punctuation stripping) must begin with a letter. Either the first character of the index name or the first character after initial punctuation characters must be a letter. A valid letter is a character that passes the [$ZNAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fzname) test. If the first character is a punctuation character (% or \_) and the second character is a number, Caché appends a lowercase “n” as the first character of the stripped name.
    
*   An index name (after punctuation stripping) must be unique. If you specify a duplicate index name, Caché generates an SQLCODE -324 error. If you specify an index name that differs only in punctuation characters from an existing index name, Caché substitute a capital letter (beginning with “A”) for the final character to create a unique index name. For this reason, it is not advisable (though possible) to create index names that differ only in their punctuation characters.
    
*   An index name may be much longer than 31 characters, but index names that differ in their first 31 alphanumeric characters are much easier to work with.
    

What happens when you try to create an index with the same name as an existing index is described below.

Existing Index

By default, Caché rejects an attempt to create an index that has the same name as an existing index for that table and issues an SQLCODE -324 error. This behavior is configurable, as follows:

*   The [$SYSTEM.SQL.SetDDLNo324()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#SetDDLNo324) method call. To determine the current setting, call [$SYSTEM.SQL.CurrentSettings()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#CurrentSettings), which displays a Suppress SQLCODE=-324 Errors setting.
    
*   Go to the Management Portal, select \[Home\] > \[Configuration\] > \[General SQL Settings\]. View the current setting of Allow DDL CREATE INDEX for Existing Index.
    

The default is “No” (0). By default, Caché rejects an attempt to create an index with the name of an existing index for that table and issues an SQLCODE -324 error. This is the recommended setting for this option.

If this option is set to “Yes” (1), Caché deletes the existing index from the class definition and then recreates it by performing the CREATE INDEX. It deletes the named index from the table specified in CREATE INDEX. This option permits the delete/recreate of a UNIQUE constraint index (which cannot be done using a DROP INDEX command). To delete/recreate a primary key index, refer to the [ALTER TABLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_altertable) command.

However, even if this option is set to allow the recreating of an existing index, you cannot recreate an IDKEY index if the table contains data. Attempting to do so generates an SQLCODE -324 error.

Table Name

You must specify the name of an existing table.

*   If table-name is a nonexistent table, CREATE INDEX fails with an SQLCODE -30 error, and sets %msg to Table 'SQLUSER.MYTABLE' does not exist.
    
*   If table-name is a view, CREATE INDEX fails with an SQLCODE -30 error, and sets %msg to Attempt to CREATE INDEX 'My\_Index' on view SQLUSER.MYVIEW failed. Indices only supported for tables, not views..
    

Creating an index modifies the table’s definition; if you do not have permission to change the table definition, CREATE INDEX fails with an SQLCODE -300 error, and sets %msg to DDL not enabled for class 'schema.tablename'.

Field Names

You must specify at least one field name to index on. Specify a field name or a comma-separated list of field names enclosed in parentheses. Duplicate field names are permitted and preserved in the index definition. Specifying more than one field may improve performance of GROUP BY operations, for example, group by state and then by city within each state. Generally, you should avoid indexing on a field or fields that have large amounts of duplicate data. For example, in a database of people indexing on a Name field would be appropriate because most names are unique. Indexing on a State field would (in most cases) not be appropriate because of the large number of duplicate data values. The fields you specify must exist in the table. Specifying a nonexistent field generates an SQLCODE -31 error.

In addition to ordinary data fields, you can use CREATE INDEX to create an index:

*   On a [serial field](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datatype#RSQL_datatype_serial) (a property of a serial class).
    
*   On an [IDENTITY field](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable#RSQL_createtable_idfield).
    
*   On the [ELEMENTS or KEYS value for a collection](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_indices#GSQLOPT_indices_collections).
    

You cannot create an index on a [stream value field](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datatype#RSQL_datatype_streams).

You cannot create an index with multiple IDKEY fields if one of the IDKEY fields (properties) is SQL Computed. This limitation does not apply to a single field IDKEY index. Because multiple IDKEY fields in an index are delimited using the “||” (double vertical bar) characters, you cannot include this character string in IDKEY field data.

WITH DATA Clause

Specifying this clause may allow a query to be resolved by only reading the index, which greatly reduces the amount of disk I/O, improving performance.

You should specify the same field in the field-name and the WITH DATA datafield-name if field-name uses [string collation](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_basics#GSQL_basics_collation); this allows retrieval of the uncollated value without having to go to the [master map](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_showplan#GSQLOPT_showplan_maps). If the value in field-name does not use string collation there is no advantage to specifying this field in the WITH DATA datafield-name.

You can specify fields in WITH DATA datafield-name that are not indexed. This allows more queries to be satisfied from the index without going to the master map. The tradeoff is how many indices you want to maintain; and that adding data to an index makes it quite a bit larger, which will slow down operations that don't need the data.

The UNIQUE Keyword

Using the UNIQUE keyword, you can specify that each record in the index has a unique value. More specifically, this ensures that no two records within the index (and hence in the table that contains the index) can have the same collated value. By default, most indices use uppercase [string collation](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_basics#GSQL_basics_collation) (to make searches not case-sensitive). In this case, the values “Smith” and “SMITH” are considered to be equal and not unique.

To make index collation and SQL queries case-sensitive, execute the following Caché ObjectScript command:

  WRITE $$SetEnvironment^%apiOBJ("collation","%String","SQLSTRING")

Go to the Management Portal, select the Classes option, select the namespace for your stored queries and use the Compile option to recompile the corresponding classes. Then rebuild all indices. They will be case-sensitive.

Caution:

Do not rebuild indices while the table’s data is being accessed by other users. Doing so may result in inaccurate query results.

The BITMAP Keyword

Using the BITMAP keyword, you can specify that this index will be a bitmap index. A bitmap index consists of one or more bit strings in which the bit position represents the row id, and each bit value represents the presence (1) or absence (0) of a specific value for the field in that row (or the value for the combined field-name fields). Caché SQL maintains these positional bits (as compressed bit strings) when inserting, updating, or deleting data; there is no significant difference in the performance of INSERT, UPDATE, or DELETE operations between using a bitmap index and a regular index. A bitmap index is highly efficient for many types of query operations. They have the following characteristics:

*   You can only define bitmap indices in tables (classes) that either use system-assigned ID with positive integer values, or use an IDKEY to define custom ID values when the IDKEY is based on a single property with type %Integer and MINVAL > 0, or type %Numeric with SCALE = 0 and MINVAL > 0. You can use the [$SYSTEM.SQL.SetBitmapFriendlyCheck()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#SetBitmapFriendlyCheck) method to set a systemwide configuration parameter to check at compile time for this restriction. You can use [$SYSTEM.SQL.GetBitmapFriendlyCheck()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#GetBitmapFriendlyCheck) to determine the current configuration of this option.
    
    You can only define a bitmap index for tables that use default ([%CacheStorage](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Library.CacheStorage)) structure. Tables with compound keys, such as a child table, cannot use a bitmap index. If you use DDL (as opposed to using class definitions) to create a table, it meets this requirement and you can make use of bitmap indices.
    
*   A bitmap index should only be used when the number of possible distinct field values is limited and relatively small. For example, a bitmap index is a good choice for a field for gender, or nationality, or timezone. A bitmap should not be used on a field with the UNIQUE constraint. A bitmap should not be used if a field can have more than 10,000 distinct values, or if multiple indexed fields can have more than 10,000 distinct values.
    
*   Bitmap indices are very efficient when used in combination with logical AND and OR operations in a [WHERE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_where) clause. If two or more fields are commonly queried in combination, it may be advantageous to define bitmap indices for those fields.
    

For more details, refer to the “[Bitmap Indices](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_indices#GSQLOPT_indices_bitmap)” section of the [Defining and Building Indices](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_indices) chapter of Caché SQL Optimization Guide.

The BITMAPEXTENT Keyword

A bitmap extent index is a bitmap index for the table itself. Caché SQL uses this index to improve performance of [COUNT(\*)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_count), which returns the number of records (rows) in the table. A table can have, at most, one bitmap extent index. Attempting to create more than one bitmap extent index results in an SQLCODE -400 error with the %msg ERROR #5445: Multiple Extent indices defined: DDLBEIndex.

At Caché 2015.2 and subsequent, all tables defined using [CREATE TABLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable) automatically define a bitmap extent index. This automatically-generated index is assigned the Index Name DDLBEIndex and the SQL MapName %%DDLBEIndex. A table defined as a class may have a bitmap extent index defined with an Index Name and SQL MapName of $ClassName.

You can use CREATE BITMAPEXTENT INDEX to add a bitmap extent index to a table, or to rename an automatically-generated bitmap extent index. The index-name you specify should be the class name corresponding to the table-name of the table. This becomes the SQL MapName for the index. No field-name or WITH DATA clause can be specified.

The following example creates a bitmap extent index with Index Name DDLBEIndex and the SQL MapName Patient. If Sample.Patient already had a %%DDLBEIndex bitmap extent index, this example renames that index to SQL MapName Patient:

  ZNSPACE "Samples"
  &sql(CREATE BITMAPEXTENT INDEX Patient ON TABLE Sample.Patient)
  WRITE !,"SQL code: ",SQLCODE

For more details, refer to the “[Bitmap Extent Index](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_indices#GSQLOPT_indices_bitmapextent)” section of the [Defining and Building Indices](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_indices) chapter of Caché SQL Optimization Guide.

The BITSLICE Keyword

Using the BITSLICE keyword, you can specify that this index will be a bitslice index. A bitslice index is used exclusively for numeric data which is used in calculations. A bitslice index represents each numeric data value as a binary bit string. Rather than indexing a numeric data value using a boolean flag (as in a bitmap index), a bitslice index creates a bit string for each numeric value, a separate bit string for each record. This is a highly specialized type of index that should only be used for fast aggregate calculations. For example, the following would be a candidate for a bitslice index:

SELECT SUM(Salary) FROM Sample.Employee

A bitslice index should not be used in a WHERE clause, because they are not used by the SQL query optimizer.

Populating and maintaining a bitslice index using INSERT, UPDATE, or DELETE operations is significantly slower than using a bitmap index or a regular index. Using several bitslice indices, and/or using a bitslice index on a field that is frequently updated may have a significant performance cost.

A bitslice index can only be used for records that have system-assigned row Ids with positive integer values. A bitslice index can only be used on a single field-name. You cannot specify a WITH DATA clause.

For more details, refer to the “[Bitslice Indices](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_indices#GSQLOPT_indices_bitslice)” section of the [Defining and Building Indices](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_indices) chapter of Caché SQL Optimization Guide.

Rebuilding an Index

Creating an index using the CREATE INDEX statement automatically builds the index. However, there are cases when you may wish to explicitly rebuild an index.

Caution:

You must take additional steps when rebuilding an index if the table’s data is being accessed by other users. Failing to do so may result in inaccurate query results. For more details, refer to [Building Indices on an Active System](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_indices#GSQLOPT_indices_build_readonly).

To rebuild all indices for an inactive table, execute the following:

  SET status \= ##class(myschema.mytable).%BuildIndices()

By default, this command purges the indices prior to rebuilding them. You can override this purge default and use the %PurgeIndices() method to explicitly purge specified indices. If you call %BuildIndices() for a range of ID values, Caché does not purge indices by default.

You can also purge/rebuild specified indices:

  SET status \= ##class(myschema.mytable).%BuildIndices($ListBuild("NameIDX","SpouseIDX"))

You may want to purge/rebuild an index if the index is corrupt or to change the case sensitivity of the index, as described above. To [recompress a bitmap index](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_indices#GSQLOPT_indices_bmap_maint), use the [%SYS.Maint.Bitmap](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYS.Maint.Bitmap) methods, rather than purge/rebuild.

You can also use the Management Portal to rebuild all of the indices for a specified class (table).

For more details, refer to [Building Indices](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_indices#GSQLOPT_indices_build) in the “Defining and Building Indices” chapter of Caché SQL Optimization Guide.

Examples

The following embedded SQL example creates a table named Fred, and then creates an index named "FredIndex" (by stripping out the punctuation from the supplied name “Fred\_Index”) on the Lastword and Firstword fields of the Fred table.

   &sql(CREATE TABLE Fred (
  TESTNUM     INT NOT NULL,
  FIRSTWORD   CHAR (30) NOT NULL,
  LASTWORD    CHAR (30) NOT NULL,
  CONSTRAINT FredPK PRIMARY KEY (TESTNUM))
  )
  IF SQLCODE\=0 { WRITE !,"Table created" }
  ELSEIF SQLCODE\=\-201 { WRITE !,"Table already exists" }
  ELSE { WRITE !,"SQL table create error code is: ",SQLCODE
         QUIT }
  &sql(CREATE INDEX Fred\_Index
       ON TABLE Fred
       (LASTWORD,FIRSTWORD))
  IF SQLCODE\=\-324 {
      WRITE !,"Index already exists" 
      QUIT }
  ELSEIF SQLCODE\=0 { WRITE !,"Index created" }
  ELSE { WRITE !,"SQL index create error code is: ",SQLCODE 
         QUIT }

 

The following example creates an index, named “CityIndex” on the City field of the Staff table:

CREATE INDEX CityIndex ON Staff (City)

The following example creates an index, named “EmpIndex” on the EmpName field of the Staff table. The UNIQUE constraint is used to avoid having rows with identical values in the fields:

CREATE UNIQUE INDEX EmpIndex ON TABLE Staff (EmpName)

The following example creates a bitmap index, named “SKUIndex” on the SKU field of the Purchases table. The BITMAP keyword indicates that this is a bitmap index:

CREATE BITMAP INDEX SKUIndex ON TABLE Purchases (SKU)

See Also

*   [DROP INDEX](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dropindex) command
    
*   [SEARCH\_INDEX](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_searchindex) function
    
*   “[Defining Tables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_tables)” chapter in Using Caché SQL
    
*   “[Defining and Building Indices](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_indices)” chapter in Caché SQL Optimization Guide
    
*   “[Using Indices](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_optquery#GSQLOPT_optquery_ind)” in the “Optimizing Query Performance” chapter in Caché SQL Optimization Guide
    
*   [SQL configuration settings](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RACS_Settings#RACS_Category_SQL) described in Caché Advanced Configuration Settings Reference.
    
*   [SQLCODE error messages](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RERR_sql) listed in the Caché Error Reference
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createfunction "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createmethod "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_createindex.xml**