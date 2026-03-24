# CREATE PROCEDURE

Source: http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?KEY=RSQL_createprocedure

---

 

Caché SQL Reference

CREATE PROCEDURE

[\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createmethod "Go back to the previous section.") [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createquery "Go to the next section.")

 

 

Server:**song-office**

Instance:**CACHE**

User:**UnknownUser**

 

  \[ [Home](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.HomePageZen.cls "Home") \]  >  \[ [Caché Development References](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=SETDevRef "Caché Development References") \]  >  \[ [Caché SQL Reference](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL "Caché SQL Reference") \]  >  \[ [SQL Commands](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_COMMANDS "SQL Commands") \]  >  \[ [CREATE PROCEDURE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createprocedure "CREATE PROCEDURE") \]

Go to: Description Arguments Examples See Also

 [Search](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.SearchPageZen.cls "Go to the Search Page"):  

 

  

Creates a method or query which is exposed as an SQL stored procedure.

Synopsis

CREATE PROCEDURE procname(parameter\_list) characteristics language
    code\_body

CREATE PROC procname(parameter\_list) characteristics language
   code\_body

Arguments

procname

The name of the stored procedure to be created, which is an [identifier](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_identifiers). This procedure name may be unqualified (StoreName) or qualified (Patient.StoreName). The procname must be followed by parentheses, even if no parameters are specified. For further details see the “Identifiers” chapter of Using Caché SQL.

parameter\_list

Optional — A list of zero or more parameters to pass to the procedure. The parameter list is enclosed in parentheses, and parameters in the list are separated by commas. The parentheses are mandatory, even when no parameters are specified. Each parameter consists of (in order): an optional IN, OUT, or INOUT keyword; the variable name; the data type; and an optional DEFAULT clause.

characteristics

Optional — One or more keywords specifying the characteristics of the procedure. When creating a method, permitted keywords are FINAL, FOR, PRIVATE, RETURNS, SELECTMODE. When creating a query, permitted keywords are CONTAINID, FINAL, FOR, RESULTS, SELECTMODE.

You can specify the characteristics keyword phrase RESULT SETS, DYNAMIC RESULT SETS, or DYNAMIC RESULT SETS n, where n is an integer. These phrases are synonyms; the DYNAMIC keyword and the n integer are no-ops provided for compatibility.

Multiple characteristics are separated by whitespace (a space or line break). Characteristics can be specified in any order.

language

Optional — A keyword clause specifying the programming language used for code\_body. Specify LANGUAGE OBJECTSCRIPT (for Caché ObjectScript) or LANGUAGE SQL. If the language clause is omitted, SQL is the default.

code\_body

The program code for the procedure.

SQL program code is prefaced with a BEGIN keyword and concludes with an END keyword. Each complete SQL statement within code\_body ends with a semicolon (;).

Caché ObjectScript program code is enclosed in curly braces. ObjectScript code lines must be indented.

Description

The CREATE PROCEDURE statement creates a method or a query which is automatically exposed as an SQL stored procedure. A stored procedure can be invoked by all processes in the current namespace. Stored procedures are inherited by subclasses.

*   If LANGUAGE SQL, the code\_body must contain a SELECT statement in order to generate a query exposed as a stored procedure. If the code does not contain a SELECT statement, CREATE PROCEDURE creates a method.
    
*   If LANGUAGE OBJECTSCRIPT, the code\_body must call [Execute()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25Library.Query#Execute) and [Fetch()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25Library.Query#Fetch) methods in order to generate a query exposed as a stored procedure. It may also call [Close()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25Library.Query#Close), [FetchRows()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25Library.Query#FetchRows), and [GetInfo()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25Library.Query#GetInfo) methods. If the code does not call Execute() and Fetch(), CREATE PROCEDURE creates a method.
    

By default, CREATE PROCEDURE creates a method exposed as a stored procedure.

To create a method not exposed as a stored procedure, use the [CREATE METHOD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createmethod) or [CREATE FUNCTION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createfunction) statement. To create a query not exposed as a stored procedure, use the [CREATE QUERY](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createquery) statement. These statements can also be used to create a method or query exposed as a stored procedure by specifying the PROCEDURE characteristic keyword.

In order to create a procedure, you must have %CREATE\_PROCEDURE administrative privilege, as specified by the [GRANT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_grant) command. If you are attempting to create a procedure for an existing class with a defined owner, you must be logged in as the owner of the class. Otherwise, the operation fails with an SQLCODE -99 error.

A stored procedure is executed using the [CALL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_call) statement.

For information on calling methods from within SQL statements, refer to [User-defined Functions](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_queries#GSQL_queries_funcs) in the “Querying the Database” chapter of Using Caché SQL.

Arguments

procname

The name of the method or query to be created as a stored procedure. This name may be unqualified (StoreName) and take the default schema name, or qualified by specifying the schema name (Patient.StoreName). If you specify \_CURRENT\_USER as the schema name, Caché uses the default schema name. You can use the [$SYSTEM.SQL.DefaultSchema()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#DefaultSchema) method to determine the default schema name. The default schema name SQLUser corresponds to the class package name User.

If procname is unqualified, the name of the generated class is the default schema name, followed by a dot, followed by “proc”, followed by the specified name. Thus the unqualified name StoreName results in a class name such as the following: User.procStoreName. For further details, see [SQL to Class Name Transformations](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_procedures#GSQL_procedures_sqltoclass) in the “Defining and Using Stored Procedures” chapter of Using Caché SQL.

Caché SQL does not allow you to specify a procname that differs only in letter case. Specifying a procname that differs only in letter case from an existing procedure name results in an SQLCODE -400 error.

If the specified procname already exists in the current namespace, Caché generates an SQLCODE -361 error. To determine if a specified procname already exists in the current namespace, use the [$SYSTEM.SQL.ProcedureExists()](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SYSTEM.SQL#ProcedureExists) method.

Note:

Caché SQL procedure names and Caché TSQL procedure names share the same set of names. Therefore, you cannot create an SQL procedure that has the same name as a TSQL procedure in the same namespace. Attempting to do so results in an SQLCODE -400 error.

The name of a procedure must be followed by parameter parentheses.

parameter\_list

A list of parameters used to pass values to the method or query. The parameter list is enclosed in parentheses, and parameter declarations in the list are separated by commas. The parentheses are mandatory, even if you specify no parameters.

Each parameter declaration in the list consists of (in order):

*   An optional keyword specifying whether the parameter mode is IN (input value), OUT (output value), or INOUT (modify value). If omitted, the default parameter mode is IN.
    
*   The parameter name. Parameter names are case-sensitive.
    
*   The [data type](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_datatype) of the parameter.
    
*   Optional: A default value for the parameter. You can specify the DEFAULT keyword followed by a default value; the DEFAULT keyword is optional. If no default is specified, the assumed default is NULL.
    

The following example creates a stored procedure with two input parameters, both of which have default values. One input parameter specifies the optional DEFAULT keyword, the other input parameter omits this keyword:

CREATE PROCEDURE AgeQuerySP(IN topnum INT DEFAULT 10,IN minage INT 20)
   BEGIN
   SELECT TOP :topnum Name,Age FROM Sample.Person
   WHERE Age \> :minage ;
   END

The following example is functionally identical to the example above. The optional DEFAULT keyword is omitted:

CREATE PROCEDURE AgeQuerySP(IN topnum INT 10,IN minage INT 20)
   BEGIN
   SELECT TOP :topnum Name,Age FROM Sample.Person
   WHERE Age \> :minage ;
   END

The following are all valid CALL statements for this procedure: CALL AgeQuerySP(6,65); CALL AgeQuerySP(6); CALL AgeQuerySP(,65); CALL AgeQuerySP().

The following example creates a method exposed as a stored procedure with three parameters:

CREATE PROCEDURE UpdatePaySP
  (IN Salary INTEGER DEFAULT 0,
   IN Name VARCHAR(50), 
   INOUT PayBracket VARCHAR(50) DEFAULT 'NULL')
BEGIN
   UPDATE Sample.Person SET Salary \= :Salary
   WHERE Name\=:Name ;
END

A stored procedure does not perform automatic format conversion of parameters. For example, an input parameter in ODBC format or Display format remains in that format. It is the responsibility of the code that calls the procedure, and the procedure code itself, to handle IN/OUT values in a format appropriate to the application, and to perform any necessary conversions.

Because the method or query is exposed as a stored procedure, it uses a procedure context handler to pass the procedure context back and forth between the procedure and its caller. When a stored procedure is called, an object of the class [%Library.SQLProcContext](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25Library.SQLProcContext) is instantiated in the %sqlcontext variable. This is used to pass the procedure context back and forth between the procedure and its caller (for example, the ODBC server).

%sqlcontext consists of several properties, including an Error object, the SQLCODE error status, the SQL row count, and an error message. The following example shows the values used to set several of these:

  SET %sqlcontext.%SQLCODE=SQLCODE
  SET %sqlcontext.%ROWCOUNT=%ROWCOUNT
  SET %sqlcontext.%Message=%msg

The values of SQLCODE and %ROWCOUNT are automatically set by the execution of an SQL statement. The %sqlcontext object is reset before each execution.

Alternatively, an error context can be established by instantiating a %SYSTEM.Error object and setting it as %sqlcontext.Error.

characteristics

Different characteristics are used for creating a method than those used to create a query.

If you specify a characteristics that is not valid, Caché generates an SQLCODE -47 error. Specifying duplicate characteristics results in an SQLCODE -44 error.

The available method characteristics keywords are as follows:

Method Keyword

Meaning

FOR className

Specifies the name of the class in which to create the method. If the class does not exist, it will be created. You can also specify a class name by qualifying the method name. The class name specified in the FOR clause overrides a class name specified by qualifying the method name.

If you specify the class name using the FOR my.class syntax, Caché defines the class method with Sqlname=procname. Therefore, the method should be invoked as my.procname() (not my.class\_procname()).

FINAL

Specifies that subclasses cannot override the method. By default, methods are not final. The FINAL keyword is inherited by subclasses.

PRIVATE

Specifies that the method can only be invoked by other methods of its own class or subclasses. By default, a method is public, and can be invoked without restriction. This restriction is inherited by subclasses.

RESULT SETS

DYNAMIC RESULT SETS \[n\]

Specifies that the method created will contain the [ReturnResultsets](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=ROBJ_method_returnresultsets) keyword. All forms of this characteristics phrase are synonyms.

RETURNS datatype

Specifies the data type of the value returned by a call to the method. If RETURNS is omitted, the method cannot return a value. This specification is inherited by subclasses, and can be modified by subclasses. This datatype can specify type parameters such as MINVAL, MAXVAL, and SCALE. For example RETURNS DECIMAL(19,4). Note that when returning a value, Caché ignores the length of datatype; for example, RETURNS VARCHAR(32) can receive a string of any length that is returned by a call to the method.

SELECTMODE mode

Only used when LANGUAGE is SQL (the default). When specified, Caché adds an [#SQLCOMPILE SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_macros#GCOS_macros_mpp_lbSQLCompile_Select)\=mode statement to the corresponding class method, thus generating the SQL statements defined in the method with the specified SELECTMODE. The possible mode values are LOGICAL, ODBC, RUNTIME, and DISPLAY. The default is LOGICAL.

The available query characteristics keywords are as follows:

Query Keyword

Description

CONTAINID integer

Specifies which field, if any, returns the ID. Set CONTAINID to the number of the column that returns the ID, or 0 if no column returns the ID. Caché does not validate that the named field actually contains the ID, so a user error here results in inconsistent data.

FOR className

Specifies the name of the class in which to create the method. If the class does not exist, it will be created. You can also specify a class name by qualifying the method name. The class name specified in the FOR clause overrides a class name specified by qualifying the method name.

FINAL

Specifies that subclasses cannot override the method. By default, methods are not final. The FINAL keyword is inherited by subclasses.

RESULTS (result\_set)

Specifies the data fields in the order that they are returned by the query. If you specify a RESULTS clause, you must list all fields returned by the query as a comma-separated list enclosed in parentheses. Specifying fewer or more fields than are returned by the query results in a SQLCODE -76 cardinality mismatch error.

For each field you specify a column name (which will be used as the column header) and a data type.

If LANGUAGE SQL, you can omit the RESULTS clause. If you omit the RESULTS clause, the ROWSPEC is automatically generated during class compilation.

SELECTMODE mode

Specifies the mode used to compile the query. The possible values are LOGICAL, ODBC, RUNTIME, and DISPLAY. The default is RUNTIME.

The SELECTMODE clause is used for SELECT query operations and for INSERT and UPDATE operations. It specifies the compile-time select mode. The value that you specify for SELECTMODE is added at the beginning of the Caché ObjectScript class method code as: #SQLCompile Select=mode. For further details, see [#SQLCompile Select](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_macros#GCOS_macros_mpp_lbSQLCompile_Select) in the “ObjectScript Macros and the Macro Preprocessor” chapter of Using Caché ObjectScript.

*   In a SELECT query, the SELECTMODE specifies the mode in which data is returned. If the mode value is LOGICAL, then logical (internal storage) values are returned. For example, dates are returned in $HOROLOG format. If the mode value is ODBC, logical-to-ODBC conversion is applied, and ODBC format values are returned. If the mode value is DISPLAY, logical-to-display conversion is applied, and display format values are returned. If the mode value is RUNTIME, the display mode can be set (to LOGICAL, ODBC, or DISPLAY) at execution time.
    
*   In an [INSERT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_insert) or [UPDATE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_update) operation, the SELECTMODE RUNTIME option supports automatic conversion of input data values from a display format (DISPLAY or ODBC) to logical storage format. This compiled display-to-logical data conversion code is applied only if the select mode setting when the SQL code is executed is LOGICAL (which is the default for all Caché SQL execution interfaces).
    

When the SQL code is executed, the [%SQL.Statement](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SQL.Statement) class [%SelectMode](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?PAGE=CLASS&LIBRARY=%25SYS&CLASSNAME=%25SQL.Statement#%SelectMode) property specifies the execution-time select mode, as described in “[Using Dynamic SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_dynsql)” chapter of Using Caché SQL. For further details on SelectMode options, refer to “[Data Display Options](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_basics#GSQL_basics_selectmode)” in the “Caché SQL Basics” chapter of Using Caché SQL.

The RESULTS clause specifies the results of a query. The SQL data type parameters in the RESULTS clause are translated into corresponding Caché data type parameters in the query’s ROWSPEC. For example, the RESULTS clause RESULTS ( Code VARCHAR(15) ) generates a ROWSPEC specification of ROWSPEC = “Code:%Library.String(MAXLEN=15)”.

language

A keyword clause specifying the language you are using for code\_body. Permitted clauses are LANGUAGE OBJECTSCRIPT (for Caché ObjectScript) or LANGUAGE SQL. If the LANGUAGE clause is omitted, SQL is the default.

code\_body

The program code for the method or query to be created. You specify this code in either SQL or Caché ObjectScript. The language used must match the LANGUAGE clause. However, code specified in Caché ObjectScript can contain embedded SQL. Caché uses the code you supply to generate the actual code of the method or query.

*   SQL program code is prefaced with a BEGIN keyword, followed by the SQL code itself. At the end of each complete SQL statement, specify a semicolon (;). A query contains only one SQL statement—a SELECT statement. You can also create procedures that insert, update, or delete data. SQL program code concludes with an END keyword.
    
    Input parameters are specified in the SQL statement as [host variables](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql#GSQL_esql_hvars), with the form :name. (Note that you should not use question marks (?) to specify input parameters in the SQL code. The procedure will successfully build, but when it is called these parameters cannot be passed or take default values.)
    
*   Caché ObjectScript program code is enclosed within curly braces: { code }. Lines of code must be indented. If specified, a label or a #Include preprocessor command must be prefaced by a colon and appear in the first column, as shown in the following example:
    
    CREATE PROCEDURE SP123()
      LANGUAGE OBJECTSCRIPT 
    {
    :Top
    :#Include %occConstant
      WRITE "Hello World"
      IF 0\=$RANDOM(2) { GOTO Top }
      ELSE {QUIT $$$OK }
    }
    
    Caché automatically includes %occInclude. If program code contains Caché Macro Preprocessor statements (# commands, ## functions, or $$$macro references) the processing and expansion of these statements is part of the procedure's method definition, and get processed and expanded when the method is compiled. For more details on preprocessor commands, see [ObjectScript Macros and the Macro Preprocessor](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GCOS_macros) in Using Caché ObjectScript.
    

Caché provides additional lines of code when generating the procedure that embed the SQL in a Caché ObjectScript “wrapper,” provide a procedure context handler, and handle return values. The following is an example of this Caché-generated wrapper code:

   NEW SQLCODE,%ROWID,%ROWCOUNT,title
   &sql(
        \-- code\_body
       )
   QUIT $GET(title)

If the code you specify is OBJECTSCRIPT, you must explicitly define the “wrapper” (which NEWs variable and uses QUIT val to return a value upon completion.

Examples

The examples that follow are divided into those that use an SQL code\_body, and those that use a Caché ObjectScript code\_body.

Examples Using SQL Code

The following example creates a simple query, named PersonStateSP, exposed as a stored procedure. It declares no parameters and takes default values for characteristics and language:

  WRITE !,"Creating a procedure"
  &sql(CREATE PROCEDURE PersonStateSP() BEGIN
       SELECT Name,Home\_State FROM Sample.Person ;
       END)
  IF SQLCODE\=0 { WRITE !,"Created a procedure" }
  ELSEIF SQLCODE\=\-361 { WRITE !,"Procedure already exists" }
  ELSE { WRITE !,"SQL error: ",SQLCODE }

 

You can go to the Management Portal, select the Classes option, then select the SAMPLES namespace. There you will find the stored procedure created by the above example: User.procPersonStateSP.cls. From this display you can delete this procedure before rerunning the above program example. You can, of course, use DROP PROCEDURE to delete a procedure:

  WRITE !,"Deleting a procedure"
  &sql(DROP PROCEDURE SAMPLES.PersonStateSP)
  IF SQLCODE\=0 { WRITE !,"Deleted a procedure" }
  ELSEIF SQLCODE\=\-362 { WRITE !,"Procedure did not exist" }
  ELSE { WRITE !,"SQL error: ",SQLCODE }

 

The following example creates a procedure to update data. It uses CREATE PROCEDURE to generate the method UpdateSalary in the class Sample.Employee:

CREATE PROCEDURE UpdateSalary ( IN SSN VARCHAR(11), IN Salary INTEGER )
   FOR Sample.Employee
   BEGIN
     UPDATE Sample.Employee SET Salary \= :Salary WHERE SSN \= :SSN;
   END

Examples Using ObjectScript Code

The following example creates the RandomLetterSP() stored procedure method that generates a random capital letter. You can then invoke this method as a function in a SELECT statement. A DROP PROCEDURE is provided to delete the RandomLetterSP() method.

CREATE PROCEDURE RandomLetterSP()
RETURNS INTEGER
LANGUAGE OBJECTSCRIPT
{
:Top
 SET x\=$RANDOM(90)
 IF x<65 {GOTO Top}
 ELSE {QUIT $CHAR(x)}
}

 

SELECT Name FROM Sample.Person
WHERE Name %STARTSWITH RandomLetterSP()

 

DROP PROCEDURE RandomLetterSP

 

The following CREATE PROCEDURE example uses Caché ObjectScript calls to the Execute(), Fetch(). and Close() methods. Such procedures may also contain FetchRows() and GetInfo() method calls:

CREATE PROCEDURE GetTitle()
    FOR Sample.Employee
    RESULTS (ID %Integer)
    CONTAINID 1
    LANGUAGE OBJECTSCRIPT
    Execute(INOUT qHandle %Binary)
    {  QUIT 1 }
    Fetch(INOUT qHandle %Binary, INOUT Row %List, INOUT AtEnd %Integer)
    {  QUIT 1 }
    Close(INOUT qHandle %Binary)
    {  QUIT 1 }

The following CREATE PROCEDURE example uses a Caché ObjectScript call to the [%SQL.Statement](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?../documatic/%25CSP.Documatic.cls?APP=1&LIBRARY=%25SYS&CLASSNAME=%25SQL.Statement) result set class:

CREATE PROCEDURE Sample\_Employee.GetTitle(
    INOUT Title VARCHAR(50) )
    RETURNS VARCHAR(30)
    FOR Sample.Employee
    LANGUAGE OBJECTSCRIPT
  {
  SET myquery\="SELECT TOP 10 Name,Title FROM Sample.Employee"
  ZNSPACE "SAMPLES"
  SET tStatement \= ##class(%SQL.Statement).%New()
  SET tStatus \= tStatement.%Prepare(myquery)
  SET rset \= tStatement.%Execute()
  DO rset.%Display()
  WRITE !,"End of data"
  }

If the Caché ObjectScript code block fetches data into a local variable (for example, Row), you must conclude the code block with the line SET Row="" to indicate an end-of-data condition.

The following example uses CREATE PROCEDURE with Caché ObjectScript code that invokes Embedded SQL. It generates the method GetTitle in the class Sample.Employee and passes out the Title value as a parameter:

CREATE PROCEDURE Sample\_Employee.GetTitle(
   IN SSN VARCHAR(11), 
   INOUT Title VARCHAR(50) )
    RETURNS VARCHAR(30)
    FOR Sample.Employee
    LANGUAGE OBJECTSCRIPT
    {
        NEW SQLCODE,%ROWCOUNT
        &sql(SELECT Title INTO :Title FROM Sample.Employee 
             WHERE SSN \= :SSN)
        IF $GET(%sqlcontext)'= "" {
           SET %sqlcontext.%SQLCODE\=SQLCODE
           SET %sqlcontext.%ROWCOUNT\=%ROWCOUNT }
           QUIT
     }

It uses the %sqlcontext object, and sets its %SQLCODE and %ROWCOUNT properties using the corresponding SQL variables. Note the curly braces enclosing the Caché ObjectScript code following the procedure’s LANGUAGE OBJECTSCRIPT keyword. Within the Caché ObjectScript code there is [Embedded SQL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_esql) code, marked by &sql and enclosed in parentheses.

See Also

*   [SELECT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_select)
    
*   [CALL](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_call)
    
*   [DROP PROCEDURE](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_dropprocedure)
    
*   [CREATE METHOD](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createmethod) [CREATE FUNCTION](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createfunction)
    
*   [GRANT](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_grant)
    
*   “[Defining and Using Stored Procedures](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_procedures)” chapter in Using Caché SQL.
    
*   “[Querying the Database](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=GSQL_queries)” chapter in Using Caché SQL
    

  

* * *

[Copyright](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?copyright.pdf "Display the copyright notice.") © 1997-2026, InterSystems Corp.

 [\[Back\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createmethod "Go back to the previous section.")    [\[Home\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls "Go to the home page.")    [\[Top of Page\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?#top "Go to the top of this page.")   [\[Next\]](http://localhost:57772/csp/docbook/DocBook.UI.Page.cls?DocBook.UI.Page.cls?KEY=RSQL_createquery "Go to the next section") 

Build: **Caché v2016.2 (736)**

Last updated: **2016-09-30 11:32:37**

Source: **RSQL\_createprocedure.xml**