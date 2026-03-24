# FETCH

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_fetch

---

 

Caché SQL Reference

FETCH

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dropview "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_from "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_COMMANDS "SQL Commands") \]  >  \[ [FETCH](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_fetch "FETCH") \]

Go to: Description Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Repositions a cursor, and retrieves data from it.

Synopsis

FETCH cursor-name \[INTO host-variable-list\]

Arguments

cursor-name

The name of a currently open cursor. The cursor name was specified in the DECLARE statement. Cursor names are case-sensitive.

INTO host-variable-list

Optional — Places data from the columns of a fetch into local variables. The host-variable-list specifies a host variable, or a comma-separated list of host variables, that are targets to contain data associated with the cursor. The INTO clause is optional. If it is not specified, the FETCH statement positions the cursor only.

Description

Within an embedded SQL application, a FETCH statement retrieves data from a [cursor](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_cursor). The required sequence of actions is: DECLARE, OPEN, FETCH, CLOSE. Attempting a FETCH on a cursor that is not open results in an SQLCODE -102 error.

As an SQL statement, this is supported only from within embedded SQL. Equivalent operations are supported through ODBC using the ODBC API. For further details, refer to the [Embedded SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql) chapter in Using Caché SQL.

An INTO clause can be specified as a clause of the DECLARE statement, as a clause of the FETCH statement, or both. The INTO clause allows data from the columns of a fetch to be placed into local [host variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_hvars). Each host variable in the list, from left to right, is associated with the corresponding column in the cursor result set. The data type of each variable must either match or be a supported implicit conversion of the data type of the corresponding result set column. The number of variables must match the number of columns in the cursor select list.

The FETCH operation completes when the cursor advances to the end of the data. This sets SQLCODE=100 (No more data). It also sets the [%ROWCOUNT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_svars) variable to the number of fetched rows.

Note:

The values returned by INTO clause host variables are only reliable while SQLCODE=0. If SQLCODE=100 (No more data) the host variable values should not be used.

The cursor-name is not namespace-specific. Changing the current namespace has no effect on use of a declared cursor. The only namespace consideration is that FETCH must occur in the namespace that contains the table(s) being queried.

FETCH does not support the [#SQLCompile Mode=Deferred](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_macros#GCOS_macros_mpp_lbSQLCompile_Mode) preprocessor directive. Attempting to use Deferred mode with a DECLARE, OPEN, FETCH, or CLOSE cursor statement generates a #5663 compilation error.

%ROWID

When a FETCH retrieves a row of an updateable cursor, it sets [%ROWID](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_svars) to the RowID value of the fetched row. An updateable cursor is one in which the top FROM clause contains exactly one element, either a table name or an updateable view name.

This setting of %ROWID for each row retrieved is subject to the following conditions:

*   The DECLARE cursorname CURSOR and OPEN cursorname statements do not initialize %ROWID; the %ROWID value is unchanged from its prior value. The first successful FETCH sets %ROWID. Each subsequent FETCH that retrieves a row resets %ROWID to the current RowID. FETCH sets %ROWID if it retrieves a row of an updateable cursor. If the cursor is not updateable, %ROWID remains unchanged. If no rows matched the query selection criteria, FETCH does not change the prior the %ROWID value. Upon CLOSE or when FETCH issues an SQLCODE 100 (No Data, or No More Data), %ROWID contains the RowID of the last row retrieved.
    
*   A cursor-based SELECT with a [DISTINCT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_distinct) keyword or a [GROUP BY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_groupby) clause does not set %ROWID. The %ROWID value is unchanged from its previous value (if any).
    
*   A cursor-based SELECT that performs only [aggregate operations](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_aggregatefunctions) does not set %ROWID. The %ROWID value is unchanged from its previous value (if any).
    

An Embedded SQL SELECT with no declared cursor does not set %ROWID. The %ROWID value is unchanged upon the completion of a simple SELECT statement.

FETCH for UPDATE or DELETE

You can use FETCH to retrieve a row for update or delete. The UPDATE or DELETE must specify the WHERE CURRENT OF clause. The DECLARE should specify the FOR UPDATE clause. The following example shows a cursor-based delete that deletes all selected rows:

  ZN "Samples"
  &sql(DECLARE MyCursor CURSOR FOR SELECT %ID,Status
       FROM Sample.Quality WHERE Status\='Bad' FOR UPDATE)
  &sql(OPEN MyCursor)
  NEW SQLCODE,%ROWCOUNT,%ROWID
  FOR {&sql(FETCH MyCursor)  QUIT:SQLCODE'=0
       &sql(DELETE FROM Sample.Quality WHERE CURRENT OF MyCursor) }
  WRITE !,"Number of rows updated=",%ROWCOUNT
  &sql(CLOSE MyCursor)

Examples

The following Embedded SQL example shows FETCH invoked by an argumentless FOR loop retrieving data from a cursor named EmpCursor. The INTO clause is specified in the DECLARE statement:

    &sql(DECLARE EmpCursor CURSOR FOR 
        SELECT Name, Home\_State
        INTO :name,:state FROM Sample.Employee
        WHERE Home\_State %STARTSWITH 'M')
   &sql(OPEN EmpCursor)
   NEW SQLCODE,%ROWCOUNT,%ROWID
   FOR { &sql(FETCH EmpCursor)
        QUIT:SQLCODE'=0  
        WRITE "count: ",%ROWCOUNT," RowID: ",%ROWID,!
        WRITE "  Name=",name," State=",state,! }
   WRITE !,"Final Fetch SQLCODE: ",SQLCODE
   &sql(CLOSE EmpCursor)

 

The following Embedded SQL example shows FETCH invoked by an argumentless FOR loop retrieving data from a cursor named EmpCursor. The INTO clause is specified as part of the FETCH statement:

   &sql(DECLARE EmpCursor CURSOR FOR 
        SELECT Name,Home\_State FROM Sample.Employee
        WHERE Home\_State %STARTSWITH 'M')
   &sql(OPEN EmpCursor)
   FOR { &sql(FETCH EmpCursor INTO :name,:state)
        QUIT:SQLCODE'=0  
        WRITE "count: ",%ROWCOUNT," RowID: ",%ROWID,!
        WRITE "  Name=",name," State=",state,! }
   WRITE !,"Final Fetch SQLCODE: ",SQLCODE
   &sql(CLOSE EmpCursor)

 

The following Embedded SQL example shows FETCH invoked using a WHILE loop:

  &sql(DECLARE C1 CURSOR FOR 
        SELECT Name,Home\_State INTO :name,:state FROM Sample.Person
        WHERE Home\_State %STARTSWITH 'M')
   &sql(OPEN C1)
   &sql(FETCH C1)
   WHILE (SQLCODE \= 0) {
        WRITE "count: ",%ROWCOUNT," RowID: ",%ROWID,!
        WRITE "  Name=",name," State=",state,!
        &sql(FETCH C1) }
   WRITE !,"Final Fetch SQLCODE: ",SQLCODE
   &sql(CLOSE C1)

 

The following Embedded SQL example shows FETCH retrieving aggregate function values. %ROWID is not set:

    &sql(DECLARE PersonCursor CURSOR FOR 
        SELECT COUNT(\*),AVG(Age) FROM Sample.Person )
   &sql(OPEN PersonCursor)
   NEW SQLCODE,%ROWCOUNT
   FOR { &sql(FETCH PersonCursor INTO :cnt,:avg)
        QUIT:SQLCODE'=0  
        WRITE %ROWCOUNT," Num People=",cnt," Average Age=",avg,! }
   WRITE !,"Final Fetch SQLCODE: ",SQLCODE
   &sql(CLOSE PersonCursor)

 

The following Embedded SQL example shows FETCH retrieving DISTINCT values. %ROWID is not set:

    &sql(DECLARE EmpCursor CURSOR FOR 
        SELECT DISTINCT Home\_State FROM Sample.Employee
        WHERE Home\_State %STARTSWITH 'M'
        ORDER BY Home\_State )
   &sql(OPEN EmpCursor)
   NEW SQLCODE,%ROWCOUNT
   FOR { &sql(FETCH EmpCursor INTO :state)
        QUIT:SQLCODE'=0  
        WRITE %ROWCOUNT," State=",state,! }
   WRITE !,"Final Fetch SQLCODE: ",SQLCODE
   &sql(CLOSE EmpCursor)

 

The following Embedded SQL example shows that a cursor persists across namespaces. This cursor is declared in SAMPLES, opened in DOCBOOK, fetched in SAMPLES, and closed in USER. Note that the FETCH must be executed in the namespace that contains the table being queried, Sample.Employee:

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

*   [CLOSE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_close), [DECLARE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_declare), [OPEN](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_open)
    
*   [SQL Cursors](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_cursor) in the “Using Embedded SQL” chapter of Using Caché SQL
    
*   [SQLCODE error messages](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RERR_sql) listed in the Caché Error Reference
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dropview "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_from "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_fetch.xml**