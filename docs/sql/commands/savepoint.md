# SAVEPOINT

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_savepoint

---

 

Caché SQL Reference

SAVEPOINT

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_rollback "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_COMMANDS "SQL Commands") \]  >  \[ [SAVEPOINT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_savepoint "SAVEPOINT") \]

Go to: Description Examples Caché ObjectScript and SQL Transactions See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Marks a point within a transaction.

Synopsis

SAVEPOINT pointname

Arguments

pointname

The name of the savepoint, specified as an [identifier](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_identifiers). For further details see the “Identifiers” chapter of Using Caché SQL.

Description

A SAVEPOINT statement marks a point within a [transaction](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_modify#GSQL_modify_transaction). Establishing a savepoint enables you to perform transaction roll back to the savepoint, undoing all work done and releasing all locks acquired during that period. In a long-running transaction, or a transaction with internal control structure, it is often desirable to be able to roll back part of the transaction without undoing all work submitted during the transaction.

The establishment of a savepoint increments the [$TLEVEL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_svars) transaction level counter. Rolling back to a savepoint decrements the [$TLEVEL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_svars) transaction level counter to its value immediately prior to the savepoint. You can establish up to 255 savepoints within a transaction. Exceeding this number of savepoints results in an SQLCODE -400 fatal error, a <TRANSACTION LEVEL> exception caught during SQL execution. The Terminal prompt displays the current transaction level as a TLn: prefix to the prompt, where n is an integer between 1 and 255 representing the current $TLEVEL count.

Each savepoint is associated with an savepoint name, a unique [identifier](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_identifiers). Savepoint names are not case sensitive. A savepoint name can be a delimited identifier.

*   If you specify a SAVEPOINT with no pointname, or with a pointname that is not a valid identifier or is an [SQL Reserved Word](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_reservedwords), a runtime SQLCODE -301 error is issued.
    
*   If you specify a SAVEPOINT with a pointname that begins with “SYS”, a runtime SQLCODE -302 error is issued. These savepoint names are reserved.
    

Savepoint names are not case sensitive; therefore resetpt, ResetPt and "RESETPT" are the same pointname. This duplication is detected during ROLLBACK TO SAVEPOINT, not during SAVEPOINT. When you specify a SAVEPOINT statement with a duplicate pointname, Caché increments the transaction level counter, just as if the pointname was unique. However, the most recent pointname overwrites all prior duplicate values in the table of savepoint names. Therefore, when you specify a ROLLBACK TO SAVEPOINT pointname, Caché rolls back to the most recently established SAVEPOINT with that pointname, and decrements the transaction level counter appropriately. However, if you again specify a ROLLBACK TO SAVEPOINT pointname with the same name, an SQLCODE -375 error is generated, with the %msg: Cannot ROLLBACK to unestablished savepoint 'name', the full transaction is rolled back and the $TLEVEL count reverts to 0.

Using Savepoints

The SAVEPOINT statement is supported for Embedded SQL, Dynamic SQL, ODBC, and JDBC. In JDBC, connection.setSavepoint(pointname) sets a savepoint, and connection.rollback(pointname) rolls back to the named savepoint.

If savepoints have been established:

*   A ROLLBACK TO SAVEPOINT pointname rolls back work done since the specified savepoint, deletes that savepoint and all intermediate savepoints, and decrements the $TLEVEL transaction level counter by the number of savepoints deleted. If pointname does not exist, or has already been rolled back, this command rolls back the entire transaction, resets $TLEVEL to 0, and releases all locks.
    
*   A ROLLBACK rolls back all work done during the current transaction, rolling back the work done since START TRANSACTION. It resets the $TLEVEL transaction level counter to zero and releases all locks. Note that a generic ROLLBACK ignores savepoints.
    
*   A COMMIT commits all work done during the current transaction. It resets the $TLEVEL transaction level counter to zero and releases all locks. Note that a COMMIT ignores savepoints.
    

Issuing a second START TRANSACTION within a transaction has no effect on savepoints or the $TLEVEL transaction level counter.

An SQLCODE -400 error is issued if a transaction operation fails to complete successfully.

Examples

The following embedded SQL example creates a transaction with two savepoints:

  NEW SQLCODE,%ROWCOUNT,%ROWID
  &sql(START TRANSACTION)
  &sql(DELETE FROM Sample.Person WHERE Name\=NULL)
  IF SQLCODE\=100 { WRITE !,"No null name records to delete" }
  ELSEIF SQLCODE'=0 {&sql(ROLLBACK)}
  ELSE {WRITE !,%ROWCOUNT," null name records deleted"}
    &sql(SAVEPOINT svpt\_age1)
    &sql(DELETE FROM Sample.Person WHERE Age\=NULL)
    IF SQLCODE\=100 { WRITE !,"No null age records to delete" }
    ELSEIF SQLCODE'=0 {&sql(ROLLBACK TO SAVEPOINT svpt\_age1)}
    ELSE {WRITE !,%ROWCOUNT," null age records deleted"}
      &sql(SAVEPOINT svpt\_age2)
      &sql(DELETE FROM Sample.Person WHERE Age\>65)
      IF SQLCODE\=0 { &sql(COMMIT)}
      ELSEIF SQLCODE\=100 { &sql(COMMIT)}
      ELSE {
       &sql(ROLLBACK TO SAVEPOINT svpt\_age2)
       WRITE !,"retirement age deletes failed" 
      }
    &sql(COMMIT)
  &sql(COMMIT)

Caché ObjectScript and SQL Transactions

Caché ObjectScript transaction processing, using [TSTART](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ctstart) and [TCOMMIT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ctcommit), differs from, and is incompatible with, SQL transaction processing using the SQL statements START TRANSACTION, SAVEPOINT, and COMMIT. Both Caché ObjectScript and Caché SQL provides limited support for nested transactions. Caché ObjectScript transaction processing does not interact with SQL lock control variables; of particular concern is the SQL lock escalation variable. An application should not attempt to mix the two types of transaction processing.

If a transaction involves SQL update statements, then the transaction should be started by the SQL START TRANSACTION statement and committed with the SQL COMMIT statement. Methods that use TSTART/TCOMMIT nesting can be included in the transaction, as long as they don't initiate the transaction. Methods and stored procedures should not normally use SQL transaction control statements, unless, by design, they are the main controller of the transaction.

See Also

*   SQL commands: [COMMIT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_commit) [ROLLBACK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_rollback) [SET TRANSACTION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_settransaction) [START TRANSACTION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_starttransaction) [$TLEVEL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_svars)
    
*   [Transaction Processing](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_modify#GSQL_modify_transaction) in the “Modifying the Database” chapter of Using Caché SQL
    
*   [SQLCODE error messages](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RERR_sql) listed in the Caché Error Reference
    
*   Caché ObjectScript command: [TCOMMIT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ctcommit)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_rollback "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_savepoint.xml**