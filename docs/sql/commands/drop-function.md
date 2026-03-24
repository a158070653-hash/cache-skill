# DROP FUNCTION

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_dropfunction

---

 

Caché SQL Reference

DROP FUNCTION

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dropdatabase "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dropindex "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_COMMANDS "SQL Commands") \]  >  \[ [DROP FUNCTION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dropfunction "DROP FUNCTION") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Deletes a function.

Synopsis

DROP FUNCTION name FROM className

Arguments

name

The name of the function to be deleted. The name is an [identifier](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_identifiers). For further details see the “Identifiers” chapter of Using Caché SQL.

FROM className

Optional — If specified, the FROM className clause deletes the function from the given class. If the FROM clause is not specified, Caché searches all classes of the schema for the function, and deletes it. However, if no function of this name is found, or more than one function of this name is found, an error code is returned. If the deletion of the function results in an empty class, DROP FUNCTION deletes the class as well.

Description

The DROP FUNCTION command deletes a function. When you drop a function, Caché revokes it from all users and roles to whom it has been granted and removes it from the database.

In order to drop a function, you must have %DROP\_FUNCTION administrative privilege, as specified by the [GRANT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_grant) command. Otherwise, Caché generates an SQLCODE -99 error (Privilege Violation).

If the specified function does not exist, DROP FUNCTION generates an SQLCODE -362 error. If the specified class does not exist, DROP FUNCTION generates an SQLCODE -360 error. If the specified function could refer to two or more functions, DROP FUNCTION generates an SQLCODE -361 error; you must specify a className to resolve this ambiguity.

Examples

The following embedded SQL example attempts to delete myfunc from the class User.Employee. (Refer to CREATE TABLE for an example that creates class User.Employee.)

   &sql(DROP FUNCTION myfunc FROM User.Employee)
  IF SQLCODE\=0 {
    WRITE !,"Function deleted" }
  ELSEIF SQLCODE\=\-360 {
    WRITE !,"Nonexistent class: ",%msg }
  ELSEIF SQLCODE\=\-362 {
    WRITE !,"Nonexistent function: ",%msg }
  ELSE {WRITE !,"Unexpected Error code: ",SQLCODE}

 

See Also

*   [CREATE FUNCTION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createfunction)
    
*   [SQLCODE error messages](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RERR_sql) listed in the Caché Error Reference
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dropdatabase "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dropindex "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_dropfunction.xml**