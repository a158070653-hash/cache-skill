# DROP TRIGGER

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_droptrigger

---

 

Caché SQL Reference

DROP TRIGGER

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_droptable "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dropuser "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_COMMANDS "SQL Commands") \]  >  \[ [DROP TRIGGER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_droptrigger "DROP TRIGGER") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Deletes a trigger.

Synopsis

DROP TRIGGER name FROM table

Arguments

name

The name of the trigger to be deleted.

FROM table

Optional — The table the trigger is to be deleted from. If the FROM clause is not specified, the entire schema is searched for the named trigger.

Description

The DROP TRIGGER statement deletes a trigger.

Privileges and Locking

The DROP TRIGGER command is a privileged operation. Prior to using DROP TRIGGER it is necessary for your process to have %DROP\_TRIGGER administrative privilege. Failing to do so results in an SQLCODE –99 error (Privilege Violation). You can use the GRANT command to assign %DROP\_TRIGGER privileges, if you hold appropriate granting privileges.

In embedded SQL, you can use the [$SYSTEM.Security.Login()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Security#Login) method to log in as a user with appropriate privileges:

   DO $SYSTEM.Security.Login("\_SYSTEM","SYS")
   &sql(      )

You must have the %Service\_Login:Use privilege to invoke the $SYSTEM.Security.Login method. For further information, refer to [%SYSTEM.Security](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Security) in the InterSystems Class Reference.

DROP TRIGGER cannot be used on a [table created by defining a persistent class](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_tables#GSQL_tables_via_classes), unless the table class definition includes \[[DdlAllowed](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ROBJ_class_ddlallowed)\]. Otherwise, the operation fails with an SQLCODE -300 error with the %msg DDL not enabled for class 'Schema.tablename'.

The DROP TRIGGER statement acquires a table-level lock on table. This prevents other processes from modifying the table’s data. This lock is automatically released at the conclusion of the DROP TRIGGER operation.

FROM Clause

A trigger and its table must reside in the same schema. A trigger name may be qualified or unqualified. If unqualified, the trigger schema defaults to the table schema, as specified in the FROM clause. If there is no FROM clause, or the table name is unqualified, the trigger schema defaults to the system default schema. If both the trigger name and the table name are qualified, they must both specify the same schema or Caché issues an SQLCODE -366 error.

In Caché SQL, a trigger name must be unique within its schema for a specific table. Thus it is possible to have more than one trigger in a schema with the same name. The optional FROM clause is used to determine which trigger to delete:

*   If no FROM clause is specified, and Caché locates a unique trigger in the schema that matches the specified name, Caché deletes the trigger.
    
*   If a FROM clause is specified, and Caché locates a unique trigger in the schema that matches both the specified name and the FROM table name, Caché deletes the trigger.
    
*   If no FROM clause is specified, and Caché locates more than one trigger that matches the specified name, Caché issues an SQLCODE -365 error.
    
*   If Caché locates no trigger that matches the specified name, either for the table specified in the FROM clause or, if there is no FROM clause, for any table in the schema, Caché issues an SQLCODE -363 error.
    

Examples

The following example deletes a trigger named Trigger\_1 associated with any table in the default schema (usually SQLUser is the default schema):

DROP TRIGGER Trigger\_1

The following example deletes a trigger named Trigger\_2 associated with any table in the A schema.

DROP TRIGGER A.Trigger\_2

The following example deletes a trigger named Trigger\_3 associated with the Patient table in the default schema. If a trigger named Trigger\_3 is found, but it is not associated with Patient, Caché issues an SQLCODE -363 error.

DROP TRIGGER Trigger\_3 FROM Patient

The following examples all delete a trigger named Trigger\_4 associated with the Patient table in the Test schema.

DROP TRIGGER Test.Trigger\_4 FROM Patient

DROP TRIGGER Trigger\_4 FROM Test.Patient

DROP TRIGGER Test.Trigger\_4 FROM Test.Patient

See Also

*   [CREATE TRIGGER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtrigger)
    
*   [GRANT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_grant)
    
*   “[Using Triggers](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_triggers)” chapter in Using Caché SQL
    
*   [SQLCODE error messages](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RERR_sql) listed in the Caché Error Reference
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_droptable "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dropuser "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_droptrigger.xml**