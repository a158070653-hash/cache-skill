# CLOSE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_close

---

 

Caché SQL Reference

CLOSE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_checkpriv "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_commit "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_COMMANDS "SQL Commands") \]  >  \[ [CLOSE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_close "CLOSE") \]

Go to: Description Example See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Closes a cursor.

Synopsis

CLOSE cursor-name

Arguments

cursor-name

The name of the cursor to be closed. The cursor name was specified in the DECLARE statement. Cursor names are case-sensitive.

Description

A CLOSE statement shuts down an open [cursor](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_cursor). It releases the current result set and frees any cursor locks held on the rows on which the cursor is positioned. However, CLOSE does not delete the cursor; it leaves the data structures accessible for reopening, but fetches and positioned updates are not allowed until the cursor is reopened. This behavior is demonstrated by the following command sequences:

*   DECLARE c1, OPEN c1, FETCH c1, CLOSE c1 is the standard sequence.
    
*   DECLARE c1, OPEN c1, CLOSE c1, OPEN c1 reopens the declared cursor c1.
    
*   DECLARE c1, OPEN c1, CLOSE c1, DECLARE c1, OPEN c1 reopens the cursor specified in the first DECLARE, the second DECLARE is ignored.
    
*   DECLARE c1, OPEN c1, FETCH c1, CLOSE c1, OPEN c1, FETCH c1 cause both fetch operations to retrieve the same record.
    

CLOSE must be issued on an open cursor. Issuing a CLOSE on a cursor that has only been declared (but not opened), or on a cursor that has already been closed results in an SQLCODE -102 error. Issuing a CLOSE on a non-existent cursor — for example, a cursor that differs from the defined cursor in letter case — results in a <SYNTAX> error.

The cursor-name is not namespace-specific. Changing the current namespace has no effect on use of a declared cursor. The only namespace consideration is that FETCH must occur in the namespace that contains the table(s) being queried.

Note that, as an SQL statement, CLOSE is only supported from Embedded SQL. Equivalent operations are supported through ODBC using the ODBC API.

CLOSE does not support the [#SQLCompile Mode=Deferred](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_macros#GCOS_macros_mpp_lbSQLCompile_Mode) preprocessor directive. Attempting to use Deferred mode with a DECLARE, OPEN, FETCH, or CLOSE cursor statement generates a #5663 compilation error.

Example

The following Embedded SQL example shows a cursor (named EmpCursor) being opened and closed:

   SET name\="LastName,FirstName",state\="##"
   &sql(DECLARE EmpCursor CURSOR FOR 
        SELECT Name, Home\_State
        INTO :name,:state FROM Sample.Employee
        WHERE Home\_State %STARTSWITH 'A')
   WRITE !,"BEFORE: Name=",name," State=",state 
   &sql(OPEN EmpCursor)
   NEW SQLCODE,%ROWCOUNT,%ROWID
   FOR { &sql(FETCH EmpCursor)
        QUIT:SQLCODE  
        WRITE !,"DURING: Name=",name," State=",state }
   WRITE !,"After FETCH error code: ",SQLCODE
   WRITE !,"After FETCH row count: ",%ROWCOUNT
   &sql(CLOSE EmpCursor)
   WRITE !,"After CLOSE error code: ",SQLCODE
   WRITE !,"After CLOSE row count: ",%ROWCOUNT
   WRITE !,"AFTER: Name=",name," State=",state

 

Note that after closing the cursor, the host variables remain set to the last fetched data values, and %ROWCOUNT remains set to the number of rows retrieved. However, the SQLCODE value at the end of the fetch (SQLCODE=100) is overwritten by the SQLCODE value for the CLOSE (SQLCODE=0).

The following Embedded SQL example shows that a cursor persists across namespaces. This cursor is declared in SAMPLES, opened in DOCBOOK, fetched in SAMPLES, and closed in USER. Note that the FETCH must be executed in the namespace that contains Sample.Employee:

    &sql(USE DATABASE "USER")
    WRITE $ZNSPACE,!
  &sql(DECLARE NSCursor CURSOR FOR SELECT Name INTO :name FROM Sample.Employee)
    &sql(USE DATABASE DOCBOOK)
    WRITE $ZNSPACE,!
  &sql(OPEN NSCursor)
    WRITE "Open SQLCODE: ",SQLCODE,!
    &sql(USE DATABASE SAMPLES)
    WRITE $ZNSPACE,!
      NEW SQLCODE,%ROWCOUNT,%ROWID
 FOR { &sql(FETCH NSCursor)
       QUIT:SQLCODE  
       WRITE "Name=",name,! }
    &sql(USE DATABASE "USER")
    WRITE $ZNSPACE,!
  &sql(CLOSE NSCursor)
    WRITE "Close SQLCODE: ",SQLCODE,!

 

See Also

*   [DECLARE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_declare), [FETCH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_fetch), [OPEN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_open)
    
*   [SQL Cursors](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_cursor) in the “Using Embedded SQL” chapter of Using Caché SQL
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_checkpriv "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_commit "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_close.xml**