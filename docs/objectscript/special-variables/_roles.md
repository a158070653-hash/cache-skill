# $ROLES

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_vroles

---

 

Caché ObjectScript Reference

$ROLES

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vquit "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vstack "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Special Variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_VARIABLES "Caché ObjectScript Special Variables") \]  >  \[ [$ROLES](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vroles "$ROLES") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Contains the roles assigned to the current process.

Synopsis

$ROLES

Description

$ROLES contains the list of roles assigned to the current process. This list of roles consists of a comma-separated string that can contain both User Roles and Added Roles.

A role is assigned to a user either by using the SQL GRANT statement, or by using the Management Portal \[Home\] > \[Security Management\] > \[Users\] options to edit the definition of the User to assign a role. A role can be defined using the SQL CREATE ROLE statement and deleted using the SQL DROP ROLE statement. A role must be defined before it can be assigned to a user. A role can be revoked from a user using the SQL REVOKE statement.

When a process is created using the JOB command, it inherits the same $ROLES and $USERNAME values as its parent process.

When a process performs I/O redirection, this redirection is performed using the user’s login $ROLES value, not the current $ROLES value.

Roles Granted to Roles Not Listed

Granting a role to another role is a concept only available through Caché SQL. Roles granted to roles are used in SQL to determine the user's role list for checking SQL privileges. They cannot be accessed by Caché ObjectScript. You cannot grant a role to another role through Caché System Security. Therefore, the $ROLES special variable list does not contain any roles that SQL operations have granted to the current roles. For further details, refer to the [GRANT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_grant) command in the Caché SQL Reference.

SET $ROLES

You can use the SET command to change the Added Roles part of the list contained in $ROLES. Setting $ROLES only alters a process' Added Roles. It cannot alter its User Roles. To set $ROLES to a different list of Added Roles is a restricted system capability. However, such restrictions do not apply to setting $ROLES to a null string, which deletes the list of Added Roles.

A role must be defined before it can be added. You can define a role using the SQL CREATE ROLE command. CREATE ROLE does not give any privileges to a role. To assign privileges to a role use either the SQL GRANT statement, or the Management Portal \[Home\] > \[Security Management\] > \[Roles\] interface.

You must issue a NEW $ROLES statement before escalating the process roles using SET $ROLES.

NEW $ROLES

NEW $ROLES stacks the current values of both $ROLES and $USERNAME. You can use the NEW command on $ROLES without security restrictions.

Issue a NEW $ROLES and then SET $ROLES to supply Added Roles. You can then create an object instance that uses these Added Roles. If you quit this routine, Caché closes the object with the Added Roles before reverting to the stacked $ROLES value.

Examples

The following example returns the list of roles for the current process.

   WRITE $ROLES

 

The following example first creates the roles Vendor, Sales, and Contractor. It then displays the comma-separated list of default roles (which contain both User Roles and Added Roles). The first SET $ROLES replaces the list of Added Roles with the two roles Sales and Contractor. The second SET $ROLES concatenates the Vendor role to the list of Added Roles. The final SET $ROLES sets the Added Roles list to the null string, removing all Added Roles. The User Roles remain unchanged throughout:

CreateRoles
   &sql(CREATE ROLE Vendor)
   &sql(CREATE ROLE Sales)
   &sql(CREATE ROLE Contractor)
   IF SQLCODE\=0 { 
     WRITE !,"Created new roles"
     DO SetRoles }
   ELSEIF SQLCODE\=\-118 {
     WRITE !,"Role already exists"
     DO SetRoles }
   ELSE { WRITE !,"CREATE ROLE failed, SQLCODE=",SQLCODE }
SetRoles()
     WRITE !,"Initial: ",$ROLES
     NEW $ROLES
     SET $ROLES\="Sales,Contractor"
     WRITE !,"Replaced: ",$ROLES
     NEW $ROLES
     SET $ROLES\=$ROLES\_",Vendor"
     WRITE !,"Concatenated: ",$ROLES
     SET $ROLES\=""
     WRITE !,"Nulled: ",$ROLES

 

See Also

*   Caché ObjectScript: [SET](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cset) command [NEW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_cnew) command [$USERNAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vusername) special variable
    
*   Caché SQL: [CREATE ROLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createrole) [DROP ROLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_droprole) [GRANT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_grant) [REVOKE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_revoke) [%CHECKPRIV](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_checkpriv)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vquit "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vstack "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_vroles.xml**