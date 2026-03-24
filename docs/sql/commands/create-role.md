# CREATE ROLE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_createrole

---

 

Caché SQL Reference

CREATE ROLE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createquery "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_COMMANDS "SQL Commands") \]  >  \[ [CREATE ROLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createrole "CREATE ROLE") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Creates a role.

Synopsis

CREATE ROLE role-name

Arguments

role-name

The name of the role to be created, which is an [identifier](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_identifiers). Role names are not case-sensitive. For further details see the “Identifiers” chapter of Using Caché SQL.

Description

The CREATE ROLE command creates a role. A role is a named set of privileges that may be assigned to multiple users. A role may be assigned to multiple users, and a user may be assigned multiple roles. A role is available system-wide, it is not limited to a specific namespace.

A role-name can be any valid identifier of up to 30 characters. A role name must follow [identifier](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_identifiers) naming conventions. If the role name is a [delimited identifier](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_identifiers#GSQL_identifiers_delimitedids) enclosed in quotation marks, it can be an SQL reserved word. A role name can contain Unicode characters. Role names are not case-sensitive.

When initially created, a role is just a name; it has no privileges. To add privileges to a role, use the [GRANT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_grant) command. You can also use the GRANT command to assign one or more roles to a role. This permits you to create a hierarchy of roles.

If you invoke CREATE ROLE to create a role that already exists, SQL issues an SQLCODE -118 error. You can determine if a role already exists by invoking the [$SYSTEM.SQL.RoleExists()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#RoleExists) method:

  WRITE $SYSTEM.SQL.RoleExists("%All"),!
  WRITE $SYSTEM.SQL.RoleExists("Madmen")

 

This method returns 1 if the specified role exists, and 0 if the role does not exist. Role names are not case-sensitive.

To delete a role, use the DROP ROLE command.

Privileges

The CREATE ROLE command is a privileged operation. Before using CREATE ROLE in embedded SQL, it is necessary to be logged in as a user with %Admin\_Secure:USE privilege. Failing to do so results in an SQLCODE -99 error (Privilege Violation). Use the [$SYSTEM.Security.Login()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Security#Login) method to assign a user with appropriate privileges:

   DO $SYSTEM.Security.Login(username,password)
   &sql(      )

You must have the %Service\_Login:Use privilege to invoke the $SYSTEM.Security.Login() method. For further information, refer to [%SYSTEM.Security](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Security) in the InterSystems Class Reference.

Examples

The following examples attempt to create a role named BkUser. The user “FRED” in the first example does not have create role privileges. The user “\_SYSTEM” in the second example does have create role privileges.

  DO $SYSTEM.Security.Login("FRED","FredsPassword")
  &sql(CREATE ROLE BkUser)
  IF SQLCODE\=\-99 {
    WRITE !,"You don't have CREATE ROLE privileges" }
  ELSEIF SQLCODE\=\-118 {
    WRITE !,"The role already exists" }
  ELSE {
    WRITE !,"Created a role. Error code is: ",SQLCODE }

 

  DO $SYSTEM.Security.Login("\_SYSTEM","SYS")
Main
  &sql(CREATE ROLE BkUser)
  IF SQLCODE\=\-99 {
    WRITE !,"You don't have CREATE ROLE privileges" }
  ELSEIF SQLCODE\=\-118 {
    WRITE !,"The role already exists" }
  ELSE {
    WRITE !,"Created a role. Error code is: ",SQLCODE }
Cleanup
   SET toggle\=$RANDOM(2)
   IF toggle\=0 { 
     &sql(DROP ROLE BkUser)
     WRITE !,"DROP USER error code: ",SQLCODE
   }
   ELSE { 
     WRITE !,"No drop this time"
     QUIT 
   }

 

(The $RANDOM toggle is provided so that you can execute this example program repeatedly.)

See Also

*   SQL statements: [DROP ROLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_droprole) [CREATE USER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createuser) [DROP USER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dropuser) [GRANT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_grant) [REVOKE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_revoke) [%CHECKPRIV](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_checkpriv)
    
*   “[Users, Roles, and Privileges](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_privileges)” chapter of Using Caché SQL
    
*   [SQLCODE error messages](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RERR_sql) listed in the Caché Error Reference
    
*   Caché ObjectScript: [$ROLES](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vroles) and [$USERNAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vusername) special variables
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createquery "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_createrole.xml**