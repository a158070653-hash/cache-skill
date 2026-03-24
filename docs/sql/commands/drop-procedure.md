# DROP PROCEDURE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_dropprocedure

---

 

Caché SQL Reference

DROP PROCEDURE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dropmethod "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dropquery "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_COMMANDS "SQL Commands") \]  >  \[ [DROP PROCEDURE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dropprocedure "DROP PROCEDURE") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Deletes a procedure.

Synopsis

DROP PROCEDURE procname \[ FROM className \]
DROP PROC procname \[ FROM className \]

Arguments

procname

The name of the procedure to be deleted, specified as an unqualified name. Do not specify the procedure’s parameter parentheses. The name is an [identifier](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_identifiers). For further details see the “Identifiers” chapter of Using Caché SQL.

FROM className

Optional — If specified, the FROM className clause deletes the procedure from the given class. If this clause is not specified, Caché searches all classes of the schema for the procedure, and deletes it. However, if no procedure of this name is found, or more than one procedure of this name is found, an error code is returned. If the deletion of the procedure results in an empty class, DROP PROCEDURE deletes the class as well.

Description

The DROP PROCEDURE command deletes a procedure in the current namespace. When you drop a procedure, Caché revokes it from all users and roles to whom it has been granted and removes it from the database.

In order to drop a procedure, you must have %DROP\_PROCEDURE administrative privilege, as specified by the [GRANT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_grant) command. If you are attempting to delete a procedure for a class with a defined owner, you must be logged in as the owner of the class. Otherwise, Caché generates an SQLCODE -99 error (Privilege Violation).

The procname is not case-sensitive. You must specify procname without parameter parentheses; specifying parameter parentheses results in an SQLCODE -25 error.

If the specified procedure does not exist, DROP PROCEDURE generates an SQLCODE -362 error. If the specified class does not exist, DROP PROCEDURE generates an SQLCODE -360 error. If the specified procedure could refer to two or more procedures, DROP PROCEDURE generates an SQLCODE -361 error; you must specify a className to resolve this ambiguity.

To determine if a specified procname exists in the current namespace, use the [$SYSTEM.SQL.ProcedureExists()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#ProcedureExists) method. This method recognizes both procedures and methods defined with the PROCEDURE keyword. A method defined with the PROCEDURE keyword can be deleted using DROP PROCEDURE.

If you execute a DROP PROCEDURE for a procedure that is an ObjectScript class query procedure, Caché will also drop the methods related to the procedure, such as myprocExecute(), myprocGetInfo(), myprocFetch(), myprocFetchRows(), and myprocClose().

Examples

The following embedded SQL example attempts to delete myprocSP from the class User.Employee. (Refer to CREATE TABLE for an example that creates class User.Employee.)

   &sql(DROP PROCEDURE myprocSP FROM User.Employee)
  IF SQLCODE\=0 {
    WRITE !,"Procedure deleted" }
  ELSEIF SQLCODE\=\-360 {
    WRITE !,"Nonexistent class: ",%msg }
  ELSEIF SQLCODE\=\-362 {
    WRITE !,"Nonexistent procedure: ",%msg }
  ELSE {WRITE !,"Unexpected Error code: ",SQLCODE}

 

See Also

*   [CREATE PROCEDURE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createprocedure)
    
*   [SQLCODE error messages](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RERR_sql) listed in the Caché Error Reference
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dropmethod "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dropquery "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_dropprocedure.xml**