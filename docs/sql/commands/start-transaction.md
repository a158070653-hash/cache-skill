# START TRANSACTION

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_starttransaction

---

 

Caché SQL Reference

START TRANSACTION

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_settransaction "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_top "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_COMMANDS "SQL Commands") \]  >  \[ [START TRANSACTION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_starttransaction "START TRANSACTION") \]

Go to: Description Caché ObjectScript and SQL Transactions Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Begins a transaction.

Synopsis

START TRANSACTION \[%COMMITMODE commitmode\]

START TRANSACTION \[transactionmodes\]

Arguments

commitmode

Optional — Specifies how future transactions will be committed to the database during the current process. Valid values are EXPLICIT, IMPLICIT, and NONE. The default is to maintain the existing commit mode; the initial commit mode default for a process is IMPLICIT.

transactionmodes

Optional — Specifies the isolation mode and access mode for the transaction. You can specify a value for either an isolation mode, an access mode, or for both modes as a comma-separated list.

Valid values for isolation mode are ISOLATION LEVEL READ COMMITTED and ISOLATION LEVEL READ UNCOMMITTED. The default is ISOLATION LEVEL READ UNCOMMITTED.

Valid values for access mode are READ ONLY and READ WRITE. Note that ISOLATION LEVEL READ UNCOMMITTED is not compatible with access mode READ WRITE and results in an SQLCODE -92 error when both are specified in the same statement.

Description

A START TRANSACTION statement initiates a [transaction](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_modify#GSQL_modify_transaction). START TRANSACTION immediately initiates a transaction, regardless of the current commit mode setting. A transaction begun with START TRANSACTION must be concluded by issuing an explicit COMMIT or ROLLBACK, regardless of the current commit mode setting.

START TRANSACTION is optional.

*   If your process is only querying the data (SELECT statements), you can use [SET TRANSACTION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_settransaction) to establish the ISOLATION LEVEL. A START TRANSACTION is not needed.
    
*   If your process is modifying the data, whether you need to explicitly begin an SQL transaction by issuing a START TRANSACTION depends on the current commit mode setting for the process (also referred to as the AutoCommit setting). If the commit mode for the current process is IMPLICIT or EXPLICIT, issuing a START TRANSACTION is optional. If you omit START TRANSACTION, Caché automatically initiates a transaction when you invoke a modify data operation (DELETE, UPDATE, INSERT, or TRUNCATE TABLE). If you specify START TRANSACTION a transaction is immediately initiated, and must be concluded by an explicit COMMIT or ROLLBACK.
    

When START TRANSACTION initiates a transaction it increments the [$TLEVEL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_svars) transaction level counter from 0 to 1, indicating a transaction is in progress. You can also determine if a transaction is in progress by checking the SQLCODE set by the [%INTRANSACTION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_intransaction) statement. Issuing a START TRANSACTION when a transaction is in progress has no effect on $TLEVEL or %INTRANSACTION.

Caché SQL does not support nested transactions. Issuing a START TRANSACTION when a transaction is already in progress does not initiate another transaction and does not return an error code. Caché SQL does support savepoints, allowing a partial rollback of a transaction.

If a transaction is not in progress when you issue a SAVEPOINT statement, SAVEPOINT initiates a transaction. However, this means of initiating a transaction is not recommended.

An SQLCODE -400 is issued if a transaction operation fails to complete successfully.

%BEGTRANS (deprecated)

The %BEGTRANS statement is functionally identical to START TRANSACTION with no arguments. %BEGTRANS cannot take arguments, and is considered deprecated. Use START TRANSACTION for all new SQL program code.

Setting Parameters

Optionally, START TRANSACTION can be used to set parameters. The parameter settings you specify take effect immediately. However, any transaction initiated with a START TRANSACTION must be concluded with an explicit COMMIT or ROLLBACK, regardless of how you set the commitmode parameter. Parameter settings continue in effect for the duration of the current process or until explicitly reset. They do not automatically reset to defaults at the end of a transaction.

A single START TRANSACTION statement can be used to set either the commitmode parameter or the transactionmodes parameters, but not both. To set both, you may issue a SET TRANSACTION and a START TRANSACTION, or two START TRANSACTION statements. Only the first START TRANSACTION initiates a transaction.

After issuing a START TRANSACTION, you can change these parameter settings during the transaction by issuing another START TRANSACTION, a SET TRANSACTION, or a method call. Changing the commitmode parameter does not remove the requirement to conclude the current transaction with an explicit COMMIT or ROLLBACK.

You can use the SET TRANSACTION statement to set the commitmode or transactionmodes parameters without starting a transaction. These parameters can also be set using method calls, either outside of a transaction or within a transaction.

%COMMITMODE

The %COMMITMODE keyword allows you to specify automatic transaction initiation and commitment behavior for the current process. A START TRANSACTION %COMMITMODE changes the commit mode setting for all future transactions on the current process. It does not affect the transaction initiated by the START TRANSACTION statement. Regardless of the current or set commit mode, a START TRANSACTION immediately initiates a transaction, and this transaction must be concluded by issuing an explicit COMMIT or ROLLBACK.

The available %COMMITMODE options are:

*   IMPLICIT: automatic transaction commitment is on (the initial process default). SQL automatically initiates a transaction when a program issues a database modification operation (INSERT, UPDATE, DELETE, or TRUNCATE TABLE). The transaction continues until either the operation completes successfully and SQL automatically commits the changes, or the operation is unable to complete successfully on all rows and SQL automatically rolls back the entire operation. Each database operation (INSERT, UPDATE, DELETE, or TRUNCATE TABLE) constitutes a separate transaction. Successful completion of the database operation automatically clears the rollback journal, releases locks, and decrements $TLEVEL. No COMMIT statement is needed.
    
*   EXPLICIT: automatic transaction commitment is off. SQL automatically initiates a transaction when a program issues the first database modification operation (INSERT, UPDATE, DELETE, or TRUNCATE TABLE). This transaction continues until it is explicitly concluded. Upon successful completion you issue a COMMIT statement. If a database modification operation fails you issue a ROLLBACK statement to revert the database to the point prior to the beginning of the transaction. In EXPLICIT mode multiple database modification operations can constitute a single transaction.
    
*   NONE: no automatic transaction processing. Transactions are not initiated unless explicitly invoked by a START TRANSACTION. All transactions must be explicitly concluded by issuing either a COMMIT or ROLLBACK statement. Thus whether a database operation is included in a transaction, and the number of database operations in a transaction are both user-defined.
    

You can set the %COMMITMODE in Caché ObjectScript using the [SetAutoCommit()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#SetAutoCommit) method call. The available method values are 0 (NONE), 1 (IMPLICIT), and 2 (EXPLICIT).

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

 

Caché ObjectScript and SQL Transactions

Caché ObjectScript and SQL transaction commands are fully compatible and interchangeable, with the following exception:

ObjectScript TSTART and SQL START TRANSACTION both start a transaction if no transaction is current. However, START TRANSACTION does not support nested transactions. Therefore, if you need (or may need) nested transactions, it is preferable to start the transaction with TSTART. If you need compatibility with the SQL standard, use START TRANSACTION.

Caché ObjectScript transaction processing provides limited support for nested transactions. SQL transaction processing supplies support for savepoints within transactions.

If a transaction involves SQL data modification statements, the transaction should be started with the SQL START TRANSACTION statement and committed with the SQL COMMIT statement. (These statements may be explicit or implicit, depending on the %COMMITMODE setting.) Methods that use TSTART/TCOMMIT nesting can be included in the transaction, as long as they don't initiate the transaction. Methods and stored procedures should not normally use SQL transaction control statements, unless, by design, they are the main controller of the transaction. Stored procedures should not normally use SQL transaction control statements, because these stored procedures are normally called from ODBC/JDBC, which has its own model of transaction control.

Examples

The following Embedded SQL example uses two START TRANSACTION statements to start a transaction and set its parameters. Note that the first START TRANSACTION initiates a transaction, setting the commit mode and incrementing the $TLEVEL transaction level counter. The second START TRANSACTION sets the isolation mode for query read operations in the current transaction, but does not increment $TLEVEL, because the transaction has already been started. The SAVEPOINT statement increments$TLEVEL:

    WRITE !,"Transaction level=",$TLEVEL
  &sql(START TRANSACTION %COMMITMODE EXPLICIT)
    WRITE !,"Start transaction commit mode, SQLCODE=",SQLCODE
    WRITE !,"Transaction level=",$TLEVEL
  &sql(START TRANSACTION ISOLATION LEVEL READ COMMITTED)
    WRITE !,"Start transaction isolation mode, SQLCODE=",SQLCODE
    WRITE !,"Transaction level=",$TLEVEL
  &sql(SAVEPOINT a)
    WRITE !,"Set Savepoint a, SQLCODE=",SQLCODE
    WRITE !,"Transaction level=",$TLEVEL
  &sql(COMMIT)
    WRITE !,"Commit transaction, SQLCODE=",SQLCODE
    WRITE !,"Transaction level=",$TLEVEL

 

See Also

*   [COMMIT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_commit) [%INTRANSACTION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_intransaction) [ROLLBACK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_rollback) [SAVEPOINT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_savepoint) [SET TRANSACTION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_settransaction) [$TLEVEL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_svars)
    
*   [Transaction Processing](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_modify#GSQL_modify_transaction) in the “Modifying the Database” chapter of Using Caché SQL.
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_settransaction "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_top "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_starttransaction.xml**