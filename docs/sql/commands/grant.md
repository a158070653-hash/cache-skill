# GRANT

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_grant

---

 

Caché SQL Reference

GRANT

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_from "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_groupby "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_COMMANDS "SQL Commands") \]  >  \[ [GRANT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_grant "GRANT") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Grants privileges to a user or role.

Synopsis

GRANT admin-privilege TO grantee \[WITH ADMIN OPTION\]
GRANT role TO grantee \[WITH ADMIN OPTION\] 

GRANT object-privilege ON object-list TO grantee \[WITH GRANT OPTION\] 
GRANT SELECT ON CUBE\[S\] object-list TO grantee \[WITH GRANT OPTION\] 
GRANT column-privilege (column-list) ON table TO grantee  \[WITH GRANT OPTION\]  

Arguments

grantee

A comma-separated list of one or more users or roles. Valid values are a list of users, a list of roles, "\*", or \_PUBLIC. The asterisk (\*) specifies all currently defined users who do not have the %All role. The \_PUBLIC keyword specifies all currently defined and yet-to-be-defined users.

admin-privilege

An administrative-level privilege or a comma-separated list of administrative-level privileges being granted. The list may consist of one or more of the following in any order:

%CREATE\_METHOD, %DROP\_METHOD, %CREATE\_FUNCTION, %DROP\_FUNCTION, %CREATE\_PROCEDURE, %DROP\_PROCEDURE, %CREATE\_QUERY, %DROP\_QUERY, %CREATE\_TABLE, %ALTER\_TABLE, %DROP\_TABLE, %CREATE\_VIEW, %ALTER\_VIEW, %DROP\_VIEW, %CREATE\_TRIGGER, %DROP\_TRIGGER

%DB\_OBJECT\_DEFINITION, which grants all 16 of the above privileges.

%NOCHECK, %NOINDEX, %NOLOCK, %NOTRIGGER privileges for INSERT, UPDATE, and DELETE operations.

role

A role or comma-separated list of roles whose privileges are being granted.

object-privilege

A basic-level privilege or comma-separated list of basic-level privileges being granted. The list may consist of one or more of the following: %ALTER, DELETE, SELECT, INSERT, UPDATE, EXECUTE, and REFERENCES. You can confer all table and view privileges using either "ALL \[PRIVILEGES\]" or “\*” as the argument value. Note that you can only grant SELECT privilege to CUBES.

object-list

A comma-separated list of one or more [tables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_tables), [views](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_views), stored procedures, or cubes for which the object-privilege(s) are being granted. You can use the SCHEMA keyword to specify granting the object-privilege to all objects in the specified schema. You can use “\*” to specify granting the object-privilege to all tables, or to all non-hidden Stored Procedures, in the current namespace. Note that a cubes object-list requires the CUBE (or CUBES) keyword, and can only be granted SELECT privilege.

column-privilege

A basic-level privilege being granted to one or more listed columns. Available options are SELECT, INSERT, UPDATE, and REFERENCES.

column-list

A list of one or more column names, separated by commas and enclosed in parentheses.

table

The name of the [table](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_tables) or view that contains the column-list columns.

Description

The GRANT command gives privileges to do specified tasks on specified tables, views, columns, or other entities to one or more specified users or roles. You can do the following basic operations:

*   Grant a privilege to a user.
    
*   Grant a privilege to a role.
    
*   Grant a role to a user.
    
*   Grant a role to a role, creating a hierarchy of roles.
    

If you grant a privilege to a user, the user can immediately exercise the privilege. If you grant a privilege to a role, users who have been granted the role can immediately exercise the privilege. If you revoke a privilege, the user immediately loses the privilege. A privilege is effectively granted to a user only once. Multiple users can grant the same privilege to a user multiple times, but a single REVOKE removes the privilege.

Privileges are granted on a per-namespace basis.

SQL privileges are only enforced through ODBC, JDBC, and Dynamic SQL ([%SQL.Statement](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_dynsql) and [%Library.ResultSet](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_dynsqlold)).

Because GRANT prepares and executes quickly, and is generally run only once, Caché does not create a cached query for GRANT in ODBC, JDBC, or Dynamic SQL. The expansion of \* is performed when the GRANT command is executed.

GRANT admin-privilege

SQL administrative (admin) privileges apply to users or roles. Any privilege that is not tied to any particular object (and thus is a general right for that user or role) is considered an admin privilege. These privileges are granted on a per-namespace basis for the current namespace.

The %DB\_OBJECT\_DEFINITION privilege grants all 16 of the data definition privileges. It does not grant %NOCHECK, %NOINDEX, %NOLOCK, and %NOTRIGGER privileges, which must be granted explicitly.

The %NOCHECK, %NOINDEX, %NOLOCK, and %NOTRIGGER privileges grant use of these options in the restriction clause of an [INSERT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_insert), [UPDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_update), [INSERT OR UPDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_insertorupdate), or [DELETE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_delete) statement. They have no effect on the use of the %NOINDEX keyword as a preface to a predicate condition. Because [TRUNCATE TABLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_truncatetable) performs a delete of all of the rows from a table with %NOTRIGGER behavior, you must have %NOTRIGGER privilege in order to run TRUNCATE TABLE. You must have the appropriate %NOCHECK, %NOINDEX, %NOLOCK, or %NOTRIGGER privilege to use that restriction when preparing an INSERT, UPDATE, INSERT OR UPDATE, or DELETE statement.

If the specified admin privilege is not a valid privilege name (for example, due to a spelling error), Caché completes successfully, issuing an SQLCODE 100 (reached end of data); Caché does not check if the specified user (or role) exists. If the specified admin privilege is valid, but the specified user (or role) does not exist, Caché issues an SQLCODE -118 error.

GRANT role

This form of GRANT assigns a user to a specified role. You can also assign a role to another role. If the specified role that receives the assignment does not exist, Caché issues an SQLCODE 100 (reached end of data). If the specified user (or role) that is assigned to a role does not exist, Caché issues an SQLCODE -118 error. If you are not the SuperUser, and you are attempting to grant a role that you don't own and don't have ADMIN OPTION for, Caché issues an SQLCODE -112 error.

Roles are created using the [CREATE ROLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createrole) statement. If the role name is a [delimited identifier](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_identifiers#GSQL_identifiers_delimitedids), you must enclose it in quotation marks when assigning to it.

Roles can be granted or revoked via either the SQL GRANT and REVOKE commands, or via Caché System Security:

*   Go to the Management Portal, select \[Home\] > \[Security Management\] > \[Users\], select Edit for the desired user, then select the Roles tab to assign (or unassign) the user to one or more roles.
    
*   Go to the Management Portal, select \[Home\] > \[Security Management\] > \[Roles\], select Edit for the desired role, then select the Assigned To tab to assign (or unassign) the role to one or more roles. Note that the Caché ObjectScript [$ROLES](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vroles) special variable does not display roles granted to roles.
    

GRANT object-privilege

Object privileges give a user or role some right to a particular object. You grant an object-privilege ON an object-list TO a grantee. An object-list can specify one or more tables, views, stored procedures, or cubes in the current namespace. By using comma-separated lists, a single GRANT statement can grant multiple object privileges on multiple objects to multiple users and/or roles.

The following are the available object-privilege values:

*   The %ALTER and DELETE privileges grant access to table or view definitions.
    
*   The SELECT, INSERT, UPDATE, DELETE, and REFERENCES privileges grant access to table data.
    
*   The EXECUTE privilege grants access to stored procedures. This privilege is required to execute a stored procedure or to call a user-defined SQL function in a query. For example, SELECT Field1,MyFunc() FROM SQLUser.MyTable requires SELECT privilege on SQLUser.MyTable and EXECUTE privilege on the SQLUser.MyFunc procedure.
    
*   The ALL PRIVILEGES privilege grants all table and view privileges; it does not grant the EXECUTE privilege.
    

You can use the asterisk (\*) wildcard as the object-list value to grant the object-privilege to all of the objects in the current namespace. For example, GRANT SELECT ON \* TO Deborah grants this user SELECT privilege for all tables and views. GRANT EXECUTE ON \* TO Deborah grants this user EXECUTE privilege for all non-hidden Stored Procedures.

You can use SCHEMA schema-name as the object-list value to grant the object-privilege to all of the tables, views, and stored procedures in the named schema, in the current namespace. For example, GRANT SELECT ON SCHEMA Sample TO Deborah grants this user SELECT privilege for all objects in the Sample schema. This includes all objects that will be defined in this schema in the future. You can specify multiple schemas as a comma-separated list; for example, GRANT SELECT ON SCHEMA Sample,Cinema TO Deborah grants SELECT privilege for all objects in both the Sample and the Cinema schemas.

Cubes are SQL identifiers that are not qualified by a schema name. To specify a cubes object-list, you must specify the CUBE (or CUBES) keyword. You can only grant SELECT privilege to a cube.

The following example demonstrates the granting of the UPDATE privilege.

   DO $SYSTEM.Security.Login("\_SYSTEM","SYS")
   &sql(CREATE USER Deborah IDENTIFY BY birdpw)
   WRITE !,"CREATE USER error code: ",SQLCODE
   &sql(CREATE TABLE SQLUSER.T1 
           (Field1 INT, Field2 INT, PRIMARY KEY (Field1)))
   WRITE !,"CREATE TABLE error code: ",SQLCODE
   &sql(GRANT UPDATE ON SQLUSER.T1 TO Deborah)
   WRITE !,"GRANT error code: ",SQLCODE

Privileges can only be granted explicitly to a table, view, or stored procedure that already exists. If the specified object does not exist, Caché issues an SQLCODE -30 error. You can, however, grant privileges to a schema, which grant privileges both to all existing objects in that schema and to all future objects in that schema that did not exist when the privilege was granted.

If the owner of a table is \_PUBLIC, users do not need to be granted object privileges to access the table.

If the specified user does not exist, Caché issues an SQLCODE -118 error. If the specified object privilege has already been granted, Caché issues an SQLCODE 100 (reached end of data).

Object privileges can be granted or revoked by any of the following:

*   The GRANT and REVOKE commands.
    
*   The [%SYSTEM.SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL) [GrantObjPriv()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#GrantObjPriv) and [RevokeObjPriv()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#RevokeObjPriv) methods.
    
*   Via Caché System Security. Go to the Management Portal, select \[Home\] > \[Security Management\] > \[Users\] (or \[Home\] > \[Security Management\] > \[Roles\]) select Edit for the desired user or role, then select the SQL Tables or SQL Views tab. Select the desired Namespace from the drop-down list. Then select the Add Tables or Add Views button. In the displayed window, choose a schema, select one or more tables, and assign privileges.
    

You can determine if the current user has a specified object privilege by invoking the [%CHECKPRIV](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_checkpriv) command. You can determine if a specified user has a specified table-level object privilege by invoking the [$SYSTEM.SQL.CheckPriv()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#CheckPriv) method.

Object Owner Privileges

The owner of a table, view, or procedure always has all SQL privileges implicitly on the SQL object. The owner of the object has privileges on the object in all namespaces to which the object is mapped. Prior to Caché 2013.1, the owner of the object would only have privileges on the object in the namespace the class was compile in. You must recompile a pre-2013.1 object in order to have privileges on the object in all namespaces.

GRANT column-privilege

Column privileges give a user or role a specified privilege to a specified list of columns on a specified table or view. This permits you to allow access to some table columns and not to other columns of the same table. This gives more specific access control than the GRANT object-privilege option, which defines privileges for an entire table or view. When granting privileges to a grantee, you should grant either table-level privilege or column-level privileges for a table, but not both. The SELECT, INSERT, UPDATE, and REFERENCE privileges can be used to grant access to data in individual columns.

A user having a SELECT, INSERT, UPDATE, or REFERENCE object-privilege on a table WITH GRANT OPTION can grant to other users a column-privilege of the same type for columns of that table.

You can specify a single column, or a comma-separated list of columns. The column-list must be enclosed in parentheses. Column names can be specified in any order, and duplication is permitted. Granting a column privilege to a column that already has that privilege has no effect.

The following example grants the UPDATE privilege for two columns:

GRANT UPDATE(Name,FavoriteColors) ON Sample.Person TO Deborah

You can grant column privileges on a table or a view. You can grant column privileges to any type of grantee, including a list of users, a list of roles, \*, and \_PUBLIC. However, you cannot use the asterisk (\*) wildcard for privileges, field names, or table names.

If a user inserts a new record into a table, data is inserted into only those fields for which column privileges have been granted. All other data columns are set to either the defined column default value, or to NULL if there is no defined default value. You cannot grant column-level INSERT or UPDATE privileges to the RowID and Identity columns. Upon INSERT, Caché SQL automatically provides a RowID and (if needed) an Identity column value.

Column-level privileges can be granted or revoked via either the SQL GRANT and REVOKE commands, or via Caché System Security. Go to the Management Portal, select \[Home\] > \[Security Management\] > \[Users\] (or \[Home\] > \[Security Management\] > \[Roles\]), select Edit for the desired user or role, then select the SQL Tables or SQL Views tab. Select the desired Namespace from the drop-down list. Then select the Add Columns button. In the displayed window, choose a schema, choose a table, select one or more columns, and assign privileges.

Granting Multiple Privileges

You can use a single GRANT statement to specify the following combinations of privileges:

*   One or more roles.
    
*   One or more table-level privileges and one or more column-level privileges. To specify multiple table-level and column-level privileges, the privilege must immediately precede a column-list to grant a column-level privilege. Otherwise, it grants a table-level privilege.
    
*   One or more admin-privileges. You cannot include admin-privileges and role names or object privileges in the same GRANT statement. Attempting to do so results in an SQLCODE -1 error.
    

The following example grants Deborah table-level SELECT and UPDATE privileges, and column-level INSERT privileges:

GRANT SELECT,UPDATE,INSERT(Name,FavoriteColors) ON Sample.Person TO Deborah

The following example grants Deborah column-level SELECT, INSERT, and UPDATE privileges:

GRANT SELECT(Name,FavoriteColors),INSERT(Name,FavoriteColors),UPDATE(FavoriteColors) ON Sample.Person TO Deborah

The WITH GRANT OPTION Clause

The owner of an object automatically holds all privileges on that object. The GRANT statement’s TO clause specifies the users or roles to whom to access is being granted. After using the TO option to specify the grantee, you may optionally specify the WITH GRANT OPTION keyword clause to allow the grantee(s) to also be able to grant the same privileges to other users. You can use the WITH GRANT OPTION keyword clause with object privileges or column privileges. The REVOKE command with CASCADE can be used to undo this cascading series of granted privileges.

For instance, you can give the user Chris %ALTER, SELECT, and INSERT privileges on the EMPLOYEES table with the following command:

GRANT %ALTER, SELECT, INSERT
     ON EMPLOYEES
     TO Chris

To also give Chris the ability to give these privileges to other users, the GRANT command includes the WITH GRANT OPTION clause:

GRANT %ALTER, SELECT, INSERT
     ON EMPLOYEES
     TO Chris WITH GRANT OPTION

You can find out the results of a GRANT statement using the [%SQLCatalogPriv.SQLUsers()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SQLCatalogPriv#SQLUsers) method call.

Granting privileges to a schema WITH GRANT OPTION allow the grantee(s) to be able to grant the same schema privileges to other users. However, it does not allow the grantee to grant a privilege on a specified object within that schema, unless the user has been explicitly granted the privilege on that particular object WITH GRANT OPTION. This is shown in the following example:

*   UserA and UserB start with no privileges.
    
*   You grant UserA SELECT privilege on schema Sample WITH GRANT OPTION.
    
*   UserA can grant SELECT privilege on schema Sample to UserB.
    
*   UserA cannot grant SELECT privilege on table Sample.Person to UserB.
    

The WITH ADMIN OPTION Clause

The WITH ADMIN OPTION clause grants the grantee the right to grant the same privileges it received to others. To grant a system privilege, you must have been granted the system privilege WITH ADMIN OPTION.

You may grant a role if either the role has been granted to you WITH ADMIN OPTION, or if you have the %Admin\_Secure:"U" resource.

A grant WITH ADMIN OPTION supersedes a previous grant of the same privilege(s) without this option. Thus, if you grant a user a privilege without WITH ADMIN OPTION, and then grant the same privilege to the user WITH ADMIN OPTION, the user has the WITH ADMIN OPTION rights. However, a grant without the WITH ADMIN OPTION does not supersede a previous grant of the same privilege(s) with this option. To remove WITH ADMIN OPTION rights from a privilege, you must revoke the privilege and then re-grant the privilege without this clause.

Exporting Privileges

You can export privileges using the [$SYSTEM.SQL.Export()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#Export) method. When you specify a table in this method, Caché exports both all table-level privileges and all column-level privileges granted for that table. For further details, refer to the InterSystems Class Reference.

Obsolete Privileges

At Caché Version 5.1 and all subsequent versions, GRANT no longer supports the following general administrative privileges: %GRANT\_ANY\_PRIVILEGE, %CREATE\_USER, %ALTER\_USER, %DROP\_USER, %CREATE\_ROLE, %GRANT\_ANY\_ROLE, %DROP\_ANY\_ROLE. Control of these privileges is handled at the system level, rather than through SQL. These SQL privileges were available in prior versions of Caché, and may appear in existing code. An attempt to grant one of these to a user may execute, but it results not in the granting of a privilege, but in the granting of a role having this name.

Caché Security

Before using GRANT in embedded SQL, it is necessary to be logged in as a user with appropriate privileges. Failing to do so results in an SQLCODE -99 error (Privilege Violation). Use the [$SYSTEM.Security.Login()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Security#Login) method to assign a user with appropriate privileges:

   DO $SYSTEM.Security.Login("\_SYSTEM","SYS")
   &sql(      )

You must have the %Service\_Login:Use privilege to invoke the $SYSTEM.Security.Login method. For further information, refer to [%SYSTEM.Security](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Security) in the InterSystems Class Reference.

Enforcement of Privileges

SQL privileges are only enforced through ODBC, JDBC, and Dynamic SQL ([%SQL.Statement](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_dynsql) and [%Library.ResultSet](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_dynsqlold)).

The enforcement of privileges depends upon the setting of the SQL Security Enabled configuration option, as follows:

*   The [$SYSTEM.SQL.SetSQLSecurity()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#SetSQLSecurity) method call. To determine the current setting, call [$SYSTEM.SQL.CurrentSettings()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#CurrentSettings), which displays a SQL Security ON: setting.
    
*   Go to the Management Portal, select \[Home\] > \[Configuration\] > \[General SQL Settings\]. View the current setting of SQL Security Enabled.
    

The default is “Yes” (1). When “Yes”, a user can only perform actions for which that user has been granted privilege. This is the recommended setting for this option.

If this option is set to “No” (0), SQL Security is disabled for any new process started after changing this setting. This means privilege-based table/view security is suppressed. You can create a table without specifying a user. In this case, the Management Portal assigns “\_SYSTEM” as user, and embedded SQL assigns "" (the empty string) as user. Any user can perform actions on a table or view even if that user has no privileges to do so. For further details, refer to [SQL configuration settings](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RACS_Settings#RACS_Category_SQL) described in Caché Advanced Configuration Settings Reference.

Examples

The following example creates a user, creates a role, and then assigns the role to the user. If the user or role already exists, it issues SQLCODE -118 error. If the assignment of the privilege or the role has already been done, no error is issued (SQLCODE = 0).

   DO $SYSTEM.Security.Login("\_SYSTEM","SYS")
   &sql(CREATE USER User1 IDENTIFY BY fredpw)
   WRITE !,"CREATE USER error code: ",SQLCODE
   &sql(CREATE ROLE workerbee)
   WRITE !,"CREATE ROLE error code: ",SQLCODE
   &sql(GRANT %CREATE\_TABLE TO workerbee)
   WRITE !,"GRANT privilege error code: ",SQLCODE
   &sql(GRANT workerbee TO User1)
   WRITE !,"GRANT role error code: ",SQLCODE

 

The following example shows the assignment of multiple privileges. It creates a user and creates two roles. A single GRANT statement assigns these roles and a list of admin-privileges to the user. If the user or a role already exists, it issues SQLCODE -118 error. If the assignment of a privilege or a role has already been done, no error is issued (SQLCODE = 0).

   DO $SYSTEM.Security.Login("\_SYSTEM","SYS")
   &sql(CREATE USER User1 IDENTIFY BY fredpw)
   WRITE !,"CREATE USER error code: ",SQLCODE
   &sql(CREATE ROLE workerbee)
   WRITE !,"CREATE ROLE 1 error code: ",SQLCODE
   &sql(CREATE ROLE drone)
   WRITE !,"CREATE ROLE 2 error code: ",SQLCODE
   &sql(GRANT workerbee,drone,%CREATE\_TABLE,%DROP\_TABLE TO User1)
   WRITE !,"GRANT roles & privileges error code: ",SQLCODE

 

The following example grants all seven basic privileges ON all tables in the current namespace TO all currently defined users who do not have the %All role:

   DO $SYSTEM.Security.Login("\_SYSTEM","SYS")
    &sql(GRANT \* ON \* TO \*)

See Also

*   [%CHECKPRIV](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_checkpriv) [REVOKE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_revoke)
    
*   [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select) [INSERT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_insert) [DELETE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_delete) [UPDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_update)
    
*   [CREATE USER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createuser) [CREATE ROLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createrole)
    
*   “[Users, Roles, and Privileges](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_privileges)” chapter of Using Caché SQL
    
*   [CREATE FUNCTION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createfunction) [CREATE METHOD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createmethod) [CREATE PROCEDURE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createprocedure) [CREATE QUERY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createquery)
    
*   [CREATE TABLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable) [CREATE VIEW](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createview) [CREATE TRIGGER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtrigger)
    
*   [SQLCODE error messages](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RERR_sql) listed in the Caché Error Reference
    
*   Caché ObjectScript: [$ROLES](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vroles) and [$USERNAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vusername) special variables
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_from "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_groupby "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_grant.xml**