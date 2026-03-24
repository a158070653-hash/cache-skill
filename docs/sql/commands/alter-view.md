# ALTER VIEW

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_alterview

---

 

Caché SQL Reference

ALTER VIEW

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_alteruser "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_call "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_COMMANDS "SQL Commands") \]  >  \[ [ALTER VIEW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_alterview "ALTER VIEW") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Modifies a view.

Synopsis

ALTER VIEW view-name \[(column-commalist)\] AS query \[WITH READ ONLY\]

ALTER VIEW view-name \[(column-commalist)\] AS query \[WITH \[level\] CHECK OPTION\]

Arguments

view-name

The view being modified, which has the same naming rules as a [table name](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_tables).

column-commalist

Optional — The column names that compose the view. If not specified here, the column names can be specified in the query, as shown below.

query

The result set (from a [query](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_queries)) that serves as a the basis for the view.

WITH READ ONLY

Optional — Specifies that no insert, update, or delete operations can be performed through this view upon the table on which the view is based. The default is to permit these operations through a view, subject to the constraints described below.

WITH level CHECK OPTION

Optional — Specifies how insert, update, or delete operations are performed through this view upon the table on which the view is based. The level can be the keywords LOCAL or CASCADED. If no level is specified, the WITH CHECK OPTION default is CASCADED. For further details, refer to [CREATE VIEW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createview).

Description

The ALTER VIEW command allows you to modify a [view](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_views). A view is based on the result set from a query consisting of a SELECT statement or a UNION of two or more SELECT statements. See [CREATE VIEW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createview) for further information on using a UNION.

To determine if a specified view exists in the current namespace, use the [$SYSTEM.SQL.ViewExists()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#ViewExists) method.

The optional column-commalist specifies the names of the columns included in the view. They must correspond in number and sequence with the table columns specified in the SELECT statement. You can also specify these view column names as column name aliases in the SELECT statement. If you specify neither, the table column names are used as the view column names.

These two ways to specify view column names are shown in the following examples:

ALTER VIEW MyView (MyViewCol1,MyViewCol2,MyViewCol3) AS
     SELECT TableCol1, TableCol2, TableCol3 FROM MyTable

is the same as:

ALTER VIEW MyView AS SELECT TableCol1 AS ViewCol1,
     TableCol2 AS ViewCol2,
     TableCol3 AS ViewCol3
     FROM MyTable

The column specification replaces any prior columns specified for the view.

A view query cannot contain [host variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_hvars) or include the [INTO](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_into) keyword. If you attempt to reference a host variable in query, Caché generates an SQLCODE -148 error.

Privileges

The ALTER VIEW command is a privileged operation. Prior to using ALTER VIEW it is necessary for your process to have either %ALTER\_VIEW administrative privilege or an %ALTER object privilege for the specified view. Failing to do so results in an SQLCODE -99 error (Privilege Violation). You can determine if the current user has %ALTER privilege by invoking the [%CHECKPRIV](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_checkpriv) command. You can determine if a specified user has %ALTER privilege by invoking the [$SYSTEM.SQL.CheckPriv()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#CheckPriv) method. You can use the GRANT command to assign %ALTER\_VIEW or %ALTER privileges, if you hold appropriate granting privileges.

In embedded SQL, you can use the [$SYSTEM.Security.Login()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Security#Login) method to log in as a user with appropriate privileges:

   DO $SYSTEM.Security.Login("\_SYSTEM","SYS")
   &sql(      )

You must have the %Service\_Login:Use privilege to invoke the $SYSTEM.Security.Login method. For further information, refer to [%SYSTEM.Security](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Security) in the InterSystems Class Reference.

Examples

The following examples create a view then alter that view. Programs are provided to query the view and to delete the view. Note that altering the view replaces the column list with a new column list; it does not preserve the prior column list.

  IF $SYSTEM.SQL.ViewExists("MassFolks")
     {WRITE "View already exists, please run DROP VIEW"  QUIT}
  &sql(CREATE VIEW MassFolks (vFullName) AS
       SELECT Name FROM Sample.Person WHERE Home\_State\='MA')
  IF SQLCODE\=0 { WRITE !,"Created a view",! }
  ELSE { WRITE "CREATE VIEW error SQLCODE=",SQLCODE }

 

SELECT \* FROM MassFolks

 

  IF 0\=$SYSTEM.SQL.ViewExists("MassFolks")
     {WRITE "View doesn't exist"  QUIT}
  &sql(ALTER VIEW MassFolks (vMassAbbrev,vCity) AS
     SELECT Home\_State,Home\_City FROM Sample.Person WHERE Home\_State\='MA')
  IF SQLCODE\=0 { WRITE !,"Altered view",! }
  ELSE { WRITE "ALTER VIEW error SQLCODE=",SQLCODE }

 

  DROP VIEW MassFolks

 

The following embedded SQL example alters a view using a query WITH CHECK OPTION:

   DO $SYSTEM.Security.Login("\_SYSTEM","SYS")
   &sql(ALTER VIEW Sample.MyTestView AS
     SELECT FIRSTWORD FROM Sample.MyTest WITH CHECK OPTION)
    IF SQLCODE\=0 { WRITE !,"Altered view" }
    ELSE { WRITE "ALTER VIEW error SQLCODE=",SQLCODE }

 

See Also

*   [CREATE VIEW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createview) [DROP VIEW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dropview) [GRANT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_grant)
    
*   “[Defining Views](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_views)” chapter in Using Caché SQL
    
*   [SQLCODE error messages](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RERR_sql) listed in the Caché Error Reference
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_alteruser "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_call "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_alterview.xml**