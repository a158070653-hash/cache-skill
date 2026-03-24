# TCOMMIT

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_ctcommit

---

 

Caché ObjectScript Reference

TCOMMIT

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cset "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cthrow "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_COMMANDS "Caché ObjectScript Commands") \]  >  \[ [TCOMMIT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ctcommit "TCOMMIT") \]

Go to: Description Argument Examples Notes See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Marks the successful completion of a transaction.

Synopsis

TCOMMIT:pc
TC:pc

Argument

pc

Optional — A postconditional expression.

Description

TCOMMIT marks the successful end of a transaction initiated by the corresponding TSTART.

TCOMMIT decrements the value of the $TLEVEL special variable. Caché terminates the transaction only if $TLEVEL goes to 0. Usually this is when TCOMMIT has been called as many times as TSTART. Changes made during nested transactions are not committed until $TLEVEL\=0.

Calling TCOMMIT when $TLEVEL is already 0 results in a <COMMAND> error. This can occur if you issue a TCOMMIT when no transaction is in progress, when the number of TCOMMIT commands is larger than the number of TSTART commands, or following a TROLLBACK command. The corresponding [$ZERROR](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vzerror) value consists of <COMMAND>, the location of the error (for example +3^mytest), and the data literal \*NoTransaction.

Argument

pc

An optional postconditional expression. Caché executes the command if the postconditional expression is true (evaluates to a nonzero numeric value). Caché does not execute the command if the postconditional expression is false (evaluates to zero). For further details, refer to [Command Postconditional Expressions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_commands#GCOS_commands_pc) in Using Caché ObjectScript.

Examples

You use TCOMMIT with the TROLLBACK and TSTART commands. See [TROLLBACK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ctrollback) and [TSTART](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ctstart) for examples of how to use these transaction processing commands together.

Notes

Nested TSTART / TCOMMIT

Caché supports the nesting of the TSTART/TCOMMIT commands, so that modules can issue their TSTART/TCOMMIT pairs correctly, independent of any other TSTART/TCOMMIT issued in the modules that called them or in the modules they call. The current nesting level of the transaction is tracked by the special variable $TLEVEL. The transaction is committed when the outermost matching TCOMMIT is issued; that is, when $TLEVEL goes back to 0.

You can roll back individual nested transactions by calling TROLLBACK 1 or roll back all current transactions by calling TROLLBACK. TROLLBACK rolls back the whole transaction that is in effect — no matter how many levels of TSTART were issued — and sets $TLEVEL to 0.

Network Transactions

To synchronize the completion of transactions over a network, use the [ZSYNC](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_czsync) command.

Synchronous Commit

A TCOMMIT command requests a flush of the journal data involved in that transaction to disk. Whether to wait for this disk write operation to complete is a configurable option.

Go to the Management Portal, select \[Home\] > \[Configuration\] > \[Compatibility Settings\]. View and edit the current setting of SynchCommit. When set to “true”, TCOMMIT does not complete until the journal data write operation completes. When set to “false”, TCOMMIT does not wait for the write operation to complete. The default is “false”. A restart is required for a change to the SynchCommit setting to take effect.

SQL and Transactions

Caché ObjectScript and SQL transaction commands are fully compatible and interchangeable, with the following exception:

ObjectScript TSTART and SQL START TRANSACTION both start a transaction if no transaction is current. However, START TRANSACTION does not support nested transactions. Therefore, if you need (or may need) nested transactions, it is preferable to start the transaction with TSTART. If you need compatibility with the SQL standard, use START TRANSACTION.

Caché ObjectScript transaction processing provides limited support for nested transactions. SQL transaction processing supplies support for savepoints within transactions.

See Also

*   [TROLLBACK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ctrollback) command
    
*   [TSTART](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ctstart) command
    
*   [$TLEVEL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vtlevel) special variable
    
*   [Using ObjectScript for Transaction Processing](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_tp) in Using Caché ObjectScript
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cset "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cthrow "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_ctcommit.xml**