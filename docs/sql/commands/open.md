# OPEN

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_open

---

 

Caché SQL Reference

OPEN

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_lock "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_order "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_COMMANDS "SQL Commands") \]  >  \[ [OPEN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_open "OPEN") \]

Go to: Description Example See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Opens a cursor.

Synopsis

OPEN cursor-name

Arguments

cursor-name

The name of the cursor, which has already been declared. The cursor name was specified in the DECLARE statement. Cursor names are case-sensitive.

Description

An OPEN statement opens a [cursor](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_cursor) according to the parameters specified in the cursor’s [DECLARE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_declare) statement. Once opened, a cursor can be fetched. An open cursor must be closed.

Attempting to open a cursor that is already open results in an SQLCODE -101 error. Attempting to fetch or close a cursor that is not open results in an SQLCODE -102 error. A successful OPEN sets SQLCODE = 0, even if the result set is empty.

OPEN does not support the [#SQLCompile Mode=Deferred](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_macros#GCOS_macros_mpp_lbSQLCompile_Mode) preprocessor directive. Attempting to use Deferred mode with a DECLARE, OPEN, FETCH, or CLOSE cursor statement generates a #5663 compilation error.

As an SQL statement, this is only supported from embedded SQL. Equivalent operations are supported through ODBC using the ODBC API.

Example

The following embedded SQL example shows a cursor (named EmpCursor) being opened and closed:

   SET name\="LastName,FirstName",state\="##"
   &sql(DECLARE EmpCursor CURSOR FOR 
        SELECT Name, Home\_State
        INTO :name,:state FROM Sample.Person
        WHERE Home\_State %STARTSWITH 'A')
   WRITE !,"BEFORE: Name=",name," State=",state 
   &sql(OPEN EmpCursor)
   IF SQLCODE '= 0 { WRITE "Open error: ",SQLCODE
                     QUIT }
   NEW SQLCODE,%ROWCOUNT,%ROWID
   FOR { &sql(FETCH EmpCursor)
        QUIT:SQLCODE  
        WRITE !,"DURING: Name=",name," State=",state }
   WRITE !,"FETCH status SQLCODE=",SQLCODE
   WRITE !,"Number of rows fetched=",%ROWCOUNT
   &sql(CLOSE EmpCursor)
   WRITE !,"AFTER: Name=",name," State=",state

 

See Also

*   [CLOSE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_close), [DECLARE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_declare), [FETCH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_fetch)
    
*   [SQL Cursors](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_cursor) in the “Using Embedded SQL” chapter of Using Caché SQL
    
*   [SQLCODE error messages](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RERR_sql) listed in the Caché Error Reference
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_lock "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_order "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_open.xml**