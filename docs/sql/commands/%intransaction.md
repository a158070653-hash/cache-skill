# %INTRANSACTION

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_intransaction

---

 

Caché SQL Reference

%INTRANSACTION

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_into "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_join "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_COMMANDS "SQL Commands") \]  >  \[ [%INTRANSACTION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_intransaction "%INTRANSACTION") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Shows transaction state.

Synopsis

%INTRANSACTION
%INTRANS

Arguments

None

Description

The %INTRANSACTION statement sets SQLCODE to indicate the transaction state:

*   SQLCODE=0 if currently in a transaction.
    
*   SQLCODE=100 if not in a transaction.
    

%INTRANSACTION returns SQLCODE=0 while a transaction is in progress. This transaction can be an SQL transaction initiated by START TRANSACTION or SAVEPOINT. It can also be a Caché ObjectScript transaction initiated by [TSTART](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_ctstart).

Transaction nesting has no effect on %INTRANSACTION. SET TRANSACTION has no effect on %INTRANSACTION.

You can also determine transaction state using [$TLEVEL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_svars). %INTRANSACTION only indicates whether a transaction is in progress. $TLEVEL indicates both whether a transaction is in progress and the current number of transaction levels.

Examples

The following embedded SQL example shows how %INTRANSACTION sets SQLCODE:

   NEW SQLCODE
   &sql(%INTRANSACTION)
   WRITE "Before %INTRANS SQLCODE=",SQLCODE," TL=",$TLEVEL,!
   &sql(SET TRANSACTION %COMMITMODE EXPLICIT)
   NEW SQLCODE
   &sql(%INTRANSACTION)
   WRITE "SetTran %INTRANS SQLCODE=",SQLCODE," TL=",$TLEVEL,!
  &sql(START TRANSACTION)
   NEW SQLCODE
   &sql(%INTRANSACTION)
   WRITE "StartTran %INTRANS SQLCODE=",SQLCODE," TL=",$TLEVEL,!
   &sql(SAVEPOINT a)
   NEW SQLCODE
   &sql(%INTRANSACTION)
   WRITE "Savepoint %INTRANS SQLCODE=",SQLCODE," TL=",$TLEVEL,!
   &sql(ROLLBACK TO SAVEPOINT a)
   NEW SQLCODE
   &sql(%INTRANSACTION)
   WRITE "Rollback to Savepoint %INTRANS SQLCODE=",SQLCODE," TL=",$TLEVEL,!
   &sql(COMMIT)
   NEW SQLCODE
   &sql(%INTRANSACTION)
   WRITE "After Commit %INTRANS SQLCODE=",SQLCODE," TL=",$TLEVEL

 

See Also

*   [COMMIT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_commit) [ROLLBACK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_rollback) [SAVEPOINT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_savepoint) [SET TRANSACTION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_settransaction) [START TRANSACTION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_starttransaction) [$TLEVEL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_svars)
    
*   [Transaction Processing](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_modify#GSQL_modify_transaction) in the “Modifying the Database” chapter of Using Caché SQL.
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_into "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_join "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_intransaction.xml**