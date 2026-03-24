# DROP QUERY

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_dropquery

---

 

Caché SQL Reference

DROP QUERY

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dropprocedure "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_droprole "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_COMMANDS "SQL Commands") \]  >  \[ [DROP QUERY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dropquery "DROP QUERY") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Deletes a query.

Synopsis

DROP QUERY name \[ FROM className \]

Arguments

name

The name of the query to be deleted. The name is an [identifier](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_identifiers). For further details see the “Identifiers” chapter of Using Caché SQL.

FROM className

Optional — If specified, the FROM className clause deletes the query from the given class. If this clause is not specified, Caché searches all classes of the schema for the query, and deletes it. However, if no query of this name is found, or more than one query of this name is found, an error code is returned. If the deletion of the query results in an empty class, DROP QUERY deletes the class as well.

Description

The DROP QUERY command deletes a query. When you drop a query, Caché revokes it from all users and roles to whom it has been granted and removes it from the database.

In order to drop a query, you must have %DROP\_QUERY administrative privilege, as specified by the [GRANT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_grant) command. If you are attempting to delete a query for a class with a defined owner, you must be logged in as the owner of the class. Otherwise, Caché generates an SQLCODE -99 error (Privilege Violation).

If the specified query does not exist, DROP QUERY generates an SQLCODE -362 error. If the specified class does not exist, DROP QUERY generates an SQLCODE -360 error. If the specified query could refer to two or more queries, DROP QUERY generates an SQLCODE -361 error; you must specify a className to resolve this ambiguity.

Examples

The following embedded SQL example attempts to delete myq from the class User.Employee. (Refer to CREATE TABLE for an example that creates class User.Employee.)

   &sql(DROP QUERY myq FROM User.Employee)
  IF SQLCODE\=0 {
    WRITE !,"Query deleted" }
  ELSEIF SQLCODE\=\-360 {
    WRITE !,"Nonexistent class: ",%msg }
  ELSEIF SQLCODE\=\-362 {
    WRITE !,"Nonexistent query: ",%msg }
  ELSE {WRITE !,"Unexpected Error code: ",SQLCODE}

 

See Also

*   [CREATE QUERY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createquery)
    
*   [SQLCODE error messages](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RERR_sql) listed in the Caché Error Reference
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dropprocedure "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_droprole "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_dropquery.xml**