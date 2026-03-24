# UNLOCK

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_unlock

---

 

Caché SQL Reference

UNLOCK

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_union "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_update "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_COMMANDS "SQL Commands") \]  >  \[ [UNLOCK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_unlock "UNLOCK") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Unlocks a table.

Synopsis

UNLOCK \[TABLE\] tablename IN EXCLUSIVE MODE \[IMMEDIATE\]
UNLOCK \[TABLE\] tablename IN SHARE MODE \[IMMEDIATE\]

Arguments

tablename

The name of the table to be unlocked. tablename must be an existing table.

IN EXCLUSIVE MODE / IN SHARE MODE

The IN EXCLUSIVE MODE keyword phrase releases a regular Caché lock. The IN SHARE MODE keyword phrase releases a shared lock at the Caché level.

IMMEDIATE

Optional — If not specified, Caché releases the lock at the end of the current transaction. If specified, Caché releases the lock immediately.

Description

The UNLOCK command unlocks an SQL table that was locked by the LOCK command. This table must be an existing table for which you have the necessary privileges. If tablename is a temporary table, the command completes successfully, but performs no operation. If tablename is a view, the command fails with an SQLCODE -400 error.

UNLOCK and UNLOCK TABLE are synonymous.

The UNLOCK command reverses the LOCK operation. The UNLOCK command completes successfully even when no lock is held. You can use LOCK to lock a table multiple times; you must explicitly UNLOCK the table as many times as it was explicitly locked.

Privileges

The UNLOCK command is a privileged operation. Prior to using UNLOCK IN SHARE MODE it is necessary for your process to have SELECT privilege for the specified table. Prior to using UNLOCK IN EXCLUSIVE MODE it is necessary for your process to have INSERT, UPDATE, or DELETE privilege for the specified table. For IN EXCLUSIVE MODE, the INSERT or UPDATE privilege must be on at least one field of the table. Failing to hold sufficient privileges results in an SQLCODE -99 error (Privilege Violation). You can determine if the current user has the necessary privileges by invoking the [%CHECKPRIV](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_checkpriv) command. You can determine if a specified user has the necessary table-level privileges by invoking the [$SYSTEM.SQL.CheckPriv()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#CheckPriv) method. For privilege assignment, refer to the [GRANT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_grant) command.

Nonexistent Table

If you try to unlock a nonexistent table, UNLOCK fails with a compile error.

Examples

The following embedded SQL examples create a table, lock it and then unlock it:

  NEW SQLCODE,%msg
  &sql(CREATE TABLE mytest (
      ID NUMBER(12,0) NOT NULL,
      CREATE\_DATE DATE DEFAULT CURRENT\_TIMESTAMP(2),
      WORK\_START DATE DEFAULT SYSDATE) )
  IF SQLCODE\=0 { WRITE !,"Table created" }
  ELSE { WRITE !,"CREATE TABLE error: ",SQLCODE
         QUIT }

 

  NEW SQLCODE,%msg
  &sql(LOCK mytest IN EXCLUSIVE MODE) 
  IF SQLCODE\=0 { WRITE !,"Table locked" }
  ELSEIF SQLCODE\=\-110 { WRITE !,"Table is locked by another process",!,%msg }
  ELSE { WRITE !,"Unexpected LOCK error: ",SQLCODE,!,%msg }
  &sql(UNLOCK mytest IN EXCLUSIVE MODE) 
  IF SQLCODE\=0 { WRITE !,"Table unlocked" }
  ELSE { WRITE !,"Unexpected UNLOCK error: ",SQLCODE,!,%msg }

 

See Also

*   [LOCK](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_lock)
    
*   [INSERT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_insert) [UPDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_update) [DELETE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_delete)
    
*   [SQLCODE error messages](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RERR_sql) listed in the Caché Error Reference
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_union "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_update "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:38**

Source: **RSQL\_unlock.xml**