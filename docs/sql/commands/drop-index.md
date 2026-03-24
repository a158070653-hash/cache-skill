# DROP INDEX

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_dropindex

---

 

Caché SQL Reference

DROP INDEX

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dropfunction "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dropmethod "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_COMMANDS "SQL Commands") \]  >  \[ [DROP INDEX](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dropindex "DROP INDEX") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Removes an index.

Synopsis

DROP INDEX index-name \[ON \[TABLE\] \[schema-name.\]table-name\]

DROP INDEX \[schema-name.\]table-name.index-name

Arguments

index-name

The name of the index to be deleted. index-name may be a standard index or a bitmap index.

ON table-name

or

ON TABLE table-name

Optional — The name of the table associated with the index. You can specify the table-name using either syntax: The first syntax uses the ON clause; the TABLE keyword is optional. The second syntax uses the qualified name syntax schema-name.table-name.index-name; the table’s schema name is optional. If you omit the table-name, Caché deletes the first index found that matches index-name, as described below.

Description

A DROP INDEX statement deletes an index. DROP INDEX does not apply to indices created by defining PRIMARY KEY or UNIQUE constraints (created by using the PRIMARY KEY or UNIQUE options of either the CREATE TABLE or ALTER TABLE statements, respectively).

You may wish to delete an index for any of the following reasons:

*   You intend to perform large numbers of INSERT, UPDATE, or DELETE operations on a table. Rather than accepting the performance overhead of having each of these operations write to the index, you can use the %NOINDEX option for the operation. Or, in certain cases, it may be preferable to delete the index, perform the bulk changes to the database, and then recreate the index and populate it.
    
*   An index exists for a field or combination of fields that are not used for query operations. In this case, the performance overhead of maintaining the index may not be worthwhile.
    
*   An index exists for a field or combination of fields that now contain large amounts of duplicate data. In this case, the minimal gain to query performance may not be worthwhile.
    

You cannot drop an IDKEY index when there is data in the table. Attempting to do so generates an SQLCODE -325 error.

Privileges and Locking

The DROP INDEX command is a privileged operation. Prior to using DROP INDEX it is necessary for your process to have either %ALTER\_TABLE administrative privileges or the %ALTER privilege for the specified table. Failing to do so results in an SQLCODE –99 error (Privilege Violation). You can determine if the current user has %ALTER privilege by invoking the [%CHECKPRIV](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_checkpriv) command. You can determine if a specified user has %ALTER privilege by invoking the [$SYSTEM.SQL.CheckPriv()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#CheckPriv) method.You can use the GRANT command to assign these privileges, if you hold appropriate granting privileges.

DROP INDEX cannot be used on a [table created by defining a persistent class](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_tables#GSQL_tables_via_classes), unless the table class definition includes \[[DdlAllowed](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ROBJ_class_ddlallowed)\]. Otherwise, the operation fails with an SQLCODE -300 error with the %msg DDL not enabled for class 'Schema.tablename'>

The DROP INDEX statement acquires a table-level lock on table-name. This prevents other processes from modifying the table’s data. This lock is automatically released at the conclusion of the DROP INDEX operation.

Index Name and Table Name

When specify an index-name to create an index, Caché generates a corresponding class index name by stripping out any punctuation characters; it retains the index-name you specified in the class as the SqlName value for the index. When you drop an index, Caché searches the class (or classes) for either the matching SqlName or the matching class index name.

You can specify the table associated with the index using either DROP INDEX syntax form:

*   index-name ON TABLE syntax: specifying the table name is optional. If omitted, Caché searches all of the classes in the namespace for the corresponding index.
    
*   table-name.index-name syntax: specifying the table name is required.
    

In either syntax, the table name can be unqualified (table), or qualified (schema.table). If the schema name is omitted, the system-wide [default schema](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_basics#GSQL_basics_schemas) is used.

Note that DROP INDEX assumes that all index names in a namespace are unique. It is strongly suggested that you create unique index names, but Caché does not enforce uniqueness of index names. If multiple indices share a name, and you do not explicitly specify the associated table, DROP INDEX removes the index associated with the first table it finds, using an alphabetically ordered search of the corresponding class names.

Nonexistent Index

If you try to delete a nonexistent index, DROP INDEX issues an SQLCODE -333 error, by default. However, this default can be overridden system-wide by setting a configuration option as follows:

*   The [$SYSTEM.SQL.SetDDLNo333()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#SetDDLNo333) method call. To determine the current setting, call [$SYSTEM.SQL.CurrentSettings()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#CurrentSettings), which displays a Suppress SQLCODE=-333 Errors setting.
    
*   Go to the Management Portal, select \[Home\] > \[Configuration\] > \[General SQL Settings\]. View the current setting of Allow DDL DROP of Non-existent Index.
    

The default is “No” (0). By default, Caché issues an SQLCODE -333 error. This is the recommended setting for this option. Set this option to “Yes” (1) if you want a DROP INDEX for a nonexistent index to perform no operation and issue no error message. For further details, refer to [SQL configuration settings](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RACS_Settings#RACS_Category_SQL) described in Caché Advanced Configuration Settings Reference.

Table Name

If you specify the optional table-name, it must correspond to an existing table.

*   If the specified table-name does not exist, Caché issues an SQLCODE -30 error and sets %msg to Table 'SQLUser.tname' does not exist.
    
*   If the specified table-name exists but does not have an index named index-name, Caché issues an SQLCODE -333 error and sets %msg to Attempt to DROP INDEX 'MyIndex' on table SQLUSER.TNAME failed - index not found.
    
*   If the specified table-name is a view, Caché issues an SQLCODE -333 error and sets %msg to Attempt to DROP INDEX 'EmpSalaryIndex' on view SQLUSER.VNAME failed. Indices only supported for tables, not views.
    

Examples

The first example creates a table named Employee, which is used in all of the examples in this section.

The following embedded SQL example creates an index named "EmpSalaryIndex" and later removes it. Note that here DROP INDEX does not specify the table associated with the index; it assumes that "EmpSalaryIndex" is a unique index name in this namespace.

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
  WRITE !,"SQLCODE=",SQLCODE," Created a table"
  &sql(CREATE INDEX EmpSalaryIndex
       ON TABLE Employee
       (Namelast,Salary))
  WRITE !,"SQLCODE=",SQLCODE," Created an index"
  /\* use the index \*/
  NEW SQLCODE,%msg
  &sql(DROP INDEX EmpSalaryIndex)
  WRITE !,"SQLCODE=",SQLCODE," Deleted an index"
  WRITE !,"message",%msg

 

The following embedded SQL example specifies the table associated with the index to be dropped using an ON TABLE clause:

  &sql(CREATE INDEX EmpVacaIndex
       ON TABLE Employee
       (NameLast,AccruedVacation))
  WRITE !,"SQLCODE=",SQLCODE," Created an index"
  /\* use the index \*/
  &sql(DROP INDEX EmpVacaIndex ON TABLE Employee)
  WRITE !,"SQLCODE=",SQLCODE," Deleted an index"

 

The following embedded SQL example specifies the table associated with the index to be dropped using qualified name syntax:

  &sql(CREATE INDEX EmpSickIndex
       ON TABLE Employee
       (NameLast,AccruedSickLeave))
  WRITE !,"SQLCODE=",SQLCODE," Created an index"
  /\* use the index \*/
  &sql(DROP INDEX Employee.EmpSickIndex)
  WRITE !,"SQLCODE=",SQLCODE," Deleted an index"

 

The following command attempts to drop a nonexistent index. It generates an SQLCODE -333 error:

DROP INDEX PeopleIndex ON TABLE Employee

 

See Also

*   [CREATE INDEX](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createindex)
    
*   “[Defining and Building Indices](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_indices)” chapter in Caché SQL Optimization Guide
    
*   [SQLCODE error messages](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RERR_sql) listed in the Caché Error Reference
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dropfunction "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dropmethod "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_dropindex.xml**