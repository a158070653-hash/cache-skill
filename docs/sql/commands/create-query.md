# CREATE QUERY

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_createquery

---

 

Caché SQL Reference

CREATE QUERY

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createprocedure "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createrole "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_COMMANDS "SQL Commands") \]  >  \[ [CREATE QUERY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createquery "CREATE QUERY") \]

Go to: Description Arguments Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Creates a query.

Synopsis

CREATE QUERY queryname(parameter\_list) characteristics language
   code\_body

Arguments

queryname

The name of the query to be created, which is an [identifier](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_identifiers). This query name may be unqualified (StoreName) or qualified (Patient.StoreName). The queryname must be followed by parentheses, even if no parameters are specified. For further details see the “Identifiers” chapter of Using Caché SQL.

parameter\_list

Optional — A list of parameters to pass to the query. The parameter list is enclosed in parentheses, and parameters in the list are separated by commas. The parentheses are mandatory, even when no parameters are specified.

characteristics

Optional — One or more keywords specifying the characteristics of the query. Permitted keywords are RESULTS, CONTAINID, FOR, FINAL, PROCEDURE, SELECTMODE. Multiple characteristics are separated by whitespace (a space or line break). Characteristics can be specified in any order.

language

Optional — A keyword clause specifying the programming language used for code\_body. Specify either LANGUAGE OBJECTSCRIPT (for Caché ObjectScript) or LANGUAGE SQL. If the language clause is omitted, SQL is the default.

code\_body

The program code for the query.

SQL program code is prefaced with a BEGIN keyword and concludes with an END keyword. The code\_body for a query consists of only one complete SQL statement (a SELECT statement). This SELECT statement ends with a semicolon (;).

Caché ObjectScript program code is enclosed in curly braces. ObjectScript code lines must be indented.

Description

The CREATE QUERY statement creates a query in a class. By default, a query named MySelect would be stored as User.queryMySelect or SQLUser.queryMySelect.

CREATE QUERY creates a query which may or may not be exposed as a stored procedure. To create a query that is exposed as a stored procedure, you must specify the PROCEDURE keyword as one of its characteristics. You can also use the [CREATE PROCEDURE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createprocedure) statement to create a query which is exposed as a stored procedure.

In order to create a query, you must have %CREATE\_QUERY administrative privilege, as specified by the [GRANT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_grant) command. If you are attempting to create a query for an existing class with a defined owner, you must be logged in as the owner of the class. Otherwise, the operation fails with an SQLCODE -99 error.

Arguments

queryname

The name of the query to be created. This name may be unqualified (StoreName) and take the default schema name, or qualified by specifying the schema name (Patient.StoreName). If you specify \_CURRENT\_USER as the schema name, Caché uses the default schema name. You can use the [$SYSTEM.SQL.DefaultSchema()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#DefaultSchema) method to determine the default schema name. The default schema name SQLUser corresponds to the class package name User.

If queryname is unqualified, the name of the generated class is the default schema name, followed by a dot, followed by “query”, followed by the specified name. Thus the unqualified name StoreName results in a class name such as the following: User.queryStoreName. For further details, see [SQL to Class Name Transformations](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_procedures#GSQL_procedures_sqltoclass) in the “Defining and Using Stored Procedures” chapter of Using Caché SQL.

Caché SQL does not allow you to specify a queryname that differs only in letter case. Specifying a queryname that differs only in letter case from an existing query name results in an SQLCODE -400 error.

If the specified queryname already exists in the current namespace, Caché generates an SQLCODE -361 error.

parameter-list

A list of parameter declarations for parameters used to pass values to the query. The parameter list is enclosed in parentheses, and parameter declarations in the list are separated by commas. The parentheses are mandatory, even if you specify no parameters.

Each parameter declaration in the list consists of (in order):

*   An optional keyword specifying whether the parameter mode is IN (input value), OUT (output value), or INOUT (modify value). If omitted, the default parameter mode is IN.
    
*   The parameter name. Parameter names are case-sensitive.
    
*   The [data type](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datatype) of the parameter.
    
*   Optional: A default value for the parameter. You can specify the DEFAULT keyword followed by a default value; the DEFAULT keyword is optional. If no default is specified, the assumed default is NULL.
    

The following example creates a query exposed as a stored procedure with two input parameters, both of which have default values. The topnum input parameter specifies the optional DEFAULT keyword; the minage input parameter omits this keyword:

CREATE QUERY AgeQuery(IN topnum INT DEFAULT 10,IN minage INT 20)
   PROCEDURE
   BEGIN
   SELECT TOP :topnum Name,Age FROM Sample.Person
   WHERE Age \> :minage ;
   END

The following are all valid CALL statements for this query: CALL AgeQuery(6,65); CALL AgeQuery(6); CALL AgeQuery(,65); CALL AgeQuery().

characteristics

The available characteristics keywords are as follows:

Characteristics Keyword

Description

CONTAINID integer

Specifies which field, if any, returns the ID. Set CONTAINID to the number of the column that returns the ID, or 0 if no column returns the ID. Caché does not validate that the named field actually contains the ID, so a user error here results in inconsistent data.

FOR className

Specifies the name of the class in which to create the method. If the class does not exist, it will be created. You can also specify a class name by qualifying the method name. The class name specified in the FOR clause overrides a class name specified by qualifying the method name.

FINAL

Specifies that subclasses cannot override the method. By default, methods are not final. The FINAL keyword is inherited by subclasses.

PROCEDURE

Specifies that the query is an SQL stored procedure. Stored procedures are inherited by subclasses. (This keyword can be abbreviated as PROC.)

RESULTS (result\_set)

Specifies the data fields in the order that they are returned by the query. If you specify a RESULTS clause, you must list all fields returned by the query as a comma-separated list enclosed in parentheses. Specifying fewer or more fields than are returned by the query results in a SQLCODE -76 cardinality mismatch error.

For each field you specify a column name (which will be used as the column header) and a data type.

If LANGUAGE SQL, you can omit the RESULTS clause. If you omit the RESULTS clause, the ROWSPEC is automatically generated during class compilation.

SELECTMODE mode

Specifies the mode used to compile the query. The possible values are LOGICAL, ODBC, RUNTIME, and DISPLAY. The default is RUNTIME.

If you specify a method keyword (such as PRIVATE or RETURNS) that is not valid for a query, Caché generates an SQLCODE -47 error. Specifying duplicate characteristics results in an SQLCODE -44 error.

The SELECTMODE clause specifies the mode in which data is returned. If the mode value is LOGICAL, then logical (internal storage) values are returned. For example, dates are returned in $HOROLOG format. If the mode value is ODBC, logical-to-ODBC conversion is applied, and ODBC format values are returned. If the mode value is DISPLAY, logical-to-display conversion is applied, and display format values are returned. If the mode value is RUNTIME, the mode can be set (to LOGICAL, ODBC, or DISPLAY) at execution time by setting the [%SQL.Statement](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SQL.Statement) class [%SelectMode](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SQL.Statement#%SelectMode) property, as described in “[Using Dynamic SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_dynsql)” chapter of Using Caché SQL. The RUNTIME mode default is LOGICAL. For further details on SelectMode options, refer to “[Data Display Options](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_basics#GSQL_basics_selectmode)” in the “Caché SQL Basics” chapter of Using Caché SQL. The value that you specify for SELECTMODE is added at the beginning of the Caché ObjectScript class method code as: #SQLCompile SELECT=mode. For further details, see [#SQLCompile Select](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_macros#GCOS_macros_mpp_lbSQLCompile_Select) in the “ObjectScript Macros and the Macro Preprocessor” chapter of Using Caché ObjectScript.

The RESULTS clause specifies the results of a query. The SQL data type parameters in the RESULTS clause are translated into corresponding Caché data type parameters in the query’s ROWSPEC. For example, the RESULTS clause RESULTS ( Code VARCHAR(15) ) generates a ROWSPEC specification of ROWSPEC = “Code:%Library.String(MAXLEN=15)”.

language

A keyword clause specifying the language you are using for code\_body. Permitted clauses are LANGUAGE OBJECTSCRIPT (for Caché ObjectScript) or LANGUAGE SQL. If the LANGUAGE clause is omitted, SQL is the default.

If the LANGUAGE is SQL a class query of type [%Library.SQLQuery](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Library.SQLQuery) is generated. If the LANGUAGE is OBJECTSCRIPT, a class query of type [%Library.Query](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Library.Query) is generated.

code\_body

The program code for the query to be created. You specify this code in either SQL or Caché ObjectScript. The language used must match the LANGUAGE clause. However, code specified in Caché ObjectScript can contain embedded SQL.

If the code you specify is SQL, it must consist of a single SELECT statement. The program code for a query in SQL is prefaced with a BEGIN keyword, followed by the program code (a SELECT statement). At the end of the program code, specify a semicolon (;) then an END keyword.

If the code you specify is OBJECTSCRIPT, it must contain calls to the [Execute()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25Library.Query#Execute) and [Fetch()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25Library.Query#Fetch) class methods of the [%Library.Query](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Library.Query) class provided by Caché, and may contain [Close()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25Library.Query#Close), [FetchRows()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25Library.Query#FetchRows), and [GetInfo()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25Library.Query#GetInfo) method calls. Caché ObjectScript code is enclosed in curly braces. If Execute() or Fetch() are missing, an SQLCODE -46 error is generated upon compilation.

If the Caché ObjectScript code block fetches data into a local variable (for example, Row), you must conclude the code block with the line SET Row="" to indicate an end-of-data condition.

If the query is exposed as a stored procedure (by specifying the PROCEDURE keyword in characteristics), it uses a procedure context handler to pass the procedure context back and forth between the procedure and its caller.

When a stored procedure is called, an object of the class [%Library.SQLProcContext](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Library.SQLProcContext) is instantiated in the %sqlcontext variable. This is used to pass the procedure context back and forth between the procedure and its caller (for example, the ODBC server).

%sqlcontext consists of several properties, including an Error object, the SQLCODE error status, the SQL row count, and an error message. The following example shows the values used to set several of these:

  SET %sqlcontext.%SQLCODE=SQLCODE
  SET %sqlcontext.%ROWCOUNT=%ROWCOUNT
  SET %sqlcontext.%Message=%msg

The values of SQLCODE and %ROWCOUNT are automatically set by the execution of an SQL statement. The %sqlcontext object is reset before each execution.

Alternatively, an error context can be established by instantiating a %SYSTEM.Error object and setting it as %sqlcontext.Error.

Caché uses the code you supply to generate the actual code of the query.

Examples

The following embedded SQL example creates a query named DocTestPersonState. It declares no parameters, sets the SELECTMODE characteristic, and takes the default (SQL) for language:

  &sql(CREATE QUERY DocTestPersonState() SELECTMODE RUNTIME
       BEGIN
       SELECT Name,Home\_State FROM Sample.Person ;
       END)
  IF SQLCODE\=0 { WRITE !,"Created a query" }
  ELSEIF SQLCODE\=\-361 { WRITE !,"Query exists: ",%msg }
  ELSE { WRITE !,"CREATE QUERY error: ",SQLCODE }

 

You can go to the Management Portal, select the Classes option, then select the SAMPLES namespace. There you will find the query created by the above example: User.queryDocTestPersonState.cls. From this display you can delete this query before rerunning the above program example. You can, of course, use DROP QUERY to delete created queries.

The following Embedded SQL example creates a method-based query named DocTestSQLCODEList which fetches a list of SQLCODEs and their descriptions. It sets a RESULTS result set characteristic, sets language as Caché ObjectScript, and calls the Execute(), Fetch(), and Close() methods:

  &sql(CREATE QUERY DocTestSQLCODEList()
  RESULTS (SQLCODE SMALLINT,Description VARCHAR(100))
    PROCEDURE
    LANGUAGE OBJECTSCRIPT
  Execute(INOUT QHandle BINARY(255))
    {
     SET QHandle\=1,%i(QHandle)\=""
     QUIT ##lit($$$OK)
    }
  Fetch(INOUT QHandle BINARY(255), INOUT Row %List, INOUT AtEnd INT)
    {
     SET AtEnd\=0,Row\=""
     SET %i(QHandle)\=$o(^%qCacheSQL("SQLCODE",%i(QHandle)))
     IF %i(QHandle)\="" {SET AtEnd\=1 QUIT ##lit($$$OK) }
       SET Row\=$lb(%i(QHandle),^%qCacheSQL("SQLCODE",%i(QHandle),1,1))
     QUIT ##lit($$$OK)
    }
  Close(INOUT QHandle BINARY(255))
    {
     KILL %i(QHandle)
     QUIT ##lit($$$OK)
    }
  )
  IF SQLCODE\=0 { WRITE !,"Created a query" }
  ELSEIF SQLCODE\=\-361 { WRITE !,"Query exists: ",%msg }
  ELSE { WRITE !,"CREATE QUERY error: ",SQLCODE }

 

You can go to the Management Portal, select the Classes option, then select the SAMPLES namespace. There you will find the query created by the above example: User.queryDocTestSQLCODEList.cls. From this display you can delete this query before rerunning the above program example. You can, of course, use DROP QUERY to delete created queries.

The following Dynamic SQL example creates a query named DocTest, then executes this query using the [%PrepareClassQuery()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SQL.Statement#%PrepareClassQuery) method of the [%SQL.Statement](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SQL.Statement) class:

  /\* Creating the Query \*/
  SET myquery\=4
    SET myquery(1)\="CREATE QUERY DocTest() SELECTMODE RUNTIME "
    SET myquery(2)\="BEGIN "
    SET myquery(3)\="SELECT TOP 5 Name,Home\_State FROM Sample.Person ; "
    SET myquery(4)\="END"
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET qStatus \= tStatement.%Prepare(.myquery)
  SET rset \= tStatement.%Execute()
  IF SQLCODE\=0 { WRITE !,"Created a query",! }
  ELSEIF SQLCODE\=\-361 { WRITE !,"Query exists: ",%msg }
  ELSE { WRITE !,"CREATE QUERY error: ",SQLCODE }
  /\* Calling the Query \*/
  WRITE !,"Calling a class query"
  SET tStatus \= tStatement.%PrepareClassQuery("User.queryDocTest","DocTest")
  WRITE !,"Dyn %PrepareClassQuery status=",tStatus,!,!
  SET rset \= tStatement.%Execute()
  WRITE "Query data",!,!
  WHILE rset.%Next() {
     DO rset.%Print() } 
  WRITE !,"End of data"
  /\* Deleting the Query \*/
  &sql(DROP QUERY DocTest)
  IF SQLCODE\=0 { WRITE !,"Deleted the query" }

 

For further details, refer to the [Dynamic SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_dynsql) chapter of Using Caché SQL.

See Also

*   [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select)
    
*   [CALL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_call)
    
*   [DROP QUERY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dropquery)
    
*   [CREATE PROCEDURE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createprocedure)
    
*   “[Querying the Database](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_queries)” chapter in Using Caché SQL
    
*   “[Defining and Using Stored Procedures](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_procedures)” chapter in Using Caché SQL
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createprocedure "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createrole "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_createquery.xml**