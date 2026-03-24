# DECLARE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_declare

---

 

Caché SQL Reference

DECLARE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createview "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_delete "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_COMMANDS "SQL Commands") \]  >  \[ [DECLARE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_declare "DECLARE") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Declares a cursor.

Synopsis

DECLARE cursor-name CURSOR FOR query

Arguments

cursor-name

The name of the cursor, which must begin with a letter and contain only letters and numbers. (Cursor names do not follow SQL identifier conventions). Cursor names are case-sensitive. They are subject to additional naming restrictions, as described below.

query

A standard [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select) statement that defines the result set of the cursor. This SELECT can include the %NOFPLAN keyword to specify that Caché should ignore the [frozen plan](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQLOPT_frozenplans) (if any) for this query. This SELECT can include an ORDER BY clause, with or without a TOP clause. This SELECT can specify a [table-valued function](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_from#RSQL_from_tvf) in the FROM clause.

Description

A DECLARE statement declares a [cursor](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_cursor) used in [cursor-based Embedded SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_cursor). After declaring a cursor, you issue an [OPEN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_open) statement to open the cursor and then a series of [FETCH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_fetch) statements to retrieve individual records. The cursor defines the SELECT query that is used to select records for retrieval by these FETCH statements. You issue a [CLOSE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_close) statement to close (but not delete) the cursor.

As an SQL statement, DECLARE is only supported from Embedded SQL. For Dynamic SQL, use instead either a simple SELECT statement (with no INTO clause), or a combination of Dynamic SQL and Embedded SQL. Equivalent operations are supported through ODBC using the ODBC API.

DECLARE declares a forward-only (non-scrollable) cursor. Fetch operations begin with the first record in the query result set and proceed sequentially through the result set records. A FETCH can only fetch a record once. The next FETCH fetches the next sequential record in the result set.

Because DECLARE is a declaration, not an executed statement, it does not set or kill the SQLCODE variable.

DECLARE does not support the [#SQLCompile Mode=Deferred](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_macros#GCOS_macros_mpp_lbSQLCompile_Mode) preprocessor directive. Attempting to use Deferred mode with a DECLARE, OPEN, FETCH, or CLOSE cursor statement generates a #5663 compilation error.

Cursor Names

A cursor name must be unique within the routine and the corresponding class. A cursor name may be of any length, but must be unique within the first 29 characters. Cursor names are case-sensitive. Attempting to declare two cursors with the same name results in an error code -52 during compilation.

Cursor names are not namespace-specific. You can DECLARE a cursor in one namespace, and OPEN, FETCH, or CLOSE this cursor when in another namespace. Note that SQL tables are namespace-specific, so the FETCH operation must be invoked in the same namespace as the table from which records are being fetched.

The first character of a cursor name must be a letter. The second and subsequent characters of a cursor name must be either a letter or a number. Unlike SQL [identifiers](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_identifiers), punctuation characters are not permitted in cursor names.

You can use a delimiter characters (double quotes) to specify an SQL reserved word as a cursor name. A delimited cursor name is not an SQL delimited identifier; delimited cursor names are still case-sensitive and cannot contain punctuation characters. In most cases, an SQL reserved word should not be used as a cursor name.

Updating through a Cursor

You can perform record updates and deletes through a declared cursor using an UPDATE or DELETE statement with the [WHERE CURRENT OF](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_wherecurrentof) clause. In Caché SQL a cursor can always be used for UPDATE or DELETE operations if you have the appropriate privileges on the affected tables and columns; refer to the [GRANT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_grant) statement for assigning object privileges.

A DECLARE statement can specify a FOR UPDATE or FOR READ ONLY keyword clause following the query. These clauses are optional and perform no operation. They are provided as a way to document in the code that the process issuing the query has or does not have the needed update and delete object privileges.

Examples

The following Embedded SQL example uses DECLARE to define a cursor for a query that specifies two output host variables. The cursor is then opened, fetched repeatedly, and closed:

   SET name\="John Doe",state\="##"
   &sql(DECLARE EmpCursor CURSOR FOR 
        SELECT Name, Home\_State
        INTO :name,:state FROM Sample.Person
        WHERE Home\_State %STARTSWITH 'A'
        FOR READ ONLY)
     WRITE !,"BEFORE: Name=",name," State=",state 
   &sql(OPEN EmpCursor)
   NEW SQLCODE,%ROWCOUNT,%ROWID
   FOR { &sql(FETCH EmpCursor)
        QUIT:SQLCODE  
        WRITE !,"DURING: Name=",name," State=",state }
   WRITE !,"FETCH status SQLCODE=",SQLCODE
   WRITE !,"Number of rows fetched=",%ROWCOUNT
   &sql(CLOSE EmpCursor)
   WRITE !,"AFTER: Name=",name," State=",state

 

The following Embedded SQL example uses DECLARE to define a cursor for a query that specifies both output host variables in the INTO clause and input host variables in the WHERE clause. The cursor is then opened, fetched repeatedly, and closed:

   NEW SQLCODE,%ROWCOUNT,%ROWID
   SET EmpZipLow\="10000"
   SET EmpZipHigh\="19999"
    &sql(DECLARE EmpCursor CURSOR FOR
     SELECT Name,Home\_Zip
     INTO :name,:zip
     FROM Sample.Employee WHERE Home\_Zip BETWEEN :EmpZipLow AND :EmpZipHigh)
        &sql(OPEN EmpCursor)
   IF (SQLCODE) {
     WRITE SQLCODE,!
     QUIT
     }
  SET SQLCODE \= 0
  WHILE (SQLCODE \= 0) {
   &sql(FETCH EmpCursor)
   WRITE !,name," ",zip
  }
  &sql(CLOSE EmpCursor)
  QUIT

 

The following Embedded SQL example uses a [table-valued function](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_from#RSQL_from_tvf) as the FROM clause of the query:

    ZNSPACE "Samples"
    &sql(DECLARE EmpCursor CURSOR FOR 
        SELECT Name INTO :name FROM Sample.SP\_Sample\_By\_Name('A')
        FOR READ ONLY)
   &sql(OPEN EmpCursor)
   NEW SQLCODE,%ROWCOUNT,%ROWID
   FOR { &sql(FETCH EmpCursor)
        QUIT:SQLCODE  
        WRITE "Name=",name,! }
   WRITE !,"FETCH status SQLCODE=",SQLCODE
   WRITE !,"Number of rows fetched=",%ROWCOUNT
   &sql(CLOSE EmpCursor)

 

See Also

*   [CLOSE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_close), [FETCH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_fetch), [OPEN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_open), [WHERE CURRENT OF](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_wherecurrentof)
    
*   [SQL Cursors](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_cursor) in the “Using Embedded SQL” chapter of Using Caché SQL
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createview "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_delete "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_declare.xml**