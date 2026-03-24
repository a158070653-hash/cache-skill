# COMMIT

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_commit

---

 

Caché SQL Reference

COMMIT

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_close "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createdatabase "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_COMMANDS "SQL Commands") \]  >  \[ [COMMIT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_commit "COMMIT") \]

Go to: Description Caché ObjectScript and SQL Transactions Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Commits work performed during a transaction.

Synopsis

COMMIT \[WORK\]

Description

A COMMIT statement commits all work completed during the current [transaction](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_modify#GSQL_modify_transaction), resets the transaction level counter, and releases all locks established. This completes the transaction. Work committed cannot be rolled back.

COMMIT and COMMIT WORK are equivalent statements; both versions are supported for compatibility.

A transaction is defined as the operations since and including the START TRANSACTION statement. A COMMIT restores the transaction level counter ($TLEVEL) to its state immediately prior to the START TRANSACTION statement that initialized the transaction. (Because Caché SQL does not support nested transactions, issuing additional START TRANSACTION statements within a transaction has no effect on the transaction initialization point.)

A single COMMIT causes all savepoints within the transaction to be committed.

A START TRANSACTION statement is used to explicitly begin a new transaction. However, use of START TRANSACTION is optional. If transaction processing is activated, the first database operation following a COMMIT implicitly begins a new transaction. A COMMIT statement is not meaningful if either transaction processing is not in effect, or transaction processing is in effect with automatic commits. If no transaction is in progress, a COMMIT completes successfully (SQLCODE 0), but performs no operation.

The effects of a COMMIT on queries are determined by the current isolation level. These transaction parameters can be set using either the SET TRANSACTION or START TRANSACTION command.

An SQLCODE -400 is issued if a transaction operation fails to complete successfully.

Caché ObjectScript and SQL Transactions

Caché ObjectScript and SQL transaction commands are fully compatible and interchangeable, with the following exception:

ObjectScript TSTART and SQL START TRANSACTION both start a transaction if no transaction is current. However, START TRANSACTION does not support nested transactions. Therefore, if you need (or may need) nested transactions, it is preferable to start the transaction with TSTART. If you need compatibility with the SQL standard, use START TRANSACTION.

Caché ObjectScript transaction processing provides limited support for nested transactions. SQL transaction processing supplies support for savepoints within transactions.

If a transaction involves SQL update statements, then the transaction should be started by the SQL START TRANSACTION statement and committed with the SQL COMMIT statement. Methods that use TSTART/TCOMMIT nesting can be included in the transaction, as long as they don't initiate the transaction. Methods and stored procedures should not normally use SQL transaction control statements, unless, by design, they are the main controller of the transaction. Stored procedures should not normally use SQL transaction control statements, because these stored procedures are normally called from ODBC/JDBC, which has its own model of transaction control.

Examples

The following Embedded SQL example demonstrates how a COMMIT restores the transaction level counter ($TLEVEL) to the level immediately prior to the START TRANSACTION, regardless of how many SAVEPOINTS have been established within the transaction. Note that the second START TRANSACTION in this program is a no-op which has no effect on $TLEVEL:

  &sql(SET TRANSACTION %COMMITMODE EXPLICIT)
    WRITE !,"Set transaction mode, SQLCODE=",SQLCODE
    WRITE !,"Transaction level=",$TLEVEL
  &sql(START TRANSACTION)
    WRITE !,"Start transaction, SQLCODE=",SQLCODE
    WRITE !,"Transaction level=",$TLEVEL
  &sql(SAVEPOINT a)
    WRITE !,"Set Savepoint a, SQLCODE=",SQLCODE
    WRITE !,"Transaction level=",$TLEVEL
  &sql(SAVEPOINT b)
    WRITE !,"Set Savepoint b, SQLCODE=",SQLCODE
    WRITE !,"Transaction level=",$TLEVEL
  &sql(START TRANSACTION) /\* Performs no operation \*/
    WRITE !,"Start transaction, SQLCODE=",SQLCODE
    WRITE !,"Transaction level=",$TLEVEL
  &sql(SAVEPOINT c)
    WRITE !,"Set Savepoint c, SQLCODE=",SQLCODE
    WRITE !,"Transaction level=",$TLEVEL
  &sql(COMMIT)
    WRITE !,"Commit transaction, SQLCODE=",SQLCODE
    WRITE !,"Transaction level=",$TLEVEL

 

The following Embedded SQL example demonstrates that the first COMMIT statement commits the entire transaction and that extra COMMIT statements have no effect and do not result in an error:

  &sql(SET TRANSACTION %COMMITMODE EXPLICIT)
    WRITE !,"Set transaction mode, SQLCODE=",SQLCODE
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
 &sql(COMMIT)   /\* Performs no operation \*/
    WRITE !,"Commit again, SQLCODE=",SQLCODE
    WRITE !,"Transaction level=",$TLEVEL
 &sql(COMMIT)   /\* Performs no operation \*/
    WRITE !,"Commit again, SQLCODE=",SQLCODE
    WRITE !,"Transaction level=",$TLEVEL

 

See Also

*   SQL commands: [ROLLBACK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_rollback) [SAVEPOINT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_savepoint) [SET TRANSACTION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_settransaction) [START TRANSACTION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_starttransaction) [$TLEVEL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_svars)
    
*   [Transaction Processing](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_modify#GSQL_modify_transaction) in the “Modifying the Database” chapter of Using Caché SQL.
    
*   Caché ObjectScript command: [TCOMMIT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ctcommit)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_close "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createdatabase "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_commit.xml**