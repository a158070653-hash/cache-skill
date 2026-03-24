# DROP DATABASE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_dropdatabase

---

 

Caché SQL Reference

DROP DATABASE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_distinct "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dropfunction "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_COMMANDS "SQL Commands") \]  >  \[ [DROP DATABASE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dropdatabase "DROP DATABASE") \]

Go to: Description Example See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Deletes a database (namespace).

Synopsis

DROP DATABASE dbname \[RETAIN\_FILES\]

Arguments

dbname

The name of the database (namespace) to be deleted.

RETAIN\_FILES

Optional — If specified, the physical database files (CACHE.DAT files) will not be deleted. The default is to delete the .DAT files along with the namespace and the other database entities.

Description

The DROP DATABASE command deletes a namespace and its associated database.

The specified dbname is the name of the namespace and the directory that contains the corresponding database files. Specify dbname as an [identifier](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_identifiers). Namespace names are not case-sensitive. If the specified dbname namespace does not exist, Caché issues an SQLCODE -340 error.

The DROP DATABASE command is a privileged operation. Prior to using DROP DATABASE, it is necessary to be logged in as a user with the %Admin\_Manage resource. The user must also have READ permission on the resource for the routines and global's database definitions. Failing to do so results in an SQLCODE -99 error (Privilege Violation).

Use the [$SYSTEM.Security.Login()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Security#Login) method to assign a user with appropriate privileges:

   DO $SYSTEM.Security.Login("\_SYSTEM","SYS")
   &sql(      )

You must have the %Service\_Login:Use privilege to invoke the $SYSTEM.Security.Login method. For further information, refer to [%SYSTEM.Security](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Security) in the InterSystems Class Reference.

DROP DATABASE cannot be used to drop a system namespace, regardless of privileges. Attempting to do so results in an SQLCODE -342 error.

DROP DATABASE cannot be used to drop the namespace that you are currently using or connected to. Attempting to do so results in an SQLCODE -344 error.

You can also delete a namespace from the Management Portal. Select \[Home\] > \[Configuration\] > \[Namespaces\] to list the existing namespaces. Click the Delete button for the namespace you wish to delete.

RETAIN\_FILES

If you specify this option, the physical file structure is retained; the database and its associated namespace is removed. After performing this operation, a subsequent attempt to use dbname results in the following:

*   DROP DATABASE without RETAIN\_FILES cannot remove this physical file structure. Instead, it results in an SQLCODE -340 error (Database not found).
    
*   DROP DATABASE with RETAIN\_FILES also results in an SQLCODE -340 error (Database not found).
    
*   CREATE DATABASE cannot create a new database with the same name. Instead, it results in an SQLCODE -341 error (Cannot create database file for database).
    
*   Attempting to use this namespace results in a <NAMESPACE> error.
    

Example

The following example deletes a namespace and its associated database (in this case 'C:\\InterSystems\\Cache\\mgr\\DocTestDB'). It retains the physical database files:

CREATE DATABASE DocTestDB ON DIRECTORY 'C:\\InterSystems\\Cache142\\mgr\\DocTestDB'

DROP DATABASE DocTestDB RETAIN\_FILES

See Also

*   [CREATE DATABASE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createdatabase) command
    
*   [USE DATABASE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_usedatabase) command
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_distinct "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dropfunction "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_dropdatabase.xml**