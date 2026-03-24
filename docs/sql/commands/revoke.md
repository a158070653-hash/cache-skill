# REVOKE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_revoke

---

 

Caché SQL Reference

REVOKE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_order "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_rollback "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_COMMANDS "SQL Commands") \]  >  \[ [REVOKE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_revoke "REVOKE") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Removes privileges from a user or role.

Synopsis

REVOKE admin-privilege FROM grantee

REVOKE role FROM grantee

REVOKE \[GRANT OPTION FOR\] object-privilege 
     ON object-list FROM grantee \[CASCADE | RESTRICT\] \[AS grantor\]

REVOKE \[GRANT OPTION FOR\] SELECT ON CUBE\[S\] object-list 
FROM grantee

REVOKE column-privilege (column-list) ON table FROM grantee  \[CASCADE | RESTRICT\]

Arguments

admin-privilege

An administrative-level privilege or a comma-separated list of administrative-level privileges previously granted to be revoked. The available syspriv options include sixteen object definition privileges and four data modification privileges.

The object definition privileges are: %CREATE\_FUNCTION, %DROP\_FUNCTION, %CREATE\_METHOD, %DROP\_METHOD, %CREATE\_PROCEDURE, %DROP\_PROCEDURE, %CREATE\_QUERY, %DROP\_QUERY, %CREATE\_TABLE, %ALTER\_TABLE, %DROP\_TABLE, %CREATE\_VIEW, %ALTER\_VIEW, %DROP\_VIEW, %CREATE\_TRIGGER, %DROP\_TRIGGER. Alternatively, you can specify %DB\_OBJECT\_DEFINITION, which revokes all 16 object definition privileges.

The data modification privileges are the %NOCHECK, %NOINDEX, %NOLOCK, %NOTRIGGER privileges for INSERT, UPDATE, and DELETE operations.

grantee

A list of one or more users having SQL System Privileges, SQL Object Privileges, or Roles. Valid values are a comma-separated list of users or roles, or "\*". The asterisk (\*) specifies all currently defined users who do not have the %All role.

AS grantor

This clause permits you to revoke a privilege granted by another user by specifying the name of the original grantor. Valid grantor values are a user name, a comma-separated list of user names, or "\*". The asterisk (\*) specifies all currently defined users who are grantors. To use the AS grantor clause, you must have the %All role or the %Admin\_Secure resource.

role

A role or comma-separated list of roles whose privileges are being revoked from a user.

object-privilege

A basic-level privilege or comma-separated list of basic-level privileges previously granted to be revoked. The list may consist of one or more of the following: %ALTER, DELETE, SELECT, INSERT, UPDATE, EXECUTE, and REFERENCES. To revoke all privileges, use either "ALL \[PRIVILEGES\]" or "\*" as the value for this argument. Note that you can only revoke SELECT privilege from cubes, because this is the only grantable cubes privilege.

object-list

A comma-separated list of one or more [tables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_tables), [views](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_views), stored procedures, or cubes for which the object-privilege(s) are being revoked. You can use the SCHEMA keyword to specify revoking the object-privilege from all objects in the specified schema. You can use “\*” to specify revoking the object-privilege from all objects in the current namespace.

column-privilege

A basic-level privilege being revoked from one or more column-list listed columns. Available options are SELECT, INSERT, UPDATE, and REFERENCES.

column-list

A list of one or more column names, separated by commas and enclosed in parentheses.

table

The name of the [table](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_tables) or view that contains the column-list columns.

Description

The REVOKE statement revokes privileges that allow a user or role to perform specified tasks on specified tables, views, columns, or other entities. REVOKE can also revoke a role assignment from a user. REVOKE reverses the actions of the [GRANT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_grant) command; see that command for more details on privileges generally.

A privilege can only be revoked by the user who granted the privilege, or through a CASCADE operation (as described below).

You can revoke a role or privilege from a specified user, a list of users, or all users (using the \* syntax).

Because REVOKE prepares and executes quickly, and is generally run only once, Caché does not create a cached query for REVOKE in ODBC, JDBC, or Dynamic SQL.

A REVOKE completes successfully, even if no actual revoke can be performed (for example, the specified privilege was never granted or has already been revoked). However, if an error occurs during the REVOKE operation, SQLCODE is set to a negative number.

Revoking Roles

Roles can be granted or revoked via either the SQL GRANT and REVOKE commands, or via ^SECURITY Caché System Security. You can use REVOKE to revoke a role from a user or to revoke a role from another role. You cannot use Caché System Security to grant or revoke roles to other roles. The $ROLES special variable does not display roles granted to roles.

REVOKE can specify a single role, or a comma-separated list of roles to revoke. REVOKE can revoke one or more roles from a specified user (or role), a list of users (or roles), or all users (using the \* syntax).

The GRANT command can grant a non-existent role to a user. You can use REVOKE to revoke a non-existent role from an existing user. However, the role name must be specified using the same letter case that was used to grant the role.

If you attempt to revoke an existing role from a non-existent user or role, Caché issues an SQLCODE -118 error. If you are not the SuperUser, and you attempt to revoke a role that you don't own and don't have ADMIN OPTION for, Caché issues an SQLCODE -112 error.

Revoking Object Privileges

Object privileges give a user or role some right to a particular object. You revoke an object-privilege ON an object-list FROM a grantee. An object-list can specify one or more tables, views, stored procedures, or cubes in the current namespace. By using comma-separated lists, a single REVOKE statement can revoke multiple object privileges on multiple objects from multiple users and/or roles.

You can use the asterisk (\*) wildcard as the object-list value to revoke the object-privilege from all of the objects in the current namespace. For example, REVOKE SELECT ON \* FROM Deborah revokes this user’s SELECT privilege for all tables and views. REVOKE EXECUTE ON \* FROM Deborah revokes this user’s EXECUTE privilege for all non-hidden Stored Procedures.

You can use SCHEMA schema-name as the object-list value to revoke the object-privilege for all of the tables, views, and stored procedures in the named schema, in the current namespace. For example, REVOKE SELECT ON SCHEMA Sample FROM Deborah revokes this user’s SELECT privilege for all objects in the Sample schema. You can specify multiple schemas as a comma-separated list; for example, REVOKE SELECT ON SCHEMA Sample,Cinema FROM Deborah revokes SELECT privilege for all objects in both the Sample and the Cinema schemas.

You can revoke an object privilege from a user or from a role. If you revoke it from a role, a user that only had that privilege through the role no longer has the privilege. A user that no longer has a privilege can no longer execute an existing cached query that requires that object privilege.

When REVOKE revokes an object privilege, it completes successfully and sets SQLCODE to 0. If REVOKE does not perform an actual revoke (for example, the specified object privilege was never granted or has already been revoked), it completes successfully and sets SQLCODE to 100 (no more data). If an error occurs during the REVOKE operation, it sets SQLCODE to a negative number.

Cubes are SQL identifiers that are not qualified by a schema name. To specify a cubes object-list, you must specify the CUBE (or CUBES) keyword. Because cubes can only have SELECT privilege, you can only revoke SELECT privilege from a cube.

Object privileges can be revoked by any of the following:

*   The REVOKE command.
    
*   The [%SYSTEM.SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL) [RevokeObjPriv()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#RevokeObjPriv) method.
    
*   Via Caché System Security. Go to the Management Portal, select \[Home\] > \[Security Management\] > \[Users\] (or \[Home\] > \[Security Management\] > \[Roles\]) select Edit for the desired user or role, then select the SQL Tables or SQL Views tab. Select the desired Namespace from the drop-down list. Scroll down to the desired table, then click revoke to revoke privileges.
    

You can determine if the current user has a specified object privilege by invoking the [%CHECKPRIV](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_checkpriv) command. You can determine if a specified user has a specified table-level object privilege by invoking the [$SYSTEM.SQL.CheckPriv()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#CheckPriv) method.

Revoking Object Owner Privileges

If you revoke the privileges on an SQL object from the owner of the object, the owner will still implicitly have privileges on the object. In order to completely revoke all privileges on the object from the owner of the object, the object must be changed to specify a different owner or no owner.

Prior to Caché 2013.1, if you revoke the privileges on an SQL object from the owner of the object, the owner will cease to have privileges on the object. However, if the class is later recompiled, the privileges for the owner would be granted again.

Revoking Table-level and Column-level Privileges

REVOKE can be used to reverse the granting of table-level privileges or column-level privileges. A table-level privilege provides access to all of the columns in a table. A column-level privilege provides access to every specified column in the table. Granting a column-level privilege to all of the columns in a table is functionally equivalent to granting a table-level privilege. However, the two are not functionally identical. A column-level REVOKE can only revoke privileges granted at the column level. You cannot grant a table-level privilege to the table, then revoke this privilege at the column level for one or more columns. In this case, the REVOKE statement has no effect on granted privileges.

CASCADE or RESTRICT

Caché supports the optional CASCADE and RESTRICT keywords to specify REVOKE object-privilege behavior. If neither keyword is specified, the default is RESTRICT.

You can use CASCADE or RESTRICT to specify whether revoking an object-privilege or column-privilege from a user will also revoke that privilege from any other users that received it via the WITH GRANT OPTION. CASCADE revokes all such associated privileges. RESTRICT (the default) causes REVOKE to fail when an associated privilege is detected. Instead it sets the SQLCODE -126 error “REVOKE with RESTRICT failed”.

The use of these keywords is shown by the following example:

\--UserA
   GRANT Select ON MyTable TO UserB WITH GRANT OPTION

\--UserB
   GRANT Select ON MyTable TO UserC

\--UserA
   REVOKE Select ON MyTable FROM UserB
   \-- This REVOKE fails with SQLCODE -126

\--UserA
   REVOKE Select ON MyTable FROM UserB CASCADE
   \-- This REVOKE succeeds
   \-- It revokes this privilege from UserB and UserC

Note that CASCADE and RESTRICT have no effect on a view created by UserB that references MyTable.

Effect on Cached Queries

When you revoke a privilege or role, Caché updates all [cached queries](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_cachedqueries) on the system to reflect this change in privileges. However, when a namespace is inaccessible — for example, when an ECP connection to a database server is down — the REVOKE successfully completes but performs no operation on cached queries in that namespace. This is because REVOKE cannot update the cached queries in the unreachable namespace to revoke the privileges at the cached query level. No error is issued.

If the database server later comes up, the privileges for the cached queries in that namespace may be incorrect. It is advised that you purge cached queries in a namespace if a role or privilege might have been revoked while the namespace was not accessible.

Obsolete Privileges

At Caché Version 5.1 and all subsequent versions, REVOKE no longer supports the following general administrative privileges: %GRANT\_ANY\_PRIVILEGE, %CREATE\_USER, %ALTER\_USER, %DROP\_USER, %CREATE\_ROLE, %GRANT\_ANY\_ROLE, %DROP\_ANY\_ROLE. Control of these privileges is handled at the system level, rather than through SQL. These SQL privileges were available in prior versions of Caché, and may appear in existing code. An attempt to revoke one of these may execute, but it results not in the revoking of a privilege, but in attempting to revoke a role having this name from the specified user(s). Similarly, attempting to revoke %THRESHOLD now results in attempting to revoke a role having this name from the specified user(s).

Caché Security

The REVOKE command is a privileged operation. Prior to using REVOKE in embedded SQL, it is necessary to be logged in as a user with appropriate privileges. Failing to do so results in an SQLCODE -99 error (Privilege Violation).

Use the [$SYSTEM.Security.Login()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Security#Login) method to assign a user with appropriate privileges:

   DO $SYSTEM.Security.Login("\_SYSTEM","SYS")
   &sql(      )

You must have the %Service\_Login:Use privilege to invoke the $SYSTEM.Security.Login method. For further information, refer to [%SYSTEM.Security](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Security) in the InterSystems Class Reference.

Examples

The following embedded SQL example creates two users, creates a role, and assigns the role to the users. It then revokes the role from all users using the asterisk (\*) syntax. If the user or the role already exists, the CREATE statement issues an SQLCODE -118 error. If the user does not exist, the GRANT or REVOKE statement issues an SQLCODE -118 error. If the user exists but the role does not, the GRANT or REVOKE statement issues SQLCODE 100. If the user and role exist, the GRANT or REVOKE statement issues SQLCODE 0. This is true even when the granting or revoking of the role has already been done, of if you are attempting to revoke a role that was never granted.

   DO $SYSTEM.Security.Login("\_SYSTEM","SYS")
   &sql(CREATE USER User1 IDENTIFY BY fredpw)
   &sql(CREATE USER User2 IDENTIFY BY barneypw)
   WRITE !,"CREATE USER error code: ",SQLCODE
   &sql(CREATE ROLE workerbee)
   WRITE !,"CREATE ROLE error code: ",SQLCODE
   &sql(GRANT workerbee TO User1,User2)
   WRITE !,"GRANT role error code: ",SQLCODE
   &sql(REVOKE workerbee FROM \*)
   WRITE !,"REVOKE role error code: ",SQLCODE

 

In the following example, one user (Joe) grants a privilege and a different user (John) revokes that privilege, using the AS grantor clause:

   /\* User Joe \*/
   GRANT SELECT ON Sample.Person TO Michael

   /\* User John \*/
   REVOKE SELECT ON Sample.Person FROM Michael AS Joe

Note that John must have the %All role or the %Admin\_Secure resource.

See Also

*   SQL statements: [CREATE USER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createuser), [DROP USER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dropuser), [CREATE ROLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createrole), [DROP ROLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_droprole), [GRANT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_grant), [%CHECKPRIV](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_checkpriv)
    
*   “[Users, Roles, and Privileges](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_privileges)” chapter of Using Caché SQL
    
*   [SQLCODE error messages](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RERR_sql) listed in the Caché Error Reference
    
*   Caché ObjectScript: [$ROLES](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vroles) and [$USERNAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vusername) special variables
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_order "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_rollback "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_revoke.xml**