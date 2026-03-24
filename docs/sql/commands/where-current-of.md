# WHERE CURRENT OF

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_wherecurrentof

---

 

Caché SQL Reference

WHERE CURRENT OF

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_where "Go back to the previous section.") 

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_COMMANDS "SQL Commands") \]  >  \[ [WHERE CURRENT OF](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_wherecurrentof "WHERE CURRENT OF") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

An UPDATE/DELETE clause that specifies the current row using a cursor.

Synopsis

WHERE CURRENT OF cursor

Arguments

cursor

Specifies that the operation is done at the current position of cursor, which is a [cursor](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_cursor) that points to the table.

Description

The WHERE CURRENT OF clause can be used in a [cursor-based Embedded SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_cursor) UPDATE or DELETE statement to specify the cursor positioned on the record to be updated or deleted. For example:

   &sql(DELETE FROM Sample.Employees WHERE CURRENT OF EmployeeCursor)

which deletes the row that the last FETCH command obtained from the "EmployeeCursor" cursor.

An Embedded SQL UPDATE or DELETE can use a [WHERE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_where) clause (with no cursor), or a WHERE CURRENT OF with a declared cursor, but not both. If you specify an UPDATE or DELETE with neither WHERE nor WHERE CURRENT OF, all of the records in the table are updated or deleted.

UPDATE Restriction

When using a WHERE CURRENT OF clause, you cannot update a field using the current field value to generate an updated value. For example, SET Salary=Salary+100 or SET Name=UPPER(Name). Attempting to do so results in an SQLCODE -69 error: SET <field> = <value expression> not allowed with WHERE CURRENT OF <cursor>.

Examples

The following Embedded SQL example shows an UPDATE operation using WHERE CURRENT OF:

  NEW SQLCODE,%ROWCOUNT,%ROWID
  &sql(DECLARE WPCursor CURSOR FOR 
        SELECT Lang FROM Sample.WordPairs
        WHERE Lang\='Sp')
   &sql(OPEN WPCursor)
   FOR { &sql(FETCH WPCursor)
        QUIT:SQLCODE 
        &sql(UPDATE Sample.WordPairs SET Lang\='Es'
       WHERE CURRENT OF WPCursor)
    IF SQLCODE\=0 {
    WRITE !,"Update succeeded"
    WRITE !,"Row count=",%ROWCOUNT," RowID=",%ROWID }
    ELSE {
    WRITE !,"Update failed, SQLCODE=",SQLCODE }
    }
    &sql(CLOSE WPCursor)

The following Embedded SQL example shows a DELETE operation using WHERE CURRENT OF:

  NEW SQLCODE,%ROWCOUNT,%ROWID
  &sql(DECLARE WPCursor CURSOR FOR 
        SELECT Lang FROM Sample.WordPairs
        WHERE Lang\='En')
   &sql(OPEN WPCursor)
   FOR { &sql(FETCH WPCursor)
        QUIT:SQLCODE 
        &sql(DELETE FROM Sample.WordPairs
       WHERE CURRENT OF WPCursor)
    IF SQLCODE\=0 {
    WRITE !,"Delete succeeded"
    WRITE !,"Row count=",%ROWCOUNT," RowID=",%ROWID }
    ELSE {
    WRITE !,"Delete failed, SQLCODE=",SQLCODE }
    }
    &sql(CLOSE WPCursor)

See Also

*   [DECLARE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_declare), [OPEN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_open), [FETCH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_fetch), [CLOSE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_close)
    
*   [DELETE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_delete), [UPDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_update), [INSERT OR UPDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_insertorupdate)
    
*   [SQL Cursors](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_cursor) in the “Using Embedded SQL” chapter of Using Caché SQL
    
*   [SQLCODE error messages](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RERR_sql) listed in the Caché Error Reference
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_where "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:38**

Source: **RSQL\_wherecurrentof.xml**