# %CHECKPRIV

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_checkpriv

---

 

Caché SQL Reference

%CHECKPRIV

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_case "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_close "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_COMMANDS "SQL Commands") \]  >  \[ [%CHECKPRIV](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_checkpriv "%CHECKPRIV") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Checks whether the user holds a specified privilege.

Synopsis

%CHECKPRIV \[GRANT OPTION FOR | ADMIN OPTION FOR\] syspriv \[,syspriv\]

%CHECKPRIV \[GRANT OPTION FOR\] objpriv ON object

%CHECKPRIV column-privilege (column-list) ON table

Arguments

GRANT OPTION FOR

Optional — This keyword phrase specifies checking whether the current user holds the WITH GRANT OPTION privilege on the specified privilege(s). A %CHECKPRIV with this option does not check whether the user holds the specified privilege(s) itself.

ADMIN OPTION FOR

Optional — This keyword phrase specifies checking whether the current user can grant the specified system privilege(s) to other users or roles. A %CHECKPRIV with this option does not check whether the user holds the specified privilege(s) itself.

syspriv

A system privilege, or a comma-separated list of system privileges. The available syspriv options include sixteen object definition privileges and four data modification privileges.

The object definition privileges are: %CREATE\_FUNCTION, %DROP\_FUNCTION, %CREATE\_METHOD, %DROP\_METHOD, %CREATE\_PROCEDURE, %DROP\_PROCEDURE, %CREATE\_QUERY, %DROP\_QUERY, %CREATE\_TABLE, %ALTER\_TABLE, %DROP\_TABLE, %CREATE\_VIEW, %ALTER\_VIEW, %DROP\_VIEW, %CREATE\_TRIGGER, %DROP\_TRIGGER. Alternatively, you can specify %DB\_OBJECT\_DEFINITION, which tests all 16 object definition privileges.

The data modification privileges are the %NOCHECK, %NOINDEX, %NOLOCK, %NOTRIGGER privileges for INSERT, UPDATE, and DELETE operations.

objpriv

An object privilege associated with a specified object. The available options are: %ALTER, DELETE, SELECT, INSERT, UPDATE, EXECUTE, and REFERENCES.

object

The name of the object for which the objpriv is being checked.

column-privilege

A column-level privilege associated with one or more listed columns. Available options are SELECT, INSERT, UPDATE, and REFERENCES.

column-list

A list of one or more column names for which privilege assignment is being checked, separated by commas and enclosed in parentheses. A space may be included or omitted between the column-privilege name and the opening parenthesis.

table

The name of the [table](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_tables) or view that contains the column-list columns.

Description

%CHECKPRIV can be used in two ways:

*   To determine if the current user holds a system privilege of a specified type.
    
*   To determine if the current user holds a user privilege of a specified type on a specified object. These objects can include table-level privileges on tables or views, column-level privileges on specified columns, and privileges on stored procedures.
    

%CHECKPRIV enables you to check whether a privilege is held. It does not enforce privileges. At runtime privileges are enforced through ODBC/JDBC and through Dynamic SQL (for example, through the Management Portal Execute SQL Statement). Privileges are not enforced for Embedded SQL. %CHECKPRIV is primarily used for Embedded SQL.

If the user holds the specified privilege, %CHECKPRIV sets SQLCODE=0. If the user does not hold the specified privilege, %CHECKPRIV sets SQLCODE=100.

Because %CHECKPRIV requires access to the SQLCODE 100 value (an SQLCODE status value, not an SQLCODE error value) to determine its result, it cannot be directly used by JDBC and other clients that can only distinguish error or no error status.

Because %CHECKPRIV prepares and executes quickly, and is generally run only once, Caché does not create a cached query for %CHECKPRIV.

You can determine if a specified user has a specified table-level privilege by invoking the [$SYSTEM.SQL.CheckPriv()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#CheckPriv) method.

Embedded SQL and Privileges

Privileges are not automatically checked or enforced for [Embedded SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql). Therefore, an Embedded SQL program should (in most cases) call %CHECKPRIV before attempting a privileged operation, such as an update:

  SET name\="fred",age\=99
  SET SQLCODE\=""
  &sql(%CHECKPRIV UPDATE ON Sample.Person)
  IF SQLCODE\=100 {
     WRITE !,"No UPDATE privilege"
     QUIT }
  ELSEIF SQLCODE < 0 {
     WRITE !,"Unexpected SQL error: ",SQLCODE
     QUIT }
  ELSE { 
     WRITE !,"Proceeding with UPDATE" }
  &sql(UPDATE Sample.Person SET Name\=:name,Age\=:age)
  IF SQLCODE\=0 { WRITE !,"UPDATE successful" }
  ELSE { WRITE "UPDATE error SQLCODE=",SQLCODE }

Examples

The following Embedded SQL example checks whether the current user holds a specific object privilege for a specific table:

  &sql(%CHECKPRIV UPDATE ON Sample.Person)
  IF SQLCODE\=0 {WRITE "Have privilege"}
  ELSEIF SQLCODE\=100 {WRITE "Do not have privilege"}
  ELSE {WRITE "Unexpected SQLCODE error: ",SQLCODE}

 

The following Embedded SQL example checks whether the current user holds system privileges on the three table operations for all tables:

  &sql(%CHECKPRIV %CREATE\_TABLE,%ALTER\_TABLE,%DROP\_TABLE)
  IF SQLCODE\=0 {WRITE "Have privileges"}
  ELSEIF SQLCODE\=100 {WRITE "Do not have one or more privileges"}
  ELSE {WRITE "Unexpected SQLCODE error: ",SQLCODE}

 

The following Embedded SQL example checks whether the current user holds all 16 object definition privileges. The SQLCODE value is set to either 0 (holds all 16 privileges) or 100 (does not hold one or more of the 16 privileges):

  &sql(%CHECKPRIV %DB\_OBJECT\_DEFINITION)
  IF SQLCODE\=0 {WRITE "Have all privileges"}
  ELSEIF SQLCODE\=100 {WRITE "Do not have one or more privileges"}
  ELSE {WRITE "Unexpected SQLCODE error: ",SQLCODE}

 

The following Embedded SQL example checks whether the current user can grant the %CREATE\_TABLE privilege to other users or roles:

  &sql(%CHECKPRIV ADMIN OPTION FOR %CREATE\_TABLE)
  IF SQLCODE\=0 {WRITE "Have admin option on privilege"}
  ELSEIF SQLCODE\=100 {WRITE "Do not have admin option on privilege"}
  ELSE {WRITE "Unexpected SQLCODE error: ",SQLCODE}

 

The following Embedded SQL example checks whether the current user holds the specified column-level privileges. Following the name of the privilege, specify the name of a column (or a comma-separated list of columns) in parentheses:

  &sql(%CHECKPRIV UPDATE(Name,Age) ON Sample.Person)
  IF SQLCODE\=0 {WRITE "Have privilege on all specified columns"}
  ELSEIF SQLCODE\=100 {WRITE "Do not have privilege on one or more specified columns"}
  ELSE {WRITE "Unexpected SQLCODE error: ",SQLCODE}

 

See Also

*   SQL statements: [GRANT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_grant) [REVOKE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_revoke)
    
*   “[Users, Roles, and Privileges](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_privileges)” chapter of Using Caché SQL
    
*   Caché ObjectScript: [$ROLES](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vroles) and [$USERNAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vusername) special variables
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_case "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_close "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_checkpriv.xml**