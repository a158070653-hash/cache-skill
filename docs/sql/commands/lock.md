# LOCK

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_lock

---

 

Caché SQL Reference

LOCK

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_join "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_open "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_COMMANDS "SQL Commands") \]  >  \[ [LOCK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_lock "LOCK") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Locks a table.

Synopsis

LOCK \[TABLE\] tablename IN EXCLUSIVE MODE \[WAIT seconds\]
LOCK \[TABLE\] tablename IN SHARE MODE \[WAIT seconds\]

Arguments

tablename

The name of the table to be locked. tablename must be an existing table.

IN EXCLUSIVE MODE / IN SHARE MODE

The IN EXCLUSIVE MODE keyword phrase creates a regular Caché lock. The IN SHARE MODE keyword phrase creates a shared Caché lock.

seconds

Optional — An integer specifying the number of seconds to attempt to acquire the lock before timing out. If omitted, the system default timeout is applied.

Description

LOCK and LOCK TABLE are synonymous.

The LOCK command explicitly locks an SQL table. This table must be an existing table for which you have the necessary privileges. If tablename is a nonexistent table, LOCK fails with a compile error. If tablename is a temporary table, the command completes successfully, but performs no operation. If tablename is a view, the command fails with an SQLCODE -400 error.

The UNLOCK command reverses the LOCK operation. An explicit LOCK remains in effect until you issue an explicit UNLOCK for the same mode, or until the process terminates.

You can use LOCK to lock a table multiple times; you must explicitly UNLOCK the table as many times as it was explicitly locked. Each UNLOCK must specify the same mode as the corresponding LOCK.

Privileges

The LOCK command is a privileged operation. Prior to using LOCK IN SHARE MODE it is necessary for your process to have SELECT privilege for the specified table. Prior to using LOCK IN EXCLUSIVE MODE it is necessary for your process to have INSERT, UPDATE, or DELETE privilege for the specified table. For IN EXCLUSIVE MODE, the INSERT or UPDATE privilege must be on at least one field of the table. Failing to hold sufficient privileges results in an SQLCODE -99 error (Privilege Violation). You can determine if the current user has the necessary privileges by invoking the [%CHECKPRIV](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_checkpriv) command. You can determine if a specified user has the necessary privileges by invoking the [$SYSTEM.SQL.CheckPriv()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#CheckPriv) method. For privilege assignment, refer to the [GRANT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_grant) command.

These privileges are required to acquire the lock; they do not define the nature of the lock. An IN EXCLUSIVE MODE lock prevents other processes from performing INSERT, UPDATE, or DELETE operations, regardless of whether the lock holder has the corresponding privilege.

LOCK Modes

LOCK supports two modes: SHARE and EXCLUSIVE. These lock modes are independent of each other. You can apply both a SHARE lock and an EXCLUSIVE lock to the same table. A lock in EXCLUSIVE mode can only be unlocked by an UNLOCK in EXCLUSIVE mode. A lock in SHARE mode can only be unlocked by an UNLOCK in SHARE mode.

*   LOCK mytable IN SHARE MODE prevents other processes from issuing an EXCLUSIVE lock on mytable, or invoking a DDL operation, such as DROP TABLE.
    
*   LOCK mytable IN EXCLUSIVE MODE prevents other processes from issuing an EXCLUSIVE lock or a SHARE lock on mytable, performing an insert, update, or delete operation, or invoking a DDL operation, such as DROP TABLE.
    

LOCK permits read access to the table. Neither LOCK mode prevents other processes from performing a SELECT on the table in READ UNCOMMITTED mode (the default SELECT mode).

Locking Conflicts

*   If a table is already locked by another user IN EXCLUSIVE MODE, you cannot lock it in any mode.
    
*   If a table is already locked by another user IN SHARE MODE, you can also lock the table IN SHARE MODE, but you cannot lock it IN EXCLUSIVE MODE.
    

These LOCK conflicts generate an SQLCODE -110 error and generates a %msg such as the following: Unable to acquire shared table-level lock for table 'Sample.Person'.

Lock Timeout

LOCK attempts to acquire the specified SQL table lock until timeout occurs. When timeout occurs, LOCK generates an SQLCODE -110 error.

*   If you have specified WAIT seconds, SQL table lock timeout occurs when that number of seconds elapses.
    
*   Otherwise, SQL table lock timeout occurs when the current process SQL timeout elapses. You can set the lock timeout for the current process using the ProcessLockTimeout methods SetProcessLockTimeout() and GetProcessLockTimeout(). You can also set the lock timeout for the current process using the SQL command [SET OPTION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_setoption) with the LOCK\_TIMEOUT option. (SET OPTION cannot be used from the SQL Shell.) The current process SQL lock timeout defaults to the system-wide SQL lock timeout.
    
*   Otherwise, SQL table lock timeout occurs when the system-wide SQL timeout elapses. The system-wide default is 10 seconds. You can set the system-wide lock timeout in two ways:
    
    *   Using the SetLockTimeout() method. This immediately changes the system-wide lock timeout default for new processes, and also resets the ProcessLockTimeout for the current process to this new system-wide value. Setting the system-wide lock timeout has no effect on the ProcessLockTimeout setting for other currently running processes.
        
    *   Using the Management Portal, select \[Home\] > \[Configuration\] > \[General SQL Settings\]. View and edit the current setting of Lock Timeout (in seconds). This changes the system-wide lock timeout default for new processes that start after you save the configuration change. It has no effect on currently running processes.
        

The [SetLockTimeout()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#SetLockTimeout) method sets a value and returns the previous value. You can use GetLockTimeout() to return the current system-wide lock timeout value:

GetSysTimeout
   DO $SYSTEM.SQL.SetLockTimeout()
   SET oldval\=$SYSTEM.SQL.GetLockTimeout()
   WRITE oldval," seconds initial system-wide lock setting",!
SetSysTimeout
   DO $SYSTEM.SQL.SetLockTimeout(30,.oldval2)
   WRITE "system-wide lock timeout changed from ",oldval2," to "
   WRITE $SYSTEM.SQL.GetLockTimeout(),!
ResetSysTimeout
   DO $SYSTEM.SQL.SetLockTimeout(oldval,.oldval3)
   WRITE "system-wide lock timeout reset from ",oldval3," to "
   WRITE $SYSTEM.SQL.GetLockTimeout()

 

The [SetProcessLockTimeout()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#SetProcessLockTimeout) method sets a value and returns a status code. You can use GetProcessLockTimeout() to return the lock timeout value for the current process:

GetTimeoutDefaults
   SET sysinit\=$SYSTEM.SQL.GetLockTimeout()
   WRITE sysinit," initial system-wide lock seconds",!
   SET procinit\=$SYSTEM.SQL.GetProcessLockTimeout()
   WRITE procinit," initial process lock seconds",!
SetProcessTimeout
  DO $SYSTEM.SQL.SetProcessLockTimeout(50,.stat)
    IF stat {WRITE $SYSTEM.SQL.GetProcessLockTimeout()," set process lock seconds",! }
SetProcessTimeoutAgain
  DO $SYSTEM.SQL.SetProcessLockTimeout(60,.stat2)
    IF stat2 {WRITE $SYSTEM.SQL.GetProcessLockTimeout()," set process lock seconds",! }
SetProcessTimeoutMinimal
  DO $SYSTEM.SQL.SetProcessLockTimeout()
  WRITE $SYSTEM.SQL.GetProcessLockTimeout()," minimal process lock seconds",!
ResetToDefault
  DO $SYSTEM.SQL.SetProcessLockTimeout(procinit)
  WRITE $SYSTEM.SQL.GetProcessLockTimeout()," reset process lock seconds",!

 

Transaction Processing

A LOCK operation is not part of a transaction. Rolling back a transaction in which a LOCK is issued does not release the lock. An UNLOCK can be defined as occurring at the conclusion of the current transaction, or occurring immediately.

Other Locking Operations

Many DDL operations, including ALTER TABLE and DELETE TABLE acquire an exclusive table lock.

The INSERT, UPDATE, and DELETE commands also perform locking. By default they lock at the record level for the duration of the current transaction; if one of these commands locks a sufficiently large number of records (1000 is the default setting), the lock is automatically elevated to a table lock. The LOCK command allows you to explicitly set a table level lock, giving you greater control over the locking of data resources. An INSERT, UPDATE, or DELETE can override a LOCK by specifying the %NOLOCK keyword.

The Caché SQL [SET OPTION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_setoption) with the LOCK\_TIMEOUT option sets the timeout for the current process for an INSERT, UPDATE, DELETE, or SELECT operation.

Caché SQL supports the [SetCachedQueryLockTimeout()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#SetCachedQueryLockTimeout) method. See “[Cached Queries](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_cachedqueries)” in the Caché SQL Optimization Guide.

Examples

The following embedded SQL examples create a table and then lock it:

  ZNSPACE "Samples"
  NEW SQLCODE,%msg
  &sql(CREATE TABLE mytest (
      ID NUMBER(12,0) NOT NULL,
      CREATE\_DATE DATE DEFAULT CURRENT\_TIMESTAMP(2),
      WORK\_START DATE DEFAULT SYSDATE) )
  IF SQLCODE\=0 { WRITE "Table created",! }
  ELSEIF SQLCODE\=\-201 { WRITE "Table already exists",! }
  ELSE { WRITE "CREATE TABLE error: ",SQLCODE
         QUIT }

 

  ZNSPACE "Samples"
  NEW SQLCODE,%msg
  SET x\=$ZHOROLOG
  &sql(LOCK mytest IN EXCLUSIVE MODE WAIT 4) 
  IF SQLCODE\=0 { WRITE !,"Table locked" }
  ELSEIF SQLCODE\=\-110 { WRITE "waited ",$ZHOROLOG\-x," seconds"
         WRITE !,"Table is locked by another process",!,%msg }
  ELSE { WRITE !,"Unexpected LOCK error: ",SQLCODE,!,%msg }

 

SQL programs run from documentation, or from the Management Portal, spawn a process that terminates as soon as the program executes. Thus a lock is almost immediately released. Therefore, to observe a lock conflict, first issue a LOCK mytest IN EXCLUSIVE MODE command from a Caché Terminal running the SQL Shell in the Samples namespace. Then run the above embedded SQL locking program. Issue an UNLOCK mytest IN EXCLUSIVE MODE from the Caché Terminal SQL Shell. Then rerun the above embedded SQL locking program.

See Also

*   [UNLOCK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_unlock)
    
*   [INSERT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_insert) [UPDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_update) [DELETE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_delete)
    
*   [SQL configuration settings](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RACS_Settings#RACS_Category_SQL) described in Caché Advanced Configuration Settings Reference.
    
*   [SQLCODE error messages](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RERR_sql) listed in the Caché Error Reference
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_join "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_open "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_lock.xml**