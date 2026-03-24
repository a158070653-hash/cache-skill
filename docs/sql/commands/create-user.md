# CREATE USER

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_createuser

---

 

Caché SQL Reference

CREATE USER

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtrigger "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createview "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_COMMANDS "SQL Commands") \]  >  \[ [CREATE USER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createuser "CREATE USER") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Creates a user account.

Synopsis

CREATE USER user-name IDENTIFY BY password

CREATE USER user-name IDENTIFIED BY password

Arguments

user-name

The name of the user to be created. The name is an [identifier](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_identifiers) with a maximum of 128 characters. It can contain Unicode letters. user-name is not case-sensitive. For further details see the “Identifiers” chapter of Using Caché SQL.

password

The password for this user. A password must be at least 3 characters, and cannot exceed 32 characters. Passwords are case-sensitive. Passwords can contain Unicode characters.

Description

The CREATE USER command creates a user account with the specified password.

A user-name can be any valid [identifier](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_identifiers) of up to 128 characters. The user name can be an SQL reserved word if it is a delimited identifier enclosed in quotation marks, and Support Delimited Identifiers\=YES. User names are not case-sensitive.

The IDENTIFY BY and IDENTIFIED BY keywords are synonyms.

A password can be a numeric literal, an identifier, or a quoted string. A numeric literal or an identifier does not have to be enclosed in quotes. A quoted string is commonly used to include blanks in a password; a quoted password can contain any combination of characters, with the exception of the quote character itself. A numeric literal must consist of only the characters 0 through 9. An [identifier](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_identifiers) must start with a letter (uppercase or lowercase) or a % (percent symbol); this can be followed by any combination of letters, numbers, or any of the following symbols: \_ (underscore), & (ampersand), $ (dollar sign), or @ (at sign).

Passwords are case-sensitive. A password must be at least three characters, and less than 33 characters, in length. Specifying a password that is too long or too short generates an SQLCODE -400 error, with a %msg value of “ERROR #845: Password does not match length or pattern requirements”.

You cannot use a host variable to specify a user-name or password value.

Creating a user does not create any roles or grant any roles to the user. Instead, the user is given permissions for the database they are logging into, and USE permission on the %SQL/Service service if the user holds at least one SQL privilege in the namespace. To assign privileges or roles to a user, use the GRANT command. To create roles, use the CREATE ROLE command.

If you invoke CREATE USER to create a user that already exists, SQL issues an SQLCODE -118 error, with a %msg value of “User named 'name' already exists”. You can determine if a user already exists by invoking the [$SYSTEM.SQL.UserExists()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#UserExists) method:

  WRITE $SYSTEM.SQL.UserExists("Admin"),!
  WRITE $SYSTEM.SQL.UserExists("BertieWooster")

 

This method returns 1 if the specified user exists, and 0 if the user does not exist. User names are not case-sensitive.

Privilges

The CREATE USER command is a privileged operation. Prior to using CREATE USER in embedded SQL, it is necessary to be logged in as a user with appropriate privileges. Failing to do so results in an SQLCODE -99 error (Privilege Violation).

Use the [$SYSTEM.Security.Login()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Security#Login) method to assign a user with appropriate privileges:

   DO $SYSTEM.Security.Login("\_SYSTEM","SYS")
   &sql( /\* SQL code here \*/  )

You must have the %Service\_Login:Use privilege to invoke the $SYSTEM.Security.Login method. For further information, refer to [%SYSTEM.Security](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.Security) in the InterSystems Class Reference.

Examples

The following embedded SQL example creates a new user named “BillTest” with a password of “Carl4SHK”. (The $RANDOM toggle is provided so that you can execute this example program repeatedly.)

Main
   DO $SYSTEM.Security.Login("\_SYSTEM","SYS")
   &sql(CREATE USER BillTest IDENTIFY BY Carl4SHK)
   WRITE !,"CREATE USER error code: ",SQLCODE
Cleanup
   SET toggle\=$RANDOM(2)
   IF toggle\=0 { 
     &sql(DROP USER BillTest)
     WRITE !,"DROP USER error code: ",SQLCODE
   }
   ELSE { 
     WRITE !,"No drop this time"
     QUIT 
   }

 

See Also

*   SQL statements: [ALTER USER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_alteruser) [DROP USER](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dropuser) [GRANT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_grant) [REVOKE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_revoke) [CREATE ROLE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createrole)
    
*   “[Users, Roles, and Privileges](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_privileges)” chapter of Using Caché SQL
    
*   [SQLCODE error messages](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RERR_sql) listed in the Caché Error Reference
    
*   Caché ObjectScript: [$ROLES](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vroles) and [$USERNAME](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RCOS_vusername) special variables
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createtrigger "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createview "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_createuser.xml**