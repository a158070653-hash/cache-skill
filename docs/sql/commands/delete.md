# DELETE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_delete

---

 

Caché SQL Reference

DELETE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_declare "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_distinct "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_COMMANDS "SQL Commands") \]  >  \[ [DELETE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_delete "DELETE") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Removes rows from a table.

Synopsis

DELETE \[%NOFPLAN\] \[restriction\] \[FROM\] table-ref \[\[AS\] t-alias\]
     \[FROM select-table1 \[\[AS\] t-alias\]
              {,select-table2 \[\[AS\] t-alias\]} \]
     \[WHERE condition-expression\]

DELETE \[restriction\] \[FROM\] table-ref \[\[AS\] t-alias\]
     \[WHERE CURRENT OF cursor\]

Arguments

%NOFPLAN

Optional — The %NOFPLAN keyword specifies that Caché will ignore the frozen plan (if any) for this operation and generate a new query plan. The frozen plan is retained, but not used. For further details, refer to [Frozen Plans](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_frozenplans) in Caché SQL Optimization Guide.

restriction

Optional — One or more of the following keywords, separated by spaces: %NOLOCK, %NOCHECK, %NOINDEX, %NOTRIGGER.

FROM table-ref

The [table](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_tables) from which you are deleting rows. This is not a FROM clause; it is a FROM keyword followed by a single table reference. (The FROM keyword is optional; the table-ref is mandatory.) Rather than a table reference, you can specify a [view](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_views) through which table rows can be deleted, or specify a subquery enclosed in parentheses. Unlike the SELECT statement FROM clause, you cannot specify optimize-option keywords here.

FROM clause

Optional — A FROM clause, specified after the table-ref. This [FROM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_from) can be used to specify a select-table table or tables used to select which rows are to be deleted.

Multiple tables can be specified as a comma-separated list or associated with ANSI join keywords. Any combination of tables or views can be specified. If you specify a comma between two select-tables here, Caché performs a [CROSS JOIN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_join) on the tables and retrieves data from the results table of the JOIN operation. If you specify ANSI join keywords between two select-tables here, Caché performs the specified join operation. For further details, refer to the [JOIN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_join) page of this manual.

You can optionally specify one or more optimize-option keywords to optimize query execution. The available options are: %ALLINDEX, %FIRSTTABLE tablename, %FULL, %INORDER, %IGNOREINDICES, %NOFLATTEN, %NOMERGE, %NOSVSO, %NOTOPOPT, %NOUNIONOROPT, and %STARTTABLE. See [FROM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_from) clause for more details.

AS t-alias

Optional — An [alias for a table or view name](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select#RSQL_select_fromclause_talias). An alias must be a valid [identifier](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_identifiers). The AS keyword is optional.

WHERE condition-expression

Optional — Specifies one or more boolean [predicates](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_predicates) used to limit which rows are to be deleted. You can specify a WHERE clause or a WHERE CURRENT OF clause, but not both. If a WHERE clause (or a WHERE CURRENT OF clause) is not supplied, DELETE removes all the rows from the table. For further details, see [WHERE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_where).

WHERE CURRENT OF cursor

Optional: Embedded SQL only — Specifies that the DELETE operation deletes the record at the current position of [cursor](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_cursor). You can specify a WHERE CURRENT OF clause or a WHERE clause, but not both. If a WHERE CURRENT OF clause (or a WHERE clause) is not supplied, DELETE removes all the rows from the table. For further details, see [WHERE CURRENT OF](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_wherecurrentof).

Description

The DELETE command removes rows from a table that meet the specified conditions. You can delete rows from a table directly, delete through a view, or delete rows selected using a subquery. Deleting through a view is subject to requirements and restrictions, as described in [CREATE VIEW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createview).

The DELETE operation sets the [%ROWCOUNT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_svars) local variable to the number of deleted rows, and the [%ROWID](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_svars) local variable to the Row ID of the last row deleted.

You must specify a table-ref; the FROM keyword before the table-ref is optional. To delete all rows from a table, you can simply specify:

DELETE FROM tablename

or

DELETE tablename

This deletes all row data from the table, but does not reset the [RowId](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_tables#GSQL_tables_idfield), [IDENTITY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable#RSQL_createtable_idfield), and [%Counter](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datatype#RSQL_datatype_rowversion) counters. The [TRUNCATE TABLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_truncatetable) command both deletes all row data from a table and resets these counters. By default, DELETE FROM tablename pulls delete triggers; you can specify DELETE %NOTRIGGER FROM tablename to not pull delete triggers. TRUNCATE TABLE does not pull delete triggers.

More commonly, a DELETE specifies the deletion of a specific row (or rows) based on a condition-expression. By default, a DELETE operation goes through all of the rows of a table and deletes all rows that satisfy the condition-expression. If no rows satisfy the condition-expression, DELETE completes successfully and sets SQLCODE=100 (No more data).

You can specify a WHERE clause or a WHERE CURRENT OF clause (but not both). If the WHERE CURRENT OF clause is used, the DELETE operation deletes the record at the current position of the cursor. For an example of DELETE using WHERE CURRENT OF, see “[Embedded SQL and Dynamic SQL Examples](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_delete#RSQL_delete_examplesrun)” below. For details on positioned operations, see [WHERE CURRENT OF](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_wherecurrentof).

By default, DELETE is an all-or-nothing event: either all specified rows are deleted completely, or no deletion is performed. Caché sets the status variable [SQLCODE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_svars), indicating the success or failure of the DELETE.

To delete a row from a table:

*   The table must exist in the current (or specified) namespace. If the specified table cannot be located, Caché issues an SQLCODE -30 error.
    
*   You must have DELETE privilege for the table. Failing to have this privilege results in an SQLCODE -99 (Privilege Violation) error. You can determine if the current user has DELETE privilege by invoking the [%CHECKPRIV](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_checkpriv) command. You can determine if a specified user has DELETE privilege by invoking the [$SYSTEM.SQL.CheckPriv()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#CheckPriv) method. For privilege assignment, refer to the [GRANT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_grant) command.
    
*   The table cannot be locked IN EXCLUSIVE MODE by another process. Attempting to delete a row from a locked table results in an SQLCODE -110 error, with a %msg such as the following: Unable to acquire lock for DELETE of table 'Sample.Person' on row with RowID = '10'. Note that an SQLCODE -110 error occurs only when the DELETE statement locates the first record to be deleted, then cannot lock it within the timeout period.
    
*   If the DELETE command’s WHERE clause specifies a non-existent field, an SQLCODE -29 is issued. To list all of the field names defined for a specified table, refer to [Column Names and Numbers](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_tables#GSQL_tables_getcols) in the “Defining Tables” chapter of Using Caché SQL. If the field exists but none of the field values fulfill the DELETE command’s WHERE clause, no rows are affected and SQLCODE 100 (end of data) is issued.
    
*   The table cannot be defined as READONLY. Attempting to compile an DELETE that references a read-only table results in an SQLCODE -115 error. Note that this error is now issued at compile time, rather than only occurring at execution time. See the description of READONLY objects in the [Other Options for Persistent Classes](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GOBJ_persother) chapter of Using Caché Objects.
    
*   If deleting through a view, the view cannot be defined as WITH READ ONLY. Attempting to do so results in an SQLCODE -35 error. See the [CREATE VIEW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createview) command for further details. Similarly, if you are attempting to delete through a subquery, the subquery must be updateable; for example, the following subquery results in an SQLCODE -35 error: DELETE FROM (SELECT COUNT(\*) FROM Sample.Person) AS x.
    
*   The row to delete must exist. Usually, attempting to delete a nonexistent row results in an SQLCODE 100 (No more data) because the specified row could not be located. However, in rare cases, a row to be deleted may be located, but then is immediately deleted by another process; this situation results in an SQLCODE -106 error.
    
*   All of the rows specified for deletion must be available for deletion. By default, if one or more rows cannot be deleted the DELETE operation fails and no rows are deleted. If a row to be deleted has been locked by another concurrent process, DELETE issues an SQLCODE -110 error. If deleting one of the specified rows would violate foreign key referential integrity (and %NOCHECK is not specified), the DELETE issues an SQLCODE -124 error. This default behavior is modifiable, as described below.
    
*   Certain %SYS namespace system–supplied facilities are protected against deletion. For example, DELETE FROM Security.Users cannot be used to delete \_SYSTEM, \_PUBLIC or UnknownUser. Attempting to do so results in an SQLCODE -134 error.
    

Atomicity

By default, DELETE, UPDATE, INSERT, and TRUNCATE TABLE are atomic operations. A DELETE either completes successfully or the whole operation is rolled back. If any of the specified rows cannot be deleted, none of the specified rows are deleted and the database reverts to its state before issuing the DELETE.

You can modify this default for the current process within SQL by invoking [SET TRANSACTION %COMMITMODE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_settransaction). You can modify this default for the current process in Caché ObjectScript by invoking the [SetAutoCommit()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#SetAutoCommit) method. The following options are available:

*   IMPLICIT or 1 (autocommit on) — The default behavior, as described above. Each DELETE constitutes a separate transaction.
    
*   EXPLICIT or 2 (autocommit off) — If no transaction is in progress, a DELETE automatically initiates a transaction, but you must explicitly COMMIT or ROLLBACK to end the transaction. In EXPLICIT mode the number of database operations per transaction is user-defined.
    
*   NONE or 0 (no auto transaction) — No transaction is initiated when you invoke DELETE. A failed DELETE operation can leave the database in an inconsistent state, with some of the specified rows deleted and some not deleted. To provide transaction support in this mode you must use START TRANSACTION to initiate the transaction and COMMIT or ROLLBACK to end the transaction.
    

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

 

FROM Syntax

A DELETE command can contain two FROM keywords that specify tables. These two uses of FROM are fundamentally different:

*   FROM before table-ref specifies the table (or view) from which rows are to be deleted. It is a FROM keyword, not a FROM clause. Only one table may be specified. No join syntax or optimize-option keywords may be specified. The FROM keyword itself is optional; the table-ref is required.
    
*   FROM after table-ref is an optional FROM clause that can be used to determine which rows should be deleted. It may specify one or more than one tables. It supports all of the FROM clause syntax available to a SELECT statement, including join syntax and optimize-option keywords. This FROM clause is commonly (but not always) used with a WHERE clause.
    

Thus any of the following are valid syntactical forms:

DELETE FROM table WHERE ...
DELETE table WHERE ...
DELETE FROM table FROM table2 WHERE ...
DELETE table FROM table2 WHERE ...

This syntax supports complex selection criteria in a manner compatible with Transact-SQL.

The following example shows how the two FROM keywords might be used. It deletes those records from the Employees table where the same EmpId is also found in the Retirees table:

DELETE FROM Employees AS Emp
       FROM Retirees AS Rt
       WHERE Emp.EmpId \= Rt.EmpId

If the two FROM keywords make reference to the same table, these references may either be to the same table, or to a join of two instances of the table. This depends on how table aliases are used:

*   If neither table reference has an alias, both reference the same table:
    
      DELETE FROM table1 FROM table1,table2   /\* join of 2 tables \*/
    
*   If both table references have the same alias, both reference the same table:
    
      DELETE FROM table1 AS x FROM table1 AS x,table2   /\* join of 2 tables \*/
    
*   If both table references have aliases, and the aliases are different, Caché performs a join of two instances of the table:
    
      DELETE FROM table1 AS x FROM table1 AS y,table2   /\* join of 3 tables \*/
    
*   If the first table reference has an alias, and the second does not, Caché performs a join of two instances of the table:
    
      DELETE FROM table1 AS x FROM table1,table2   /\* join of 3 tables \*/
    
*   If the first table reference does not have an alias, and the second has a single reference to the table with an alias, both reference the same table, and this table has the specified alias:
    
      DELETE FROM table1 FROM table1 AS x,table2   /\* join of 2 tables \*/
    
*   If the first table reference does not have an alias, and the second has more than one reference to the table, Caché considers each aliased instance a separate table and performs a join on these tables:
    
      DELETE FROM table1 FROM table1,table1 AS x,table2   /\* join of 3 tables \*/
      DELETE FROM table1 FROM table1 AS x,table1 AS y,table2   /\* join of 4 tables \*/
    

Restriction Arguments

To use a restriction argument, you must have the corresponding admin-privilege for the current namespace. Refer to [GRANT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_grant) for further details.

Specifying restriction argument(s) restricts processing as follows:

*   %NOCHECK — suppress referential integrity checking for foreign keys that reference the rows being deleted.
    
*   %NOLOCK — suppress row locking of the row being deleted. This should only be used when a single user/process is updating the database.
    
*   %NOINDEX — suppresses deleting index entries in all indices for the rows being deleted. This should be used with extreme caution, because it leaves orphaned values in the table indices.
    
*   %NOTRIGGER — suppress the pulling of base table triggers that are otherwise pulled during DELETE processing.
    

You can specify multiple restriction arguments in any order. Multiple arguments are separated by spaces.

If you specify a restriction argument when deleting a parent record, the same restriction argument will be applied when deleting the corresponding child records.

Referential Integrity

If you do not specify %NOCHECK, Caché uses the system configuration setting to determine whether to perform foreign key referential integrity checking. You can set the system default as follows:

*   The [$SYSTEM.SQL.SetFilerRefIntegrity()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#SetFilerRefIntegrity) method call.
    
*   Go to the Management Portal, select \[Home\] > \[Configuration\] > \[General SQL Settings\]. View and edit the current setting of Perform Referential Integrity Checks on Foreign Keys for INSERT, UPDATE, and DELETE. The default is “Yes”. If you change this setting, any new process started after changing it will have the new setting.
    

During a DELETE operation, for every foreign key reference a shared lock is acquired on the corresponding row in the referenced table. This row is locked until the end of the transaction. This ensures that the referenced row is not changed before a potential rollback of the DELETE.

If a series of foreign key references are defined as CASCADE, a DELETE operation could potentially result in a circular reference. Caché prevents DELETE with CASCADE referential action from performing a circular reference loop recursion. Caché ends the cascade sequence when it returns to the original table.

If a DELETE operation with %NOLOCK is performed on a [foreign key field defined with CASCADE, SET NULL, or SET DEFAULT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable#RSQL_createtable_fkey), the corresponding referential action changing the foreign key table is also performed with %NOLOCK.

Transaction Locking

If you do not specify %NOLOCK, Caché automatically performs standard record locking on INSERT, UPDATE, and DELETE operations. Each affected record (row) is locked for the duration of the current transaction.

The default lock threshold is 1000 locks per table. This means that if you delete more than 1000 records from a table during a transaction, the lock threshold is reached and Caché automatically escalates the locking level from record locks to a table lock. This permits large-scale deletes during a transaction without overflowing the lock table.

Caché applies one of the two following lock escalation strategies:

*   “E”-type lock escalation: Caché uses this type of lock escalation if the following are true: (1) the class uses [%CacheStorage](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GOBJ_defpersobj#GOBJ_defpersobj_storage) (you can determine this from the [Catalog Details](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_smp#GSQL_smp_catdetails) in the Management Portal SQL schema display). (2) the class either does not specify an IDKey index, or specifies a single-property IDKey index. “E”-type lock escalation is described in the [LOCK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_clock) command in the Caché ObjectScript Reference.
    
*   Traditional SQL lock escalation: The most likely reason why a class would not use “E”-type lock escalation is the presence of a multi-property IDKey index. In this case, each %Save increments the lock counter. This means if you do 1001 saves of a single object within a transaction, Caché will attempt to escalate the lock.
    

For both lock escalation strategies, you can determine the current system-wide lock threshold value using the [$SYSTEM.SQL.GetLockThreshold()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#GetLockThreshold) method. The default is 1000. This system-wide lock threshold value is configurable:

*   Using the [$SYSTEM.SQL.SetLockThreshold()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#SetLockThreshold) method.
    
*   Using the Management Portal. Go to \[Home\] > \[Configuration\] > \[General SQL Settings\]. View and edit the current setting of Lock Threshold. The default is 1000 locks. If you change this setting, any new process started after changing it will have the new setting.
    

You must have USE permission on the %Admin Manage Resource to change the lock threshold. Caché immediately applies any change made to the lock threshold value to all current processes.

On potential consequence of automatic lock escalation is a deadlock situation that might occur when an attempt to escalate to a table lock conflicts with another process holding a record lock in that table. There are several possible strategies to avoid this: (1) increase the lock escalation threshold so that lock escalation is unlikely to occur within a transaction. (2) substantially lower the lock escalation threshold so that lock escalation occurs almost immediately, thus decreasing the opportunity for other processes to lock a record in the same table. (3) apply a table lock for the duration of the transaction and do not perform record locks. This can be done at the start of the transaction by specifying LOCK TABLE, then UNLOCK TABLE (without the IMMEDIATE keyword, so that the table lock persists until the end of the transaction), then perform deletes with the %NOLOCK option.

Automatic lock escalation is intended to prevent overflow of the lock table. However, if you perform such a large number of deletes that a <LOCKTABLEFULL> error occurs, DELETE issues an SQLCODE -110 error.

For further details on transaction locking refer to [Transaction Processing](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_modify#GSQL_modify_transaction) in the “Modifying the Database” chapter of Using Caché SQL.

Examples

The following examples both delete all rows from the TempEmployees table. Note that the FROM keyword is optional:

DELETE FROM TempEmployees

DELETE TempEmployees

The following example deletes employee number 234 from the Employees table:

DELETE
     FROM Employees
     WHERE EmpId \= 234

The following example deletes all rows from the ActiveEmployees table in which the CurStatus column is set to "Retired":

DELETE FROM ActiveEmployees
     WHERE CurStatus \= 'Retired'

The following example deletes rows using a subquery:

DELETE FROM (SELECT Name,Age FROM Sample.Person WHERE Age \> 65)

Embedded SQL and Dynamic SQL Examples

In the following set of program examples, the first program creates a table named Sample.WordPairs with three columns. The next program inserts six records. Subsequent programs delete all English records using [cursor-based Embedded SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_cursor), and delete all French records using [Dynamic SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_dynsql). The final program displays the remaining records, then deletes the table.

CreateTable
   ZNSPACE "Samples"
   &sql(CREATE TABLE Sample.WordPairs (
        Lang        CHAR(2) NOT NULL,
        Firstword   CHAR(30),
        Lastword    CHAR(30) )
       )
  IF SQLCODE\=0 {
    WRITE !,"Table created" }
  ELSEIF SQLCODE\=\-201 {WRITE !,"Table already exists"  QUIT}
  ELSE {
    WRITE !,"CREATE TABLE failed. SQLCODE=",SQLCODE }

 

InsertSixRecords
  #SQLCompile Path\=Cinema,Sample
  ZNSPACE "Samples"
  &sql(INSERT INTO WordPairs (Lang,Firstword,Lastword) VALUES 
   ('En','hello','goodbye'))
  IF SQLCODE \= 0 { WRITE !,"1st record inserted" }
  ELSE { WRITE !,"Insert failed, SQLCODE=",SQLCODE
         QUIT}
  &sql(INSERT INTO WordPairs (Lang,Firstword,Lastword) VALUES 
   ('Fr','bonjour','au revoir'))
  IF SQLCODE \= 0 { WRITE !,"2nd record inserted" }
  ELSE { WRITE !,"Insert failed, SQLCODE=",SQLCODE  QUIT}
  &sql(INSERT INTO WordPairs (Lang,Firstword,Lastword) VALUES 
   ('It','pronto','ciao'))
  IF SQLCODE \= 0 { WRITE !,"3rd record inserted" }
  ELSE { WRITE !,"Insert failed, SQLCODE=",SQLCODE  QUIT}
  &sql(INSERT INTO WordPairs (Lang,Firstword,Lastword) VALUES 
   ('Fr','oui','non'))
  IF SQLCODE \= 0 { WRITE !,"4th record inserted" }
  ELSE { WRITE !,"Insert failed, SQLCODE=",SQLCODE  QUIT}
  &sql(INSERT INTO WordPairs (Lang,Firstword,Lastword) VALUES 
   ('En','howdy','see ya'))
  IF SQLCODE \= 0 { WRITE !,"5th record inserted" }
  ELSE { WRITE !,"Insert failed, SQLCODE=",SQLCODE  QUIT}
  &sql(INSERT INTO WordPairs (Lang,Firstword,Lastword) VALUES 
   ('Es','hola','adios'))
  IF SQLCODE \= 0 { WRITE !,"6th record inserted",!!
     SET myquery \= "SELECT %ID,\* FROM Sample.WordPairs"
     SET tStatement \= ##class(%SQL.Statement).%New()
     SET tStatus \= tStatement.%Prepare(myquery)
       IF tStatus '=1 {WRITE !,"%Prepare failed"  QUIT }
     SET rset \= tStatement.%Execute()
     DO rset.%Display()
     WRITE !,"End of data" }
  ELSE { WRITE !,"Insert failed, SQLCODE=",SQLCODE }

 

EmbeddedSQLDeleteEnglish
  #SQLCompile Path\=Sample
  NEW SQLCODE,%ROWCOUNT,%ROWID
  &sql(DECLARE WPCursor CURSOR FOR 
        SELECT Lang FROM WordPairs
        WHERE Lang\='En')
   &sql(OPEN WPCursor)
   FOR { &sql(FETCH WPCursor)
        QUIT:SQLCODE 
        &sql(DELETE FROM WordPairs
       WHERE CURRENT OF WPCursor)
    IF SQLCODE\=0 {
    WRITE !,"Delete succeeded"
    WRITE !,"Row count=",%ROWCOUNT," RowID=",%ROWID }
    ELSE {
    WRITE !,"Delete failed, SQLCODE=",SQLCODE }
    }
    &sql(CLOSE WPCursor)

 

DynamicSQLDeleteFrench
  SET sqltext \= "DELETE FROM WordPairs WHERE Lang=?"
  SET tStatement \= ##class(%SQL.Statement).%New(0,"Sample")
  SET tStatus \= tStatement.%Prepare(sqltext)
    IF tStatus '=1 {WRITE !,"%Prepare failed"  QUIT }
  SET rtn \= tStatement.%Execute("Fr")
  IF rtn.%SQLCODE\=0 {
    WRITE !,"Delete succeeded"
    WRITE !,"Row count=",rtn.%ROWCOUNT," RowID of last record=",rtn.%ROWID }
  ELSE {
    WRITE !,"Delete failed, SQLCODE=",rtn.%SQLCODE }

 

DisplayAndDeleteTable
  ZNSPACE "Samples"
  SET myquery \= "SELECT %ID,\* FROM Sample.WordPairs"
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET tStatus \= tStatement.%Prepare(myquery)
    IF tStatus '=1 {WRITE !,"%Prepare failed"  QUIT }
  SET rset \= tStatement.%Execute()
  DO rset.%Display()
  WRITE !,"End of data"
  &sql(DROP TABLE Sample.WordPairs)
   IF SQLCODE\=0 {
    WRITE !!,"Table deleted"
    QUIT }
  ELSE {
    WRITE !,"Table delete failed, SQLCODE=",SQLCODE }

 

See Also

*   [FROM](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_from)
    
*   [TRUNCATE TABLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_truncatetable)
    
*   [INSERT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_insert) [UPDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_update)
    
*   [CREATE VIEW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createview)
    
*   [WHERE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_where)
    
*   [WHERE CURRENT OF](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_wherecurrentof)
    
*   “[Modifying the Database](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_modify)” chapter in Using Caché SQL
    
*   “[Defining Tables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_tables)” chapter in Using Caché SQL
    
*   “[Defining Views](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_views)” chapter of Using Caché SQL
    
*   [Transaction Processing](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_modify#GSQL_modify_transaction) in the “Modifying the Database” chapter of Using Caché SQL
    
*   [SQL configuration settings](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RACS_Settings#RACS_Category_SQL) described in Caché Advanced Configuration Settings Reference.
    
*   [SQLCODE error messages](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RERR_sql) listed in the Caché Error Reference
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_declare "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_distinct "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_delete.xml**