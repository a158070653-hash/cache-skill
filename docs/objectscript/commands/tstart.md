# TSTART

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_ctstart

---

 

Caché ObjectScript Reference

TSTART

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ctry "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cuse "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_COMMANDS "Caché ObjectScript Commands") \]  >  \[ [TSTART](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ctstart "TSTART") \]

Go to: Description Argument Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Marks the beginning of a transaction.

Synopsis

TSTART:pc
TS:pc

Argument

pc

Optional — A postconditional expression.

Description

TSTART marks the beginning of a transaction. Following TSTART, database operations are journaled to enable a subsequent TCOMMIT or TROLLBACK command.

TSTART increments the value of the $TLEVEL special variable. A $TLEVEL value of 0 indicates that no transaction is in effect. The first TSTART begins a transaction and increments $TLEVEL to 1. Subsequent TSTART commands can create nested transactions, further incrementing $TLEVEL.

Not all operations that occur within a transaction can be rolled back. For example, setting global variables within a transaction can be rolled back; setting local variables within a transaction cannot be rolled back. Refer to [Using ObjectScript for Transaction Processing](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_tp) in Using Caché ObjectScript for further details.

By default, a lock issued within a transaction will be held until the end of the transaction, even if the lock is released within the transaction. This default can be overridden when setting the lock. Refer to the [LOCK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_clock) command for more details.

Nested Transactions

If you issue a TSTART within a transaction it begins a nested transaction. Issuing a TSTART increments the $TLEVEL value, indicating the number of levels of transaction nesting. You end a nested transaction by issuing either a TCOMMIT to commit the nested transaction, or a TROLLBACK 1 to roll back the nested transaction. Ending a nested transaction decrements the $TLEVEL value by 1.

*   Issuing a TROLLBACK 1 for a nested transaction rolls back changes made in that nested transaction and decrements $TLEVEL. You can issue a TROLLBACK to roll back the whole transaction, no matter how many levels of TSTART were issued.
    
*   Issuing a TCOMMIT for a nested transaction decrements $TLEVEL, but the actual commitment of the nested transaction is deferred. Changes made during a nested transaction are only irreversibly committed when the outermost transaction is committed; that is, when a TCOMMIT decrements the $TLEVEL value to 0.
    

You can use the [GetImageJournalInfo()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYS.Journal.System#GetImageJournalInfo) method of the [%SYS.Journal.System](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYS.Journal.System) class to search the journal file for TSTART commands, and thus identify open transactions. A TSTART writes either a “BT” (Begin Transaction) journal file record if $TLEVEL was zero, or a “BTL” (Begin Transaction with Level) journal file record if $TLEVEL was greater than 0.

The maximum number of levels of nested transactions is 255. Attempting to exceed this nesting levels limit results in a <TRANSACTION LEVEL> error.

SQL and Transactions

Caché ObjectScript and SQL transaction commands are fully compatible and interchangeable, with the following exception:

ObjectScript TSTART and SQL START TRANSACTION both start a transaction if no transaction is current. However, START TRANSACTION does not support nested transactions. Therefore, if you need (or may need) nested transactions, it is preferable to start the transaction with TSTART. If you need compatibility with the SQL standard, use START TRANSACTION.

Caché ObjectScript transaction processing provides limited support for nested transactions. SQL transaction processing supplies support for savepoints within transactions.

Argument

pc

An optional postconditional expression. Caché executes the TSTART command if the postconditional expression is true and does not execute the TSTART command if the postconditional expression is false. For further details, refer to [Command Postconditional Expressions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_commands#GCOS_commands_pc) in Using Caché ObjectScript.

Examples

The following example uses a single-level transaction to transfer a random amount of money from one account to another. If the transfer amount is more than the available balance, the program rolls back the transaction:

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

 

The following examples use TSTART to create nested transactions. They show three scenarios for rollback of nested transactions:

Roll back the innermost transaction, commit the middle transaction, commit the outermost transaction:

  KILL ^a,^b,^c
  TSTART  SET ^a\=1 WRITE "tlevel=",$TLEVEL,!
    TSTART  SET ^b\=2 WRITE "tlevel=",$TLEVEL,!
      TSTART  SET ^c\=3 WRITE "tlevel=",$TLEVEL,!
      TROLLBACK 1 WRITE "tlevel=",$TLEVEL,!
    TCOMMIT  WRITE "tlevel=",$TLEVEL,!
  TCOMMIT  WRITE "tlevel=",$TLEVEL,!
  IF $DATA(^a) {WRITE "^a=",^a,!} ELSE {WRITE "^a is undefined",!}
  IF $DATA(^b) {WRITE "^b=",^b,!} ELSE {WRITE "^b is undefined",!}
  IF $DATA(^c) {WRITE "^c=",^c,!} ELSE {WRITE "^c is undefined",!}

 

Commit the innermost transaction, roll back the middle transaction, commit the outermost transaction:

  KILL ^a,^b,^c
  TSTART  SET ^a\=1 WRITE "tlevel=",$TLEVEL,!
    TSTART  SET ^b\=2 WRITE "tlevel=",$TLEVEL,!
      TSTART  SET ^c\=3 WRITE "tlevel=",$TLEVEL,!
      TCOMMIT  WRITE "tlevel=",$TLEVEL,!
    TROLLBACK 1 WRITE "tlevel=",$TLEVEL,!
  TCOMMIT  WRITE "tlevel=",$TLEVEL,!
  IF $DATA(^a) {WRITE "^a=",^a,!} ELSE {WRITE "^a is undefined",!}
  IF $DATA(^b) {WRITE "^b=",^b,!} ELSE {WRITE "^b is undefined",!}
  IF $DATA(^c) {WRITE "^c=",^c,!} ELSE {WRITE "^c is undefined",!}

 

Commit the innermost transaction, commit the middle transaction, roll back the outermost transaction:

  KILL ^a,^b,^c
  TSTART  SET ^a\=1  WRITE "tlevel=",$TLEVEL,!
    TSTART  SET ^b\=2  WRITE "tlevel=",$TLEVEL,!
      TSTART  SET ^c\=3  WRITE "tlevel=",$TLEVEL,!
      TCOMMIT  WRITE "tlevel=",$TLEVEL,!
    TCOMMIT  WRITE "tlevel=",$TLEVEL,!
  TROLLBACK 1  WRITE "tlevel=",$TLEVEL,!
  IF $DATA(^a) {WRITE "^a=",^a,!} ELSE {WRITE "^a is undefined",!}
  IF $DATA(^b) {WRITE "^b=",^b,!} ELSE {WRITE "^b is undefined",!}
  IF $DATA(^c) {WRITE "^c=",^c,!} ELSE {WRITE "^c is undefined",!}

 

Note that in this third case, TROLLBACK 1 and TROLLBACK would have the same result, because both would decrement $TLEVEL to 0.

See Also

*   [TCOMMIT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ctcommit) command
    
*   [TROLLBACK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ctrollback) command
    
*   [$TLEVEL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vtlevel) special variable
    
*   [Using ObjectScript for Transaction Processing](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_tp) in Using Caché ObjectScript
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ctry "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cuse "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_ctstart.xml**