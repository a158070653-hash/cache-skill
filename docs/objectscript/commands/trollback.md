# TROLLBACK

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_ctrollback

---

 

Caché ObjectScript Reference

TROLLBACK

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cthrow "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ctry "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_COMMANDS "Caché ObjectScript Commands") \]  >  \[ [TROLLBACK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ctrollback "TROLLBACK") \]

Go to: Description Arguments Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Rolls back an unsuccessful transaction.

Synopsis

TROLLBACK:pc
TRO:pc

TROLLBACK:pc 1
TRO:pc 1

Arguments

pc

Optional — A postconditional expression.

1

Optional — The integer 1. Rolls back one level of nesting. Must be specified as a literal.

Description

TROLLBACK terminates the current transaction and restores all journaled database values to the values they held at the start of the transaction. TROLLBACK has two forms:

*   TROLLBACK rolls back all transactions in progress (no matter how many levels of TSTART were issued) and resets $TLEVEL to 0.
    
*   TROLLBACK 1 rolls back the current level of nested transactions (the one initiated by the most recent TSTART) and decrements $TLEVEL by 1. The 1 argument must be the literal number 1; it cannot be a variable or an expression that resolve to 1. Numbers other than 1 are not supported.
    

You can determine the level of nested transactions from the [$TLEVEL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vtlevel) special variable. Calling TROLLBACK when $TLEVEL is 0 has no effect.

You can use the [GetImageJournalInfo()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYS.Journal.System#GetImageJournalInfo) method of the [%SYS.Journal.System](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYS.Journal.System) class to search the journal file for TSTART commands, and thus identify open transactions. A TSTART increments $TLEVEL and writes a journal file record: either a “BT” (Begin Transaction) record if $TLEVEL was zero, or a “BTL” (Begin Transaction with Level) record if $TLEVEL was greater than 0. Use the [Sync()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYS.Journal.System#Sync) method of the [%SYS.Journal.System](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYS.Journal.System) class to flush the journal buffer following a successful rollback operation.

TROLLBACK disables Ctrl-C interrupts for the duration of the rollback operation.

What Is and Isn’t Rolled Back

TROLLBACK rolls back all journaled operations. These include the following:

*   TROLLBACK rolls back most changes to global variables, including SET and KILL operations. Global variables underlie many Caché operations. Local variables are not reverted by a rollback operation.
    
    TROLLBACK rolls back changes to [bit string](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_types#GCOS_types_strings_bit) values in global variables during a transaction. However, a rollback operation does not return the global variable bit string to its previous internal string representation.
    
*   TROLLBACK rolls back insert, update, and delete changes to SQL data.
    

However, not all changes made by an application are journaled:

*   TROLLBACK does not roll back changes to local variables or process-private globals.
    
*   TROLLBACK does not roll back changes to special variables, such as [$TEST](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vtest).
    
*   TROLLBACK does not roll back changes to the current namespace.
    
*   TROLLBACK does not roll back [LOCK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_clock) command lock or unlock operations.
    
*   TROLLBACK does not roll back [$INCREMENT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fincrement) (or $ZINCREMENT) changes to global variables.
    
*   TROLLBACK does not roll back [$SEQUENCE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_fsequence) changes to global variables.
    

Caché treats a SET or KILL of a global variable as a journaled transaction event; rolling back the transaction reverses these operations. Caché does not treat a SET or KILL of a local variable or a process-private global variable as a journaled transaction event; rolling back the transaction has no effect on these operations. By default, a SET or KILL of a global variable is immediately visible by other processes while the transaction is in progress. To prevent the SET or KILL of a global variable invoked within a transaction from being seen by other users before the transaction has been committed, you must coordinate access to the global variable via the LOCK command.

Transaction Rollback Logging

If an error occurs during a roll back operation, Caché issues a <ROLLFAIL> error message, and logs an error message in the cconsole.log operator console log file. You can use the Management Portal System Operation option to view cconsole.log: [\[Home\] > \[System Logs\] > \[View Console Log\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?/csp/sys/op/UtilSysConsoleLog.csp).

By default, the operator console log file is cconsole.log in the Caché system management directory (Mgr). This default is configurable. Go to the Management Portal System Administration option, select Configuration, then Additional Settings, then Advanced Memory (\[Home\] > \[Configuration\] > \[Advanced Memory Settings\]). View and edit the current setting of ConsoleFile. By default this setting is blank, routing console messages to cconsole.log in the MGR directory. If you change this setting, you must restart Caché for this change to take effect.

<ROLLFAIL> Errors

If TROLLBACK cannot successfully roll back the transaction, a <ROLLFAIL> error occurs. The process behavior depends on the setting of the system-wide journal configuration setting flag Freeze on error (\[Home\] > \[Configuration\] > \[Jounal Settings\]):

*   If Freeze on error is not set (the default), the process gets a <ROLLFAIL> error. The transaction is closed and any locks retained for the transaction are released. This option trades data integrity for system availability.
    
*   If Freeze on error is set, the process halts and the clean job daemon (CLNDMN) retries rolling back the open transaction. During the CLNDMN retry period, locks retained for the transaction are intact and, as a result, the system might hang. This option trades system availability for data integrity.
    

For further details, refer to [Journal IO Errors](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCDI_journal#GCDI_journal_iofreeze_trollback) in the Caché Data Integrity Guide.

When a <ROLLFAIL> occurs, the Caché %msg records both the <ROLLFAIL> error itself, and the previous error that caused the roll back. For example, attempting to update a date with an out-of-range value and then failing roll back might return the following %msg: SQLCODE = -105 %msg = Unexpected error occurred: <ROLLFAIL>%0Ac+1^dpv during TROLLBACK. Previous error: SQLCODE=-105, %msg='Field 'Sample.Person.DOB' (value '5888326') failed validation'.

A <ROLLFAIL> occurs upon transaction rollback if within the transaction a global accessed a remote database, and then the program explicitly dismounted that remote database.

A <ROLLFAIL> occurs upon transaction rollback if the process disabled journaling before making database changes and an error occurred that invoked transaction rollback. A <ROLLFAIL> does not occur upon transaction rollback if the process disabled journaling after all database changes had been made but before issuing the TROLLBACK command. Instead, Caché temporarily enables journaling for the duration of the rollback operation. Upon completion of the rollback operation Caché again disables journaling.

SQL and Transactions

Caché ObjectScript and SQL transaction commands are fully compatible and interchangeable, with the following exception:

ObjectScript TSTART and SQL START TRANSACTION both start a transaction if no transaction is current. However, START TRANSACTION does not support nested transactions. Therefore, if you need (or may need) nested transactions, it is preferable to start the transaction with TSTART. If you need compatibility with the SQL standard, use START TRANSACTION.

Caché ObjectScript transaction processing provides limited support for nested transactions. SQL transaction processing supplies support for savepoints within transactions.

Purging Cached Queries

If during a transaction you call the [Purge()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#Purge) method of [%SYSTEM.SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL) class to purge cached queries, the cached queries are permanently deleted. A subsequent TROLLBACK will not restore purged cached queries.

Globals and TROLLBACK 1

TROLLBACK 1 rolls back and restores all globals changed within its nested transaction. However, if globals are changed that are mapped to a remote system that does not support nested transactions, these changes are treated as occurring at the outermost nested level. Such globals are only rolled back when a rollback resets $TLEVEL to 0, either by calling TROLLBACK or by calling TROLLBACK 1 when $TLEVEL\=1.

Locks and TROLLBACK 1

TROLLBACK 1 does not restore locks established during its nested transaction to their prior state. All locks established during a transaction remain in the lock table until the transaction is concluded by a TROLLBACK to level 0 or a TCOMMIT. At that point Caché releases all locks created during the nested transaction, and restores all preexisting locks to their state before TSTART.

A TCOMMIT of a nested transaction does not release the corresponding locks, so a subsequent TROLLBACK can effect locks in a committed sub-transaction.

Arguments

pc

An optional postconditional expression. Caché executes the command if the postconditional expression is true (evaluates to a nonzero numeric value). Caché does not execute the command if the postconditional expression is false (evaluates to zero). For further details, refer to [Command Postconditional Expressions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_commands#GCOS_commands_pc) in Using Caché ObjectScript.

Examples

The following example uses a single-level transaction to transfer a random amount of money from one account to another. If the transfer amount is more than the available balance, the program uses TROLLBACK to roll back the transaction:

SetupBankAccounts
   SET num\=12345
   SET ^CHECKING(num,"balance")\=500.99
   SET ^SAVINGS(num,"balance")\=100.22
   IF $DATA(^NumberOfTransfers)\=0 {SET ^NumberOfTransfers\=0}
BankTransfer
   WRITE "Before transfer:",!,"Checking=$",^CHECKING(num,"balance")," Savings=$",^SAVINGS(num,"balance"),!
   // Transfer funds from one account to another
   SET transfer\=$RANDOM(1000)
   WRITE "transfer amount $",transfer,!
   DO CkToSav(num,transfer)
   IF ok\=1 {WRITE "sucessful transfer",!,"Number of transfers to date=",^NumberOfTransfers,!}
   ELSE {WRITE "\*\*\* INSUFFICIENT FUNDS \*\*\*",!}
   WRITE "After transfer:",!,"Checking=$",^CHECKING(num,"balance")," Savings=$",^SAVINGS(num,"balance"),!
   RETURN
CkToSav(acct,amt)
  TSTART
  SET ^CHECKING(acct,"balance") \= ^CHECKING(acct,"balance") \- amt
  SET ^SAVINGS(acct,"balance") \= ^SAVINGS(acct,"balance") + amt
  SET ^NumberOfTransfers\=^NumberOfTransfers + 1
  IF ^CHECKING(acct,"balance") \> 0 {TCOMMIT  SET ok\=1 QUIT:ok}
  ELSE {TROLLBACK  SET ok\=0 QUIT:ok}

 

The following example shows the effects of TROLLBACK on nested transactions. Each TSTART increments $TLEVEL and sets a global. Issuing a TCOMMIT on the inner nested transaction decrements $TLEVEL, but the commitment of changes made in a nested transaction is deferred. In this case, the subsequent TROLLBACK on the outer transaction rolls back all changes made, including those in the inner “committed” nested transaction.

  SET ^a(1)\="\[- - -\]",^b(1)\="\[- - -\]"
  WRITE !,"level:",$TLEVEL," ",^a(1)," ",^b(1)
  TSTART
   LOCK +^a(1)
   SET ^a(1)\="hello"
   WRITE !,"level:",$TLEVEL," ",^a(1)," ",^b(1)
     TSTART
     LOCK +^b(1)
     SET ^b(1)\="world"
     WRITE !,"level:",$TLEVEL," ",^a(1)," ",^b(1)
     TCOMMIT
  WRITE !,"After TCOMMIT"
  WRITE !,"level:",$TLEVEL," ",^a(1)," ",^b(1)
  TROLLBACK
  WRITE !,"After TROLLBACK"
  WRITE !,"level:",$TLEVEL," ",^a(1)," ",^b(1)
  QUIT

 

The following example shows how TROLLBACK rolls back global variables, but not local variables:

  SET x\="default",^y\="default"
  WRITE !,"level:",$TLEVEL
  WRITE !,"local:",x," global:",^y
  TSTART
  SET x\="first",^y\="first"
  WRITE !,"TSTART level:",$TLEVEL
  WRITE !,"local:",x," global:",^y
    TSTART
    SET x\=x\_" second",^y\=^y\_" second"
    WRITE !,"TSTART level:",$TLEVEL
    WRITE !,"local:",x," global:",^y
     TSTART
     SET x\=x\_" third",^y\=^y\_" third"
    WRITE !,"TSTART level:",$TLEVEL
    WRITE !,"local:",x," global:",^y
  TROLLBACK
  WRITE !!,"After Rollback:"
  WRITE !,"TROLLBACK level:",$TLEVEL
  WRITE !,"local:",x," global:",^y

 

The following example shows how $INCREMENT changes to a global are not rolled back.

  SET ^x\=\-1,^y\=0
  WRITE !,"level:",$TLEVEL
  WRITE !,"Increment:",$INCREMENT(^x)," Add:",^y
  TSTART
  SET ^y\=^y+1
  WRITE !,"level:",$TLEVEL
  WRITE !,"Increment:",$INCREMENT(^x)," Add:",^y
    TSTART
    SET ^y\=^y+1,^z\=^z\_" second"
    WRITE !,"level:",$TLEVEL
    WRITE !,"Increment:",$INCREMENT(^x)," Add:",^y
     TSTART
     SET ^y\=^y+1,^z\=^z\_" third"
     WRITE !,"level:",$TLEVEL
     WRITE !,"Increment:",$INCREMENT(^x)," Add:",^y
  TROLLBACK
  WRITE !!,"After Rollback"
  WRITE !,"level:",$TLEVEL
  WRITE !,"Increment:",^x," Add:",^y

 

See Also

*   [TCOMMIT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ctcommit) command
    
*   [TSTART](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ctstart) command
    
*   [$TLEVEL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vtlevel) special variable
    
*   [Using ObjectScript for Transaction Processing](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_tp) in Using Caché ObjectScript
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cthrow "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ctry "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_ctrollback.xml**