# SET TRANSACTION

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_settransaction

---

 

Caché SQL Reference

SET TRANSACTION

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_setoption "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_starttransaction "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_COMMANDS "SQL Commands") \]  >  \[ [SET TRANSACTION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_settransaction "SET TRANSACTION") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Sets parameters for transactions.

Synopsis

SET TRANSACTION %COMMITMODE commitmode

SET TRANSACTION transactionmodes

Arguments

%COMMITMODE commitmode

Optional — Specifies the manner in which transactions are committed to the database. Available values are EXPLICIT, IMPLICIT, and NONE. The default is IMPLICIT.

transactionmodes

Optional — Specifies the isolation mode and access mode for the transaction. You can specify a value for either an isolation mode, an access mode, or for both modes as a comma-separated list.

Valid values for isolation mode are ISOLATION LEVEL READ COMMITTED and ISOLATION LEVEL READ UNCOMMITTED. The default is ISOLATION LEVEL READ UNCOMMITTED.

Valid values for access mode are READ ONLY and READ WRITE. Note that ISOLATION LEVEL READ UNCOMMITTED is not compatible with access mode READ WRITE and results in an SQLCODE -92 error when both are specified in the same statement.

Description

A SET TRANSACTION statement sets parameters that govern SQL transactions for the current process. These parameters take effect at the beginning of the next transaction and continue in effect for the duration of the current process or until explicitly reset. They do not automatically reset to defaults at the end of a transaction.

A single SET TRANSACTION statement can be used to set either the commitmode parameter or the transactionmodes parameters, but not both.

The same parameters can be set using the START TRANSACTION command, which can both set parameters and begin a new transaction. The parameters can also be set using method calls.

SET TRANSACTION does not begin a transaction, and therefore does not increment the [$TLEVEL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_svars) transaction level counter.

%COMMITMODE

The %COMMITMODE keyword allows you to specify whether or not automatic transaction commitment is performed. The available options are:

*   IMPLICIT: automatic transaction commitment is on (the default). SQL automatically initiates a transaction when a program issues a database modification operation (INSERT, UPDATE, DELETE, or TRUNCATE TABLE). The transaction continues until either the operation completes successfully and SQL automatically commits the changes, or the operation is unable to complete successfully on all rows and SQL automatically rolls back the entire operation. Each database operation (INSERT, UPDATE, DELETE, or TRUNCATE TABLE) constitutes a separate transaction. Successful completion of the database operation automatically clears the rollback journal, releases locks, and decrements $TLEVEL. No COMMIT statement is needed. This is the default setting.
    
*   EXPLICIT: automatic transaction commitment is off. SQL automatically initiates a transaction when a program issues the first database modification operation (INSERT, UPDATE, DELETE, or TRUNCATE TABLE). This transaction continues until it is explicitly concluded. Upon successful completion you issue a COMMIT statement. If a database modification operation fails you issue a ROLLBACK statement to revert the database to the point prior to the beginning of the transaction. In EXPLICIT mode the number of database operations per transaction is user-defined.
    
*   NONE: no automatic transaction processing. A transaction is not initiated unless explicitly invoked by a START TRANSACTION statement. The transaction must be explicitly concluded by issuing either a COMMIT or ROLLBACK statement. Thus whether a database operation is included in a transaction, and the number of database operations in a transaction are both user-defined.
    

You can determine the %COMMITMODE setting for the current process using the [GetAutoCommit()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#GetAutoCommit) method, as shown in the following Caché ObjectScript example:

  DO $SYSTEM.SQL.SetAutoCommit($RANDOM(3))
  SET x\=$SYSTEM.SQL.GetAutoCommit()
  IF x\=1 {
    WRITE "%COMMITMODE IMPLICIT (default behavior):",!,
          "each database operation is a separate transaction",!,
          "with automatic commit or rollback" }
  ELSEIF x\=0 {
    WRITE "%COMMITMODE NONE:",!,
          "No automatic transaction support",!,
          "You must use START TRANSACTION to start a transaction",!,
          "and COMMIT or ROLLBACK to conclude one" }
  ELSE { 
    WRITE "%COMMITMODE EXPLICIT:",!,
          "the first database operation automatically",!,
          "starts a transaction; to end the transaction",!,
          "explicit COMMIT or ROLLBACK required" }

 

The %COMMITMODE can be set in Caché ObjectScript using the [SetAutoCommit()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#SetAutoCommit) method call. The available method values are 0 (NONE), 1 (IMPLICIT), and 2 (EXPLICIT).

ISOLATION LEVEL

You specify an ISOLATION LEVEL for a process that is issuing a query. The ISOLATION LEVEL options permit you to specify whether or not changes that are in progress should be available for read access by the query. If another concurrent process is performing inserts or updates to a table and those changes to the table are in a transaction, those changes are in progress, and could, potentially, be rolled back. By setting the ISOLATION LEVEL for your process that is querying that table, you can specify whether you wish to include or exclude these changes in progress should be included in the query results.

*   READ UNCOMMITTED states that all changes are immediately available for query access. This includes changes that may subsequently be rolled back. READ UNCOMMITTED insures that your query will return results without waiting for a concurrent insert or update process, and will not fail due to a lock timeout error. However, the results of a READ UNCOMMITTED may include values that are not committed; these values may be internally inconsistent because the insert or update operation has only partially completed, and these values may be subsequently rolled back. READ COMMITTED is the default if your query process is not in an explicit transaction, or if the transaction does not specify an ISOLATION LEVEL. READ UNCOMMITTED is incompatible with READ WRITE access; attempting to specify both in the same statement results in an SQLCODE -92 error.
    
*   READ COMMITTED states that only those changes that have been committed are available for query access. This ensures that a query is performed on the database in a consistent state, not while a group of changes are being made, a group of changes which may be subsequently rolled back. If requested data has been changed, but the changes have not been committed (or rolled back), the query waits for transaction completion. If a lock timeout occurs while waiting for this data to be available, an SQLCODE -114 error is issued.
    

Exceptions to READ COMMITTED

When ISOLATION LEVEL read committed is in effect, either through setting ISOLATION LEVEL READ COMMITTED or $SYSTEM.SQL.SetIsolationMode(1), SQL can retrieve only those changes to the data that have been committed. However, there are significant exceptions to this rule:

*   A deleted row is never returned by a query, even when the transaction that deleted the row is in progress and the delete may be subsequently rolled back. ISOLATION LEVEL READ COMMITTED ensures that inserts and updates are in a consistent state, but not deletes.
    
*   If you query contains an [aggregate function](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_aggregatefunctions), the aggregate result returns the current state of the data, regardless of the specified ISOLATION LEVEL. Therefore, inserts and updates are in progress (and may subsequently be rolled back) are included in aggregate results. Deletes that are in progress (and may subsequently be rolled back) are not included in aggregate results. This is because an aggregate operation requires access to data from many rows of a table.
    
*   A SELECT query that contains a [DISTINCT clause](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_distinct) or a [GROUP BY clause](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_groupby) is unaffected by the ISOLATION LEVEL setting. A query containing one of these clauses returns the current state of the data, including changes in progress that may be subsequently rolled back. This is because these query operations require access to data from many rows of a table.
    
*   A query with the [%NOLOCK keyword](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select).
    

Note:

On Caché implementations with ECP (Enterprise Cache Protocol) use of READ COMMITTED may result in significantly slower performance when compared to READ UNCOMMITTED. Developers should weigh the superior performance of READ UNCOMMITTED against the greater data accuracy of READ COMMITTED when defining transactions that involve ECP.

For further details, refer to [Transaction Processing](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_modify#GSQL_modify_transaction) in the “Modifying the Database” chapter of Using Caché SQL.

ISOLATION LEVEL in Effect

You can set the ISOLATION LEVEL for a process using SET TRANSACTION (without starting a transaction), START TRANSACTION (setting isolation mode and starting a transaction), or a SetIsolationMode() method call.

The specified ISOLATION LEVEL remains in effect until explicitly reset by a SET TRANSACTION, START TRANSACTION, or a SetIsolationMode() method call. Because COMMIT or ROLLBACK is only meaningful for changes to the data, not data queries, a COMMIT or ROLLBACK operation has no effect on the ISOLATION LEVEL setting.

you can determine the ISOLATION LEVEL for the current process using the [GetIsolationMode()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#GetIsolationMode) method call. You can also set the isolation mode for the current process using the [SetIsolationMode()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#SetIsolationMode) method call. These methods specify READ UNCOMMITTED (the default) as 0, and READ COMMITTED as 1. Specifying any other numeric value leaves the isolation mode unchanged. No error or change occurs if you set the isolation mode to the current isolation mode. Use of these methods is shown in the following example:

   WRITE $SYSTEM.SQL.GetIsolationMode()," default",!
   &sql(START TRANSACTION ISOLATION LEVEL READ COMMITTED,READ WRITE)
   WRITE $SYSTEM.SQL.GetIsolationMode()," after START TRANSACTION",!
   DO $SYSTEM.SQL.SetIsolationMode(0,.stat)
   IF stat\=1 {
     WRITE $SYSTEM.SQL.GetIsolationMode()," after SetIsolationMode(0) call",! }
   ELSE { WRITE "SetIsolationMode() error" }
   &sql(COMMIT)

 

The isolation mode and the access mode must always be compatible. Changing the access mode changes the isolation mode, as shown in the following example:

   WRITE $SYSTEM.SQL.GetIsolationMode()," default",!
   &sql(SET TRANSACTION ISOLATION LEVEL READ COMMITTED,READ WRITE)
   WRITE $SYSTEM.SQL.GetIsolationMode()," after SET TRANSACTION",!
   &sql(START TRANSACTION READ ONLY)
   WRITE $SYSTEM.SQL.GetIsolationMode()," after changing access mode",!
   &sql(COMMIT)

 

Examples

The following Embedded SQL example uses two SET TRANSACTION statements to set transaction parameters. Note that SET TRANSACTION does not increment the transaction level ($TLEVEL). The START TRANSACTION command initiates a transaction and increments $TLEVEL:

  &sql(SET TRANSACTION %COMMITMODE EXPLICIT)
    WRITE !,"Set transaction commit mode, SQLCODE=",SQLCODE
    WRITE !,"Transaction level=",$TLEVEL
  &sql(SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED)
    WRITE !,"Set transaction isolation mode, SQLCODE=",SQLCODE
    WRITE !,"Transaction level=",$TLEVEL
  &sql(START TRANSACTION)
    WRITE !,"Start transaction, SQLCODE=",SQLCODE
    WRITE !,"Transaction level=",$TLEVEL
  &sql(SAVEPOINT a)
    WRITE !,"Set Savepoint a, SQLCODE=",SQLCODE
    WRITE !,"Transaction level=",$TLEVEL
  &sql(COMMIT)
    WRITE !,"Commit transaction, SQLCODE=",SQLCODE
    WRITE !,"Transaction level=",$TLEVEL

 

See Also

*   [COMMIT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_commit) [ROLLBACK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_rollback) [SAVEPOINT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_savepoint) [START TRANSACTION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_starttransaction) [$TLEVEL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_svars)
    
*   [Transaction Processing](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_modify#GSQL_modify_transaction) in the “Modifying the Database” chapter of Using Caché SQL
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_setoption "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_starttransaction "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_settransaction.xml**