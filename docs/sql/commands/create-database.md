# CREATE DATABASE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_createdatabase

---

 

Caché SQL Reference

CREATE DATABASE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_commit "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createfunction "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_COMMANDS "SQL Commands") \]  >  \[ [CREATE DATABASE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createdatabase "CREATE DATABASE") \]

Go to: Description See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Creates a database (namespace).

Synopsis

CREATE DATABASE dbname \[ON DIRECTORY pathname\]
   \[WITH \[ENCRYPTED\_DB\]  \[GLOBAL\_JOURNAL\_STATE \[=\] {YES | NO}\] \]

Arguments

dbname

The name of the database (namespace) to be created.

pathname

Optional — The root pathname location for the databases, specified as a quoted string. The C and D directories are created as subdirectories of this root path. The default is to create the database in the mgr directory.

WITH ENCRYPTED\_DB

Optional — Specifies whether or not the database is encrypted. The default is not encrypted.

WITH GLOBAL\_JOURNAL\_STATE

Optional — Specifies whether or not the database is journaled. YES specifies that the database is journaled (which is recommended). NO specifies that the database is not journaled. The equal sign (=) is optional. The default is journaled.

Description

The CREATE DATABASE command creates a namespace and two associated databases. This allows you to create a namespace within SQL.

The specified dbname is the name of the created namespace and the directory that contains the corresponding database files. Namespace names are not case-sensitive. A dbname follows the naming conventions for an [SQL identifier](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_identifiers), with the following additional restrictions:

*   An underscore (\_) character is not permitted as the first character of dbname (but may be used elsewhere within the name). The @, #, and $ characters are not permitted in dbname. Attempting to include these invalid characters in dbname generates an SQLCODE -343 error.
    
*   A hyphen (-) character is not permitted in dbname (hyphen is not a valid SQL identifier character). However, a namespace name created by other means can include a hyphen character.
    
*   A dbname cannot be longer than 63 characters; specifying a longer dbname generates an SQLCODE -400 fatal error with the appropriate %msg.
    

If the specified dbname namespace already exists, Caché issues an SQLCODE -341 error.

You can specify neither, either, or both WITH options: ENCRYPTED\_DB and/or GLOBAL\_JOURNAL\_STATE. If you specify both, they are separated by a space, as follows: WITH ENCRYPTED\_DB GLOBAL\_JOURNAL\_STATE=NO.

By default, CREATE DATABASE creates two databases in the mgr directory with the dbname name subdirectory containing two subdirectories, C (code) and D (data). Each of these subdirectories contains a CACHE.DAT file, a cache.lck file, and an empty stream folder. For example, on a Windows system, CREATE DATABASE Barney would create the namespace BARNEY and the following database files:

C:\\InterSystems\\Cache\\mgr\\Barney\\C containing CACHE.DAT, cache.lck, stream folder
C:\\InterSystems\\Cache\\mgr\\Barney\\D containing CACHE.DAT, cache.lck, stream folder

The C (code) directory is used for the namespace routines database. The D (data) directory is used for the namespace globals database.

The optional ON DIRECTORY pathname clause allows you to specify a different location for the database files, rather than a directory with the same name as the namespace. For example:

CREATE DATABASE Flintstone ON DIRECTORY 'C:\\InterSystems\\Cache\\mgr\\Fred'

If you specify a pathname that already exists, Caché issues an SQLCODE -341 error.

The CREATE DATABASE command is a privileged operation. Prior to using CREATE DATABASE, it is necessary to be logged in as a user with the %Admin\_Manage resource. Failing to do so results in an SQLCODE -99 error (Privilege Violation).

Use the [$SYSTEM.Security.Login()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Security#Login) method to assign a user with appropriate privileges:

   DO $SYSTEM.Security.Login("\_SYSTEM","SYS")
   &sql(      )

You must have the %Service\_Login:Use privilege to invoke the $SYSTEM.Security.Login method. For further information, refer to [%SYSTEM.Security](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Security) in the InterSystems Class Reference.

You can also create a namespace from the Management Portal. Select \[Home\] > \[Configuration\] > \[Namespaces\] to list the existing namespaces. At the top of this table of existing namespaces you can click Create New Namespace.

See Also

*   [DROP DATABASE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dropdatabase) command
    
*   [USE DATABASE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_usedatabase) command
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_commit "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createfunction "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_createdatabase.xml**