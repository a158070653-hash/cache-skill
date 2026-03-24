# DROP METHOD

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_dropmethod

---

 

Caché SQL Reference

DROP METHOD

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dropindex "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dropprocedure "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_COMMANDS "SQL Commands") \]  >  \[ [DROP METHOD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dropmethod "DROP METHOD") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Deletes a method.

Synopsis

DROP METHOD name \[ FROM className \]

Arguments

name

The name of the method to be deleted as an unqualified name. Do not specify the method’s parameter parentheses. The name is an [identifier](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_identifiers). For further details see the “Identifiers” chapter of Using Caché SQL.

FROM className

Optional — If specified, the FROM className clause deletes the method from the given class. If this clause is not specified, Caché searches all classes of the schema for the method, and deletes it. However, if no method of this name is found, or more than one method of this name is found, an error code is returned. If the deletion of the method results in an empty class, DROP METHOD deletes the class as well.

Description

The DROP METHOD command deletes a method. When you delete a method, Caché revokes it from all users and roles to whom it has been granted and removes it from the database.

In order to delete a method, you must have %DROP\_METHOD administrative privilege, as specified by the [GRANT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_grant) command. If you are attempting to delete a method for a class with a defined owner, you must be logged in as the owner of the class. Otherwise, Caché generates an SQLCODE -99 error (Privilege Violation).

If the specified method does not exist, DROP METHOD generates an SQLCODE -362 error. If the specified className does not exist, DROP METHOD generates an SQLCODE -360 error. If the specified method could refer to two or more methods, DROP METHOD generates an SQLCODE -361 error; you must specify a className to resolve this ambiguity.

If a method has been defined with the PROCEDURE characteristic keyword, you can determine if it exists in the current namespace by invoking the [$SYSTEM.SQL.ProcedureExists()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#ProcedureExists) method. A method defined with the PROCEDURE keyword can be deleted either by DROP METHOD or DROP PROCEDURE.

Examples

The following embedded SQL example attempts to delete mymeth from the class User.Employee. (Refer to CREATE TABLE for an example that creates class User.Employee.)

   &sql(DROP METHOD mymeth FROM User.Employee)
  IF SQLCODE\=0 {
    WRITE !,"Method deleted" }
  ELSEIF SQLCODE\=\-360 {
    WRITE !,"Nonexistent class: ",%msg }
  ELSEIF SQLCODE\=\-362 {
    WRITE !,"Nonexistent method: ",%msg }
  ELSE {WRITE !,"Unexpected Error code: ",SQLCODE}

 

See Also

*   [CREATE METHOD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createmethod)
    
*   [SQLCODE error messages](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RERR_sql) listed in the Caché Error Reference
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dropindex "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dropprocedure "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_dropmethod.xml**