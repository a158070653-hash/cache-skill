# DROP TABLE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_droptable

---

 

Caché SQL Reference

DROP TABLE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_droprole "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_droptrigger "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_COMMANDS "SQL Commands") \]  >  \[ [DROP TABLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_droptable "DROP TABLE") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Deletes a table and (optionally) its data.

Synopsis

DROP TABLE table 
     \[RESTRICT | CASCADE\] \[%DELDATA | %NODELDATA\]

Arguments

table

The name of the [table](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_tables) to be deleted.

RESTRICT

CASCADE

Optional — RESTRICT only allows a table with no dependent views or integrity constraints to be deleted. CASCADE allow a table with dependent views or integrity constraints to be deleted; any referencing views or integrity constraints will also be deleted as part of the table deletion. (See restriction on CASCADE below.)

%DELDATA

%NODELDATA

Optional — These keywords specify whether to delete data associated with a table when deleting the table. The default is to delete table data.

Description

The DROP TABLE command deletes a table. By default, it deletes both the table definition and the table’s data (if any exists). The %NODELDATA keyword allows you to specify deletion of the table definition but not the table’s data.

In order to delete a table, the following conditions must be met:

*   The table must exist in the current namespace. Attempting to delete a non-existent table generates an SQLCODE -30 error.
    
*   The table definition must be modifiable. If the class that projects the table is defined without [DllAllowed](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ROBJ_class_ddlallowed), attempting to delete the table generates an SQLCODE -300 error.
    
*   The table must not be locked by another concurrent process. If the table is locked, DROP TABLE waits indefinitely for the lock to be released. If lock contention is a possibility, it is important that you [LOCK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_lock) the table IN EXCLUSIVE MODE before issuing a DROP TABLE.
    
*   You must have the necessary privileges to delete the table. Attempting to delete a table without the necessary privileges generates an SQLCODE -99 error.
    

This statement can also be invoked using the [$SYSTEM.SQL.DropTable()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#DropTable) method call:

$SYSTEM.SQL.DropTable(tablename,deldata,SQLCODE,%msg)

Privileges

The DROP TABLE command is a privileged operation. Prior to using DROP TABLE it is necessary for your process to have either %DROP\_TABLE administrative privilege or a DELETE object privilege for the specified table. Failing to do so results in an SQLCODE –99 error (Privilege Violation). You can determine if the current user has DELETE privilege by invoking the [%CHECKPRIV](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_checkpriv) command. You can determine if a specified user has DELETE privilege by invoking the [$SYSTEM.SQL.CheckPriv()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#CheckPriv) method. You can use the [GRANT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_grant) command to assign %DROP\_TABLE privileges, if you hold appropriate granting privileges.

In embedded SQL, you can use the [$SYSTEM.Security.Login()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Security#Login) method to log in as a user with appropriate privileges:

   DO $SYSTEM.Security.Login("\_SYSTEM","SYS")
   &sql(      )

You must have the %Service\_Login:Use privilege to invoke the $SYSTEM.Security.Login method. For further information, refer to [%SYSTEM.Security](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Security) in the InterSystems Class Reference.

DROP TABLE cannot be used on a [table created by defining a persistent class](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_tables#GSQL_tables_via_classes), unless the table class definition includes \[[DdlAllowed](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ROBJ_class_ddlallowed)\]. Otherwise, the operation fails with an SQLCODE -300 error with the %msg DDL not enabled for class 'Schema.tablename'>

Existing Object Privileges

Deleting a table does not delete the object privileges for that table. For example, the privilege granted to a user to insert, update, or delete data on that table. This has the following two consequences:

*   If a table is deleted, and then another table with the same name is created, users and roles will have the same privileges on the new table that they had on the old table.
    
*   Once a table is deleted, it is not possible to revoke object privileges for that table.
    

For these reasons, it is generally recommended that you use the [REVOKE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_revoke) command to revoke object privileges from a table before deleting the table.

Table Containing Data

By default, DROP TABLE also deletes the table’s data. This table data delete is an atomic operation; if DROP TABLE encounters data that cannot be deleted (for example, a row with a referential constraint) any data deletion already performed is automatically rolled back, with the result that no table data is deleted.

The deletion of data can be overridden on a per-table basis, or system-wide. When deleting a table, you can specify DROP TABLE with the %NODELDATA option to prevent the automatic deletion of the table’s data. The default system configuration setting is to delete table data. If, however, the system-wide default is set to not delete table data, you can delete data on a per-table basis by specifying DROP TABLE with the %DELDATA option.

You can set the system-wide default for table data deletion as follows:

*   The [$SYSTEM.SQL.SetDDLDropTabDelData()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#SetDDLDropTabDelData) method call. To determine the current setting, call [$SYSTEM.SQL.CurrentSettings()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#CurrentSettings), which displays a DROP TABLE Deletes data setting.
    
*   Go to the Management Portal, select \[Home\] > \[Configuration\] > \[General SQL Settings\]. View the current setting of Does DDL DROP TABLE Delete the Table’s Data.
    

The default is “Yes” (1). This is the recommended setting for this option. Set this option to “No” (0) if you want DROP TABLE to not delete the table’s data when it deletes the table definition.

Lock Applied

The DROP TABLE statement acquires an exclusive table-level lock on table. This prevents other processes from modifying the table definition or the table data while table deletion is in process. This table-level lock is sufficient for deleting both the table definition and the table data; DROP TABLE does not acquire a lock on each row of the table data. This lock is automatically released at the end of the DROP TABLE operation.

Foreign Key Constraints

By default, you cannot drop a table if any foreign key constraints are defined on another table that references the table you are attempting to drop. You must drop all referencing foreign key constraints before dropping the table they reference. Failing to delete these foreign key constraints before attempting a DROP TABLE operation results in an SQLCODE -320 error.

This default behavior is consistent with the RESTRICT keyword option. The CASCADE keyword option is not supported for foreign key constraints.

To change this default foreign key constraint behavior, refer to the COMPILEMODE=NOCHECK option of the [SET OPTION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_setoption) command.

Associated Queries

Dropping a table automatically purges any related [cached queries](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_cachedqueries) and purges query information as stored in [%SYS.PTools.SQLQuery](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYS.PTools.SQLQuery). Dropping a table automatically purges any [SQL runtime statistics (SQL Stats)](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_optquery#GSQLOPT_optquery_stats) information for any related query.

Nonexistent Table

To determine if a specified table exists in the current namespace, use the [$SYSTEM.SQL.TableExists()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#TableExists) method.

If you try to delete a nonexistent table, DROP TABLE issues an SQLCODE -30 error by default. However, this error-reporting behavior can be overridden by setting the system configuration as follows:

*   The [$SYSTEM.SQL.SetDDLNo30()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#SetDDLNo30) method call. To determine the current setting, call [$SYSTEM.SQL.CurrentSettings()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#CurrentSettings), which displays a Suppress SQLCODE=-30 Errors: setting.
    
*   Go to the Management Portal, select \[Home\] > \[Configuration\] > \[General SQL Settings\]. View the current setting of Allow DDL DROP of Non-existent Table or View.
    

The default is “No” (0). This is the recommended setting for this option. Set this option to “Yes” (1) if you want a DROP TABLE for nonexistent table to perform no operation and not issue an error message.

Examples

The following embedded SQL example creates a table named "MyEmployees" and later deletes it. This example specifies that any data associated with this table not be deleted when the table is deleted:

  &sql(CREATE TABLE MyEmployees (
       NAMELAST     CHAR (30) NOT NULL,
       NAMEFIRST    CHAR (30) NOT NULL,
       STARTDATE    TIMESTAMP,
       SALARY       MONEY))
  WRITE !,"Created a table"
  /\*
    &sql(SQL code populating MyEmployees table)
    &sql(SQL code using MyEmployees table)
  \*/
  NEW SQLCODE,%msg
  &sql(DROP TABLE MyEmployees %NODELDATA)
  IF SQLCODE\=0 {WRITE !,"Table deleted"}
  ELSE {WRITE !,"SQLCODE=",SQLCODE,": ",%msg }

 

See Also

*   [ALTER TABLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_altertable) [CREATE TABLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable) [INSERT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_insert) [JOIN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_join) [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select) [UPDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_update)
    
*   “[Defining Tables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_tables)” chapter in Using Caché SQL
    
*   [SQL configuration settings](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RACS_Settings#RACS_Category_SQL) described in Caché Advanced Configuration Settings Reference.
    
*   [SQLCODE error messages](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RERR_sql) listed in the Caché Error Reference
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_droprole "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_droptrigger "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_droptable.xml**