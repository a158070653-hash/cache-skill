# TRUNCATE TABLE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_truncatetable

---

 

Caché SQL Reference

TRUNCATE TABLE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_top "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_union "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_COMMANDS "SQL Commands") \]  >  \[ [TRUNCATE TABLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_truncatetable "TRUNCATE TABLE") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Removes all data from a table and resets counters.

Synopsis

TRUNCATE TABLE tablename

Arguments

tablename

The [table](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_tables) from which you are deleting all rows. You can also specify a view through which table rows can be deleted. For further details, see [FROM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_from).

Description

The TRUNCATE TABLE command removes all rows from a table, and resets all table counters. You can truncate a table directly, or through a view. Truncating a table through a view is subject to delete requirements and restrictions, as described in [CREATE VIEW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createview).

The TRUNCATE TABLE operation resets the internal counters used for generating [ID field](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_tables#GSQL_tables_idfield), [IDENTITY field](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable#RSQL_createtable_idfield), and [%Counter field](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datatype#RSQL_datatype_rowversion) sequential integer values. Caché assigns a value of 1 for these fields in the first row inserted into a table following a TRUNCATE TABLE. Performing a DELETE on all rows of a table does not reset these internal counters. TRUNCATE TABLE does not reset the [ROWVERSION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datatype#RSQL_datatype_rowversion) counter.

TRUNCATE TABLE suppresses the pulling of base table triggers that are otherwise pulled during DELETE processing. Because TRUNCATE TABLE performs a delete with %NOTRIGGER behavior, the user must have been granted the %NOTRIGGER privilege (using the [GRANT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_grant) statement) in order to run TRUNCATE TABLE. This aspect of TRUNCATE TABLE is functionally identical to:

DELETE %NOTRIGGER FROM tablename

The TRUNCATE TABLE operation sets the [%ROWCOUNT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_svars) local variable to the number of deleted rows, and the [%ROWID](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_svars) local variable to the Row ID of the last row deleted.

Note:

The [DELETE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_delete) command provides more functionality than TRUNCATE TABLE, and is the preferred command for deleting data from a table. TRUNCATE TABLE is provided for compatibility for code migration from other database software.

To truncate a table:

*   The table must exist in the current (or specified) namespace. If the specified table cannot be located, Caché issues an SQLCODE -30 error.
    
*   You must have DELETE privilege for the table. Failing to have this privilege results in an SQLCODE -99 (Privilege Violation) error. You can determine if the current user has DELETE privilege by invoking the [%CHECKPRIV](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_checkpriv) command. You can determine if a specified user has DELETE privilege by invoking the [$SYSTEM.SQL.CheckPriv()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#CheckPriv) method. For privilege assignment, refer to the [GRANT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_grant) command.
    
*   The table cannot be defined as READONLY. Attempting to compile a TRUNCATE TABLE that references a read-only table results in an SQLCODE -115 error. Note that this error is now issued at compile time, rather than only occurring at execution time. See the description of READONLY objects in the [Other Options for Persistent Classes](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GOBJ_persother) chapter of Using Caché Objects.
    
*   If deleting through a view, the view must be updateable; it cannot be defined as WITH READ ONLY. Attempting to do so results in an SQLCODE -35 error. See the [CREATE VIEW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createview) command for further details.
    
*   All of the rows must be available for deletion. By default, if one or more rows cannot be deleted the TRUNCATE TABLE operation fails and no rows are deleted. If a row cannot be locked, TRUNCATE TABLE fails to delete any rows and issues an error. If deleting a row would violate foreign key referential integrity, TRUNCATE TABLE fails to delete any rows and instead issues an SQLCODE -124 error. This default behavior is modifiable, as described below.
    

Atomicity

By default, TRUNCATE TABLE, DELETE, UPDATE, and INSERT are atomic operations. A TRUNCATE TABLE either completes successfully or the whole operation is rolled back. If any row cannot be deleted, none of the rows are deleted and the database reverts to its state before issuing the TRUNCATE TABLE.

You can modify this default for the current process within SQL by invoking [SET TRANSACTION %COMMITMODE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_settransaction). You can modify this default for the current process in Caché ObjectScript by invoking the [SetAutoCommit()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#SetAutoCommit) method. The following options are available:

*   IMPLICIT or 1 (autocommit on) — The default behavior, as described above. Each TRUNCATE TABLE constitutes a separate transaction.
    
*   EXPLICIT or 2 (autocommit off) — If no transaction is in progress, a TRUNCATE TABLE automatically initiates a transaction, but you must explicitly COMMIT or ROLLBACK to end the transaction. In EXPLICIT mode the number of database operations per transaction is user-defined.
    
*   NONE or 0 (no auto transaction) — No transaction is initiated when you invoke TRUNCATE TABLE. A failed TRUNCATE TABLE operation can leave the database in an inconsistent state, with some rows deleted and some not deleted. To provide transaction support in this mode you must use START TRANSACTION to initiate the transaction and COMMIT or ROLLBACK to end the transaction.
    

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

 

Referential Integrity

Caché uses the system configuration setting to determine whether to perform foreign key referential integrity checking. You can set the system default as follows:

*   The [$SYSTEM.SQL.SetFilerRefIntegrity()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#SetFilerRefIntegrity) method call.
    
*   Go to the Management Portal, select \[Home\] > \[Configuration\] > \[General SQL Settings\]. View and edit the current setting of Perform Referential Integrity Checks on Foreign Keys for INSERT, UPDATE, and DELETE. The default is “Yes”. If you change this setting, any new process started after changing it will have the new setting.
    

During a TRUNCATE TABLE operation, for every foreign key reference a shared lock is acquired on the corresponding row in the referenced table. This row is locked until the end of the transaction. This ensures that the referenced row is not changed before a potential rollback of the TRUNCATE TABLE.

Transaction Locking

Caché performs standard locking on a TRUNCATE TABLE operation. Unique field values are locked for the duration of the current transaction.

The default lock threshold is 1000 locks per table. This means that if you delete more than 1000 unique field values from a table during a transaction, the lock threshold is reached and Caché automatically elevates the locking level from unique field value locks to a table lock. This permits large-scale deletes during a transaction without overflowing the lock table.

You can determine the current systemwide lock threshold value using the [GetLockThreshold()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#GetLockThreshold) method. This systemwide lock threshold value is configurable:

*   Using the [SetLockThreshold()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#SetLockThreshold) method.
    
*   Using the Management Portal. Go to \[Home\] > \[Configuration\] > \[General SQL Settings\]. View and edit the current setting of Lock Threshold.
    

You must have USE permission on the %Admin Manage Resource to change the lock threshold. Caché immediately applies any change made to the lock threshold value to all current processes.

For further details on transaction locking refer to [Transaction Processing](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_modify#GSQL_modify_transaction) in the “Modifying the Database” chapter of Using Caché SQL.

Imported SQL Code

The [DDLImport("CACHE")](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#DDLImport) and [Cache()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#Cache) methods do not support the TRUNCATE TABLE command. A TRUNCATE TABLE command found in an SQL code file imported by these methods is ignored. These import methods do support the DELETE command.

Examples

The following two Dynamic SQL examples compare DELETE and TRUNCATE TABLE. Each example creates a table, inserts rows into the table, deletes all the rows in the table, then inserts a single row into the now empty table.

The first example uses [DELETE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_delete) to delete all the records in the table. Note that DELETE does not reset the %RowID counter:

  ZNSPACE "SAMPLES"
  SET tcreate \= "CREATE TABLE Sample.MyStudents (StudentName VARCHAR(32),StudentDOB DATE)"
  SET tinsert \= "INSERT INTO Sample.MyStudents (StudentName,StudentDOB) "\_
                "SELECT Name,DOB FROM Sample.Person WHERE Age <= '21'"
  SET tinsert1 \= "INSERT INTO Sample.MyStudents (StudentName,StudentDOB) VALUES ('Bob Jones',60123)"
  SET tdelete \= "DELETE Sample.MyStudents"
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET qStatus \= tStatement.%Prepare(tcreate)
   IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
  SET rset \= tStatement.%Execute()
   WRITE rset.%StatementTypeName,!

  SET qStatus \= tStatement.%Prepare(tinsert)
   IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
  SET rset \= tStatement.%Execute()
   WRITE rset.%StatementTypeName," rowcount ",rset.%ROWCOUNT," last rowID: ",rset.%ROWID,!

  SET qStatus \= tStatement.%Prepare(tdelete)
   IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
  SET rset \= tStatement.%Execute()
   WRITE rset.%StatementTypeName," rowcount ",rset.%ROWCOUNT," last rowID: ",rset.%ROWID,!

  SET qStatus \= tStatement.%Prepare(tinsert1)
   IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
  SET rset \= tStatement.%Execute()
   WRITE rset.%StatementTypeName," rowcount ",rset.%ROWCOUNT," last rowID: ",rset.%ROWID,!
  &sql(DROP TABLE Sample.MyStudents)

 

The second example uses TRUNCATE TABLE to delete all the records in the table. Note that [%StatementTypeName](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SQL.StatementResult#%StatementTypeName) returns “DELETE” for TRUNCATE TABLE. Note that TRUNCATE TABLE does reset the %RowID counter to 0:

  ZNSPACE "SAMPLES"
  SET tcreate \= "CREATE TABLE Sample.MyStudents (StudentName VARCHAR(32),StudentDOB DATE)"
  SET tinsert \= "INSERT INTO Sample.MyStudents (StudentName,StudentDOB) "\_
                "SELECT Name,DOB FROM Sample.Person WHERE Age <= '21'"
  SET tinsert1 \= "INSERT INTO Sample.MyStudents (StudentName,StudentDOB) VALUES ('Bob Jones',60123)"
  SET ttrunc \= "TRUNCATE TABLE Sample.MyStudents"
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET qStatus \= tStatement.%Prepare(tcreate)
   IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
  SET rset \= tStatement.%Execute()
   WRITE rset.%StatementTypeName,!

  SET qStatus \= tStatement.%Prepare(tinsert)
   IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
  SET rset \= tStatement.%Execute()
   WRITE rset.%StatementTypeName," rowcount ",rset.%ROWCOUNT," last rowID: ",rset.%ROWID,!

  SET qStatus \= tStatement.%Prepare(ttrunc)
   IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
  SET rset \= tStatement.%Execute()
   WRITE rset.%StatementTypeName," rowcount ",rset.%ROWCOUNT," last rowID: ",rset.%ROWID,!

  SET qStatus \= tStatement.%Prepare(tinsert1)
   IF qStatus'=1 { WRITE "%Prepare failed",$System.Status.DisplayError(qStatus) QUIT}
  SET rset \= tStatement.%Execute()
   WRITE rset.%StatementTypeName," rowcount ",rset.%ROWCOUNT," last rowID: ",rset.%ROWID,!
  &sql(DROP TABLE Sample.MyStudents)

 

See Also

*   [FROM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_from)
    
*   [DELETE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_delete) [INSERT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_insert) [UPDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_update)
    
*   [CREATE VIEW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createview)
    
*   “[Defining Tables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_tables)” chapter in Using Caché SQL
    
*   “[Defining Views](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_views)” chapter of Using Caché SQL
    
*   [Transaction Processing](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_modify#GSQL_modify_transaction) in the “Modifying the Database” chapter of Using Caché SQL
    
*   [SQL configuration settings](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RACS_Settings#RACS_Category_SQL) described in Caché Advanced Configuration Settings Reference.
    
*   [SQLCODE error messages](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RERR_sql) listed in the Caché Error Reference
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_top "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_union "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:38**

Source: **RSQL\_truncatetable.xml**