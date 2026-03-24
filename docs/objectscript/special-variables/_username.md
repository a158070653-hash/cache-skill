# $USERNAME

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_vusername

---

 

Caché ObjectScript Reference

$USERNAME

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vtlevel "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vx "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché ObjectScript Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS "Caché ObjectScript Reference") \]  >  \[ [Caché ObjectScript Special Variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_VARIABLES "Caché ObjectScript Special Variables") \]  >  \[ [$USERNAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vusername "$USERNAME") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Contains the username for the current process.

Synopsis

$USERNAME

Description

$USERNAME contains the username for the current process. This can be in one of two forms:

*   The name of the current user; for example: Mary. This value is returned if multiple security domains are not allowed.
    
*   The name and system address of the current user; for example: Mary@jupiter. This value is returned if multiple security domains are allowed.
    

To allow multiple security domains, go to the Management Portal, select \[Home\] > \[Security Management\] > \[System Security Settings\] > \[System-wide Security Parameters\]. Select the Allow multiple security domains check box. Changes to this setting apply to new invoked processes; changing it does not affect the value returned by the current process.

You cannot use the SET command or the NEW command to modify this value. However, NEW $ROLES also stacks the current $USERNAME value.

Commonly, the $USERNAME value is the username specified at connection time. However, if unauthenticated access is permitted, a user terminal or an ODBC client may connect to Caché without specifying a username. In this case, $USERNAME contains the string “UnknownUser”.

When a process is created using the JOB command, it inherits the same $USERNAME and $ROLES values as its parent process.

A username can be created using the SQL CREATE USER statement and deleted using the SQL DROP USER statement. A user password can be changed using the SQL ALTER USER statement. A user can have roles assigned to it, either by using the SQL GRANT statement, or by using system utilities to add a role to the user. You can access the list of roles assigned to the current process with the $ROLES special variable. A role can be revoked from a user using the SQL REVOKE statement.

$USERNAME is used in Caché SQL as the USER, CURRENT\_USER, and SESSION\_USER default field values.

You can return the username for the current process, or for a specified process, by invoking the [$SYSTEM.Process.UserName()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Process#UserName) method.

Examples

The following example returns the username for the current process.

   WRITE $USERNAME

 

The following example returns the domain name for the current process.

   WRITE $PIECE($USERNAME,"@",2)

 

See Also

*   Caché ObjectScript [$ROLES](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vroles) special variable
    
*   Caché SQL: [CREATE TABLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtable) [CREATE USER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createuser) [DROP USER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dropuser) [ALTER USER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_alteruser) [GRANT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_grant) [REVOKE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_revoke) [%CHECKPRIV](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_checkpriv)
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vtlevel "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vx "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RCOS\_vusername.xml**